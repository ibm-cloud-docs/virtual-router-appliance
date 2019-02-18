---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 High Availability Configuration

Vyatta high availability is supported through the use of VRRP, Virtual Routing Redundancy Protocol. Each gateway group will have two primary VRRP IP addresses, one for the private, and one for the public side of the networks. 

**NOTE:** Private only Vyattas will have only the private VRRP. 

These IP addresses are the target IPs for the Softlayer network infrastructure to route all the subnets on VLANs that are associated with the gateway members. Only one Vyatta will have these VRRP IPs running at a time, the other members of the gateway group will have them administratively down.

Configuration can be synchronized between the two Vyattas with the "config-sync" configuration commands. This configuration will allow one member to push the configuration of specific options to the other Vyatta in a group, and do so selectively. You can push only the firewall rules, or only the IPsec configuration, or any combination of rulesets. 

It is recommended that you do not try to push IP addresses or other network configurations, as config-sync will instantly commit your changes on the other Vyattas, bringing those interfaces online. If you want to dynamically enable interfaces and services, you should use transition scripts to perform this action on failover. Additionally, it is recommended that you use more VRRP IP addresses for your gateway IPs on associated VLANs, this will make failover easier to manage.

Your basic VRRP configuration on both machines will look like this sample configuration:

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

VRRP requires that a real IP address be bound to a virtual interface first before any VRRP advertisements can be sent. In many cases you can simply add an IP from the primary subnet, but this may conflict with future provisions, or you may want to route a VLAN that already has every primary subnet IP allocated to a server. To get around this, you can use an out of band IP address pair on both Vyattas that they can use to talk to each other. Here's an example:

On the first vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

On the second vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

In this case, both vyattas will have their own IP that won't conflict with the subnet that is allocated. You can choose almost any private range you want, just pick a small subnet that will not conflict with any other routes you might have, such as a subnet range over an IPsec tunnel, or a 10.0.0.0/8 address that conflicts with Softlayer's, and you will be fine.

You will also want to add a "sync-group" name as well. All VRRP addresses should be a part of the same sync-group. This is so any failure on one interface will cause all the interfaces in the same sync-group to fail over as well. Otherwise, you could end up with some being MASTER, and others in BACKUP. Use the same name in the bond0 and bond1 native VLAN configuration.

NOTE: The bond0 and bond1 VRRP configuration may have a line for rfc3768-compatibility. This is not needed for VRRP on a vif, only the native VLAN of bond0 and bond1.

On a newly provisioned gateway pair, config-sync will only have minimal configuration:


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

You will have to add rules to determine which configuration branches to migrate over. For example, if you want to have the firewall and ipsec configuration to be pushed, add these commands:


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Once these are committed, any changes to the firewall or ipsec configuration will be pushed to the other vyatta on commit.

**NOTE:** sync-group and sync-map are two different things. The sync-map configuration is for rules to push configuration changes to another Vyatta. The other, sync-group, is for VRRP IPs to fail over as a group instead of one at a time. Configuring one does not affect the other.