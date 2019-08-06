---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: updates, changes, additions

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


# {{site.data.keyword.vra_full}} 的最近更新
{: #recent-updates-for-ibm-virtual-router-appliance}

瞭解 {{site.data.keyword.vra_full}} (VRA) 的新增特性及更新特性。

## 2018 年 8 月
{: #august-2018}

### Brocade Operating System 18.x 版
{: #brocade-operating-system-version-18-x}

現在提供 {{site.data.keyword.vra_full}} 的 Brocade OS 18.x 版。在其他新特性中，此版本提供 Spectre 安全侵害的補救。

下列各主題討論 18.x VRA 的新增特性：

* [設定使用區域防火牆的 IPsec 通道](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [使用 IPsec 及區域防火牆配置 VFP 介面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [搭配使用 NAT 與以字首為基礎的 IPsec](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [VFP 介面疑難排解](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

如果您是從 Vyatta 5400 移轉，則升級至 18.x 的最佳方式是透過完整 OS 重新載入的[一般程序](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)。

因為 Vyatta 5400 與 {{site.data.keyword.vra_full}} 之間的功能沒有簡單的一對一對映，所以建立 VRA 的基準線配置會有所幫助。IBM 合作夥伴 WanClouds 可協助您完成此處理程序，並提供有關建立與您 VRA 上 Vyatta 5400 類似之功能的指引。

如需在此升級處理程序期間所遇到之一般問題的相關資訊，請參閱[其他文件](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)。
