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

# 操作系统 V18.01 的升级注意事项

本文件包含将 VRA (Vyatta 5600) 从 Brocade 5.X 操作系统升级为 1801 操作系统（包括针对 Spectre 安全性违规的修复）时要了解的事项列表。执行此升级时，需要了解一些问题。

## 谁需要阅读本主题

* 当前有 VRA 未运行 1801 操作系统的任何人。（当前版本为 1801g）

* 希望为虚拟路由器设备安装新的 1801f 版本的任何人（例如，从 17.2X 升级的任何人）。

* 如果您拥有的是 Vyatta 5400，那么这些信息不适用于您。

## 为何需要这些信息

VRA V1801f 包含针对 Spectre 漏洞的安全修订，但是必须先对安装程序本身进行更改，然后才能安装此补丁。这需要一个中间步骤来安装 V1801C。

## 常规升级过程
要升级到 V1801f，必须先下载并安装 18.01C 补丁以更新 VRA 安装程序：

1. 从此位置下载 1801c 补丁，然后按照[常规过程](upgrade-os.html)进行安装。

2. 重新引导后，从此位置下载 1801f 补丁，并使用[常规过程](upgrade-os.html)进行安装。

现在，您应该运行的是 V1801f，并且已修正 Spectre 安全性限制。

## 完全重装过程
作为替代方法，您还可以在 1801f 级别执行 VRA 的完全重装：

*警告：*此过程允许您跳过下载和安装两个补丁的中间步骤，但在使用 ISO 执行完全重装期间将丢失所有数据。

1. 从此位置获取 1801f ISO。
2. 按照[常规过程](upgrade-os.html)进行安装并重新引导。

现在，您应该运行的是 V1801f，并且已修正 Spectre 安全性限制。

## 其他指南

由于 Vyatta 5400 和虚拟路由器设备之间的功能没有简单的一对一映射，因此为 VRA 创建基线配置非常有用。IBM 合作伙伴 WanClouds 可帮助您完成此过程，并提供有关在 VRA 上创建类似于 Vyatta 5400 的功能的指导。

有关此升级过程中遇到的常见问题的更多信息，请参阅[其他文档](/docs/infrastructure/virtual-router-appliance/migration-issues.html#vyatta-5400-common-migration-issues)。
