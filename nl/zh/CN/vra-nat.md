---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, prefix, IPsec, rules

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

# 将 NAT 与基于前缀的 IPsec 配合使用
{: #using-nat-with-prefix-based-ipsec}

在[为 VFP 接口配置 IPSec 和区域防火墙](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)主题中，我们创建了一个 VFP 接口，并将其设置为用于 IPsec 隧道。

我们可以在 NAT 规则中使用相同的接口，还可以使用入站和出站接口声明，但会有一个额外的警告。

下面是一些示例 NAT 规则：

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

先前示例是相同 IP 的标准双向一对一源和目标 NAT。但是，要确保 NAT 流量正确通过隧道，您需要在另一端使用静态路由：

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

使用静态路由的原因是 IPsec 守护程序已经为远程前缀创建了内核路径：

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

对源 `10.177.137.251` 执行 ping 操作使其转换为 `172.16.100.2` 时，流量将通过 `dp0bond1` 流出，但无法通过隧道传输，并且绝不会正确匹配 NAT 规则。静态路由解决了此问题：

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

这将为流量创建一个更明确的路径来通过 `vfp0`。

此时，NAT 将按配置工作，并且流量将穿过隧道。

NAT 需要一个路径，其中的 CIDR 小于 IPsec 远程前缀（其大小不能相同）并将流量指向 `vfp0` 虚拟接口。
{: tip}

一切就绪后，可以对其执行 ping 操作并进行验证：

```
[root@acs-jmat-migserver ~]# ping 172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.2 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.3 ms
^C
--- 172.16.100.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 44.247/44.431/44.727/0.272 ms
 
vyatta@acs-jmat-migsim01:~$ show nat source translations
Pre-NAT                 Post-NAT                Prot    Timeout
10.177.137.251:7553     172.16.200.2:7553       icmp    48
```
