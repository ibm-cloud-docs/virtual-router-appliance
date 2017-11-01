---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Getting Started
To get started with the IBM Virtual Router Appliance (VRA), navigate to the order page in the Customer Portal:

1. From your browser, open  [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} and log into your account.
2. In the Customer Portal navigation, select **Network > Gateway Appliances**.
3. From the **Gateway Appliances** list page, click **Order Gateway**.
4. From the **Order** page, select your desired data center from dropdown menu, then choose the desired type of server hardware.

    **NOTE:** VRA minimum server requirements call for 8 GB of RAM and one CPU core for every 10 Gbps of network capacity. For example, a system with dual 10 Gbps public and private uplinks requires at least four cores. Reserve more than the default setting disk if you plan to run network diagnostics that generate detailed logs. Finally, additional cores beyond the minimum can increase VPN performance

5. In the **Configure your Network Gateway Appliance** page, select **High Availability Pair** option if desired, the VRA version, and the network uplink speeds.
6. Review your selections then click **Add to Order**, and the order will be verified automatically.
7. On the **CHECKOUT** page, if you already own VLANs in the selected data center, select the backend VLANs which need to be protected. Give a hostname and domain name for your VRA. Check all boxes for the IBM Cloud service terms and Third Party Service Agreement. Then click **Submit Order**.

After your order is approved, the provisioning of your VRA starts automatically. When the provisioning process is complete, the new VRA displays in the **Gateway Appliances** list page in the Customer Portal. Click the gateway name to open the **Gateway Details** page, then click on each gateway member to open the **Device Details** page. You will find the IP addresses, login username, and password for the device.  
 
## VLANs and the Gateway Appliance's role
A VLAN (virtual LAN) is a mechanism that segregates a physical network into many virtual segments. For convenience, traffic from multiple selected VLANs can be delivered through a single network cable, a process commonly called "trunking."

VRA is delivered in two parts: The VRA server(s) and the Gateway Appliance fixture. Gateway Appliance provides you with an interface (GUI and API) for selecting the VLANs you want to associate with your VRA. Associating a VLAN with a Gateway Appliance reroutes (or "trunks") that VLAN and all of its subnets to your VRA, giving you control over filtering, forwarding, and protection. Servers in an associated VLAN can only be reached from other VLANs by going through your VRA; it's not possible to circumvent the VRA unless you bypass or disassociate the VLAN.

By default, a new Gateway Appliance is associated with two non-removable "transit" VLANs, one each for public and private. These are typically used for administration and can be separately secured by VRA commands.

The VRA can only manage VLANs that are associated with it through the Gateway Appliance.

For information on how to manage VLANs from the Gateway Appliances Details screen, refer to [Managing VLANs](manage-vlans.html).
