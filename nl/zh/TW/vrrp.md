---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, vrrp, vip

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# 使用高可用性及 VRRP
{: #working-with-high-availability-and-vrrp}

{{site.data.keyword.vra_full}} (VRA) 支援使用「虛擬路由器備援通訊協定 (VRRP)」作為高可用性通訊協定。裝置的部署是以主動/被動方式來執行，其中一部機器是主要機器，另一部是備份機器。這兩部機器上的所有介面都會是相同 "sync-group" 的成員，因此如果一個介面發生錯誤，則相同群組中的其他介面也會錯誤，裝置將會停止作為主要機器。現行備份機器將會偵測到主要機器不再播送「保持作用中/活動訊號訊息」，並奪取 VRRP 虛擬 IP 的控制權，成為主要機器。

佈建「閘道」時，VRRP 是配置中最重要的部分。高可用性功能取決於活動訊號訊息，因此務必要確定不會封鎖它們。

## VRRP 虛擬 IP (VIP) 位址
{: #vrrp-virtual-ip-vip-addresses}

`dp0bond1` 或 `dp0bond0` 的 VRRP 虛擬 IP 或 VIP 是指，發生失效接手時，從主要裝置變更為備份裝置的浮動 IP 位址。當 VRA 部署時，它將具有公用及專用結合的網路連線，以及在每個介面上指派的實際 IP。無論裝置是獨立式還是在 HA 配對中，都會在兩個介面上指派 VIP。資料流量的目的地 IP 若在與 VRA 相關聯的 VLAN 中的子網路，則資料流量會透過 FCR/BCR 上的靜態路徑直接傳送到這些 VRRP VIP。

任何閘道群組的 VRRP 虛擬 IP 位址永遠不應該變更，也不應該停用 VRRP 介面。這些 IP 位址是當 VLAN 已關聯時，資料流量用來遞送至閘道的方法。因此，如果關閉它們，也會關閉 VLAN 資料流量。如果 IP 位址不存在，則無法將資料流量從 SoftLayer BCR/FCR 轉遞至 VRA 本身。此時無法變更這個虛擬位址或 VIP。此限制可能會在未來有所變更，但目前既不支援在 POD/FCR/BCR 之間移轉 VRA，也不支援變更 VIP。

下列是特定 VRA 之 `dp0bond0` 和 `dp0bond1` VIP 的預設配置範例。請注意，您的 IP 位址和 vrrp-groups 可能與下列範例不同：

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 virtual-address '10.127.170.2/26'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 virtual-address '159.8.98.214/29'
```

如需配置 VIF 虛擬位址的相關資訊，請參閱[將多個子網路新增至單一 VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans#add-multiple-subnets-to-a-single-vlan)或[建立 VLAN 子網路與 VRRP 的關聯](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp)。

**依預設，VRRP 會設為已停用。**這可確保新的佈建和重新載入不會導致「主要」裝置發生中斷。若要讓 VLAN 資料流量能夠運作，在佈建或重新載入之後，就必須重新啟用 VRRP。下列範例詳述預設配置：

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

若要在佈建或重新載入之後，在這兩個介面上啟用 VRRP（**獨立式及 HA 配對都需要這樣做**），請執行下列指令，然後確定變更：

```
delete interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
delete interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

## VRRP 群組
{: #vrrp-group}

VRRP 群組由介面或虛擬介面的叢集組成，這些介面或虛擬介面為群組中的主要介面提供備援。群組中的每個介面通常都在個別的路由器上。備援由每個系統上的 VRRP 處理程序管理。VRRP 群組具有唯一的數值 ID，且可以獲得指派最多 20 個虛擬 IP 位址。  

VRRP 群組 ID 是由 SoftLayer 指派，不應變更。當第一次在「前端客戶路由器 (FCR)」/「後端客戶路由器 (BCR)」後面佈建閘道群組時，它會是 VRRP 群組 1。後續的閘道群組佈建將遞增此值以防止衝突，下一個群組將為群組 2，然後是群組 3，依此類推。接著 SoftLayer 佈建處理程序會計算並指派它。變更此值可能會導致與其他作用中群組的衝突，然後發生主要/主要競用，這可能會導致兩個閘道群組都停止。

如果您是從前一個配置移轉，建議您再確認配置程式碼，以確定 VRRP 群組 ID 不是以靜態方式指派。

VRRP 群組 ID 會持續保存在 SoftLayer 資料庫中，因此在 OS 重新載入或升級期間會使用相同的群組 ID 值。在 OS 重新載入期間，使用者修改的任何 VRRP 群組 ID 都將會改寫為系統指派值。  

## VRRP 優先順序
{: #vrrp-priority}

閘道群組中的第一部機器的優先順序為 254，而對下一台佈建的裝置將會遞減此值。您絕不應該將優先順序設為 255，因為這會把 VIP 定義為「位址擁有者」，當使用與執行中的作用中主要伺服器不同的配置讓機器連線時，可能會產生非預期的行為。

## VRRP 先占
{: #vrrp-preemption}

應該一律在所有 VRRP 介面上停用先占，這樣新的裝置或正在重新載入 OS 的裝置才不會嘗試接管叢集。

## VRRP 通告間隔
{: #vrrp-advertise-interval}

為了指出它仍在服務中，主要介面或 VIF 會將稱為「公告」「活動訊號」封包傳送至 LAN 區段，方法為針對 VRRP 使用 IANA 指派的多重播送位址（IPv4 為 `224.0.0.18`，IPv6 為 `FF02:0:0:0:0:0:0:12`）。

依預設，間隔會設為 1。可以增加這個值，但不建議您將值設定為超過 5。在繁忙的網路上，所有 VRRP 通知要從主要裝置抵達備份裝置的時間，可能會比一秒鐘長上許多，因此應該針對高資料流量網路使用 2 這個值。

## VRRP 同步化 (sync-group)
{: #vrrp-synchronization-sync-group-}

VRRP 同步化群組 ("sync group") 中的介面會同步化，以便如果群組中的其中一個介面失效接手至備份介面，則群組中的所有介面都會失效接手至備份介面。例如，在許多情況中，如果主要路由器上的一個介面故障，則整個路由器會失效接手至備份路由器。

此值與 VRRP 群組不同，因為它定義了當該群組中的介面登錄錯誤時，裝置上的哪些介面將會失效接手。建議所有介面都屬於相同的同步化群組，否則部分介面將會是主要介面且具有閘道 IP，而其他介面則是備份介面，資料流量將不再能正確地轉遞。不支援主動/主動配置。

## VRRP rfc-compatibility
{: #vrrp-rfc-compatibility}

rfc-compatibility 會啟用或停用介面上 VRRP 的 RFC 3768 MAC 位址。在原生介面上應該啟用，但在任何已配置的 VIF 上則不應啟用。部分虛擬交換器（大部分是 vmware）在啟用此項時發生問題，並將導致捨棄資料流量，而不會從 Hypervisor 主機傳送至閘道 IP。請不要理會此設定，且不要配置任何 VIF 的設定。

## VRRP 鑑別
{: #vrrp-authenticaton}

如果為 VRRP 鑑別設定密碼，則也必須定義鑑別類型。如果已設定密碼，且未定義鑑別類型，則當您嘗試確定配置時，系統會產生錯誤。

同樣地，在不刪除 VRRP 鑑別類型的情況下，您無法刪除 VRRP 密碼。如果這樣做，當您嘗試確定配置時，系統會產生錯誤。如果您同時刪除 VRRP 鑑別密碼和鑑別類型，則會停用 VRRP 鑑別。

IETF 決定鑑別不要用於 VRRP 第 3 版。如需相關資訊，請參閱 RFC 5798 VRRP。

## 版本 2/3 支援
{: #version-2-3-support}

VRA 支援 VRRP 第 2 版（預設值）和第 3 版通訊協定。在第 2 版中，您不能同時有 IPv4 和 IPv6。但在第 3 版中，您可以同時擁有 IPv4 和 IPv6。

## 搭配 VRRP 的高可用性 VPN
{: #high-availability-vpn-with-vrrp}

VRA 能夠藉由使用一對 {{site.data.keyword.vra_full}} 搭配 VRRP，而維護通過一條 IPsec 通道的連線功能。當一台路由器故障或是被卸下以進行維護時，新的 VRRP 主要路由器會還原本端與遠端網路之間的 IPsec 連線功能。

配置高可用性 VPN 搭配 VRRP 時，只要新增 VRRP 虛擬位址至 VRA 介面，您就必須重新起始設定 IPsec 常駐程式，因為 IPsec 服務只會接聽對於起始設定「網際網路金鑰交換 (IKE)」服務常駐程式時，VRA 上存在之位址的連線。

在主要和備份路由器上執行下列指令，以便在發生失效接手時，VIP 轉移之後，會在新的主要裝置上重新啟動 IPsec 常駐程式，如下列範例所示：

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## 搭配 VRRP 的高可用性防火牆
{: #high-availability-firewalls-with-vrrp}

當兩台裝置處於 HA 配對時，您的主要裝置必須小心，不要封鎖另一台裝置的存取權。必須容許兩台裝置之間的埠 443，以便 config-sync 能運作，且必須容許傳送及接收 VRRP，包括 224.0.0.0/24 的多重播送範圍。

## 相關聯的 VLAN 子網路與 VRRP
{: #associated-vlan-subnets-with-vrrp}

對於具有 VRRP 的任何 VIF 配置，您將需要虛擬 IP 位址（子網路中的第一個可用 IP，該子網路上一切內容將遞送到的閘道 IP），以及兩台裝置上 VIF 的實際介面 IP 位址。若要節省可用的 IP，建議實際介面 IP 使用頻外範圍，例如 192.168.0.0/30，其中一台裝置的位址為 .1，而另一台裝置的位址為 .2。您可以具有多個 VRRP 虛擬 IP，但每個 VIF 都只需要一個實際位址。

以下是具有兩個專用 VLAN 及三個子網路的 VRRP 配置範例：

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* VRRP sync-group 與 VRRP 群組不同。屬於 sync-group 的介面變更狀態時，相同 sync-group 的所有其他成員都會轉移為相同的狀態。
*  VLAN 介面 (VIF) 的 vrrp-group 號碼應該幾乎一律與其相符的原生介面相同。例如，`dp0bond1.789` 應該一律具有與 `dp0bond1` 相同的 vrrp-group 號碼，除非這兩個介面共用相同的 sync-group。對 VIF 具有不同的群組號碼，且原生介面可能會在與不同的同步群組配對時造成「裂腦」狀況。當這兩個介面共用相同的 sync-group 時，即使它們是在不同的 vrrp-group 上，也會同時在主要介面與備份介面之間轉移，以防止裂腦。另一方面，如果原生介面設定了不同的 vrrp-group 號碼和不同的 sync-group 作為 VLAN 介面，因為 VLAN 介面上設定的子網路會靜態遞送至原生介面上的虛擬位址，但如果 VLAN 顯示為備份，而原生介面是主要的，則路由器仍會將 VLAN 介面資料流量傳送至原生介面是主要介面的 RVA，即使 VLAN 介面是次要介面，而且在 VRA 上不是作用中也一樣。此特定狀況將導致「裂腦」及運作中斷。這就是為何明白如何一起設定 sync-group 與 vrrp-group 極其重要的原因。在相同傳輸 VLAN 的多個 HA VRA 配對之間不使用相同 vrrp-group 號碼也是非常重要的，因為使用相同群組的四個 Vyatta 將會導致三個 VRA，潛在假設只有一個是「主要」時，「備份」狀態會導致運作中斷。
* 原生 VLAN 上的實際介面位址（例如：dp0bond1: 169.110.20.28/29）不一定是在與其 VIP (169.110.21.26/29) 相同的子網路。


## 手動 VRRP 失效接手
{: #manual-vrrp-failover}

如果您需要強制執行 VRRP 失效接手，則在目前為 VRRP 主節點的裝置上執行下列作業即可達成：

`vyatta@vrouter$ reset VRRP master interface dp0bond0 group 1`

群組 ID 是原生介面的 VRRP 群組 ID，而且如上所述，在您的配對中可能會不同。

## 連線同步化
{: #connection-synchronization}

當兩台 VRA 裝置處於 HA 配對時，追蹤兩台裝置之間的有狀態連線可能很有用，如果發生失效接手，在故障裝置上的所有連線的現行狀態會抄寫到備份裝置，以便任何現行作用中階段作業（例如 SSL 連線）不需要從頭重建，這可能會導致使用者體驗受到干擾。

這稱為連線追蹤同步化。

若要進行配置，您必須宣告失效接手方法、您將用來傳送連線追蹤資訊的介面，以及遠端對等節點的 IP：

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

另一個 VRA 將具有相同的配置，但具有不同的 `remote-peer`。

請注意，如果在其他介面上有大量連線進入，且將與宣告鏈結上的其他資料流量競爭，則這可能會導致鏈結飽和。

## VRRP 啟動延遲特性
{: #vrrp-start-delay-feature}

Vyatta OS 版本 1801p 及更新版本包括新的 `vrrp` 指令。

`vrrp` 指定選取通訊協定，動態將虛擬路由器的責任指派給 LAN 上的其中一個 VRRP 路由器。控制與虛擬路由器相關聯之 IPv4 或 IPv6 位址的 VRRP 路由器稱為「主節點」，而且它會轉遞傳送至這些 IPv4 或 IPv6 位址的封包。選取處理程序會在轉遞責任中提供動態失效接手，因此「主節點」會變成無法使用。所有通訊協定傳訊都是使用 IPv4 或 IPv6 多點播送資料包來執行；因此，通訊協定可以透過支援 IPv4/IPv6 多重播送的各種多重存取 LAN 技術來運作。

若要最小化網路資料流量，只有每個虛擬路由器的「主節點」才會傳送定期「VRRP 公告」訊息。除非「備份」路由器的優先順序較高，否則「備份」路由器不會嘗試先占「主節點」。除非有更偏好的路徑可用，否則這會消除服務中斷。也可以由管理方式禁止所有先佔嘗試。如果「主節點」變成無法使用，則最高優先順序「備份」會在短暫延遲之後轉移至「主節點」，從而提供虛擬路由器的受管制轉移責任，而且服務岔斷最小。

在 IBM© Cloud 佈建的部署中，啟動延遲值會設為預設值。建議您視需要變更此值，因為您會測試失效接手及高可用性方法。
{: note}

### 先占與無先占
{: #preemption-vs-no-preemption}

`vrrp` 通訊協定所定義的邏輯可決定網路上哪個 VRRP 對等節點具有較高的優先順序，因此成為將角色執行為「主節點」的最佳對等節點。使用預設配置，將會啟用 VRRP 來執行先占，這表示網路上的新較高優先順序對等節點會強制失效接手「主節點」角色。

停用先占時，如果網路上不再有現有較低優先順序對等節點，則較高優先順序對等節點只會失效接手「主節點」角色。停用先占有時在真實世界情境下十分有用，因為它可以妥善處理下列情況：基於對等節點本身或它的其中一個網路連線的可靠性問題，可能已啟動較高優先順序對等節點來定期飄動。它也適用於防止過早失效接手至尚未完成網路聚合的新較高優先順序對等節點。

### 先占的假設及限制
{: #assumptions-and-limitations-of-preemption}

如果 VRRP 對等節點已配置為停用先占，則在某些情況下，「似乎」可能會發生先占，但實際上，此情境只是標準 VRRP 失效接手。如上所述，VRRP 會使用 IP 多重播送資料包作為確認目前已選取「主節點」路由器之可用性的方法。因為它是偵測到 VRRP 對等節點失敗的第 3 層通訊協定，所以 VRRP 中的失效接手偵測會延遲至 VRRP，而且已備妥並聚合網路堆疊的第 1 層到第 2 層。在某些情況下，執行 VRRP 的網路介面可能會確認到啟動該介面的通訊協定，但其他基礎服務（如 Spanning Tree 或 Bonding）可能尚未完成。因此，無法建立對等節點之間的 IP 連線功能。如果發生此情況，則新較高對等節點上的 VRRP 會變成「主節點」，因為它偵測不到來自網路上現行「主節點」對等節點的 VRRP 訊息。聚合之後，會有短暫時間有兩個「主節點」VRRP 對等節點，這會導致執行 VRRP 的雙重「主節點」邏輯、較高優先順序對等節點將會保持「主節點」，而較低優先順序會變成「備份」。此情境「似乎」會示範「無先占」功能中的失敗。

### 啟動延遲特性
{: #start-delay-feature}

為了容納在介面啟動事件期間聚合較低層次網路堆疊之延遲有關的問題，以及其他促成因素，在 1801p 修補程式中引進稱為「啟動延遲」的新特性。此特性會讓已「重新載入」的機器上的 VRRP 狀態保持處於 INIT 狀態，直到預先定義的延遲（可由網路操作員配置）為止。此延遲值的彈性可讓網路操作員在實際情況下自訂其網路及裝置的特徵。

### 指令詳細資料
{: #command-details}

```
vrrp {
start-delay <0-600>
}
```

`start-delay` 的值可以介於 0（預設值）與 600 秒之間。

### 範例配置
{: #example-configuration}

```
interfaces {
    bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
