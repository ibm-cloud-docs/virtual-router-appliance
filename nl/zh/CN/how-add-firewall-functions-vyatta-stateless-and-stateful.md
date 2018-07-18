---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 如何向 Vyatta 添加防火墙功能（无状态和有状态）

使用 Brocade 5400 vRouter (Vyatta) 设备时，建立防火墙的其中一种方法是对每个接口应用防火墙规则集。每个接口有三个可能的防火墙实例：In、Out 和 Local。每个实例都有可应用的规则。缺省操作是“丢弃”，其中允许特定流量的规则以从规则 1 到 N 的方式进行应用。只要匹配一条规则，防火墙就会立即应用匹配规则的特定操作。

对于下面三个防火墙实例，**只有一个**能应用。

* **IN**：防火墙过滤进入接口并遍历 Brocade 系统的包。您需要允许某些 SoftLayer IP 范围有权访问前端和后端以进行管理（ping、监视等）。
* **OUT**：防火墙过滤流出接口的包。这些包可以遍历 Brocade 系统或以该系统为源。
* **LOCAL**：防火墙过滤通过系统接口发往 Brocade vRouter 系统自身的包。您应该对从不受限制的外部 IP 地址进入 Brocade vRouter 设备的访问端口建立限制。<sup>1</sup>

使用以下步骤设置示例防火墙规则以禁用 Brocade 5400 vRouter 的接口的因特网控制报文协议 (ICMP)*（ping - IPv4 回传回复消息）*（这是无状态操作；有状态操作将在稍后讨论）：

1\. 在命令提示符中输入 *show configuration commands* 以查看设置了哪些配置。您将看到设备上已设置的所有命令的列表（如果您决定迁移并希望查看所有配置，使用此命令会非常方便）。请注意，*set firewall all-ping 'enable'* 命令指示设备仍然启用了 ICMP。

2\. 输入 *configure*。

3\. 输入 *set firwall all-ping 'disable'*。

4\. 输入 *commit*。

如果现在尝试对 Brocade 5400 vRouter 设备执行 ping 操作，您将不再收到响应。

为了将防火墙规则分配给路由的流量，必须将规则应用于 Brocade 5400 vRouter 的**接口**，或者**创建区域**，然后将规则应用于这些区域。

对于此示例，将为到目前为止使用的 VLAN 创建区域。

**VLAN = 区域**

bond1 = dmz

bond1.2007 = prod（用于生产环境）

bond0.2254 = private（用于开发环境）

bond1.1280 = 保留供未来使用

bond1.1894 = 保留供未来使用

**创建防火墙规则**

在实际创建区域之前，最好先创建要应用于区域的防火墙规则。先创建规则再创建区域，可以立即应用这些规则，而先创建区域再创建规则，必须返回到区域才可应用规则。<sup>2</sup>

以下命令将用于：

* 创建名为 **dmz2private** 的防火墙规则，其缺省操作为丢弃所有包
* 针对名为 **dmz2private** 的规则，启用已接受和已拒绝流量的日志记录


1\. 在命令提示符中输入 *configure*。

2\. 在提示符中输入以下命令：

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

下一组命令用于使 **iptables** 允许返回已建立会话的流量。缺省情况下，**iptables** 不允许这样做，这就是为什么需要显式规则的原因。

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

第三组命令允许 TCP/UDP 流经端口 22（用于 SSH 的缺省端口）。

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

最后一组命令允许 TCP/UDP 流经端口 80（用于 HTTP 的缺省端口）。

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. 输入 *commit* 以确保完成时执行所有规则。

4\. 通过在命令提示符中输入 *show firewall name dmz2private* 以查看配置。

创建的下一个防火墙规则将应用于 **dmz** 区域。该规则将命名为 **public**。在提示符中输入以下命令。

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**创建区域**

区域是接口的逻辑表示。以下命令将用于：

* 创建名为 **dmz** 的区域和策略，其缺省操作为丢弃发往此区域的包。
* 将 **dmz** 策略设置为使用 **bond1** 接口
* 将 **prod** 策略设置为使用 **bond1.2007** 接口
* 创建名为 **private** 的区域策略，其缺省操作为丢弃发往此区域的包
* 将名为 **private** 的策略设置为使用 **bond0.2254** 接口

1\. 在提示符中输入以下命令：

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. 使用以下命令为区域设置防火墙策略：

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

请注意，如果不想将某个防火墙规则应用于区域策略，那么可以将其应用于特定接口。使用以下命令将规则应用于接口。

* *set interfaces bonding bond1 firewall local name public*
* *commit*

**注：**

<sup>1</sup>*有状态*防火墙会保留先前看到的流的表，并且可以根据与先前包的关系来接受或丢弃包。一般来说，在应用程序流量很普遍的情况下，会首选有状态防火墙。 

<span style="text-decoration: underline">*使用缺省配置时，Brocade 5400 vRouter 不会跟踪连接的状态。防火墙会保持无状态，直到满足以下其中一个条件：*</span>

* 配置防火墙和具有状态参数的任一规则
* 配置防火墙 state-policy
* 配置 NAT
* 启用透明 Web 代理服务
* 启用 WAN 负载均衡配置

*无状态*防火墙孤立地看待各个包。只能根据基本访问控制表 (ACL) 条件（例如，IP 或传输控制协议/用户数据报协议 (TCP/UDP) 头中的源和目标字段）来接受或丢弃包。无状态的 Brocade 5400 vRouter 不会存储连接信息，也不需要查找每个包与先前流的关系，这两项操作会占用少量内存和 CPU 时间。因此，原始转发性能在无状态系统上最佳。如果不需要特定于有状态的功能，Brocade 建议使路由器保持无状态，以获得最佳性能。

<sup>2</sup>区域和规则集都有缺省操作语句。使用区域策略时，缺省操作由 zone-policy 语句设置，并由规则 10,000 表示。另外，务必记住防火墙规则集的缺省操作是**丢弃**所有流量。

<sup>3</sup>防火墙规则需要通过 **prod** 出站流出到 **dmz**。使用以下命令来帮助对网络流进行故障诊断：*sudo tcpdump -i any host 10.52.69.202*
