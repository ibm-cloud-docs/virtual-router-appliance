---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vfp, ipsec, firewall

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

# 为 VFP 接口配置 IPsec 和区域防火墙
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

IPSec 数据报到达时，将通过防火墙规则对其进行处理，然后对其解封装。出现的新数据报与接口完全无关联。通常，这并不是问题，数据报可以继续到达目标接口，但区域防火墙会阻止处理该数据报。将丢弃不是来自区域策略中接口的任何数据报。但是，VFP 接口会告知区域防火墙该数据报确实来自某个接口，这将允许应用规则。

要配置 VFP 接口以使用 IPsec 流量，请首先通过为 VFP 定义单个 IP 来创建功能点：

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

然后，将 VFP 添加到隧道：

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

接下来，将该接口添加到区域（在本例中是具有 dp0bond1 的 INTERNET 区域）：

```
set security zone-policy zone INTERNET interface 'vfp0'
```

现在对服务器执行 ping 操作：

```
[root@acs-jmat-migserver ~]# ping -c 5  172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.3 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.8 ms

--- 172.16.100.1 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 4006ms

rtt min/avg/max/mdev = 44.377/44.728/44.996/0.243 ms
```

可以在多个隧道上使用同一 VFP 接口，或者为其他隧道创建不同的 VFP，只要确保其他任何 VFP 都位于一个区域中，并且具有允许用于未封装框架的流量的规则即可。

还可以将 VFP 添加到其自己的区域中。例如：

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

虽然 VFP 接口是一个“真实”接口，可以使用像 `tshark` 这样的命令对其进行监视，并且可以直接路由流量，但这样做并没什么用。与隧道策略不匹配的任何流量在到达另一端时都会被丢弃。因此，它的通用性不如 VTI 接口。
