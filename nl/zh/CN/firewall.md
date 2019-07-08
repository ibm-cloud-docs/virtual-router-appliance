---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: firewall, manage, stateless, stateful, alg, firewall, rules, CPP, Logging

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
{:important: .important}

# 管理 IBM 防火墙
{: #manage-your-ibm-firewalls}

虚拟路由器设备 (VRA) 能够处理防火墙规则，以保护通过设备路由的 VLAN。VRA 中的防火墙可以分为两个步骤：

1. 定义一组或多组规则。
2. 将一组规则应用于一个接口或一个区域。一个区域包含一个或多个网络接口。

创建防火墙规则后对其进行测试非常重要，这可确保规则按预期工作，并且确保新规则不会限制对设备的管理访问权。

在 `dp0bond1` 接口上处理规则时，建议使用 `dp0bond0` 连接到设备。也可以选择使用智能平台管理接口 (IPMI) 连接到控制台。

## 无状态和有状态
{: #stateless-vs-stateful}

缺省情况下，防火墙是无状态的，但如果需要，也可以将其配置为有状态。无状态防火墙需要针对双向流量的规则，而有状态防火墙用于跟踪连接并自动允许已接受流的返回流量。要配置有状态防火墙，必须指明要以有状态方式操作的规则。

要启用对 `tcp`、`udp` 或 `icmp` 流量的“有状态”跟踪，请运行以下命令：

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

请注意，`global-state-policy` 命令仅跟踪与显式设置相应协议的防火墙规则相匹配的流量状态。例如：

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

由于 `GLOBAL_STATELESS` 未指定 `protocol tcp`，因此 `global-state-policy tcp` 命令不会应用于此规则。

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

在本例中，`protocol tcp` 已显式定义。`global-state-policy tcp` 命令将对与 `GLOBAL_STATEFUL_TCP` 的规则 1 相匹配的流量启用有状态跟踪。


要使单个防火墙规则为“有状态”，请运行以下命令：

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
这将对所有可以进行有状态跟踪且与 `TEST` 的规则 1 相匹配的流量启用有状态跟踪，而不管 `global-state-policy` 命令是否存在。

## 用于协助有状态跟踪的 ALG
{: #alg-for-assisted-stateful-tracking}

有些协议（如 FTP）所利用的会话更为复杂，一般的有状态防火墙操作难以跟踪。有些预配置的模块支持对这些协议进行有状态管理。

建议禁用这些 ALG 模块，除非必须使用这些模块才能成功使用相应协议。

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## 防火墙规则集
{: #firewall-rule-sets}

防火墙规则可分组为多个命名集，这样可更轻松地将规则应用于多个接口。每个规则集都有一个关联的缺省操作。请考虑以下示例：
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

在规则集 `ALLOW_LEGACY` 中，定义了两个规则。第一个规则用于丢弃源自名为 `network-group1` 的地址组的所有流量。第二个规则用于废弃并记录来自名为 `network-group2` 的地址组且以 telnet 端口 (`tcp/23`) 为目标的所有流量。缺省操作指示接受其他所有流量。

## 允许访问数据中心
{: #allowing-data-center-access}

IBM© 提供多个 IP 子网，用于为在数据中心内运行的系统提供服务和支持。例如，DNS 解析器服务在 `10.0.80.11` 和 `10.0.80.12` 上运行。在供应和支持期间会使用其他子网。可以在[此主题](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges)中找到在数据中心中使用的 IP 范围。

通过在防火墙规则集开头放置正确的 `SERVICE-ALLOW` 规则以及操作 `accept`，可允许访问数据中心。必须应用该规则集的位置取决于实现的路由和防火墙设计。

建议将防火墙规则放入产生重复工作最少的位置。例如，在 `dp0bond0` 上允许后端子网入站流量所涉及的工作，要少于对每个 VLAN 虚拟接口允许后端子网出站流量。

### 逐个接口应用的防火墙规则
{: #per-interface-firewall-rules}

在 VRA 上配置防火墙的一种方法是对每个接口应用防火墙规则集。在这种情况下，接口可以是数据平面接口 (`dp0s0`) 或虚拟接口 (`dp0bond0.303`)。每个接口有三种可能的防火墙分配：

`in` - 根据通过接口进入的包来检查防火墙。这些包可以遍历 VRA 或以 VRA 为目标。

`out` - 根据通过接口流出的包来检查防火墙。这些包可以遍历 VRA 或以 VRA 为源。

`local` - 根据直接发往 VRA 的包来检查防火墙。

一个接口可以在每个方向上应用多个规则集。规则集按配置顺序应用。请注意，如果防火墙流量源自的 VRA 设备使用了逐个接口应用的防火墙，那么无法做到这一点。

例如，要将 `ALLOW_LEGACY` 规则集分配给 `bp0s1` 接口的 `in` 选项，您将使用以下配置命令：

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## 控制平面管制 (CPP)
{: #control-plane-policing-cpp-}

控制平面管制 (CPP) 通过允许您配置分配给所需接口的防火墙策略，并将这些策略应用于进入 VRA 的包，从而防御对虚拟路由器设备的攻击。

在分配给任何类型的 VRA 接口（例如，数据平面接口或回送）的防火墙策略中使用 `local` 关键字时，都会实现 CPP。与应用于遍历 VRA 的包的防火墙规则不同，针对进出控制平面的流量的防火墙规则，其缺省操作为 `Allow`。如果不希望执行缺省行为，用户必须添加显式丢弃规则。

VRA 提供基本的 CPP 规则集作为模板。可以通过运行以下命令，将其合并到自己的配置中：

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

合并此规则集后，将添加名为 `CPP` 的新防火墙规则集，并将其应用于回送接口。建议修改此规则集以适合您的环境。

请注意，CPP 规则不能是有状态的，并且仅应用于入口流量。

## 区域防火墙
{: #zone-firewalling}

虚拟路由器设备中的另一个防火墙概念是基于区域的防火墙。在基于区域的防火墙操作中，会将接口分配给区域（每个接口仅一个区域），并且会将防火墙规则集分配给区域之间的边界，其理念是使一个区域中的所有接口都具有相同的安全级别并能够自由路由。仅当流量从一个区域传递到另一个区域时，才会审查流量。区域会丢弃未显式允许的所有传入流量。

一个接口可以属于一个区域，也可以具有逐个接口应用的防火墙配置；但一个接口不能两者兼备。

假设有以下办公室方案，其中包含三个部门，每个部门都有自己的 VLAN：

* 部门 A - VLAN 10 和 20（接口 dp0bond1.10 和 dp0bond1.20）
* 部门 B - VLAN 30 和 40（接口 dp0bond1.30 和 dp0bond1.40）
* 部门 C - VLAN 50（接口 dp0bond1.50）

可以为每个部门创建一个区域，然后可以将该部门的接口添加到该区域。以下示例说明此过程：
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

`commit` 命令会将每个区域作为一个接口填充，并且缺省丢弃规则将废弃尝试从外部进入该区域的所有流量。在此示例中，VLAN 10 和 20 可以传递流量，因为它们位于同一区域 (`DEPARTMENA`) 中，但 VLAN 10 和 VLAN 30 无法传递流量，因为 VLAN 30 位于不同区域 (`DEPARTMENTB`) 中。

每个区域中的接口都可以自由传递流量，并且可为区域之间的交互定义规则。可以从离开一个区域而进入另一个区域的角度来配置规则集。

以下命令显示有关如何配置规则的示例：

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

此命令将从 DEPARTMENTC 到 DEPARTMENTB 的转换与名为 `ALLOW_PING` 的规则集相关联。将根据此规则集来检查从 DEPARTMENTC 区域进入 DEPARTMENTB 区域的流量。

请务必了解，这种从区域 DEPARTMENTC 进入到区域 DEPARTMENTB 的分配不会针对逆向情况做出任何规定。如果没有规则允许从区域 DEPARTMENTB 进入区域 DEPARTMENTC，那么流量（ICMP 应答）不会返回给 DEPARTMENTC 中的主机。

`ALLOW_PING` 将作为 DEPARTMENTB 区域中各接口（dp0bond1.30 和 dp0bond1.40）上的 `out` 防火墙应用。由于这是由区域策略安装的，因此只会根据规则集来检查源自源区域的接口 (dp0bond1.50) 的流量。

## 会话和包日志记录
{: #session-and-packet-logging}

VRA 支持两种类型的日志记录：

1. 会话日志记录。使用 ``security firewall session-log`` 命令可配置防火墙会话日志记录。

	对于 UDP、ICMP 和所有非 TCP 流，会话在流的生命周期内会依次转换为四个状态。可以配置 VRA 对每次转换记录一条消息。TCP 有更多状态转换，可通过配置对每次转换都进行记录。以下是设置防火墙会话日志的示例：

```
set security firewall session-log icmp established
set security firewall session-log tcp established
set security firewall session-log udp established
```

2. 逐个包应用的日志记录。在防火墙或 NAT 规则中包含关键字 ``log`` 可记录与规则匹配的每个网络包。

	逐个包应用的日志记录发生在包转发路径中，并会生成大量输出。请务必注意，逐个包的日志记录会大大降低 VRA 的吞吐量，导致性能问题，并显著增加用于日志文件的磁盘空间。我们建议仅将逐个包的日志记录用于调试用途。对于所有运作用途，应该使用有状态会话日志记录，而不是逐个包的日志记录。
	{: important}
