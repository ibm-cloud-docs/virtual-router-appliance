---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HA 和 VRRP
Virtual Router Appliance (VRA) 支援使用「虛擬路由器備援通訊協定 (VRRP)」作為高可用性通訊協定。裝置的部署是以主動/被動方式來執行，其中一部機器是主要機器，另一部是備份機器。這兩部機器上的所有介面都會是相同 "sync-group" 的成員，因此如果一個介面發生錯誤，則相同群組中的其他介面也會錯誤，裝置將會停止作為主要機器。現行備份機器將會偵測到主要機器不再播送「保持作用中/活動訊號訊息」，並奪取 VRRP 虛擬 IP 的控制權，成為主要機器。

佈建「閘道」時，VRRP 是配置中最重要的部分。高可用性功能取決於活動訊號訊息，因此務必要確定不會封鎖它們。

## VRRP 虛擬 IP (VIP) 位址

VRRP 虛擬 IP（或稱 VIP）是指當發生失效接手時，從主要裝置變更為備份裝置的浮動 IP 位址。當 VRA 部署時，它將具有公用及專用結合的網路連線，以及在每個介面上指派的實際 IP。無論裝置是獨立式還是在 HA 配對中，都會在兩個介面上指派 VIP。資料流量的目的地 IP 若在與 VRA 相關聯的 VLAN 中的子網路，則資料流量會直接傳送到這些 VRRP VIP。

任何閘道群組的 VRRP 虛擬 IP 位址都不應該變更，也不應該停用 VRRP 介面。這些 IP 位址是當 VLAN 已關聯時，資料流量用來遞送至閘道的方法。如果 IP 位址不存在，則無法將資料流量從 SoftLayer BCR/FCR 轉遞至閘道本身。

## VRRP 群組

VRRP 群組由介面或虛擬介面的叢集組成，這些介面或虛擬介面為群組中的主要介面提供備援。群組中的每個介面通常都在個別的路由器上。備援由每個系統上的 VRRP 處理程序管理。VRRP 群組具有唯一的數值 ID，且可以獲得指派最多 20 個虛擬 IP 位址。  

VRRP 群組 ID 是由 SoftLayer 指派，不應變更。當第一次在「前端客戶路由器 (FCR)」/「後端客戶路由器 (BCR)」後面佈建閘道群組時，它會是 VRRP 群組 1。後續的閘道群組佈建將遞增此值以防止衝突，下一個群組將為群組 2，然後是群組 3，依此類推。接著 SoftLayer 佈建處理程序會計算並指派它。變更此值可能會導致與其他作用中群組的衝突，然後發生主要/主要競用，這可能會導致兩個閘道群組都停止。

如果您是從前一個配置移轉，建議您再確認配置程式碼，以確定 VRRP 群組 ID 不是以靜態方式指派。

VRRP 群組 ID 會持續保存在 SoftLayer 資料庫中，因此在 OS 重新載入或升級期間會使用相同的群組 ID 值。在 OS 重新載入期間，使用者修改的任何 VRRP 群組 ID 都將會改寫為系統指派值。  

## VRRP 優先順序

閘道群組中的第一部機器的優先順序為 254，而對下一台佈建的裝置將會遞減此值。您絕不應該將優先順序設為 255，因為這會把 VIP 定義為「位址擁有者」，當使用與執行中的作用中主要伺服器不同的配置讓機器連線時，可能會產生非預期的行為。

## VRRP 先占

應該一律在所有 VRRP 介面上停用先占，這樣新的裝置或正在重新載入 OS 的裝置才不會嘗試接管叢集。

## VRRP 通告間隔

為了指出它仍在服務中，主要介面或 VIF 會將稱為「公告」「活動訊號」封包傳送至 LAN 區段，方法為針對 VRRP 使用 IANA 指派的多重播送位址（IPv4 為 `224.0.0.18`，IPv6 為 `FF02:0:0:0:0:0:0:12`）。

依預設，間隔會設為 1。可以增加這個值，但不建議您將值設定為超過 5。在繁忙的網路上，所有 VRRP 通知要從主要裝置抵達備份裝置的時間，可能會比一秒鐘長上許多，因此應該針對高資料流量網路使用 2 這個值。

## VRRP 同步化 (sync-group)

VRRP 同步化群組 ("sync group") 中的介面會同步化，以便如果群組中的其中一個介面失效接手至備份介面，則群組中的所有介面都會失效接手至備份介面。例如，在許多情況中，如果主要路由器上的一個介面故障，則整個路由器會失效接手至備份路由器。

此值與 VRRP 群組不同，因為它定義了當該群組中的介面登錄錯誤時，裝置上的哪些介面將會失效接手。建議所有介面都屬於相同的同步化群組，否則部分介面將會是主要介面且具有閘道 IP，而其他介面則是備份介面，資料流量將不再能正確地轉遞。不支援主動/主動配置。

## VRRP rfc-compatibility

rfc-compatibility 會啟用或停用介面上 VRRP 的 RFC 3768 MAC 位址。在原生介面上應該啟用，但在任何已配置的 VIF 上則不應啟用。部分虛擬交換器（大部分是 vmware）在啟用此項時發生問題，並將導致捨棄資料流量，而不會從 Hypervisor 主機傳送至閘道 IP。請不要理會此設定，且不要配置任何 VIF 的設定。

## VRRP 鑑別
如果為 VRRP 鑑別設定密碼，則也必須定義鑑別類型。如果已設定密碼，且未定義鑑別類型，則當您嘗試確定配置時，系統會產生錯誤。

同樣地，在不刪除 VRRP 鑑別類型的情況下，您無法刪除 VRRP 密碼。如果這樣做，當您嘗試確定配置時，系統會產生錯誤。如果您同時刪除 VRRP 鑑別密碼和鑑別類型，則會停用 VRRP 鑑別。

IETF 決定鑑別不要用於 VRRP 第 3 版。如需相關資訊，請參閱 RFC 5798 VRRP。

## 版本 2/3 支援
VRA 支援 VRRP 第 2 版（預設值）和第 3 版通訊協定。在第 2 版中，您不能同時有 IPv4 和 IPv6。但在第 3 版中，您可以同時擁有 IPv4 和 IPv6。

## 搭配 VRRP 的高可用性 VPN
VRA 能夠藉由使用一對 Virtual Router Appliance 搭配 VRRP，而維護通過一條 IPsec 通道的連線功能。當一台路由器故障或是被卸下以進行維護時，新的 VRRP 主要路由器會還原本端與遠端網路之間的 IPsec 連線功能。

配置高可用性 VPN 搭配 VRRP 時，只要新增 VRRP 虛擬位址至 VRA 介面，您就必須重新起始設定 IPsec 常駐程式，因為 IPsec 服務只會接聽對於起始設定「網際網路金鑰交換 (IKE)」服務常駐程式時，VRA 上存在之位址的連線。

在主要和備份路由器上執行下列指令，以便在發生失效接手時，VIP 轉換之後，會在新的主要裝置上重新啟動 IPsec 常駐程式，如下列範例所示：

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## 搭配 VRRP 的高可用性防火牆

當兩台裝置處於 HA 配對時，您的主要裝置必須小心，不要封鎖另一台裝置的存取權。必須容許兩台裝置之間的埠 443，以便 config-sync 能運作，且必須容許傳送及接收 VRRP，包括 224.0.0.0/24 的多重播送範圍。

## 相關聯的 VLAN 子網路與 VRRP

對於具有 VRRP 的任何 VIF 配置，您將需要虛擬 IP 位址（子網路中的第一個可用 IP，該子網路上一切內容將遞送到的閘道 IP），以及兩台裝置上 VIF 的實際介面 IP 位址。若要節省可用的 IP，建議實際介面 IP 使用頻外範圍，例如 192.168.0.0/30，其中一台裝置的位址為 .1，而另一台裝置的位址為 .2。您可以具有多個 VRRP 虛擬 IP，但每個 VIF 都只需要一個真實位址。

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

* vrrp sync-group 與 vrrp 群組不同。屬於 sync-group 的介面變更狀態時，相同 sync-group 的所有其他成員都會轉移為相同的狀態。 
* VLAN 介面 (VIF) 的 vrrp-group 數目不需要與原生介面或其他 VLAN 的 vrrp-group 數目相同。不過，強烈建議保留一個 vrrp-group 中相同 VLAN 的所有虛擬位址，如 VLAN 10 中所示。
* 原生 VLAN 上的實際介面位址（例如：dp0bond1: 169.110.20.28/29）不一定是在與其 VIP (169.110.21.26/29) 相同的子網路。 

## 手動 VRRP 失效接手
如果您需要強制執行 vrrp 失效接手，則在目前為 VRRP 主節點的裝置上執行下列作業即可達成：

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

群組 ID 是原生介面的 vrrp 群組 ID，而且如上所示，在您的配對中可能會不同。

## 連線同步化
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
