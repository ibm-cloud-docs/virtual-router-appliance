---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"} 
{:codeblock: .codeblock}
{:pre: .pre} 
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external} 
{:generic: data-hd-programlang="generic”}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"} 

# About the VRA
{: #about-the-vra}

The {{site.data.keyword.vra_full}} (VRA) provides the latest Vyatta 5600 operating system for x86 bare metal servers. It is offered as a High Availability (HA) or stand-alone configuration.
{: shortdesc}

The {{site.data.keyword.vra_full}} allows you to route private and public network traffic selectively, through a full-featured enterprise router that has firewall, traffic shaping, policy-based routing, VPN, and other features. The VRA offers performance with ease of configuration. It has the maintenance advantages of running on a normal hardware server. The VRA hardware appliance is sized to handle the routing load for multiple VLANs, and it can be ordered with redundant network links and redundant RAID arrays. All VRA features are managed by you, the customer.

**Alternatives:** The FortiGate Security Appliance (FSA) 10 Gbps is a single-tenant (dedicated), high-throughput (10 Gbps) hardware firewall with next-generation features, such as AntiVirus (AV), Intrusion Prevention (IPS), and web filtering. It might be an alternative to VRA for achieving similar goals. For more information, see the [FSA documentation](/docs/fortigate-10g?topic=fortigate-10g-getting-started).

## Firewall
{: #firewall}

To protect your environment from external threats, the {{site.data.keyword.vra_full}} can be leveraged as a firewall. You can add firewall rules to allow or deny inbound or outbound network traffic to the ports on which your application runs, and you can filter the traffic within your own networks. The {{site.data.keyword.vra_full}} also can be configured to perform stateful IPv4 and IPv6 filtering to protect your critical data.

## Virtual Private Network (VPN) gateway
{: #virtual-private-network-vpn-gateway}

Connect your on-site data center or office to the {{site.data.keyword.cloud_notm}} using VPN tunneling by provisioning your {{site.data.keyword.vra_full}} as a network gateway device. You can use an IPsec site-to-site VPN tunnel for secure communication from your enterprise data center or office to your {{site.data.keyword.cloud_notm}} network. Other VPN options are; Remote access IPsec VPN (client-to-site), OpenVPN, GRE, L2TP and DMVPN.

Have a look at the Brocade VPN configurations guides in our [Supplemental VRA documentation section](/docs/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).

## Network Address Translation (NAT)
{: #network-address-translation-nat-}

With the {{site.data.keyword.vra_full}}, you can provision application and database servers without public network interfaces while still allowing your servers to access the internet using Source NAT. You can also hide your servers behind the gateway device with Destination NAT for enhanced security.

## Enterprise-grade routing
{: #enterprise-grade-routing}

For multi-tiered applications on different isolated networks, the {{site.data.keyword.vra_full}} gives you flexible ways to build connectivity between these networks. You can set up dynamic routing using BGP, which allows you to announce your own public IP space on the {{site.data.keyword.cloud_notm}} routers. BGP also offers more flexibility for custom private network configurations when using various tunnels and direct link solutions.

Have a look at the Brocade BGP configurations guides in our [Supplemental VRA documentation section](/docs/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).
