---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

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

# 將 Vyatta 5400 移轉至 Juniper vSRX 或 Fortigate Security Appliance (FSA) 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

除了能夠將 Vyatta 5400 裝置升級至 IBM Virtual Router Appliance 之外，您還可以移轉至市場上部分最具成本效益、可靠且安全的選項：Juniper vSRX 和 Fortigate Security Appliance。
不過，您無法重複使用現有的 Vyatta 5400 裝置，或保留關聯的 IP 位址。

下列程序提供的指示，說明如何將獨立式 Vyatta 5400 或在「高可用性 (HA)」配對中運作的兩個 Vyatta 5400 裝置升級至 Juniper vSRX 或 FSA。

## 升級獨立式 Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

若要將單一 Vyatta 5400 升級至 Juniper vSRX 或 FSA - 10G 應用裝置，且關閉時間最低，我們建議您執行下列程序。

1. 確定您已備份 5400，並將資料儲存在兩個不同的位置。這包括 SSH 金鑰、SSL 憑證、Script，以及任何其他回復現行 Vyatta 5400 配置所需的檔案（必要的話）。若要這樣做，請遵循[這些指示](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)。

2. 聯絡 nwom@us.ibm.com，要求轉換您的配置

3. 遵循適用於 [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 或 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps) 的指示來訂購新裝置。

  這是考量訂購「高可用性」解決方案的好機會。
  {: note}

4. 將您從 nwom@us.ibm.com 收到的配置載入至新訂購的裝置。

5. 排定將 VLAN 移轉至新裝置的時間。

  此步驟需要短暫的關閉時間。
  {: note}

6. 測試並確認您的新裝置正常運作。

7. 開立支援問題單，以取消 Vyatta 5400 裝置。

## 升級 Vyatta 5400 高可用性配對
{: #upgrading-a-vyatta-5400-high-availability-pair}

若要升級 HA 配對中的兩個 Vyatta 5400，請執行下列程序：

1. 確定您已備份 5400，並將資料儲存在兩個不同的位置。這包括 SSH 金鑰、SSL 憑證、Script，以及任何其他回復現行 Vyatta 5400 配置所需的檔案（必要的話）。若要這樣做，請遵循[這些指示](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)。

2. 聯絡 nwom@us.ibm.com，要求轉換您的配置。

3. 遵循適用於 [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 或 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps) 的指示來訂購新裝置。

4. 將您從 nwom@us.ibm.com 收到的配置載入至新訂購的裝置。

5. 排定將 VLAN 移轉至新裝置的時間。

  此步驟需要短暫的關閉時間。
  {: note}

6. 測試並確認您的新裝置正常運作。

7. 開立支援問題單，以取消 Vyatta 5400 裝置。
