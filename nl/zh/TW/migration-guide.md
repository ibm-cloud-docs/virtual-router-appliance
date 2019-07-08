---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# 移轉概觀
{: #migration-overview}

舊式 Vyatta 5400 客戶應該盡快移轉至 IBM© Virtual Router Appliance (VRA)，因為將在 2019 年 3 月 31 日移除 Vyatta 5400 支援。若要閱讀完整的結束支援公告，以及更多的相關資訊，請按一下[這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)。
{: important}

## 升級對我有什麼好處
{: #how-upgrading-can-benefit-you}

除了 IBM Virtual Router Appliance 帶來的各種新增特性和功能之外，它還提供一些改良功能。如需詳細資料，請參閱[常見問題](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-)。

升級至新的裝置將有來自 Vyatta 5400 的不同配置。因此，Vyatta 5400 配置（檔案）不會傳送至新的應用裝置。
{: note}

## 移轉文件
{: #migration-documentation}

為了協助您從 Vyatta 5400 進行移轉，我們準備了下列文件和支援：

| 移轉文件 |說明|
| ------------- | ------------- |
| [升級 Vyatta 5400 並重複使用其 IP 位址](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | 這些指示會引導您將 Vyatta 5400 升級至對等的 IBM Virtual Router Appliance，同時重複使用 Vyatta 5400 IP 位址。|
| [將 Vyatta 5400 移轉至 Juniper vSRX 或 Fortigate Security Appliance (FSA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | 這些指示會引導您移轉至 Juniper vSRX 或 Fortigate Security Appliance。此選項不容許您重複使用現有的 Vyatta 5400 裝置，或保留關聯的 IP 位址。|
| [常見的移轉問題](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | 此資訊說明從 Vyatta 5400 裝置移轉至 IBM Virtual Router Appliance 之後您可能遇到的常見問題或行為變更。在許多情況下，它還包括處理這些問題的暫行解決方法。|

如果您只需訂購新的 Virtual Router Appliance，[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)以取得詳細資料。
{: note}
