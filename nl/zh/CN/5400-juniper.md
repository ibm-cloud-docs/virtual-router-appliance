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

# 将 Vyatta 5400 迁移到 Juniper vSRX 或 Fortigate Security Appliance (FSA) 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

除了能够将 Vyatta 5400 设备升级到 IBM 虚拟路由器设备外，还可以迁移到市场上最经济有效、最可靠、最安全的选项：Juniper vSRX 和 Fortigate Security Appliance。但是，您无法复用现有的 Vyatta 5400 设备，也无法保持关联的的 IP 地址。

以下过程提供了有关将独立 Vyatta 5400 或在高可用性 (HA) 对中运行的两个 Vyatta 5400 设备升级到 Juniper vSRX 或 FSA 的指示信息。

## 升级独立 Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

要将单个 Vyatta 5400 升级到 Juniper vSRX 或 FSA - 10G 设备，并使停机时间最短，我们建议您执行以下过程。

1. 确保已备份 5400 并将数据存储在两个不同位置。这包括 SSH 密钥、SSL 证书、脚本以及恢复当前 Vyatta 5400 配置所需的任何其他文件（如果需要）。要执行此操作，请遵循[这些指示信息](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)。

2. 请联系 nwom@us.ibm.com 以请求转换配置

3. 按照 [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 或 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps) 的指示信息订购新设备。 

  这是考虑订购高可用性解决方案的好机会。
  {: note}

4. 将您从 nwom@us.ibm.com 收到的配置装入新订购的设备。

5. 安排将 VLAN 迁移到新设备的时间。

  此步骤需要短暂的停机时间。
  {: note}

6. 测试并确认新设备是否正常运行。

7. 开具支持凭单以取消 Vyatta 5400 设备。

## 升级 Vyatta 5400 高可用性对
{: #upgrading-a-vyatta-5400-high-availability-pair}

要在 HA 对中升级两个 Vyatta 5400，请执行以下过程：

1. 确保已备份 5400 并将数据存储在两个不同位置。这包括 SSH 密钥、SSL 证书、脚本以及恢复当前 Vyatta 5400 配置所需的任何其他文件（如果需要）。要执行此操作，请遵循[这些指示信息](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)。

2. 请联系 nwom@us.ibm.com 以请求转换配置。

3. 按照 [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 或 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps) 的指示信息订购新设备。 

4. 将您从 nwom@us.ibm.com 收到的配置装入新订购的设备。

5. 安排将 VLAN 迁移到新设备的时间。

  此步骤需要短暂的停机时间。
  {: note}

6. 测试并确认新设备是否正常运行。

7. 开具支持凭单以取消 Vyatta 5400 设备。
