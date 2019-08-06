---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# 升級 Vyatta 5400 並重複使用其 IP 位址
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

此選項容許您重複使用現有的 Vyatta 5400 裝置作為對等的 {{site.data.keyword.vra_full}} (VRA)，或保留關聯的 IP 位址。下列程序提供的指示，說明如何升級獨立式 Vyatta 5400 或在「高可用性 (HA) 」配對中運作的兩個 Vyatta 5400 裝置。

## 升級獨立式 Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

若要將單一 Vyatta 5400 升級至 {{site.data.keyword.vra_full}}，請執行下列程序：

1. 確定您已備份 5400，並將資料儲存在兩個不同的位置。這包括 SSH 金鑰、SSL 憑證、Script，以及任何其他回復現行 Vyatta 5400 配置所需的檔案（必要的話）。
2. 使用[這個主題](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os)中的指示，將 Vyatta 5400 重新載入至預設 {{site.data.keyword.vra_full}}。

  OS 重新載入將為 root 和 Vyatta 使用者 ID 產生新的密碼。
  {: note}

4. 記下新的 {{site.data.keyword.vra_full}} 密碼。
5. 使用[這些指示](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)，利用所需設定來配置剛才重新載入的 VRA。

## 升級 Vyatta 5400 高可用性配對
{: #upgrading-a-vyatta-5400-high-availability-pair}

若要升級 HA 配對中的兩個 Vyatta 5400，請執行下列程序：

1. 識別 Backup Vyatta 5400 裝置，然後先使用上述程序將它重新載入為 {{site.data.keyword.vra_full}}。

  理想情況下，功能 HA 配置會將所有介面顯示為特定節點上的 BACKUP 或 MASTER。不確定時，請使用 `show vrrp` 指令來確認。
  {: note}
2. 使用[這些指示](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)，利用所需設定來配置剛才重新載入的 VRA。
3. 「主要 Vyatta 5400」和「備份 {{site.data.keyword.vra_full}}」將位於 VRRP 配對中。請使用指令 `reset VRRP master`，將現行「備份 {{site.data.keyword.vra_full}}」變成「主要」。

  此動作會導致任何現有階段作業運作中斷，且階段作業狀態會列為「遺失」。同時會重設任何現有的 NAT 階段作業。
  {: note}

5. 驗證新的「主要 {{site.data.keyword.vra_full}}」如預期般運作。
6. 對現行「備份 Vyatta 5400」執行上述的重新載入程序，以將它升級至 VRA。
7. 在第二次重新載入之後，請視需要使用[這些指示](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)來配置「備份 VirtualRouter Appliance」。
