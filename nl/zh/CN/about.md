---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 关于 VRA
{: #about-the-vra}

IBM© 虚拟路由器设备 (VRA) 为 **x86** 裸机服务器提供最新的 Vyatta 5600 操作系统，可作为高可用性 (HA) 或独立配置提供。

虚拟路由器设备支持通过全功能的企业路由器（具有防火墙、流量塑形、基于策略的路由、VPN 和其他诸多功能）有选择地路由专用和公用网络流量。VRA 的性能通过轻松配置即可实现，并具有在正常硬件服务器上运行的维护优势。VRA 硬件设备的大小调整为可处理多个 VLAN 的路由负载，并且可以订购冗余网络链路和冗余 RAID 阵列。所有 VRA 功能均由您（即客户）管理。 

**替代方法：**FortiGate Security Appliance (FSA) 10 Gbps 是一种单租户（专用）高吞吐量 (10 Gbps) 硬件防火墙，具有防病毒 (AV)、入侵防御 (IPS) 和 Web 过滤等下一代功能。它可以作为 VRA 的替代方案，用于实现类似的目标。有关更多信息，请参阅 [FSA 文档](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps)。

## 防火墙
为了保护环境免受外部威胁，可以将虚拟路由器设备用作防火墙。您可以添加防火墙规则，以允许或拒绝与运行应用程序的端口之间的入站或出站网络流量，并且可以过滤自己网络中的流量。还可以将虚拟路由器设备配置为执行有状态的 IPv4 和 IPv6 过滤，以保护关键数据。

## 虚拟专用网 (VPN) 网关
通过将虚拟路由器设备作为网关设备供应，可使用 VPN 隧道将现场数据中心或办公室连接到 IBM Cloud。可以使用 IPsec 站点到站点 VPN 隧道来保护从企业数据中心或办公室到 IBM Cloud 网络的通信。其他 VPN 选项是：远程访问 IPsec VPN（客户机到站点）、OpenVPN、GRE、L2TP 和 DMVPN。

请参阅[补充 VRA 文档部分](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation)中的 Brocade VPN 配置指南。

## 网络地址转换 (NAT)
通过虚拟路由器设备，可以在没有公用网络接口的情况下供应应用程序和数据库服务器，同时仍允许服务器使用源 NAT 来访问因特网。此外，还可以使用目标 NAT 将服务器隐藏在网关设备后面，以增强安全性。

## 企业级路由

对于不同隔离网络上的多层应用程序，虚拟路由器设备支持灵活地在这些网络之间构建连接。您可以使用 BGP 来设置动态路由，这将允许您在 IBM Cloud 路由器上公布自己的公共 IP 空间。BGP 在使用各种隧道和直接链接解决方案时，还为定制专用网络配置提供了更多灵活性。

请参阅[补充 VRA 文档部分](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation)中的 Brocade BGP 配置指南。
