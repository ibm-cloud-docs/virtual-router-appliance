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

# 建立安全周邊
網路隔離的基礎層面是建立安全周邊。安全周邊會控制公用網際網路與 IBM© Cloud 中所管理客戶資產之間的資料流量。安全周邊也藉由使用「虛擬私密網路 (VPN)」通道及 IBM Cloud Direct Link，啟用與客戶企業的直接連線。

安全周邊使用「安全周邊區段 (SPS)」，以隔離安全周邊內資產之間的網路。這項隔離有數個好處，包括區段之間的存取控制及服務資料流量隔離。「安全周邊區段 (SPS)」包含兩個「虛擬區域網路 (VLAN)」（前端 VLAN 及後端 VLAN）以及一個連接至 VLAN 的 VRA 來管理出入 SPS 的資料流量。安全周邊可以包括多個「安全周邊區段」（例如，基於高可用性目的）。

## 設定安全周邊

以下是設定安全周邊的步驟。如需這些步驟的詳細說明，請參閱 [Set up a Secure Perimeter in the IBM Cloud ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter){:new_window}。

1. 在 IBM Cloud 和基礎架構中，配置存取安全周邊相關雲端服務及基礎架構元件所需的許可權。
2. 透過公用 VLAN 及防火牆，在公用網際網路中建立外部界限。此外部界限將用來隔離 SPS。
3. 將 SPS 置於該界限內
4. 設定閘道以橋接公用 VLAN 與 SPS
5. 建立及配置 Virtual Router Appliance (VRA)
6. 建立及配置一個以上的「安全周邊區段 (SPS)」
7. 建立前端 VLAN
8. 建立後端 VLAN
9. 建立 Vyatta 配對
10. 配置 Vyatta
11. 使 VLAN 與閘道產生關聯
12. 配置「公用 VLAN」的閘道介面
13. 在 Vyatta 主節點上配置閘道
14. 在 Vyatta 備份上配置閘道
15. 配置專用 VLAN 的閘道介面
16. 在 Vyatta 主節點上配置閘道
17. 在 Vyatta 備份上配置閘道
18. 啟用網路調整
19. 設定防火牆規則
20. 配置 Kubernetes 叢集
21. 配置使用者提供的 IP（選用）。
22. 部署 Kubernetes 叢集。

## IBM Cloud 中的專用解決方案
安全周邊以及運算隔離和資料加密，能夠促成公用 IBM Cloud 中的完整專用解決方案。如需雲端專用解決方案模式的說明，請參閱 [Implementing a Dedicated Solution Pattern ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/){:new_window}。
