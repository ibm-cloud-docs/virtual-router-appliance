---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: vlan, route

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Routing Your VLANs
{: #routing-your-vlans}

The {{site.data.keyword.vra_full}} is able to route multiple VLANs over the same network interface (for example, `dp0bond0` or `dp0bond1`). This is accomplished by setting the switch port into trunk mode and configuring virtual interfaces (VIFs) on the device.
{: shortdesc}

For example: 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.14
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

The commands above create two virtual interfaces on the `dp0bond0` interface for a stand-alone Virtual Router Appliances (VRA). The interface `dp0bond0.1432` processes traffic for VLAN 1432 while the interface `dp0bond0.1693` processes traffic for VLAN 1693. `10.0.10.1/24` and `10.0.20.1/24` are assumed to be the gateway IP addresses for `10.0.10.0/24` and `10.0.20.0/24` respectively, and are also assumed to be primary or secondary static/portable subnets from the [Subnet List](https://cloud.ibm.com/classic/network/subnets) in portal.

## High Availability Commands
{: #high-availability-commands}

For a High Availability (HA) pair of VRAs, the commands will be different, as shown below. The VIF addresses are chosen by the user or made up from the `192.168.0.0/16` private space and are only used for Layer 2 connectivity between the VRAa. More specifically, that connection is used for VRRP advertisements.

The first command below can also be used on a single VRA that is not set up for High Availability.
{: tip}

For more information, please see [Associated VLAN subnets with VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp).
{: note}

On the first (or only) VRA, run the following commands:

```
set interfaces bonding dp0bond0 vif 1432 address 192.168.132.1/30
set interfaces bonding dp0bond0 vif 1432 virtual-address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 192.168.193.1/30
set interfaces bonding dp0bond0 vif 1693 virtual-address 10.0.20.1/24
```

Then, on the second VRA, run:

```
set interfaces bonding dp0bond0 vif 1432 address 192.168.132.2/30
set interfaces bonding dp0bond0 vif 1432 virtual-address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 192.168.193.2/30
set interfaces bonding dp0bond0 vif 1693 virtual-address 10.0.20.1/24
```
