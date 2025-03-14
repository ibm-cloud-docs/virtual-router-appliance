

{{site.data.keyword.attribute-definition-list}}

# Getting started with IBM Cloud Virtual Router Appliance (VRA)
{: #getting-started-vra}

On 30 November 2025, all 2204 versions of IBM Cloud Virtual Router Appliance will be considered End of Support by Ciena. To maintain official support, be sure to update to version 2308 by opening a [support case](/docs/gateway-appliance?topic=gateway-appliance-getting-help) and requesting the latest subrelease ISO. Once you receive your ISO, you can then follow the instructions for [Upgrading the OS](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) to finish updating your version. Please ask the IBM Cloud Support Security team for available information on latest known issues before updating to the latest 2308 when you request the latest version.
{: deprecated}

The {{site.data.keyword.vra_full}} (VRA) provides the Ciena Vyatta NOS, a debian-based network OS, installed directly onto specific {{site.data.keyword.cloud}} Classic Bare Metal Servers along with expanded abilities for managing Classic Infrastructure VLANs and subnets. 
{: shortdesc}

This offering acts as a user-managed router and firewall at the Classic pod level demarcation and enables you to route, filter, NAT and otherwise control private (rfc1918) and public network traffic selectively. This offering is commonly used to deploy VPNs, including IKE-based RA VPN and IPSec VPN tunnels, to help customize and secure access into the IBM Cloud environment. Because this is a router and firewall, the customizability and use cases are very broad and can include the protection of a single server but also include acting as a router that facilitates connections between on-prem, Classic, VPC, Power and other environments.

The {{site.data.keyword.vra_full}} (VRA) is limited to specific Bare Metal server hardware profiles, However, the Bare Metal server can still be customized with varying amounts of RAM and local hard disk or solid state disk configurations. As with other Bare Metal servers, the VRA comes built with a 4 port Intel X710-based NIC that can be ordered in different combinations of 1 gigabit and 10 gigabit connections. It is offered as a High Availability (HA) deployment, which is a set of 2 separate appliances clustered with VRRP in an active-passive setup. It is also offered as a stand-alone deployment. When planning your VRA build, consider your overall environment in regards to standard traffic patterns, network capacity requirements, and encryption overhead for all of the services, servers, and connections that will traverse the VRA.

## Ordering an IBM Cloud Virtual Router Appliance
{: #order-vra}

To order a VRA, follow these steps:

1. From your browser, open the Gateway Appliances page in the [IBM Cloud UI console](https://{DomainName}/gen1/infrastructure/provision/gateway){: external} and log in to your account.

   You can also access the page by selecting the navigation menu in the upper left of the [IBM Cloud catalog](https://{DomainName}/){: external} and selecting **Infrastructure >  Classic Infrastructure**. Then choose **Network > Gateway appliance**.
   {: tip}

2. From the **Gateway Vendor** section, select the **AT&T** option (a blue checkmark appears on the button). From the list menu on the same button choose your bandwidth (either 20 Gbps or 2 Gbps).

3. From the **Gateway appliance** section, enter your **hostname** and **Domain** name information. These fields are already populated with default information, so ensure that the values are correct. Check the **High Availability** option if wanted, then select your data center **Location**, and the specific **Pod** you want from the list menu.

   Only pods that already have an associated VLAN appear here. If you want to provision your gateway appliance in a pod that you don't see listed, first create a VLAN there.
   {: note}

4. From the **Configuration** section, choose your processor's RAM. You can also define an SSH key if you want to use it to authenticate access to your new gateway.

   The appropriate processor is chosen for you based on the license version you selected in step 2. However, you can choose different RAM configurations.
   {: note}

5. From the **Storage disks** section, choose the options that meet your storage requirements.

     RAID configurations are available for customizing storage performance, size and data protection. RAID1 or RAID10 are recommended for a combination of data protection and performance. RAID0 provides the highest read/write performance with the least data protection. If you choose RAID0, ensure that you have a data backup plan, as one disk failure needs the complete rebuild of the RAID with new disks and the rebuilding and reloading of the appliance.

   You can have up to four disks per VRA. "Disk size" with a RAID configuration is the usable disk size, as RAID configurations for every type of RAID (other than RAID0) change the amount of storage available.
   {: note}

7. From the **Network interface** section, select your **Uplink port speeds**. The default selection is a single interface, but there are redundant and private only options as well. Choose the one that best fits your needs.

   With the Network Interface **Add-Ons** section you can select an IPv6 address, which shows you any additional included default options.

8. Review your selections, read the Third-Party Service Agreements, then click **Create**. The order is verified automatically.

After your order is approved, the provisioning of your {{site.data.keyword.vra_full}} starts automatically. When the provisioning process is complete, the new VRA appears in the Gateway Appliances list page. Click the gateway name to open the Gateway Details page, where you can find the IP addresses, login username, and password for the device.  

Upon the provisioning and delivery of the VRA, you should consider the configuration similar to factory settings and not representativ of the latest firmware. As a result, open a support ticket to request the latest recommended version (which comes as an ISO) along with a list of any known issues. Next, SSH to the VRA, update the VRA version, harden the OS/configuration to prevent compromise over public ports. Then you can configure the VRA for your intended workloads. For standardization, ease of management, security and scaling, you should automate this process.
{: tip}

## Network access considerations
{: #network-access-considerations}

The VRA can be deployed with either public and private network access or private network access only. By default, SSH access to the public IP is disabled on provisions of version 2012g and later. Access to the host can be achieved through the private IP address. Additionally, HTTPS access is disabled to both the public and private IPs.

## VLANs and the gateway appliance's role
{: #vlans-and-the-gateway-appliance-s-role}

A VLAN (virtual LAN) is a mechanism that segregates a physical network into many virtual segments. For convenience, traffic from multiple selected VLANs can be delivered through a single network cable, a process commonly called "trunking."

{{site.data.keyword.vra_full}} is delivered in two parts: The VRA servers and the gateway appliance fixture. The gateway appliance provides you with an interface (GUI and API) for selecting the VLANs you want to associate with your VRA. Associating a VLAN with a gateway appliance reroutes (or "trunks") that VLAN and all of its subnets to your VRA, giving you control over filtering, forwarding, and protection. For every VLAN that is associated to the gateway appliance, that VLAN is allowed on the switch ports that the VRA is connected to, Any subnet on that VLAN is statically routed to your VRA's public VRRP IP (if the subnet is a public subnet) or statically routed to your VRA's private VRRP IP (if the subnet is a private subnet). This routing is done at the router that the VRA is behind, which is the Frontend Customer Router (FCR) or the Backend Customer Router (BCR) for public and private traffic respectively.
