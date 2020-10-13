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
{:download: .download}
{:faq: data-hd-content-type='faq'} 
{:support: data-reuse='support'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"} 
{:DomainName: data-hd-keyref="DomainName"}


# Technical FAQs for {{site.data.keyword.cloud_notm}} Virtual Router Appliance
{: #technical-faqs-for-ibm-virtual-router-appliance}

These frequently asked questions address the configuration of the {{site.data.keyword.vra_full}} (VRA), and issues that involve migrating to the VRA from Vyatta 5400.
{: shortdesc}

## How do I allow internet-bound traffic from hosts that are on a private VLAN?
{: #VLAN}
{: faq}
{: support}

This traffic must obtain a public source IP; thus, a Source NAT must masquerade the private IP with the public one of the VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

This configuration performs only SNAT from traffic originating from servers in the private `10.0.0.0/8` network.

This ensures it does not interfere with packets that already have an internet-routeable source address.

## How can I filter internet-bound traffic and only allow specific protocols and destinations?
{: #specific-protocol}
{: faq}
{: support}

This is a common question when Source NAT and a firewall must be combined.

Keep in mind the order of operations in the VRA you design your rulesets.

In short, firewall rules are applied *after* SNAT.

To block all outgoing traffic in a firewall, but allow specific SNAT flows, you must move the filtering logic onto your SNAT. For example, to only allow HTTPS internet-bound traffic for a host, the SNAT rule would be:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` would be a public address for the VRA.

It is recommended to use the VRRP public address of the VRA so that you can differentiate between host and VRA public traffic.

Assume that `150.1.2.3` is the VRRP VRA address, and `150.1.2.5` is the real dp0bond1 address. The stateful firewall applied on `dp0bond1 out` would be:

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

The combination of Source NAT and firewall achieves the required design goal.
{: note}

Ensure that the rules are appropriate for your design, and that no other rules allow traffic that should be blocked.

## How do I protect the VRA itself with a zone-based firewall?
{: #zone-based}
{: faq}
{: support}

The VRA does not have a `local zone`. You can use the Control Plane Policing (CPP) functionality instead as it is applied as a `local` firewall on loopback.

This is a stateless firewall and you must explicitly allow the returning traffic of outbound sessions that originate on the VRA itself.
{: note}

## How do I restrict SSH and block connections that come from the internet?
{: #SSH}
{: faq}
{: support}

It is considered a best practice to not allow SSH connections from the internet, and to use another means of accessing the private address, such as SSL VPN.

By default, the VRA accepts SSH on all interfaces. To listen only for SSH connections on the private interface, you must set the following configuration:

```
set service ssh listen-address '10.1.2.3'
```

Keep in mind that you must replace the IP address with the address that belongs to the VRA.
