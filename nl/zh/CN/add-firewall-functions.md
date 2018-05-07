---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 向虚拟路由器设备添加防火墙功能（无状态和有状态）
使用虚拟路由器设备 (VRA) 时，建立防火墙的其中一种方法是在每个接口上应用防火墙规则集。每个接口有三个可能的防火墙实例 - In、Out 和 Local - 每个实例具有可应用的规则。 

缺省操作是“丢弃”，其中允许特定流量的规则以从规则 1 到 N 的方式进行应用。只要匹配一条规则，防火墙就会立即应用匹配规则的特定操作。

对于下面三个防火墙实例，只能应用其中一个实例。

**IN**：防火墙过滤进入接口并遍历系统的包。您需要允许某些 IP 范围有权访问前端和后端以进行管理（ping、监视等）。

**OUT**：防火墙过滤流出接口的包。这些包可以遍历 VRA 系统或以该系统为源。

**LOCAL**：防火墙过滤使用系统接口发往 VRA 系统自身的包。您应该对从不受限制的外部 IP 地址进入设备的访问端口建立限制。

使用以下步骤设置示例防火墙规则以禁用虚拟路由器设备接口的因特网控制报文协议 (ICMP)（ping - IPv4 回传回复消息）（这是无状态操作；有状态操作将在稍后讨论）：

1. 在命令提示符中输入 `show configuration commands` 以查看设置了哪些配置。您将看到设备上已设置的所有命令的列表（如果您决定迁移并希望查看所有配置，使用此命令会非常方便）。请注意，`set firewall all-ping enable` 命令指示设备仍然启用了 ICMP。

2. 输入 `configure`。

3. 输入 `set firwall all-ping disable`。

4. 输入 `commit`。

如果现在尝试对 VRA 设备执行 ping 操作，您将不再收到响应。

为了将防火墙规则分配给路由的流量，必须将规则应用于 VRA 的接口，或者创建区域，然后将规则应用于这些区域。

对于此示例，将为到目前为止使用的 VLAN 创建区域。

 VLAN| 区域
 ---- | ---- 
bond1| dmz
bond1.2007| prod（用于生产环境）
bond0.2254| private（用于开发环境）
bond1.1280| 保留供未来使用
bond1.1894| 保留供未来使用

## 创建防火墙规则
在实际创建区域之前，最好先创建要应用于区域的防火墙规则。先创建规则再创建区域，可以立即应用这些规则，而先创建区域再创建规则，需要返回到区域才可应用规则。

以下命令将用于：

* 创建名为 `dmz2private` 的防火墙规则，其缺省操作为丢弃所有包。
* 针对名为 `dmz2private` 的规则，启用已接受和已拒绝流量的日志记录。

1. 在命令提示符中输入 `configure`。

2. 输入以下命令：

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. 输入下一组命令，以便 iptables 允许返回已建立会话的流量。缺省情况下，iptables 不允许这样做，这就是为什么需要显式规则的原因。

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. 输入以下命令以允许 TCP/UDP 流经端口 22（用于 SSH 的缺省端口）。
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. 输入以下命令以允许 TCP/UDP 流经端口 80（用于 HTTP 的缺省端口）。

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. 输入 `commit` 以确保完成时执行所有规则。

7. 通过在命令提示符中输入 `show firewall name dmz2private` 以查看新配置。

8. 通过在提示中输入以下命令，创建要应用于 dmz 区域的防火墙规则。该规则将命名为 public。 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## 创建区域

区域是接口的逻辑表示。在此部分中，您将执行以下操作：

* 创建名为 dmz 的区域和策略，其缺省操作为丢弃发往此区域的包
* 将 dmz 策略设置为使用 `bond1` 接口
* 将 prod 策略设置为使用 `bond1.2007` 接口
* 创建名为 `private` 的区域策略，其缺省操作为丢弃发往此区域的包
* 将名为 `private` 的策略设置为使用 `bond0.2254` 接口

1. 在提示符中输入以下命令：

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. 使用以下命令为区域设置防火墙策略：

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
如果不想将某个防火墙规则应用于区域策略，那么可以将其应用于特定接口。使用以下命令将规则应用于接口。

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
