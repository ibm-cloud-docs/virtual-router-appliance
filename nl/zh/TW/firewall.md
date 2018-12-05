---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 管理防火牆
Virtual Router Appliance (VRA) 能夠處理防火牆規則，以保護透過裝置遞送的 VLAN。VRA 中的防火牆可以分成兩個步驟：

1. 定義一組以上的規則。
2. 將一組規則套用至介面或區域。一個區域由一個以上的網路介面組成。

在建立防火牆規則之後務必要加以測試，以確保規則的運作符合預期，且新的規則不會限制對裝置的管理存取。

對 `dp0bond1` 介面操作規則時，建議使用 `dp0bond0` 連接至裝置。也可以使用「智慧型平台管理介面 (IPMI)」連接至主控台。

## 無狀態與有狀態
依預設，防火牆為無狀態，但可以視需要配置為有狀態。無狀態的防火牆將需要雙向資料流量規則，而有狀態的防火牆會追蹤連線並自動容許已接受流程的傳回資料流量。若要配置有狀態防火牆，您必須指定要以有狀態方式運作的規則。

若要啟用 `tcp`、`udp` 或 `icmp` 資料流量的「有狀態」追蹤，請執行下列指令：

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

請注意，`global-state-policy` 指令只會追蹤資料流量的狀態，而資料流量已符合明確地設定對應通訊協定的防火牆規則。例如：

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

因為 `GLOBAL_STATELESS` 未指定 `protocol tcp`，所以 `global-state-policy tcp` 指令不會套用至此規則。 

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

在此情況下，會明確地定義 `protocol tcp`。`global-state-policy tcp` 指令會啟用資料流量的有狀態追蹤，而資料流量符合 `GLOBAL_STATEFUL_TCP` 的規則 1


若要將個別防火牆規則設為「有狀態」，請執行下列指令：

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
這會啟用所有資料流量的有狀態追蹤，而資料流量可以進行有狀態地追蹤，並符合 `TEST` 規則 1，而不論 `global-state-policy` 指令是否存在。 

## 協助有狀態追蹤的 ALG
有些通訊協定（例如 FTP）會利用一般有狀態防火牆作業可追蹤的更複雜階段作業。
有些預先配置的模組會啟用以有狀態方式管理這些通訊協定。
除非需要有這些 ALG 模組才能成功使用個別的通訊協定，否則建議予以停用。

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## 防火牆規則集
防火牆規則會分組成具名集，以更輕鬆地將規則套用至多個介面。每一個規則集都有與其相關聯的預設動作。請考量下列範例：
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

在規則集 `ALLOW_LEGACY` 中，定義了兩個規則。第一個規則會捨棄來自名為 `network-group1` 之位址群組的任何資料流量。第二個規則會捨棄並記載目的地為 telnet 埠 (`tcp/23`) 且來自名為 `network-group2` 之位址群組的任何資料流量。default-action 表示接受任何其他內容。

## 容許資料中心存取
IBM 提供數個 IP 子網路，以提供服務及支援給資料中心內執行的系統。例如，DNS 解析器服務在 `10.0.80.11` 和 `10.0.80.12` 上執行。佈建和支援期間會使用其他子網路。您可以在[這裡](/docs/infrastructure/hardware-firewall-dedicated/ips.html)找到資料中心內所使用的 IP 範圍。

您可以將適當的 `SERVICE-ALLOW` 規則放在防火牆規則集開頭，並搭配 `accept` 的動作來容許資料中心存取。規則集必須套用的位置，取決於要實作的遞送和防火牆設計。

建議您將防火牆規則放在導致最少重複工作的位置。例如，容許 `dp0bond0` 上的後端子網路入埠，工作量會比容許對每個 VLAN 虛擬介面的後端子網路出埠要來得少。

### 每個介面的防火牆規則
在 VRA 上配置防火牆的一個方法是將防火牆規則集套用至每個介面。在此情況下，介面可以是資料平面介面 (`dp0s0`) 或虛擬介面 (`dp0bond0.303`)。每一個介面有三個可能的防火牆指派：

`in` - 會針對透過介面進入的封包檢查防火牆。這些封包可以遍訪或送往 VRA。

`out` - 會針對透過介面離開的封包檢查防火牆。這些封包可以遍訪或起源於 VRA。

`local` - 會針對直接送往 VRA 的封包檢查防火牆。

一個介面可在每一個方向套用多個規則集。它們會依配置順序套用。請注意，起源自 VRA 裝置的防火牆資料流量無法使用每個介面的防火牆。

例如，若要指派 `ALLOW_LEGACY` 規則集給 `bp0s1` 介面的 `in` 選項，您可以使用配置指令： 

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
Control Plane Policing (CPP) 提供對 Virtual Router Appliance 上之攻擊的抵禦，方法是容許您配置要指派給所需介面的防火牆原則，並且將這些原則套用至進入 VRA 的封包。

CPP 的實作時機是在 `local` 關鍵字用於指派給任何類型 VRA 介面的防火牆原則時，例如資料平面介面或迴圈。與套用於遍訪 VRA 之封包的防火牆規則不同，對於進入或離開控制平面之資料流量的防火牆規則，其預設動作是 `Allow`。如果不想要預設行為，則使用者必須新增明確的捨棄規則。

VRA 提供基本 CPP 規則集作為範本。您可以執行下列指令，將它合併至配置中： 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

合併此規則集之後，會新增名為 `CPP` 的新防火牆規則集，並將它套用至迴圈介面。建議您修改此規則集，以符合您的環境。

請注意，CPP 規則不能是有狀態的，而且只會套用至輸入資料流量。

## 區域防火牆
Virtual Router Appliance 內的另一個防火牆概念是以區域為基礎的防火牆。在以區域為基礎的防火牆作業中，會將介面指派給一個區域（每個介面只有一個區域），且防火牆規則集指派給區域之間的界限，而區域內的所有介面都具有相同的安全層次，且允許自由遞送。只有在資料流量從一個區域傳遞至另一個區域時，才會對資料流量進行詳細檢查。區域會捨棄未明確容許的任何進入資料流量。

介面可以屬於某個區域，或具有每個介面的防火牆配置；這兩者不能同時兼具。

想像有三個部門的下列辦公室情境，且每個部門都有自己的 VLAN： 

* 部門 A - VLAN 10 及 20（介面 dp0bond1.10 及 dp0bond1.20）
* 部門 B - VLAN 30 和 40（介面 dp0bond1.30 和 dp0bond1.40）
* 部門 C - VLAN 50（介面 dp0bond1.50）

可以為每個部門建立一個區域，該部門的介面可以新增至該區域。下列範例說明此點：
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

`commit` 指令會將每一個區域移入為介面，且預設捨棄規則會捨棄嘗試從外部進入區域的任何資料流量。在範例中，VLAN 10 和 VLAN 20 可以傳遞資料流量，因為它們在相同的區域 (`DEPARTMENTA`)，但 VLAN 10 和 VLAN 30 不能傳遞資料流量，因為 VLAN 30 在不同的區域 (`DEPARTMENTB`)。

每個區域內的介面可以自由傳遞資料流量，且可以針對區域之間的互動定義規則。規則集是透過從將一個區域離開至另一個區域的觀點來配置。 

下列指令顯示如何配置規則的範例：

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

此指令會將從 DEPARTMENTC 至 DEPARTMENTB 的轉移與名為 `ALLOW_PING` 的規則集產生關聯。會針對此規則集檢查從 DEPARTMENTC 區域進入 DEPARTMENTB 區域的資料流量。

請務必瞭解，從 DEPARTMENTC 區域到 DEPARTMENTB 區域的這項指派，不會對於反向進行任何陳述。如果沒有規則容許從 DEPARTMENTB 區域到 DEPARTMENTC 區域的資料流量，則資料流量（ICMP 回覆）將不會回到 DEPARTMENTC 中的主機。

在 DEPARTMENTB 區域的介面（dp0bond1.30 及 dp0bond1.40）上，會將 `ALLOW_PING` 套用為 `out` 防火牆。因為這是透過區域原則所安裝，所以只會針對規則集檢查源自來源區域之介面 (dp0bond1.50) 的資料流量。

## 階段作業及封包記載
VRA 支援兩種類型的記載：

1. 階段作業記載。請使用 ``security firewall session-log`` 指令來配置防火牆階段作業記載。
  
	若為 UDP、ICMP 及所有非 TCP 流程，階段作業會隨著流程的生命週期轉移到四種狀態。對於每一次轉移，您可以配置 VRA 來記載訊息。TCP 有大量的狀態轉移，每一個都可以配置為記載。  

2. 每個封包記載。在防火牆或 NAT 規則中包含關鍵字 ``log``，可以記載符合規則的每個網路封包。

	每個封包記載會在封包轉遞路徑中發生，並產生大量輸出。它可能會大幅減少 VRA 的傳輸量，並急劇地增加日誌檔的使用磁碟空間。建議只將個別封包記載用來進行除錯。對於所有作業用途，應該使用有狀態的階段作業記載。
