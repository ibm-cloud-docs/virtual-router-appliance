---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vra, virtual router, order

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Getting Started with IBM Virtual Router Appliance
{: #getting-started-with-ibm-virtual-router-appliance}

To get started with the IBM© Virtual Router Appliance (VRA), navigate to the order page in the Customer Portal:

1. From your browser, open the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} and log into your account.
2. In the Customer Portal navigation, select **Network > Gateway Appliances**.
3. From the **Gateway Appliances** list page, click **Order Gateway**.
4. From the **Order** page, select your desired data center from dropdown menu, then choose the desired type of server hardware.

    **NOTE:** VRA minimum server requirements call for 8 GB of RAM and one CPU core for every 10 Gbps of network capacity. For example, a system with dual 10 Gbps public and private uplinks requires at least four cores. Reserve more than the default disk setting if you plan to run network diagnostics that generate detailed logs. Finally, if your intent is to setup VPN services with encryption, you may want to add additional cores. Adding additional cores for VPN Services will ensure the VRA will not get bogged down by heavy load when routing and simultaneously encrypting/decrypting data.

5. On the Order page select **High Availability Pair** option if desired, select Memory size, select appropriate version of VRA Operating System, and then select network uplink speed.

6. Review your selections then click **Add to Order**, and the order will be verified automatically.
7. On the **CHECKOUT** page, if you already own VLANs in the selected data center, select the backend VLANs which need to be protected. Give a hostname and domain name for your VRA. Check all boxes for the IBM Cloud service terms and Third Party Service Agreement. Then click **Submit Order**.

After your order is approved, the provisioning of your VRA starts automatically. When the provisioning process is complete, the new VRA will appear in the **Gateway Appliances** list page. Click the gateway name to open the **Gateway Details** page, then click on each gateway member to open the **Device Details** page. You will find the IP addresses, login username, and password for the device.  

Remember that once you order and configure your VRA from the IBM Cloud Customer Portal, you must also configure the device itself with the same settings.
{: tip}

## VLANs and the Gateway Appliance's role
A VLAN (virtual LAN) is a mechanism that segregates a physical network into many virtual segments. For convenience, traffic from multiple selected VLANs can be delivered through a single network cable, a process commonly called "trunking."

VRA is delivered in two parts: The VRA server(s) and the Gateway Appliance fixture. The Gateway Appliance provides you with an interface (GUI and API) for selecting the VLANs you want to associate with your VRA. Associating a VLAN with a Gateway Appliance reroutes (or "trunks") that VLAN and all of its subnets to your VRA, giving you control over filtering, forwarding, and protection. For every VLAN that is associated to the Gateway Appliance, that VLAN is allowed on the switch ports that the VRA is connected to, and any subnet on that VLAN is statically routed to your VRA's public IP if the subnet is a public subnet or statically routed to your VRA's private IP if the subnet is a private subnet. That routing is done at the router that the VRA is behind, which will be the Frontend Customer Router (FCR) or the Backend Customer Router (BCR) for public and private traffic respectively. Servers in an associated VLAN can only be reached from other VLANs by going through your VRA; it is not possible to circumvent the VRA unless you bypass or disassociate the VLAN.

By default, a new Gateway Appliance is associated with two non-removable "transit" VLANs, one each for public and private. These are typically used for administration and can be separately secured by VRA commands.

Transit VLANs are for network devices like firewalls or load balancers so that they can route traffic while keeping other devices, such as servers or containers, isolated from the internet.

In comparison, "gateway" VLANs are where devices, such as servers and containers, are hosted.

The VRA can only manage VLANs that are associated with it through the Gateway Appliance.

For information on how to manage VLANs from the Gateway Appliances Details screen, refer to [Managing VLANs](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
