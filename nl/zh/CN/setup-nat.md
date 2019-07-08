---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, setup, 5400

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

# 在 Vyatta 5400 上设置 NAT 规则
{: #setting-up-nat-rules-on-vyatta-5400}

本主题包含 Vyatta 上使用的网络地址转换 (NAT) 规则的示例。

## 一对多 NAT 规则 (masquerade)
{: #one-to-many-nat-rule-masquerade-}

在提示符中输入以下命令：

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

来自 `10.xxx.xxx.xxx` 网络中机器的连接请求会映射到 bond1 上的 IP，并在出站时接收关联的临时端口。目的是分配较大的一对多 masquerade 规则编号，使其不会与您可能拥有的较小编号的 NAT 规则冲突。

必须将服务器配置为通过 VRA 传递其因特网流量，以使其缺省网关为受管虚拟 LAN (VLAN) 的专用 IP 地址。例如，对于 `bond0.2254`，网关为 `10.52.69.201`。这应该是传递因特网流量的服务器的网关地址。
{: note}

使用以下命令来帮助对 NAT 进行故障诊断：

  '''
  run show nat source translations detail
  '''
  {: tip}

## 一对一 NAT 规则
{: #one-to-one-nat-rule}

以下命令显示如何设置一对一 NAT 规则。请注意，规则编号设置为小于 masquerade 规则编号。这是为了使一对一规则优先于一对多规则。

不能对一对一映射的 IP 地址进行伪装。如果转换了某个 IP 的入站方向，那么还必须转换该 IP 的出站方向，才能使流量双向传输。
{: tip}

以下命令适用于源和目标规则。在配置方式下输入 `show nat` 以查看 NAT 规则类型。

  使用以下命令来帮助对 NAT 进行故障诊断：`run show nat source translations detail`。
  {: tip}

确保处于配置方式下后，在提示符中输入以下命令：

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

如果流量进入 bond1 上的 IP `50.97.203.227`，该 IP 会映射到 IP `10.52.69.202`（在定义的任意接口上）。如果流量通过 IP `10.52.69.202`（在定义的任意接口上）流出，那么该 IP 会转换为 IP `50.97.203.227`，然后在接口 bond1 上继续执行出站。

不能对一对一映射的 IP 地址进行伪装。如果转换了某个 IP 的入站方向，那么还必须转换该 IP 的出站方向，才能使其流量双向传输。
{: note}

## 通过 VRA 添加 IP 范围
{: #adding-ip-ranges-through-your-vra}

根据 VRA 配置，您可能希望接受特定的 IBM© Cloud IP 地址。

新的 vRouter 部署随附在名为 `SERVICE-ALLOW` 的防火墙规则中定义的 IBM Cloud 服务网络 IP 地址。

下面是 `SERVICE-ALLOW` 的示例。这并不是完整的专用 IP 规则集。

```
~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~
```

一旦定义了防火墙规则，现在就可以根据需要相应对其进行分配。下面列出了两个示例。

应用于专区：`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

应用于结合接口：`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
