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

# 升级 Vyatta 5400 并复用其 IP 地址
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

此选项允许您将现有 Vyatta 5400 设备复用为等效的虚拟路由器设备 (VRA)，并保留关联的 IP 地址。以下过程提供了有关升级独立 Vyatta 5400 或高可用性 (HA) 对中运行的两个 Vyatta 5400 设备的指示信息。

## 升级独立 Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

要将单个 Vyatta 5400 升级到 IBM 虚拟路由器设备，请执行以下过程：

1. 确保已备份 5400 并将数据存储在两个不同位置。这包括 SSH 密钥、SSL 证书、脚本以及恢复当前 Vyatta 5400 配置所需的任何其他文件（如果需要）。
2. 使用[本主题](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os)中的指示信息将 Vyatta 5400 重装到缺省虚拟路由器设备。

  操作系统重装将为 root 用户和 Vyatta 用户标识生成新密码。
  {: note}

4. 记下新的虚拟路由器设备密码。
5. 使用[这些指示信息](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)，通过所需设置来配置刚重装的 VRA。

## 升级 Vyatta 5400 高可用性对
{: #upgrading-a-vyatta-5400-high-availability-pair}

要在 HA 对中升级两个 Vyatta 5400，请执行以下过程：

1. 使用上述过程，确定备份 Vyatta 5400 设备并首先将其重装为虚拟路由器设备。

  理想情况下，功能性 HA 配置在特定节点上将所有接口显示为 BACKUP 或 MASTER。如果不确定，请使用 `show vrrp` 命令进行确认。
  {: note}
2. 使用[这些指示信息](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)，通过所需设置来配置刚重装的 VRA。
3. 主 Vyatta 5400 和备份虚拟路由器设备将位于 VRRP 对中。使用 VRRP 命令 `reset VRRP master` 将当前备份虚拟路由器设备更改为主设备。

  此操作将导致任何现有会话中断，会话状态将列示为“丢失”。任何现有 NAT 会话也将重置。
  {: note}

5. 验证新的主虚拟路由器设备是否按期望状况正常运行。
6. 对当前备份 Vyatta 5400 上执行上述重装过程，以将其升级到 VRA。
7. 第二次重装后，使用[这些指示信息](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)来根据需要配置备份虚拟路由器设备。
