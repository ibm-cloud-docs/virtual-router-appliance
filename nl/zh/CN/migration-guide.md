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

# 迁移概述
{: #migration-overview}

旧 Vyatta 5400 客户应尽快迁移到 IBM© 虚拟路由器设备 (VRA)，因为从 2019 年 3 月 31 日开始，将除去对 Vyatta 5400 的支持。要阅读完整的支持结束声明以及有关更多信息，请单击[此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)。
{: important}

## 升级如何使您受益
{: #how-upgrading-can-benefit-you}

除了 IBM 的虚拟路由器设备所贡献的各种新功能外，还提供了若干改进。有关更多详细信息，请参阅[常见问题](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-)。

升级到新设备将拥有不同于 Vyatta 5400 的配置。因此，Vyatta 5400 配置（文件）不会传输到新设备。
{: note}

## 迁移文档
{: #migration-documentation}

为了帮助您从 Vyatta 5400 迁移，我们已准备好以下文档和支持：

|迁移文档|描述|
| ------------- | ------------- |
| [升级 Vyatta 5400 并复用其 IP 地址](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses)|有关将 Vyatta 5400 升级到等效的 IBM 虚拟路由器设备并复用 Vyatta 5400 IP 地址的指示信息。|
| [将 Vyatta 5400 迁移到 Juniper vSRX 或 Fortigate Security Appliance (FSA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) |有关迁移到 Juniper vSRX 或 Fortigate Security Appliance 的指示信息。此选项不允许您复用现有的 Vyatta 5400 设备，也无法保持关联的 IP 地址。|
| [常见迁移问题](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)|有关从 Vyatta 5400 设备迁移到 IBM 虚拟路由器设备后可能遇到的最常见问题或行为更改的信息。在很多情况下，这包括用于解决这些问题的变通方法。|

如果只需订购新的虚拟路由器设备，请[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)以了解更多详细信息。
{: note}
