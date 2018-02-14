---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# How to view the Vyatta in the Portal

A single, 1 Gbps gateway has been depoloyed in the San Jose datacenter; it has redundant access to both the public and private infrastructure. Use the **Devices** tab under the **Network > Gateway Appliances** menu to see the actual configuration of hardware deployed.

Note the **2546 Public VLAN** and **2576 Private VLAN** and **Network Hardware Show Details** link. The networks are dedicated and only contain gateway devices. They are under the control of the provisioning platform (you cannot provision normal servers through this tab).

To view the networking associations, click the **Network, Gateway Appliances** menu in the Web Portal.

The network switch ports and Ethernet adapters on the Brocade 5400 vRouter (Vyatta) are all configured by SoftLayer as trunks. Our associations simply update the port configuration to permit the additional virtual LANs (VLANs) selected.<sup>1</sup>

The **Associate a VLAN** drop-down menu allows you to select a specific VLAN to be known, or "associated", to your Brocade 5400 vRouter device from within your datacenter. You **cannot** associate a VLAN that is protected by a hardware firewall or FortiGate appliance as this would break the VLAN's control. If no additional VLANs are listed in the drop-down menu, then you can order more by submitting a ticket to Softlayer.

You can see the VLANs associated with the Brocade 5400 vRouter appliance under Associated VLANs in Figure 4. The default Status is Bypassed (SoftLayer has behind the scenes identified the trunk switch ports to include 1894, 2254 from private, and 2007 and 1280 from the public).<sup>2</sup>

On the **Details** screen (**Network, Gateway Appliances**) under **Associated VLAN, Actions** drop-down menu, there is an option to **Route VLAN**. If you selection this option, you have effectively asked the SoftLayer FCR and BCR to remove the default gateway and to instead add a route for the subnets using the transit VLANs to the Brocade 5400 vRouter device.

The Brocade vRouter appliance is now the only valid route in and out of the VLAN. The key to remember is that this is not only for your application traffic, but also any additional SoftLayer services that now need to pass through your device.<sup>4</sup>

Configuration has yet to be done on the Brocade 5400 vRouter side, yet anything on the VLAN with the subnets is no longer reachable. This also means new servers provisioned as the Web Portal cannot reach the VLAN either.

**Notes:**

<sup>1</sup>Vlan association is the process of linking an eligible VLAN to a network gateway so that it may be routed to the gateway in the future. The process of association does not automatically route a VLAN to a gateway; the VLAN continues to use front-end and back-end customer routers until it is manually routed to the gateway. VLANs may be associated to one gateway at a time and must not have a firewallin order to be considered eligible for association.

<sup>2</sup>The gateway may be bypassed at any time so traffic will return to the front-end and back-end customer routers (FCR and BCR). Bypassing a VLAN allows the VLAN to remain associated to the network gateway.

<sup>3</sup>Portable subnets can be purchased and added to primary gateways.

<sup>4</sup>Brocade 5400 vRouter devices are normally provisioned with the SoftLayer Service IP address predefined.