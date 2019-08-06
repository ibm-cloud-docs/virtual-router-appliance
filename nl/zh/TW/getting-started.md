---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

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


# 開始使用 {{site.data.keyword.vra_full}}
{: #getting-started}

{{site.data.keyword.vra_full}} (VRA) 為 x86 裸機伺服器提供最新的 Vyatta 5600 作業系統。它是以「高可用性 (HA)」或獨立式配置的方式來提供，可讓您透過具有防火牆、資料流量塑形、原則型遞送、VPN 及其他特性的完整特性企業路由器，選擇性地遞送專用與公用網路資料流量。

VRA 最低伺服器需求，針對每 10 Gbps 的網路容量，需要 8 GB 的 RAM 以及一個 CPU 核心。例如，具有雙重 10 Gbps 公用及專用上行鏈路的系統至少需要四個核心。此外，如果您的目的是設定有加密的 VPN 服務，則可能會想要新增其他核心。新增「VPN 服務」的其他核心可確保 VRA 在遞送及同時加密/解密資料時不會因大量負載而動彈不得。

## 訂購 {{site.data.keyword.vra_full}}
{: #order-vra}

若要訂購 VRA，請執行下列程序：

1. 從您的瀏覽器中，開啟 [IBM Cloud 使用者介面主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} 中的「閘道應用裝置」，並登入您的帳戶。 

  您也可以藉由選取 [IBM Cloud 型錄 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com) 左上方的導覽功能表，然後選取**標準基礎架構 > 網路 > 閘道應用裝置**，來到達此頁面。
  {: tip}

2. 從**閘道供應商**區段中，選取 **AT&T** 選項（選取時，按鈕上會出現藍色勾號）。從同一個按鈕上的下拉清單中，選擇您的頻寬（20Gbps 或 2Gbps）。

  	<img src="images/ordering_vra.png" alt="圖片" style="width: 500px;"/>

3. 從**閘道應用裝置**區段中，輸入**主機名稱**和**網域**名稱資訊。這些欄位中已移入預設資訊，因此確保這些值是正確的。需要時，請勾選**高可用性**選項，然後從下拉功能表中選取您需要的資料中心**位置**，以及您想要的 **Pod**。

  只有已具有關聯 VLAN 的 Pod 才會在這裡顯示。如果您想要在未列出的 Pod 中佈建「閘道應用裝置」，請先在該處建立一個 VLAN。
  {: note}

4. 從**配置**區段中，藉由選取 RAM 和 SSH 金鑰（必要的話）來選擇您的處理器。

  <img src="images/ordering_vra_2.png" alt="圖片" style="width: 600px;"/>
  
  即會根據您在步驟 2 中選取的授權版本，為您選擇適當的處理器。不過，您可以選擇不同的 RAM 配置。
  {: note}

5. 從**儲存空間磁碟**區段中，選擇符合儲存空間需求的選項。 

  RAID0 和 RAID1 選項可增加保護以防止資料流失，因為它們是緊急備用選項（主要元件失敗時，可立即提供服務的備份元件）。
  {: note}

  每個 VRA 最多可有四個磁碟。具有 RAID 配置的「磁碟大小」是可用磁碟大小，因為 RAID 配置已鏡映。
  {: note}

  如果您計劃執行會產生詳細日誌的網路診斷程式，請保留比預設磁碟設定還要多的磁碟。
  {: tip}

6. 從**網路介面**區段中，選取**上行鏈路埠速度**。預設選項是單一介面，但也有備援和僅限專用選項。請選擇最適合您需要的選項。

  「網路介面」**附加程式**區段可讓您選取 IPv6 位址（必要的話），並顯示任何其他併入的預設選項。 
  
8. 檢閱您的選項、確認您已閱讀「協力廠商服務合約」，然後按一下**建立**。系統即會自動驗證訂單。

在核准訂單之後，{{site.data.keyword.vra_full}} 的佈建就會自動開始。佈建處理程序完成時，新的 VRA 會出現在「閘道應用裝置」清單頁面中。請按一下閘道名稱，以開啟「閘道詳細資料」頁面。您會發現裝置的 IP 位址、登入使用者名稱及密碼。  

  <img src="images/gateway_details.png" alt="圖片" style="width: 500px;"/>

請記住，從「IBM Cloud 型錄」中訂購並配置 VRA 之後，您必須同時使用相同的設定來配置裝置本身。
{: tip}

## VLAN 及閘道應用裝置的角色
{: #vlans-and-the-gateway-appliance-s-role}

VLAN（虛擬 LAN）是將實體網路隔離成許多虛擬區段的一種機制。為了方便起見，多個所選 VLAN 的資料流量可以透過單一網路纜線來遞送，這個程序通常稱為「幹線」。

{{site.data.keyword.vra_full}} 以兩部分遞送：VRA 伺服器和「閘道應用裝置」。「閘道應用裝置」提供一個介面（GUI 及 API），供您選取要與 VRA 相關聯的 VLAN。將 VLAN 與閘道應用裝置相關聯，會將該 VLAN 及其所有子網路重新遞送（或 "trunk"）至您的 VRA，讓您能控制過濾、轉遞及保護。對於與「閘道應用裝置」關聯的每個 VLAN，在 VRA 連接至的交換器埠上可接受該 VLAN，且該 VLAN 上的任何子網路都會靜態遞送至您的 VRA 公用 VRRP IP（如果子網路是公用子網路），或靜態遞送至您的 VRA 專用 VRRP IP（如果子網路是專用子網路）。此遞送是在 VRA 位於其後的路由器上完成，它將分別是公用和專用資料傳輸的「前端客戶路由器 (FCR)」或「後端客戶路由器 (BCR)」。 

請注意，依預設會停用 VRRP，因此必須啟用它，才能讓 VLAN 資料流量運作，即使是獨立式 vyattas。這是關聯 VLAN 上的子網路遞送至 VRRP IP 或指派給 VRA 之虛擬位址的結果。如需相關資訊，請參閱 [VRRP 虛擬 IP (VIP) 位址](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses)。
{: important}

關聯 VLAN 中的伺服器只能從其他 VLAN 透過您的 {{site.data.keyword.vra_full}} 連接；除非您略過或取消與 VLAN 的關聯，否則無法規避 VRA。

依預設，新的「閘道應用裝置」會與兩個非抽取式的「轉移」VLAN 相關聯，公用及專用各一。這些通常用於管理，且可以用 VRA 指令個別保護。

傳輸 VLAN 用於防火牆或負載平衡器這類的網路裝置，以便它們可以在將其他裝置（例如伺服器或容器）隔離於網際網路的同時遞送資料流量。

相較之下，「閘道」VLAN 是裝置（例如伺服器及容器）管理所在的位置。

VRA 只能透過「閘道應用裝置」來管理與其相關聯的 VLAN。

如需如何從「閘道應用裝置詳細資料」畫面管理 VLAN 的相關資訊，請參閱[管理 VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)。
