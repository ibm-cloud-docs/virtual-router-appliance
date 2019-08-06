---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vlan, associate, gateway, route, bypass, disassociate

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

# Managing Your VLANs
{: #managing-your-vlans}

You can perform a variety of actions from the [Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details).

## Associate a VLAN to a Gateway Appliance
{: #associate-a-vlan-to-a-gateway-appliance}

A VLAN needs to be associated to a Gateway Appliance before it can be routed. VLAN association is the linking of an eligible VLAN to a Network Gateway so that it may be routed to a Gateway Appliance in the future. The process of association does not automatically route a VLAN to a Gateway Appliance; the VLAN continues to use front-end and back-end customer routers until it is routed to the Gateway.

VLANs may be associated to only one Gateway at a time and must not have a firewall. Perform the following procedure to associate a VLAN to a Network Gateway.

1. [Access the Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) in the Customer Portal.
2. Select the desired VLAN from the **Associate a VLAN** dropdown list.
3. Click the **Associate** button to associate the VLAN.

After associating a VLAN to the Gateway Appliance, it appears in the Associated VLANs section of the Gateway Appliance Details screen. From this section, the VLAN may be routed to the Gateway or may be disassociated from the Gateway. Additional eligible VLANs may be associated to a Gateway Appliance at any time by repeating the steps above.

## Route an Associated VLAN
{: #route-an-associated-vlan}

Associated VLANs are linked to a Gateway Appliance, but traffic in and out of the VLAN does not hit the Gateway until the VLAN has been routed. After an associated VLAN has been routed, all front and back-end traffic is routed through the Gateway Appliance as opposed to customer routers.

Perform the following procedure to route an associated VLAN:

1. [Access the Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) in the Customer Portal.
2. Locate the desired VLAN in the Associated VLANs section.
3. Select **Route VLAN** from the Actions dropdown menu.
4. Click **Yes** to route the VLAN.

After routing a VLAN, all front-end and back-end traffic moves from the customer routers to the Network Gateway. Additional controls related to traffic and the Gateway Appliance itself may be taken by accessing the Gateway's management tool. Routing through the Network Gateway may be discontinued at any time by [bypassing the Gateway Appliance](#bypass-gateway-appliance-routing-for-a-vlan).

## Bypass Gateway Appliance Routing for a VLAN
{: #bypass-gateway-appliance-routing-for-a-vlan}

After a VLAN has been routed, all front and back-end traffic travels through the Network Gateway. At any time, the Gateway Appliance may be bypassed so that traffic will return to the front and back-end customer routers (FCR and BCR).

Bypassing a VLAN allows the VLAN to remain associated to the Network Gateway. If the VLAN should no longer be associated with the Gateway Appliance, refer to [Disassociate a VLAN from a Gateway Appliance](#disassociate-a-vlan-from-a-gateway-appliance).

Perform the following procedure to bypass Gateway routing for a VLAN:

1. [Access the Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) in the Customer Portal.
2. Locate the desired VLAN in the Associated VLANs section.
3. Select **Bypass VLAN** from the Actions dropdown menu.
4. Click **Yes** to bypass the Gateway.

After bypassing the Network Gateway, all front-end and back-end traffic routes through the FCR and BCR associated with the VLAN. The VLAN will remain associated with the Gateway Appliance and may be routed back to the Gateway Appliance at any time.

## Disassociate a VLAN from a Gateway Appliance
{: #disassociate-a-vlan-from-a-gateway-appliance}

VLANs may be linked to one Gateway Appliance at a time through [association](#associate-a-vlan-to-a-gateway-appliance). Association allows the VLAN to be routed to the Gateway Appliance at any time. If a VLAN should be associated to another Gateway Appliance, or if the VLAN should no longer be associated to its Gateway, disassociation is required. Disassociation removes the "link" from the VLAN to the Gateway Appliance, allowing it to be associated to another Gateway, if necessary.

Perform the following procedure to disassociate a VLAN from a Gateway Appliance:

1. [Access the Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) in the Customer Portal.
2. Locate the desired VLAN in the Associated VLANs section.
3. Select **Disassociate** from the **Actions** dropdown menu.
4. Click **Yes** to disassociate the VLAN.

After disassociating a VLAN from a Gateway Appliance, the VLAN may be associated to another Gateway. The VLAN may also be associated back to the Gateway Appliance at any time. After disassociating a VLAN from a Gateway Appliance, the VLAN's traffic cannot be routed through the Gateway. VLANs must be associated to a Gateway Appliance before they can be routed.

## Route Multiple VLANs Over Same Network Interface
{: #route-multiple-vlans-over-same-network-interface}

The {{site.data.keyword.vra_full}} is able to route multiple VLANs over the same network interface (for example, `dp0bond0` or `dp0bond1`). This is accomplished by setting the switch port into trunk mode and configuring virtual interfaces (VIFs) on the device.

For example:

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

The commands above create two virtual interfaces on the `dp0bond0` interface. The interface `dp0bond0.1432` processes traffic for VLAN 1432 while the interface `dp0bond0.1693` processes traffic for VLAN 1693.

## Add Multiple Subnets to a Single VLAN
{: #add-multiple-subnets-to-a-single-vlan}

The following is an example configuration that includes, at the end, the addition of a sample subnet (`159.8.67.96/28`) for a public VLAN (1451). The address for each VIF (VLAN Interface) is not routed at the BCR (Backend Customer Router) or FCR (Frontend Customer Router). It is only used for VRRP/High Availability communication between two Vyattas.

Subnets can be chosen from any unused private IP space. As a result, `10.0.0.0/8` is generally excluded here. Subnets from `192.168.0.0/16` were chosen for the examples below, but subnets from `172.16.0.0/12` can also be used.

The `virtual-address` is where the new subnet should be configured. In most cases, the gateway IP address of the subnet is what should be configured. The gateway IP bound to the VIF is then used as the next-hop or default route for any Baremetal or Virtual Servers set up on the new subnet behind the VRA.

The following example shows `159.8.67.97/28` being bound to the VIF, so that all traffic for the `159.8.67.98/28` subnet can be managed by the VRA(s).

```
set interfaces bonding dp0bond0 vif 1623 address '192.168.10.2/30'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 virtual-address '10.127.132.129/26'
set interfaces bonding dp0bond0 vif 1750 address '192.168.20.2/30'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 virtual-address '10.126.19.129/26'
set interfaces bonding dp0bond1 vif 788 address '192.168.150.2/30'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 virtual-address '159.8.106.129/28'
set interfaces bonding dp0bond1 vif 1451 address '192.168.200.2/30'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.67.97/28'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.86.49/29'
```
