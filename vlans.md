---

copyright:
  years: 2017, 2025
lastupdated: "2025-03-14"

keywords:

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Configuring your VLANs
{: #routing-your-vlans}

The {{site.data.keyword.vra_full}} can manage traffic across multiple VLANs through a standard router-on-a-stick design by using a sub-interface on the upstream interface. This means that the upstream and downsteam links are the same physical port(s). The upstream interfaces are generally `dp0bond0` for private (rfc1918) traffic and `dp0bond1` for public traffic. The downstream devices are generally virtual and bare metal servers that were assigned VLANs when they were provisioned. 
{: shortdesc}

To manage VLAN traffic on a VRA, there are two important initial requirements:

* That the VLAN(s) are associated to the VRA and set to "route through" in the cloud portal.
* That you configure a VLAN interface (VIF) with a VLAN number on the corresponding public or private upstream interface.

When creating the VLAN interface(s), you must assign at least one subnet gateway IP address to the interface to act as the subnet gateway for traffic on that VLAN. You should configure every subnet assigned to a VLAN in the IBM Cloud portal in this manner.

For example, in a standalone VRA:

```sh
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.14/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```
{: codeblock}

The commands in the example create two virtual interfaces on the `dp0bond0` interface for a stand-alone Virtual Router Appliances (VRA). The interface `dp0bond0.1432` processes traffic for VLAN 1432 while the interface `dp0bond0.1693` processes traffic for VLAN 1693. `10.0.10.1/24` and `10.0.20.1/24` are assumed to be the gateway IP addresses for `10.0.10.0/24` and `10.0.20.0/24` respectively. They are also assumed to be primary or secondary static/portable subnets from the [Subnets page](https://cloud.ibm.com/classic/network/subnets). In the console, the subnet gateway IP is labelled as such, but for these subnets, the rule is that it is always the second IP in the subnet.

It is common for new users to setup the network ID instead of the subnet gateway on the VLAN interface. This will cause what would be perceived as an outage.
{: tip}

## High availability commands
{: #high-availability-commands}

For a High Availability (HA) pair of VRAs, the commands are different. The VIF addresses are chosen by the user or made up from the `192.168.0.0/16` or `172.16.0.0/12` private space, and are only used for multicast connectivity between the VRAs. More specifically, that connection is used for VRRP advertisements. In addition, the virtual-addresses in the following examples are the gateway IP addresses for each of a customer's primary or secondary subnets. The `vrrp-group` number should match the group number shown in your gateway details page, as well as match the `vrrp-group` set in the rest of the default configuration.

The first set of commands in the following example can also be used on a single VRA that is not set up for HA.
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
