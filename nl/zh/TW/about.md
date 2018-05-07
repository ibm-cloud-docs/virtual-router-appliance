---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 關於
IBM Virtual Router Appliance (VRA) 容許您選擇性地透過具有防火牆、資料流量塑形、原則式遞送、VPN 及其他特性之主機的完整特性企業路由器，遞送專用與公用網路資料流量。Virtual Router Appliance 提供效能、配置簡化，以及在一般硬體伺服器上執行的維護優點。系統會調整硬體的大小，以處理多個 VLAN 的遞送負載，且可以搭配備援網路鏈結及備援 RAID 陣列來訂購。所有 VRA 特性都是由客戶管理。 

IBM Virtual Router Appliance 為 x86 裸機伺服器提供最新的 Vyatta 5600 作業系統。它同時以高可用性 (HA) 和獨立式供應項目的方式來提供。

**附註：**FortiGate Security Appliance (FSA) 10Gbps 是具有下一代特性（例如防毒 (AV)、入侵防禦 (IPS) 及 Web 過濾）的單一承租戶（專用）、高傳輸量 (10Gbps) 硬體防火牆。它可能可以代替 VRA 達到類似的目標。如需相關資訊，請參閱 [FSA 文件](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started)。

## 防火牆
為了保護環境免於遭受外部威脅，可以將 Virtual Router Appliance 當作防火牆來使用。您可以新增防火牆規則，以容許或拒絕入埠或出埠網路資料流量送至您應用程式執行所在的埠，也可以過濾您自己網路內的資料流量。Virtual Router Appliance 也可以配置為執行有狀態的 IPv4 和 IPv6 過濾，以保護您的重要資料。

## 虛擬專用網路 (VPN) 閘道
藉由將 Virtual Router Appliance 佈建為網路閘道裝置，使用 VPN 通道作業將現場資料中心或辦公室連接至 IBM Cloud。您可以使用 IPsec 網站至網站 VPN 通道，以進行從企業資料中心或辦公室到 IBM Cloud 網路的安全通訊。其他 VPN 選項包含：遠端存取 IPsec VPN（用戶端到網站）、OpenVPN、GRE、L2TP 及 DMVPN。

請查看 [VRA 補充文件](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)一節中的 Brocade VPN 配置手冊。

## 網址轉換 (NAT)
使用 Virtual Router Appliance，您可以在沒有公用網路介面的情況下佈建應用程式及資料庫伺服器，同時仍容許伺服器使用來源 NAT 存取網際網路。您也可以使用目的地 NAT 將伺服器隱藏在閘道裝置之後，以加強安全。

## 企業級遞送

針對不同隔離網路上的多層應用程式，Virtual Router Appliance 可讓您在這些網路之間建立連線功能，且有更大的彈性。您可以使用 BGP 設定動態遞送，這容許您在 IBM Cloud 路由器上發表自己的公用 IP 空間。使用各種通道及直接鏈結解決方案時，BGP 也會提供更多的彈性來進行自訂專用網路配置。

請查看 [VRA 補充文件](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)一節中的 Brocade BGP 配置手冊。
