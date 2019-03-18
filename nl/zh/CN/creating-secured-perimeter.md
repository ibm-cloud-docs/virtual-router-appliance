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

# 创建安全周界
网络隔离的一个基本方面是建立安全周界。安全周界可控制公用因特网与 IBM© Cloud 中托管的客户资产之间的流量。安全周界还支持使用虚拟专用网 (VPN) 隧道和 IBM Cloud Direct Link 与客户企业直接连接。

安全周界使用安全周界分段 (SPS) 来隔离安全周界内的各资产之间的网络。此隔离具有多个优点，包括分段之间的访问控制和服务流量隔离。安全周界分段 (SPS) 由两个虚拟局域网 (VLAN)（一个前端 VLAN 和一个后端 VLAN）和一个连接到 VLAN 的 VRA（用于管理进出 SPS 的流量）组成。安全周界可以包含多个安全周界分段（例如，用于高可用性目的）。

## 设置安全周界

下面是设置安全周界的步骤。有关这些步骤的详细描述，请参阅 [Set up a Secure Perimeter in the IBM Cloud ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter){:new_window}。

1. 在 IBM Cloud 和基础架构中配置访问安全周界中涉及的云服务和基础架构组件所需的许可权。
2. 在公用因特网中通过公用 VLAN 和防火墙创建外部边界。此外部边界将用于隔离 SPS。
3. 将 SPS 放在该边界内
4. 设置网关以桥接公用 VLAN 和 SPS
5. 创建并配置虚拟路由器设备 (VRA)
6. 创建并配置一个或多个安全周界分段 (SPS)
7. 创建前端 VLAN
8. 创建后端 VLAN
9. 创建 Vyatta 对
10. 配置 Vyatta
11. 将 VLAN 与网关相关联
12. 配置用于公用 VLAN 的网关接口
13. 在 Vyatta 主控上配置网关
14. 在 Vyatta 备份上配置网关
15. 配置用于专用 VLAN 的网关接口
16. 在 Vyatta 主控上配置网关
17. 在 Vyatta 备份上配置网关
18. 启用网络调整
19. 设置防火墙规则
20. 配置 Kubernetes 集群
21. 配置用户提供的 IP（可选）。
22. 部署 Kubernetes 集群。

## IBM Cloud 中的专用解决方案
安全周界以及计算隔离和数据加密有助于在公共 IBM Cloud 中提供完整的专用解决方案。请参阅 [Implementing a Dedicated Solution Pattern ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/){:new_window}，以获取有关云专用解决方案模式的描述。
