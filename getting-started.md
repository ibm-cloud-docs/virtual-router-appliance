---

copyright:
  years: 2017
lastupdated: "2019-05-03"

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


# Getting Started with IBM Cloud Virtual Router Appliance
{: #getting-started}

The {{site.data.keyword.vra_full}} (VRA) provides the latest Vyatta 5600 operating system for x86 bare metal servers. It is offered as a High Availability (HA) or standalone configuration, and lets you route private and public network traffic selectively, through a full-featured enterprise router that has firewall, traffic shaping, policy-based routing, VPN, and other features.

VRA minimum server requirements call for 8 GB of RAM and one CPU core for every 10 Gbps of network capacity. For example, a system with dual 10 Gbps public and private uplinks requires at least four cores. Also, if your intent is to setup VPN services with encryption, you may want to add additional cores. Adding additional cores for VPN Services will ensure the VRA will not get bogged down by heavy load(s) when routing and simultaneously encrypting/decrypting data.

## Ordering a {{site.data.keyword.vra_full}}
{: #order-vra}

To order a VRA, perform the following procedure:

1. From your browser, open the Gateway Appliances page in the [IBM Cloud UI Console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} and log into your account.

  You can also get to this page by selecting the navigation menu in the top left of the [IBM Cloud Catalog ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com) and selecting **Classic Infrastructure > Network > Gateway appliance**.
  {: tip}

2. From the **Gateway Vendor** section, select the **AT&T** option (when it is selected, a blue check mark appears on the button). From the drop down on that same button, choose your bandwidth (either 20Gbps or 2Gbps).

  	<img src="images/ordering_vra.png" alt="drawing" style="width: 500px;"/>

3. From the **Gateway appliance** section, enter your **Hostname** and **Domain** name information. These fields will already be populated with default information, so ensure the values are correct. Check the **High Availability** option if desired, then select your desired data center **Location**, and the specific **Pod** you want from the dropdown menu.

  Only pods that already have an associated VLAN will display here. If you wish to provision your Gateway Appliance in a pod you don't see listed, first create a VLAN there.
  {: note}

4. From the **Configuration** section, choose your processor's RAM. You can also define an SSH key, if you want to use it to authenticate access to your new Gateway.

  <img src="images/ordering_vra_2.png" alt="drawing" style="width: 600px;"/>

  The appropriate processor is chosen for you based on the license version you selected in step two. You can choose different RAM configurations, however.
  {: note}

5. From the **Storage disks** section, choose the options that meet your storage requirements.

  RAID0 and RAID1 options are available for added protection against data loss, as are hot spares (backup components that can be placed into service immediately when a primary component fails).
  {: note}

  You may have up to four disks per VRA. "Disk size" with a RAID configuration is the usable disk size, as RAID configurations are mirrored.
  {: note}

  Reserve more than the default disk setting if you plan to run network diagnostics that generate detailed logs.
  {: tip}

6. From the **Network interface** section, select your **Uplink port speeds**. The default selection is a single interface, but there are redundant and private only options as well. Choose the one that best fits your needs.

  The Network Interface **Add Ons** section allows you to select an IPv6 address if required, and shows you any additional included default options.

8. Review your selections, check that you have read the Third Party Service Agreements, then click **Create**. The order is verified automatically.

After your order is approved, the provisioning of your {{site.data.keyword.vra_full}} starts automatically. When the provisioning process is complete, the new VRA will appear in the Gateway Appliances list page. Click the gateway name to open the Gateway Details page. You will find the IP addresses, login username, and password for the device.  

  <img src="images/gateway_details.png" alt="drawing" style="width: 500px;"/>

Remember that once you order and configure your VRA from the IBM Cloud Catalog, you must also configure the device itself with the same settings.
{: tip}

## VLANs and the Gateway Appliance's role
{: #vlans-and-the-gateway-appliance-s-role}

A VLAN (virtual LAN) is a mechanism that segregates a physical network into many virtual segments. For convenience, traffic from multiple selected VLANs can be delivered through a single network cable, a process commonly called "trunking."

{{site.data.keyword.vra_full}} is delivered in two parts: The VRA server(s) and the Gateway Appliance fixture. The Gateway Appliance provides you with an interface (GUI and API) for selecting the VLANs you want to associate with your VRA. Associating a VLAN with a Gateway Appliance reroutes (or "trunks") that VLAN and all of its subnets to your VRA, giving you control over filtering, forwarding, and protection. For every VLAN that is associated to the Gateway Appliance, that VLAN is allowed on the switch ports that the VRA is connected to, and any subnet on that VLAN is statically routed to your VRA's public VRRP IP (if the subnet is a public subnet) or statically routed to your VRA's private VRRP IP (if the subnet is a private subnet). This routing is done at the router that the VRA is behind, which will be the Frontend Customer Router (FCR) or the Backend Customer Router (BCR) for public and private traffic respectively.

Be aware that VRRP is disabled by default, and it must be enabled in order for VLAN traffic to work, even on stand-alone vyattas. This is a consequence of the subnets on the associated VLAN's being routed to the VRRP IP or virtual-address assigned to the VRA. For more information, refer to [VRRP virtual IP (VIP) addresses](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses).
{: important}

Servers in an associated VLAN can only be reached from other VLANs by going through your {{site.data.keyword.vra_full}}; it is not possible to circumvent the VRA unless you bypass or disassociate the VLAN.

By default, a new Gateway Appliance is associated with two non-removable "transit" VLANs, one each for public and private. These are typically used for administration and can be separately secured by VRA commands.

Transit VLANs are for network devices like firewalls or load balancers so that they can route traffic while keeping other devices, such as servers or containers, isolated from the internet.

In comparison, "gateway" VLANs are where devices, such as servers and containers, are hosted.

The VRA can only manage VLANs that are associated with it through the Gateway Appliance.

For information on how to manage VLANs from the Gateway Appliances Details screen, refer to [Managing VLANs](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
