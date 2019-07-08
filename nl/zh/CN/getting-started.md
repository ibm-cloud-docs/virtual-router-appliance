---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

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


# IBM 虚拟路由器设备入门
{: #getting-started}

IBM© 虚拟路由器设备 (VRA) 为 x86 裸机服务器提供最新的 Vyatta 5600 操作系统。它作为高可用性 (HA) 或独立配置提供，并允许您通过具有防火墙、流量塑形、基于策略的路由、VPN 和其他功能的全功能企业路由器，有选择地路由专用和公共网络流量。

VRA 最低服务器需求是每 10 Gbps 网络容量要求 8 GB RAM 和一个 CPU 核心。例如，带有双 10 Gbps 公用和专用上行链路的系统至少需要四个核心。此外，如果您打算为 VPN 服务设置加密，那么可能需要添加更多核心。为 VPN 服务添加更多核心将确保 VRA 在路由并同时加密/解密数据时，不会因负载过重而陷入困境。

## 订购虚拟路由器设备
{: #order-vra}

要订购 VRA，请执行以下过程：

1. 在浏览器中，打开 [IBM Cloud UI 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} 中的“网关设备”页面，然后登录到您的帐户。 

  您还可以通过选择 [IBM Cloud 目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com) 左上角的导航菜单并选择**经典基础架构 > 网络 > 网关设备**来访问此页面。
  {: tip}

2. 在**网关供应商**部分中，选择 **AT&T** 选项（选择该选项时，按钮上显示蓝色复选标记）。从同一按钮上的下拉菜单中，选择带宽（20 Gbps 或 2 Gbps）。

  	<img src="images/ordering_vra.png" alt="图样" style="width: 500px;"/>

3. 在**网关设备**部分中，输入**主机名**和**域名**信息。这些字段已经填充了缺省信息，因此请确保值正确。如果需要，可检查**高可用性**选项，然后在下拉菜单中选择所需的数据中心**位置**以及您需要的特定 **pod**。

  此处仅显示已关联 VLAN 的 pod。如果希望在您未看到列出的 pod 中供应网关设备，请先在其中创建 VLAN。
  {: note}

4. 在**配置**部分中，通过选择 RAM 和 SSH 密钥（如果需要）来选择处理器。

  <img src="images/ordering_vra_2.png" alt="图样" style="width: 600px;"/>
  
  根据您在步骤 2 中选择的许可证版本，将为您选择相应的处理器。但是，您可以选择不同的 RAM 配置。
  {: note}

5. 在**存储磁盘**部分中，选择符合您存储需求的选项。 

  有 RAID0 和 RAID1 选项可用，以针对数据丢失提供额外的防护措施，就像热备用（在主组件发生故障时可立即投入使用的备份组件）那样。
  {: note}

  每个 VRA 最多可以有四个磁盘。RAID 配置的“磁盘大小”是可用磁盘大小，因为 RAID 配置是镜像配置。
  {: note}

  如果计划运行生成详细日志的网络诊断，请保留大于缺省设置的磁盘空间。
  {: tip}

6. 在**网络接口**部分中，选择**上行端口速度**。缺省选择是单个界面，但也有冗余的和仅限专用的选项。请选择最符合您需求的选项。

  在网络接口**附加组件**部分中，可以根据需要选择 IPv6 地址，并且其中显示了包含的其他任何缺省选项。 
  
8. 检查您的选择，确保已阅读“第三方服务协议”，然后单击**创建**。将自动验证订单。

订单核准后，会自动开始供应虚拟路由器设备。供应过程完成后，新的 VRA 会显示在“网关设备”列表页面中。单击网关名称可打开“网关详细信息”页面。在其中将找到设备的 IP 地址、登录用户名和密码。  

  <img src="images/gateway_details.png" alt="图样" style="width: 500px;"/>

请记住，只要在 IBM Cloud 目录中订购并配置了 VRA，就必须还使用相同的设置来配置设备本身。
{: tip}

## VLAN 和网关设备的角色
{: #vlans-and-the-gateway-appliance-s-role}

VLAN（虚拟 LAN）是用于将物理网络划分为多个虚拟分段的机制。为了方便起见，可以通过单一网络电缆来传递来自多个所选 VLAN 的流量，这一过程通常称为“中继”。

虚拟路由器设备分两部分交付：VRA 服务器和网关设备装置。网关设备为您提供接口（GUI 和 API），用于选择要与 VRA 相关联的 VLAN。将 VLAN 与网关设备相关联会将该 VLAN 及其所有子网重新路由（或“中继”）到 VRA，这将给予您对过滤、转发和保护的控制权。对于与网关设备关联的每个 VLAN，在 VRA 连接到的交换机端口上允许该 VLAN，并且该 VLAN 上的任何子网都静态路由到 VRA 的公共 VRRP IP（如果子网是公共子网），或者静态路由到 VRA 的专用 VRRP IP（如果子网是专用子网）。此路由是在 VRA 前方的路由器上完成的，即，用于公共流量的是前端客户路由器 (FCR)，用于专用流量的是后端客户路由器 (BCR)。 

请注意，缺省情况下，VRRP 处于禁用状态，并且必须启用 VRRP 才能使 VLAN 流量正常工作，即使在独立 vyattas 上也是如此。这是将关联 VLAN 上的子网路由到分配给 VRA 的 VRRP IP 或虚拟地址的结果。有关更多信息，请参阅 [VRRP 虚拟 IP (VIP) 地址](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses)。
{: important}

要从其他 VLAN 访问关联 VLAN 中的服务器，只能通过虚拟路由器设备 (VRA) 来实现；除非绕过该 VLAN 或解除该 VLAN 的关联，否则无法避开该 VRA。

缺省情况下，新的网关设备与两个不可除去的“转换”VLAN 相关联，这两个 VLAN 分别用于公用和专用网络。这两个 VLAN 通常用于管理，并且可以通过 VRA 命令单独保护。

转换 VLAN 用于网络设备（例如，防火墙或负载均衡器），以便可以在使其他设备（例如，服务器或容器）与因特网保持隔离的情况下路由流量。

相比之下，“网关”VLAN 用于托管设备，例如服务器和容器。

VRA 只能管理通过网关设备与其关联的 VLAN。

有关如何在“网关设备详细信息”屏幕中管理 VLAN 的信息，请参阅[管理 VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)。
