---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vlan, associate, gateway, route, bypass, disassociate

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# 管理 VLAN
{: #managing-your-vlans}

您可以從[閘道應用裝置詳細資料畫面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)執行各種動作。

## 使 VLAN 與閘道應用裝置產生關聯
{: #associate-a-vlan-to-a-gateway-appliance}

VLAN 需要先與「閘道應用裝置」相關聯，然後才能遞送。VLAN 關聯是將合格的 VLAN 鏈結至「網路閘道」，使得它未來可能會遞送至「閘道應用裝置」。關聯的處理程序不會自動將 VLAN 遞送至「閘道應用裝置」；VLAN 會繼續使用前端及後端客戶路由器，直到將它遞送至「閘道」為止。

VLAN 一次只能與一個「閘道」相關聯，且不能有防火牆。請執行下列程序，使 VLAN 與「網路閘道」產生關聯。

1. 在「客戶入口網站」中[存取「閘道應用裝置詳細資料」畫面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)。
2. 從**使 VLAN 產生關聯**下拉清單中選取想要的 VLAN。
3. 按一下**關聯**按鈕，以使 VLAN 產生關聯。

將 VLAN 與「閘道應用裝置」相關聯之後，它會出現在「閘道應用裝置詳細資料」畫面的「相關聯的 VLAN」區段中。在此區段中，VLAN 可以遞送至「閘道」，或與「閘道」取消關聯。您隨時可以重複上述步驟，使其他合格的 VLAN 與「閘道應用裝置」相關聯。

## 遞送相關聯的 VLAN
{: #route-an-associated-vlan}

相關聯的 VLAN 鏈結至「閘道應用裝置」，但在 VLAN 遞送之前，進出 VLAN 的資料流量不會到達「閘道」。遞送相關聯的 VLAN 之後，所有前端及後端資料流量會透過「閘道應用裝置」來遞送，這與客戶路由器相反。

請執行下列程序來遞送關聯的 VLAN：

1. 在「客戶入口網站」中[存取「閘道應用裝置詳細資料」畫面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)。
2. 在「相關聯的 VLAN」區段中，找出想要的 VLAN。
3. 從「動作」下拉功能表，選取**遞送 VLAN**。
4. 按一下**是**以遞送 VLAN。

遞送 VLAN 之後，所有前端及後端資料流量會從客戶路由器移至「網路閘道」。與資料流量及「閘道應用裝置」本身相關的其他控制項，可透過存取「閘道」的管理工具來取得。透過[略過「閘道應用裝置」](#bypass-gateway-appliance-routing-for-a-vlan)，即可隨時停止透過「網路閘道」進行的遞送。

## 略過 VLAN 的閘道應用裝置遞送
{: #bypass-gateway-appliance-routing-for-a-vlan}

在遞送了 VLAN 之後，所有前端及後端資料流量都會通過「網路閘道」。隨時都可以略過「閘道應用裝置」，使資料流量回到前面和後端客戶路由器（FCR 和 BCR）。

略過 VLAN 可讓 VLAN 保持與「網路閘道」的關聯。如果 VLAN 應該不再與「閘道應用裝置」相關聯，請參閱[使 VLAN 與閘道應用裝置取消關聯](#disassociate-a-vlan-from-a-gateway-appliance)。

請執行下列程序，以略過 VLAN 的閘道遞送：

1. 在「客戶入口網站」中[存取「閘道應用裝置詳細資料」畫面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)。
2. 在「相關聯的 VLAN」區段中，找出想要的 VLAN。
3. 從「動作」下拉功能表，選取**略過 VLAN**。
4. 按一下**是**以略過閘道。

略過「網路閘道」之後，所有前端及後端資料流量都會透過與 VLAN 相關聯的 FCR 及 BCR 遞送。VLAN 將繼續與「閘道應用裝置」相關聯，且隨時可遞送回到「閘道應用裝置」。

## 取消 VLAN 與閘道應用裝置的關聯
{: #disassociate-a-vlan-from-a-gateway-appliance}

VLAN 可透過[關聯](#associate-a-vlan-to-a-gateway-appliance)，一次鏈結至一個「閘道應用裝置」。關聯可容許隨時將 VLAN 遞送至「閘道應用裝置」。如果應該將 VLAN 關聯至另一個「閘道應用裝置」，或者 VLAN 應該不再與其「閘道」相關聯，則需要取消關聯。取消關聯會移除從 VLAN 到「閘道應用裝置」的「鏈結」，並視需要容許它與另一個「閘道」相關聯。

請執行下列程序，使 VLAN 與「閘道應用裝置」取消關聯：

1. 在「客戶入口網站」中[存取「閘道應用裝置詳細資料」畫面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)。
2. 在「相關聯的 VLAN」區段中，找出想要的 VLAN。
3. 從**動作**下拉功能表，選取**取消關聯**。
4. 按一下**是**以取消 VLAN 的關聯。

將 VLAN 與「閘道應用裝置」取消關聯之後，VLAN 可以與另一個「閘道」相關聯。VLAN 也可以隨時回到與「閘道應用裝置」相關聯。將 VLAN 與「閘道應用裝置」取消關聯之後，VLAN 的資料流量無法透過「閘道」遞送。VLAN 必須先與「閘道應用裝置」相關聯，才能遞送。

## 透過相同的網路介面遞送多個 VLAN
{: #route-multiple-vlans-over-same-network-interface}

Virtual Router Appliance 可以透過相同的網路介面來遞送多個 VLAN（例如，`dp0bond0` 或 `dp0bond1`）。作法是將交換器埠設為幹線模式，並在裝置上配置虛擬介面 (VIF)。

例如：

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

以上指令會在 `dp0bond0` 介面上建立兩個虛擬介面。介面 `dp0bond0.1432` 會處理 VLAN 1432 的資料流量，而介面 `dp0bond0.1693` 會處理 VLAN 1693 的資料流量。

## 將多個子網路新增至單一 VLAN
{: #add-multiple-subnets-to-a-single-vlan}

下列配置範例最後包含新增公用 VLAN (1451) 的範例子網路 (`159.8.67.96/28`)。每個 VIF（VLAN 介面）的位址並不會在 BCR（後端客戶路由器）或 FCR（前端客戶路由器）中遞送。其只用於兩個 Vyatta 之間的 VRRP/高可用性通訊。

可以從任何未使用的專用 IP 空間中選擇子網路。因此，這裡一般已排除 `10.0.0.0/8`。下列範例選擇來自 `192.168.0.0/16`的子網路，但是也可以使用來自 `172.16.0.0/12` 的子網路。

新的子網路應該在 `virtual-address` 中進行配置。在大部分情況下，應該配置的是子網路的閘道 IP 位址。然後，連結至 VIF 的閘道 IP 會用作在新子網路（在 VRA 後面）上所設定之任何「裸機伺服器」或「虛擬伺服器」的下一個躍點或預設路徑。

下列範例顯示 `159.8.67.97/28` 連結至 VIF，因此，`159.8.67.98/28` 子網路的所有資料流量都可以由 VRA 管理。

```
set interfaces bonding dp0bond0 vif 1623 address '192.168.10.2/30'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 virtual-address '10.127.132.129/26'
set interfaces bonding dp0bond0 vif 1750 address '192.168.20.2/30'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 virtual-address '10.126.19.129/26'
set interfaces bonding dp0bond1 vif 788 address '192.168.150.2/30'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 virtual-address '159.8.106.129/28'
set interfaces bonding dp0bond1 vif 1451 address '192.168.200.2/30'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.67.97/28'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.86.49/29'
```
