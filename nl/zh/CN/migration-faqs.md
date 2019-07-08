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

# Vyatta 5400 支持结束常见问题
{: #vyatta-5400-end-of-support-faqs}

下面是从 Vyatta 5400 迁移时的常见问题。

## 为什么 Vyatta 5400 的“支持结束”(EoS) 日期是 2019 年 3 月 31 日？
{: faq}

在 2017 年 9 月，旧 Vyatta 5400 声明根据 IBM 的支持生命周期策略，其 EoS 日期为 2018 年 2 月 20 日，即在下一个版本 IBM 虚拟路由器设备 (VRA) 7 月正式发布 (GA) 日期之后的六个月。

为了给客户充足的迁移时间，Vyatta 5400 EoS 日期延长到 2019 年 3 月 31 日。由于 Debian 7 软件现在不再受 Debian 开放式源代码社区支持，因此未来没有延长来自 AT&T 的供应商支持的计划。

[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)可查看正式的“支持结束声明”。
{: important}

## 作为客户，“支持结束”对我意味着什么？
{: faq}

在支持结束日期之后，AT&T 将不再提供任何代码补丁，也不再接受来自 IBM 的支持上报。

与此类似，IBM Cloud 支持人员将不再对 Vyatta 5400 部署上的配置或联网问题进行故障诊断。支持将仅限于硬件级别请求（硬盘驱动器、RAM 等）、电源和频带外（IPMI）连接。

我们强烈建议客户立即行动以迁移到备用解决方案，例如虚拟路由器设备（VRA；基于 Vyatta 5600）或 Juniper vSRX。[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)以开始操作。

## 如果我在 3 月 31 日之后仍在使用 Vyatta 5400 运行 IBM Cloud 工作负载，会发生什么情况？
{: faq}

Vyatta 5400 在 3 月 31 日之后仍可运行。但是，由于 Vyatta 5400 软件中的潜在漏洞，您的业务和应用程序环境可能会遭受潜在的安全威胁和其他篡改违例。

如果遇到网络问题而导致您的业务和应用程序环境停止运行，并且根本原因追溯至 Vyatta 5400，请将此问题上报给我们的 5400 产品经理，因为 IBM 或 AT&T 将不再提供支持。您可以通过电子邮件 (`nwom@us.ibm.com`) 联系产品管理团队。

## 我的底层裸机服务器硬件会怎么样，仍受支持吗？
{: faq}

将支持硬件更换，但如果故障诊断表明问题与 Vyatta 操作系统相关，那么会指示您立即迁移到受支持的硬件产品。[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)以开始操作。

## 作为拥有 Vyatta 5400 的客户，我需要在 2019 年 3 月 31 日之前做什么？
{: faq}

有 Vyatta 5400 的客户应该迁移到 VRA (Vyatta 5600)、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G。VRA (Vyatta 5600) 仍完全受支持。IBM Cloud 和 AT&T 目前都没有实施针对 VRA 的支持结束日期，也未计划实施。[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)以开始操作。

  VRA 和 vSRX 是客户管理的设备。
  {: note}

## IBM 是否提供支持来帮助从 Vyatta 5400 迁移到 VRA、vSRX 或 FSA 10G？
{: faq}

Vyatta 5400 到 VRA (5600) 配置转换服务仍可用：

* 对于现有客户，IBM Cloud 提供了免费服务，以帮助将现有 Vyatta 5400 配置重构为虚拟路由器设备 (VRA)、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G 格式。要提交针对配置转换服务的请求，请发送电子邮件至 nwom@us.ibm.com，主题为：`Request for Configuration Conversion to aaaaaaaaaaaa : IBM Cloud Account ID xxxxxx. `。

  请确保在主题行中插入您的应用程序选项（虚拟路由器设备、Juniper vSRX 或 Fortigate Security Appliance (FSA) 10G）来代替 `aaaaaaaa`，并插入您的具体帐号来代替 `xxxxxx`。
  {: note}

* 我们在此配置转换流程中的合作伙伴 Wanclouds 已经完成了几百次成功迁移操作。他们将转换现有 Vyatta 5400 以在 Vyatta 5600 平台上创建类似功能。他们在两个层中提供服务，如下所述：

  <img src="images/tiers.png" alt="图样" style="width: 700px;"/>

[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)以开始操作。

## IBM 业务合作伙伴是否提供有任何额外的付费迁移服务来帮助从 Vyatta 5400 进行迁移？
{: faq}

我们有多个业务合作伙伴提供对 Vyatta 5400 迁移的付费支持。[单击此处](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)以了解更多信息。


## IBM 是否有 Vyatta 5400 产品管理支持联系人可供咨询 Vyatta 5400 迁移的相关问题？
{: faq}

如有问题，请联系 IBM Vyatta 5400 和 VRA 网络产品管理：`nwom@us.ibm.com`。您还可以使用 Slack 通过以下 IBM Watson Cloud Platform 工作空间联系他们：`#vyatta-migration`

## 我还可以使用哪些额外的资源来帮助进行此迁移？
{: faq}

请查看以下虚拟路由器设备文档资源以获取更多信息：

  * [IBM 虚拟路由器设备入门](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [关于 VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Vyatta 5400 迁移概述](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
