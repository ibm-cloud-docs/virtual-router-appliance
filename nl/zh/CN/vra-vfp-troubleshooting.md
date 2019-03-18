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

# VFP 接口故障诊断
{: #troubleshooting-your-vfp-interface}

本主题包含 VFP 接口的故障诊断信息。

* VFP 接口并不是“真实”接口，例如 `dp0bond0`（或者甚至是 VIF 或 TUN）。它是一个占位符，供防火墙和 NAT 进程用于挂起接口，以便这些进程能够正确处理流量。您仍可以通过 VFP（如常规接口）来路由流量，但 `tshark` 和其他监视命令不会显示任何流量。
* 使用 NAT 时，您必须使用更具体的子网范围使流量路由到 VFP，而不是使用由 IPsec 创建的内核路由。如果未设置静态路由，那么将执行内核路由。您可以使用 `show ip route x.x.x.x` 对此进行测试。
* 来自 VFP 的 DNAT 应该会得到正确处理，但返回流量仍需要静态路由集。请查找 IPsec 接口 `dp0bond1` 或 `dp0bond0`（或使用 IPsec 流量的任何接口）流出的非 NAT 流量。
* 通过 VFP 使用路由协议未经测试。 
* 通过 VFP 使用 GRE 隧道也未经测试。
