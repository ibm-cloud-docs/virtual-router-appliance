---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 常见技术问题
以下常见问题涉及 IBM 虚拟路由器设备 (VRA) 的配置以及从 Vyatta 5400 迁移到 VRA。

## 如何允许从专用 VLAN 上的主机发往因特网的流量？
此流量需要获取公共源 IP，因此源 NAT 需要使用 VRA 的公共 IP 来伪装专用 IP。

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

上面的配置仅从源自专用 `10.0.0.0/8` 网络中服务器的流量执行 SNAT。

这可以确保它不会干扰已具有可通过因特网路由的源地址的包。

## 如何过滤发往因特网的流量，而只允许特定的协议/目标？
这是源 NAT 和防火墙需要组合使用时的一个常见问题。

在设计规则集时，请记住 VRA 中的操作顺序。

简而言之，防火墙规则在 SNAT *之后*应用。

为了阻止防火墙中的所有出局流量，而只允许特定的 SNAT 流，您需要将过滤逻辑移至 SNAT 上。

例如，为了仅允许某个主机发往因特网的 HTTPS 流量，SNAT 规则将为：

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` 是 VRA 的公共地址。 

强烈建议使用 VRA 的 VRRP 公共地址，以便可以区分主机和 VRA 公共流量。

假定 `150.1.2.3` 是 VRRP VRA 地址，`150.1.2.5` 是真实的 dp0bond1 地址。

应用于 `dp0bond1 out` 的有状态防火墙将为：

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

请注意，通过源 NAT 和防火墙的组合使用，实现了所需的设计目标。 

确保规则适合您的设计，并且没有其他规则会允许应该阻止的流量。 

## 如何使用基于区域的防火墙来保护 VRA 自身？
VRA 没有`本地区域`。

可以改为利用控制平面管制 (CPP) 功能，因为此功能在回送时会作为 `local` 防火墙来应用。

请注意，这是一个无状态的防火墙，因此需要显式允许以 VRA 自身为源的出站会话的返回流量。

## 如何限制 SSH 并阻止来自因特网的连接？
公认的最佳做法是不允许来自因特网的 SSH 连接，而使用其他访问专用地址的方法（如 SSL VPN）。

缺省情况下，VRA 在所有接口上都接受 SSH。为了仅在专用接口上侦听 SSH 连接，需要设置以下配置：

```
set service ssh listen-address '10.1.2.3'
```

请记住，IP 地址需要替换为属于 VRA 的地址。
