---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Vyatta 5400 結束支援常見問題
{: #vyatta-5400-end-of-support-faqs}

下列是從 Vyatta 5400 移轉時的常見問題。

## 為什麼 Vyatta 5400 會在 2019 年 3 月 31 日「結束支援」(EoS)？
{: faq}

2017 年 9 月，舊式 Vyatta 5400 宣告基於 IBM 的支援生命週期原則，將於 2018 年 2 月 20 日結束支援：下一版 ({{site.data.keyword.vra_full}} (VRA)) 的「通用版 (GA)」7 月日期之後的六個月。

為了遵循客戶移轉時間表，Vyatta 5400 結束支援日期已延長至 2019 年 3 月 31 日。因為「Debian 開放程式碼」社群現在不再支援 Debian 7 軟體，所以沒有未來計劃可延長 AT&T 提供的供應商支援。

[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)，以檢視正式的「結束支援公告」。
{: important}

## 對於作為客戶的我而言，「結束支援」有何意義？
{: faq}

在「結束支援」日期之後，AT&T 將不再提供任何程式碼修補程式，或接受來自 IBM 的支援提升。

同樣地，「IBM Cloud 支援中心」將不再對 Vyatta 5400 部署的配置或網路問題進行疑難排解。支援將限制為硬體層次要求（硬碟、RAM 等等）、電源及「頻外」(IPMI) 連線功能。

強烈建議客戶採取立即行動，以移轉至替代方案，例如 {{site.data.keyword.vra_full}}（VRA；基於 Vyatta 5600）或 Juniper vSRX。[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)，以開始進行。

## 如果我在 3 月 31 日之後仍然使用 Vyatta 5400 執行 IBM Cloud 工作負載，會發生什麼情況？
{: faq}

您的 Vyatta 5400 將在 3 月 31 日之後運作。不過，由於 Vyatta 5400 軟體中存在潛在漏洞，您的商務和應用程式環境可能會面臨潛在的安全威脅和其他竄改違規行為。

如果您遇到網路問題導致商務和應用程式環境停止運作，且主要原因追溯至 Vyatta 5400，請將此問題呈報給我們的「5400 供應項目經理」，因為 IBM 或 AT&T 將不再提供支援。您可以透過電子郵件位址 `nwom@us.ibm.com` 來聯繫「供應項目管理」團隊。

## 我的基礎 Bare Metal Server 硬體呢 – 仍受到支援嗎？
{: faq}

支援硬體更換，但如果疑難排解指出您的問題與 Vyatta 作業系統相關，則會引導您立即移轉至支援的硬體供應項目。[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)，以開始進行。

## 作為擁有 Vyatta 5400 的客戶，在 2019 年 3 月 31 日前我需要做什麼？
{: faq}

擁有 Vyatta 5400 的客戶應該移轉至 VRA (Vyatta 5600)、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G。仍然完全支援 VRA (Vyatta 5600)。對於來自 IBM Cloud 或 AT&T 的 VAR，沒有現行或預計的結束支援日期。[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)，以開始進行。

  VRA 和 vSRX 是客戶管理裝置。
  {: note}

## IBM 是否支援從 Vyatta 5400 移轉至 VRA、vSRX 或 FSA 10G？
{: faq}

Vyatta 5400 至 VRA (5600) 的「配置轉換服務」仍然可用：

* 對於現有客戶，IBM Cloud 會提供免費的供應項目，協助將現有的 Vyatta 5400 配置重構為 {{site.data.keyword.vra_full}} (VRA)、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G 格式。若要提交「配置轉換」服務的要求，請將電子郵件傳送至 nwom@us.ibm.com，主旨為：`Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx。`。

  務必插入您的應用程式選擇，以取代 `aaaaaaaa`（{{site.data.keyword.vra_full}}、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G），以及插入您的特定帳號，以取代主旨行中的 `xxxxxx`。
  {: note}

* 在此轉換配置處理程序中，我們的夥伴 Wanclouds 已完成數百個移轉約定。它們會轉換現有的 Vyatta 5400，以在「Vyatta 5600 平台」上建立類似的功能。它們會在兩個層級提供其服務，如下所述：

  <img src="images/tiers.png" alt="圖片" style="width: 700px;"/>

[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)，以開始進行。

## 「IBM 事業夥伴」是否提供其他付費移轉服務來用於移轉 Vyatta 5400？
{: faq}

我們有幾個事業夥伴，為 Vyatta 5400 移轉提供付費支援。如需相關資訊，[請按一下這裡](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)。

## IBM 中是否有「Vyatta 5400 供應項目管理」支援聯絡人，我可以向其提問與 Vyatta 5400 移轉相關的問題？
{: faq}

請透過 `nwom@us.ibm.com` 聯絡 IBM Vyatta 5400 和 VRA「網路供應項目管理」，來解決問題。您也可以使用 Slack 與「IBM Watson 雲端平台」工作區搭配來聯絡他們：`#vyatta-migration`

## 還有哪些其他資源可協助我進行此移轉？
{: faq}

如需相關資訊，請檢閱下列 {{site.data.keyword.vra_full}} 文件資源：

  * [開始使用 {{site.data.keyword.vra_full}}](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [關於 VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Vyatta 5400 移轉概觀](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
