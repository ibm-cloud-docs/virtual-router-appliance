---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords:

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Configuring your VLANs
{: #routing-your-vlans}

The {{site.data.keyword.vra_full}} is able to route multiple VLANs over the same network interface (for example, `dp0bond0` or `dp0bond1`). This is accomplished by setting the switch port into trunk mode and configuring virtual interfaces (VIFs) on the device.
{: shortdesc}

For example:

```sh
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.14/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```
{: codeblock}

The commands in the example create two virtual interfaces on the `dp0bond0` interface for a stand-alone Virtual Router Appliances (VRA). The interface `dp0bond0.1432` processes traffic for VLAN 1432 while the interface `dp0bond0.1693` processes traffic for VLAN 1693. `10.0.10.1/24` and `10.0.20.1/24` are assumed to be the gateway IP addresses for `10.0.10.0/24` and `10.0.20.0/24` respectively, and are also assumed to be primary or secondary static/portable subnets from the [Subnets page](https://cloud.ibm.com/classic/network/subnets) in the console.

## High availability commands
{: #high-availability-commands}

For a High Availability (HA) pair of VRAs, the commands are different. The VIF addresses are chosen by the user or made up from the `192.168.0.0/16` or `172.16.0.0/12` private space, and are only used for layer 2 connectivity between the VRAs. More specifically, that connection is used for VRRP advertisements. In addition, the virtual-addresses in the following examples are the gateway IP addresses for each of a customer's primary or secondary subnets. The `vrrp-group` number should match the group number shown in your gateway details page, as well as match the `vrrp-group` set in the rest of the default configuration.

The first command in this example can also be used on a single VRA that is not set up for HA.
{: tip}

For more information on a full HA configuration, including setting the sync-group for the VIF's, see [Associated VLAN subnets with VRRP](/docs/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp).
{: note}

On the first (or only) VRA, run the following commands:

```sh
set interfaces bonding dp0bond0 vif 1432 address 192.168.132.1/30
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 preempt false
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 priority 254
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 sync-group vgroup1
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 virtual-address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 192.168.193.1/30
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 preempt false
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 priority 254
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 sync-group vgroup1
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 virtual-address 10.0.20.1/24
```
{: codeblock}

Then, on the second VRA, run:

```sh
set interfaces bonding dp0bond0 vif 1432 address 192.168.132.2/30
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 preempt false
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 priority 253
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 sync-group vgroup1
set interfaces bonding dp0bond0 vif 1432 vrrp vrrp-group 1 virtual-address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 192.168.193.2/30
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 preempt false
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 priority 253
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 sync-group vgroup1
set interfaces bonding dp0bond0 vif 1693 vrrp vrrp-group 1 virtual-address 10.0.20.1/24
```
{: codeblock}
