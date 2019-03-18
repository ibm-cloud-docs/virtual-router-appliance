---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 高可用性配置
{: #vyatta-5400-high-availability-configuration}

Vyatta 高可用性通过使用 VRRP、虚拟路由冗余协议进行支持。每个网关组会有两个主 VRRP IP 地址，一个用于网络的专用端，一个用于公用端。 

**注：**仅专用的 Vyatta 将只有专用 VRRP。 

这些 IP 地址是 SoftLayer 网络基础架构的目标 IP，用于路由与网关成员关联的 VLAN 上的所有子网。在某一时间，网关组中只有一个 Vyatta 会运行这些 VRRP IP，该组中的其他成员将通过管理方式使其停止运行。

可以通过“config-sync”配置命令使配置在两个 Vyatta 之间同步。此配置将允许一个成员将特定选项的配置推送到组中的其他 Vyatta，并支持有选择性地执行此操作。您可以仅推送防火墙规则，仅推送 IPsec 配置，或者推送这两个规则集的任意组合。 

建议不要尝试推送 IP 地址或其他网络配置，因为 config-sync 会立即在其他 Vyatta 上落实更改，从而使这些接口联机。如果要动态启用接口和服务，应使用转换脚本在故障转移时执行此操作。此外，建议将更多 VRRP IP 地址用于关联 VLAN 上的网关 IP，这将使故障转移更易于管理。

两台机器上的基本 VRRP 配置类似于以下样本配置：

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP 需要先将一个真实的 IP 地址绑定到虚拟接口后，才能发送任何 VRRP 通知。在许多情况下，可以简单地从主子网添加 IP，但这可能会与将来的供应相冲突，或者您可能希望将已经分配有每个主子网 IP 的 VLAN 路由到服务器。为了解决此问题，可以在两个 Vyatta 上使用一个频带外 IP 地址对，使这两个设备可以用来互相通信。下面是一个示例：

在第一个 Vyatta 上：

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

在第二个 Vyatta 上：

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

在本例中，两个 Vyatta 都有自己的 IP，因此不会与分配的子网发生冲突。您可以选择所需的几乎任何专用 IP 范围，只要选取一个不会与您可能拥有的其他任何路由相冲突的小型子网（例如，通过 IPsec 隧道路由的子网范围），或者选择与 Softlayer 的地址相冲突的 10.0.0.0/8 地址，那么就不会有问题。

您还需要添加“sync-group”名称。所有 VRRP 地址都应该属于同一个 sync-group。这样做的目的是，一个接口上发生的任何故障都会导致同一 sync-group 中的所有接口也执行故障转移。如果 VRRP 地址不属于同一个 sync-group，那么最终会出现一些是 MASTER 而另一些是 BACKUP 的情况。请在 bond0 和 bond1 本机 VLAN 配置中使用相同的名称。

注：bond0 和 bond1 VRRP 配置中可能有一行用于表示 RFC3768 兼容性。vif 中的 VRRP 不需要此行，只有 bond0 和 bond1 的本机 VLAN 需要。

在新供应的网关对上，config-sync 只有最少的配置：


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

您必须添加规则来确定要迁移的配置分支。例如，如果要推送防火墙和 IPSec 配置，请添加以下命令：


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

一旦落实完这些配置，对防火墙或 IPSec 配置进行的所有更改都将在落实时推送到其他 Vyatta。

**注：**sync-group 和 sync-map 是两个不同的配置。sync-map 配置用于供规则将配置更改推送到其他 Vyatta。而 sync-group 用于使 VRRP IP 作为一个组整体进行故障转移，而不是一次故障转移一个 VRRP IP。配置其中一项不会影响另一项。
