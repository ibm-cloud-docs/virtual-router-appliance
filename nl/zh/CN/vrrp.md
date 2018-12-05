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

# HA 和 VRRP
虚拟路由器设备 (VRA) 支持虚拟路由器冗余协议 (VRRP) 作为高可用性协议。设备的部署以主动/被动方式完成，其中一台机器是主设备，另一台机器是备份设备。两台机器上的所有接口都是同一个“sync-group”的成员，因此，如果一个接口遇到故障，同一组中的其他接口也将发生故障，这样该设备将不再是主设备。当前备份设备将检测到主设备不再广播保持活动/脉动信号消息，然后接管对 VRRP 虚拟 IP 的控制权而成为主设备。

VRRP 是供应网关时配置中最重要的部分。高可用性功能取决于脉动信号消息，因此请务必确保不会阻止这些消息。

## VRRP 虚拟 IP (VIP) 地址

发生故障转移时，VRRP 虚拟 IP（即 VIP）是从主设备更改为备份设备的浮动 IP 地址。VRA 部署时，会在每个接口上分配公用和专用绑定网络连接以及真实 IP。一个 VIP 会同时在两个接口上进行分配，不管设备是独立的还是采用 HA 对形式。如果流量在与 VRA 关联的 VLAN 的子网中有目标 IP，这些流量将直接发送到这些 VRRP VIP。

不应更改任何网关组的 VRRP 虚拟 IP 地址，也不应禁用 VRRP 接口。这些 IP 地址是在关联有 VLAN 时将流量路由到网关的方法。如果 IP 地址不存在，那么无法将流量从 softLayer BCR/FCR 转发到网关本身。

## VRRP 组

VRRP 组包含一组接口或虚拟接口，用于为组中的主（或“主控”）接口提供冗余。组中的每个接口通常位于单独的路由器上。冗余由每个系统上的 VRRP 进程进行管理。VRRP 组具有唯一的数字标识，最多可以分配有 20 个虚拟 IP 地址。  

VRRP 组标识由 SoftLayer 分配，并且不应更改。首次在前端客户路由器 (FCR)/后端客户路由器 (BCR) 后面供应新的网关组时，该组将接收到一个 VRRP 组 1。后续网关组供应将递增此值以防止冲突，即下一个组为组 2，接着是组 3，依此类推。然后，由 SoftLayer 供应过程对其进行计算和分配。改变此值可能会与其他活动组发生冲突，进而发展成为主/主争用，这可能会导致两个网关组都中断。

如果是从先前配置进行迁移的，那么建议您仔细检查配置代码，以确保 VRRP 组标识未以静态方式分配。

VRRP 组标识持久存储在 SoftLayer 数据库中，因此在操作系统重装或升级期间将使用相同的组标识值。在操作系统重装期间，系统分配的值将覆盖任何用户修改的 VRRP 组标识。  

## VRRP 优先级

网关组中的第一台机器的优先级为 254，对于下一个供应的设备，此值将递减。切勿将优先级设置为 255，因为这会将 VIP 定义为“地址所有者”，并且当机器使用不同于运行的活动主设备的配置联机时，可能会导致意外行为。

## VRRP 抢占

应该始终禁用所有 VRRP 接口上的抢占，这样新的设备或正在重装其操作系统的设备才不会尝试接管集群。

## VRRP 通知时间间隔

主接口或 VIF 为了表示自己仍在使用中，会使用 IANA 为 VRRP 分配的多点广播地址（对于 IPv4 为 `224.0.0.18`，对于 IPv6 为 `FF02:0:0:0:0:0:0:12`）将称为“通知”的“脉动信号”包发送到 LAN 网段。

缺省情况下，时间间隔设置为 1。可以增大此值，但建议不要将此值设置为大于 5。在繁忙的网络上，所有 VRRP 通知从主设备到达备份设备所需时间可能远超 1 秒，因此对于大流量网络，应该使用值 2。

## VRRP 同步 (sync-group)

VRRP 同步组（“sync-group”）中接口的同步方式是，如果组中的某个接口故障转移到备份，那么该组中的其他所有接口都将故障转移到备份。例如，在许多情况下，如果主路由器上的一个接口发生故障，那么整个路由器都会故障转移到备用路由器。

此值与 VRRP 组不同，因为此值定义的是当该组中的某个接口表现出故障时，设备上的哪些接口会故障转移。建议所有接口都属于同一个 sync-group，否则某些接口将为主接口并具有网关 IP，而其他接口将为备份接口，这会导致流量不再正常转发。不支持主动/主动配置。

## VRRP RFC 兼容性

RFC 兼容性在接口上启用或禁用用于 VRRP 的 RFC 3768 MAC 地址。这应该在本机接口上启用，而不应该在任何已配置的 VIF 上启用。某些虚拟交换机（大部分为 Vmware）在启用此功能时会遇到问题，并且会导致流量丢弃，而不发送到系统管理程序主机的网关 IP。请保留此设置不变，并且不为任何 VIF 配置此设置。

## VRRP 认证
如果为 VRRP 认证设置了密码，那么还必须定义认证类型。如果设置了密码而未定义认证类型，那么在您尝试落实配置时，系统会生成错误。

与此类似，不能只删除 VRRP 密码而不删除 VRRP 认证类型。如果这样做，那么在您尝试落实配置时，系统会生成错误。如果同时删除了 VRRP 认证密码和认证类型，那么会禁用 VRRP 认证。

IETF 已决定 VRRP V3 不再使用认证。有关更多信息，请参阅 RFC 5798 VRRP。

## V2/3 支持
VRA 同时支持 VRRP V2（缺省）和 V3 协议。在 V2 中，不能同时具有 IPv4 和 IPv6。但是在 V3 中，可以同时具有 IPv4 和 IPv6。

## 使用 VRRP 的高可用性 VPN
借助 VRA，能够将一对虚拟路由器设备与 VRRP 配合使用，通过一个 IPSec 隧道来保持连接。当一个路由器发生故障或因维护而停止运行时，新的 VRRP 主路由器将复原本地和远程网络之间的 IPsec 连接。

配置使用 VRRP 的高可用性 VPN 时，每次将 VRRP 虚拟地址添加到 VRA 接口时，就必须重新初始化 IPsec 守护程序，因为 IPsec 服务只会侦听在初始化因特网密钥交换 (IKE) 服务守护程序时，VRA 上存在的地址的连接。

在主路由器和备份路由器上运行以下命令，以便在发生故障转移时，VIP 换入后，在新的主设备上重新启动 IPsec 守护程序，如以下示例所示：

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## 使用 VRRP 的高可用性防火墙

两个设备采用 HA 对形式时，在主设备上操作时必须小心，确保不会阻止来自其他设备的访问。必须允许两个设备之间通过端口 443 交互，config-sync 才能正常运行，并且必须允许发送和接收 VRRP，包括多点广播范围 224.0.0.0/24。

## 将 VLAN 子网与 VRRP 相关联

对于任何使用 VRRP 的 VIF 配置，需要一个虚拟 IP 地址（子网中的第一个可用 IP，即该子网中的所有内容都将路由到的网关 IP）以及用于两个设备上 VIF 的一个真实接口 IP 地址。为了节省可用 IP，建议真实接口 IP 使用频带外范围（如 192.168.0.0/30），其中一个设备具有 .1 地址，另一个设备具有 .2 地址。可以有多个 VRRP 虚拟 IP，但每个 VIF 上只需要一个真实地址。

下面是具有两个专用 VLAN 和三个子网的 VRRP 配置示例：

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* VRRP sync-group 与 VRRP 组不同。属于 sync-group 的接口的状态更改时，该 sync-group 的其他所有成员都会转换为相同的状态。
* VLAN 接口 (VIF) 的 vrrp-group 编号不必与本机接口中的其中一个编号相同，也不必与其他 VLAN 的相同。但是，强烈建议使同一 VLAN 的所有虚拟地址保持在一个 vrrp-group 中，如 VLAN 10 中所示。
* 本机 VLAN 上的真实接口地址（如 dp0bond1：169.110.20.28/29）并不一定与其 VIP（169.110.21.26/29）在同一子网中。

## 手动 VRRP 故障转移
如果需要强制执行 VRRP 故障转移，可以通过在当前作为 VRRP 主设备的设备上运行以下命令来实现：

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

组标识是本机接口的 VRRP 组标识，并且在您的对中可能不同（如上所述）。

## 连接同步
两个 VRA 设备采用 HA 对形式时，跟踪两个设备之间的有状态连接可能会非常有用，这样在发生故障转移时，就可将故障设备上所有连接的当前状态复制到备份设备，而不必从头开始重建任何当前活动会话（例如，SSL 连接），因为重建会导致用户体验中断。

这称为连接跟踪同步。

要对其进行配置，必须声明故障转移方法是什么、哪个接口将用于发送连接跟踪信息，以及远程同级的 IP：

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

另一个 VRA 将具有相同的配置，但具有不同的 `remote-peer`。

请注意，如果在其他接口上有大量传入连接，那么这可能会使链路饱和，并且会与已声明链路上的其他流量进行竞争。

## VRRP 的启动延迟功能
Vyatta 操作系统 V1801 和更高版本中包含新的 `vrrp` 命令。

`vrrp` 用于指定一种选举协议，该协议将虚拟路由器的责任动态分配给 LAN 上的其中一个 VRRP 路由器。用于控制与虚拟路由器相关联的 IPv4 或 IPv6 地址的 VRRP 路由器称为主控，该路由器会转发发送到这些 IPv4 或 IPv6 地址的包。如果主控变为不可用，那么选举进程将在转发责任中提供动态故障转移。所有协议消息传递都使用 IPv4 或 IPv6 多点广播数据报来执行；因此，该协议可以基于支持 IPv4/IPv6 多点广播的各种多路访问 LAN 技术来运行。

为了最大限度减少网络流量，各个虚拟路由器中只有主控会定期发送 VRRP 通告消息。除非备份路由器具有更高优先级，否则备份路由器不会尝试抢占主控。这将避免服务中断，除非更优先的路径变得可用。此外，还可以通过管理方式禁止所有抢占尝试。如果主控变得不可用，那么最高优先级的备份将在短暂延迟后转换为主控，从而以最短的服务中断时间提供虚拟路由器责任的受控转换。

**注：**在 IBM Cloud 供应的部署中，启动延迟值设置为缺省值。在测试故障转移和高可用性方法时，您可能希望自主决定是否对此进行更改。


### 抢占与不抢占

`vrrp` 协议定义用于确定网络上哪个 VRRP 同级具有较高优先级的逻辑，并据此确定扮演主控角色的最佳同级。使用缺省配置时，将支持 VRRP 执行抢占，这意味着网络上更高优先级的新同级将强制执行主控角色的故障转移。

禁用抢占时，仅当较低优先级的现有同级在网络上不再可用，较高优先级的同级才会执行主控角色的故障转移。在现实世界场景中，禁用抢占有时会非常有用，因为这可更好地应对更高优先级的同级由于该同级本身或其某个网络连接的可靠性问题，而可能已开始定期翻转的情况。此外，这对于防止过早地故障转移到尚未完成网络融合的更高优先级的新同级也非常有用。

### 抢占的假设及限制

如果已将 VRRP 同级配置为禁用抢占，那么在某些情况下可能“看似”发生了抢占，但实际上该场景只是标准的 VRRP 故障转移。如上所述，VRRP 利用 IP 多点广播数据报来确认当前选举的主路由器的可用性。由于是采用第 3 层协议来检测 VRRP 同级的故障，因此请务必将 VRRP 中的故障转移检测延迟到 VRRP 以及网络堆栈的第 1 层到第 2 层已就绪并融合后再执行。在某些情况下，运行 VRRP 的网络接口可能会向协议确认接口在正常运行，但其他底层服务（如“生成树”或“绑定”）可能尚未完成。因此，无法建立同级之间的 IP 连接。如果发生这种情况，那么更高优先级的新同级上的 VRRP 将成为主控，因为无法检测到来自网络上当前主控同级的 VRRP 消息。融合后，会在很短时间内有两个主控 VRRP 同级的情况，这将导致执行 VRRP 的双主控逻辑，这一更高优先级的同级将保持为主控，而较低优先级的同级会成为备份。此场景可能“看似”演示了“无抢占”功能的失败。

### 启动延迟功能

为了解决在接口正常运行事件期间与网络堆栈较低级别融合中延迟的关联问题，同时考虑到其他影响因素，1801p 补丁中引入了称为“启动延迟”的新功能。该功能可使“已重装的”机器上的 VRRP 状态一直保持在 INIT 状态，直至达到预定义的延迟时间（可由网络操作员配置）。通过此延迟值的灵活性，网络操作员可定制其网络和设备在现实世界状况下的特征。

### 命令详细信息

```
vrrp {
start-delay <0-600>
}
```

`start-delay` 的值可以介于 0（缺省值）到 600 秒之间。

### 示例配置

```
interfaces {
    bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
