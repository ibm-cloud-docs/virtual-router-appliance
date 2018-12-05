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

# Vyatta 5400 常见迁移问题
下表说明在从 Vyatta 5400 设备迁移到 IBM 虚拟路由器设备后可能遇到的常见问题或行为更改。在某些情况下，包含用于解决问题的变通方法。

## 针对有状态防火墙的基于接口的全局状态策略

### 问题
从 R5.1 开始，针对有状态防火墙设置“State of State-policy”时的行为发生了更改。在 R5.1 之前的版本中，如果为有状态防火墙设置 `state - global -state -policy`，那么 vRouter 针对自动返回会话通信自动添加暗含的 `Allow` 规则。

在 R5.1 及更高发行版中，必须在虚拟路由器设备上添加 `Allow` 规则设置。有状态设置在 Vyatta 5400 设备上对接口生效，在 VRA 设备上对协议生效。

### 变通方法
如果在 Ingress/Inside 接口上应用 `firewall-in` 规则，那么必须在 Egress/Outside 接口上应用 `Firewall-out` 规则。否则，将在 Egress/Outside 接口丢弃返回流量。        

## 防火墙规则中的状态启用

### 问题
如果未配置 `global-state-policy`，那么此行为更改不受影响。

另外，如果在每个规则中使用 `state enable` 选项而不是 `global-state-policy`，那么行为更改将不受影响。

## 基于区域的策略：本地区域处理

### 问题
不存在“local-zone”伪接口以分配给区域策略。

### 变通方法
可通过将基于区域的防火墙应用于物理接口并将接口防火墙应用于回送接口，从而模拟此行为。回送接口中的防火墙过滤流入和流出路由器的一切流量。

例如：

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## 防火墙、NAT、路由和 DNS 的操作顺序

### 问题
如果在 IBM 虚拟路由器设备上部署 Masquerade Source NAT，那么您无法使用防火墙来确定主机的因特网访问权。这是因为发布 NAT 地址将相同。

对于 Vyatta 5400 设备，此操作可行，因为是在 NAT 之前通过防火墙，从而允许限制主机访问因特网。

### 变通方法
VRA 需要新的路由方案：
![路由 DNS](./images/routing-dns.png)

## 基于策略的路由表

### 问题
配置中的词“Table”在 v5400 基于策略的路由中为可选，但是对于 VRA，如果操作为 `accept`，那么 **Table** 字段为必需。如果 VRA 配置中的操作为 `drop`，那么 Table 字段为可选。

### 变通方法
“Table Main”是 Vyatta 5400 基于策略的路由中的可用选项。VRA 中的等效项为“routing-instance default”。



## 隧道接口上基于策略的路由

### 问题
在虚拟路由器设备 PBR（基于策略的路由）上，策略可应用于处理入站流量的数据平面接口，但是不能应用于回送、隧道、网桥、OpenVPN、VTI 和 IP 未编号接口。

### 变通方法
此问题当前无变通方法。

## TCP-MSS

### 问题
IBM 虚拟路由器设备使用 nftables 并且不支持 TCP-MSS。

### 变通方法
此问题当前无变通方法。

## OpenVPN

### 问题
在虚拟路由器设备上使用 `push-route` 参数时，OpenVPN 不会开始工作。

### 变通方法
使用 `openvpn-option` 参数，而不使用 `push-route`。

## 基于 IPSEC + OSPF 的 GRE/VTI

### 问题
* 如果 VIF 配置了多个子网，那么流量无法穿过 VRA 中的这些子网。                                             
* VLAN 间路由在 VRA 上无效。

### 变通方法
使用隐含的 allow 规则以接受各 VIF 接口之间的流量。

## IPSec

### 问题
IPSec（基于前缀）不适用于 IN 过滤器。

### 变通方法
使用 IPSec（基于 VTI）。

## IPSEC“match-none”

### 问题
使用 Vyatta 5400 设备时，允许以下防火墙规则：

set firewall name allow rule 10 ipsec 



但是，使用 IBM 虚拟路由器设备时，没有 IPSec。



### 变通方法
VRA 设备的可行备用规则：

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                    
```

## IPSEC“match-ipsec”

### 问题
利用 Vyatta 5400 设备，允许以下防火墙规则：

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'



但是，使用 IBM 虚拟路由器设备时，没有 IPSec。



### 变通方法
添加协议 `ESP` 和 `AH`（分别是 IP 协议 50 和 51）。

`action` 规则可以为 `accept` 或 `drop`，如下所示：

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## 使用 DNAT 的站点到站点 IPSEC

### 问题
IPSec（基于前缀）不适用于 DNAT。                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1)===S-S-IPsec=== (12.0.0.1)
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

以上片段是在 Vyatta 5400 中解密 IPSec 包后 DNAT 转换的小型设置示例。在示例中，有两个 Vyatta，即 `vyatta1 (11.0.0.1)` 和 `vyatta2 (12.0.0.1)`。在 `11.0.0.1` 和 `12.0.0.1` 之间建立了 IPsec 对等连接。在本例中，客户机的目标为 `172.16.1.245`，源自 `10.103.0.1`，两者之间为端到端连接。此场景的预期行为是，在包头中，目标地址 `172.16.1.245` 将转换为 `10.71.68.245`。

最初，Vyatta 5400 设备在入站 IPSec 上执行 DNAT，终止接口，并使用连接跟踪表将流量正常返回至 IPsec 隧道。


在虚拟路由器设备上，配置的运行方式不同。会创建会话，但是在连接跟踪表撤销 DNAT 更改后，返回流量会绕过 IPsec 隧道。然后，VRA 在不使用 IPSec 加密的连线上发送包。上游设备预期没有此流量，很有可能将其丢弃。虽然端到端连接中断了，这是预期的行为。


### 变通方法
为适用以上联网场景，IBM 创建了 RFE。

当前正在评估该 RFE，此时我们建议以下变通方法：



**接口配置命令**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN 配置命令**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT 配置命令**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**协议配置命令**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR 配置命令**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### 问题
虚拟路由器设备中不再支持 PPTP。                                                                                                                                                   

### 变通方法
请改为使用 L2TP 协议。

## 用于 IPSec 重新启动的脚本

### 问题
每次将 VRRP 虚拟地址添加到高可用性 VPN 上的 IBM 虚拟路由器设备后，就必须重新初始化 IPsec 守护程序。这是因为在初始化 IKE 服务守护程序时，IPsec 服务仅侦听与 VRA 上提供的地址的连接。


对于一对具有 VRRP 的 VRA，如果主路由器没有提供初始化期间在设备上提供的 VRRP 虚拟地址，那么备用路由器可能没有此地址。因此，要在发生 VRRP 状态转换时重新初始化 IPSec 守护程序，请在主和备份路由器上运行以下命令：


```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## 最新计数和最新时间

### 问题

以下规则的目的是针对使用任何地址的 SSH，将 SSH 连接限制为每 30 秒 3 个：


```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

在 IBM 虚拟路由器设备上，此规则有以下问题：



* 最新计数和最新时间选项已弃用。

* 由于先前的问题，规则无法按预期运行，并且将阻止与应用的接口的所有 SSH 连接。

### 变通方法
请改为使用 CPP。

## 设置系统连接跟踪问题

### 问题

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## 设置系统连接跟踪超时

### 问题
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## 基于时间的防火墙

### 问题
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### 问题
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## S-S Ipsec VPN 中的特定应用程序或端口已中断

### 问题

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
可能的补全：
   <Enter> 执行当前命令
   port    任何 TCP 或 UDP 端口
   prefix  远程 IPv4 或 IPv6 前缀
                           set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## 日志记录行为中的重大更改

### 问题
在 Vyatta 5400 设备和 IBM 虚拟路由器设备之间，日志记录行为有一项重大更改，从逐个会话变为逐个包进行日志记录。

* 会话日志记录：记录有状态会话状态转换

* 包日志记录：记录与规则匹配的所有包。由于包日志记录在日志文件中“以包为单位”进行记录，因此吞吐量和磁盘容量压力会显著下降。

* vRouter 的日志记录功能可用于捕获防火墙活动。与任何日志记录功能类似，仅应在对特定问题进行故障诊断时启用此功能，并尽快禁用日志记录。



管理防火墙/NAT 会话的有状态防火墙写入“session units”中。建议使用会话日志记录。以下描述了每个设置示例：



**会话/日志记录**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**包日志记录防火墙**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>`
