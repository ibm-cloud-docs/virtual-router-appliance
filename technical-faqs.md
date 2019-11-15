---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: faqs, vlan, traffic, firewall, SSH,

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Technical FAQs for IBM Cloud Virtual Router Appliance
{: #technical-faqs-for-ibm-virtual-router-appliance}

These frequently asked questions address the configuration of the {{site.data.keyword.vra_full}} (VRA), as well as issues surrounding migrating to the VRA from Vyatta 5400.
{: shortdesc}

## How do I allow Internet-bound traffic from hosts that are on a private VLAN?
{: faq}

This traffic needs to obtain a public source IP, thus a Source NAT needs to masquerade the private IP with the public one of the VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

The configuration above only performs SNAT from traffic originating from servers in the private `10.0.0.0/8` network.

This ensures it will not interfere with packets that already have an Internet-routeable source address.

## How can I filter Internet-bound traffic and only allow specific protocols/destinations?
{: faq}

This is a common question when Source NAT and a firewall need to be combined.

Please keep in mind the order of operations in the VRA when designing your rulesets.

In short, firewall rules are applied *after* SNAT.

In order to block all outgoing traffic in a firewall, but allow specific SNAT flows, you would need to move the filtering logic onto your SNAT.

For example, in order to only allow HTTPS Internet-bound traffic for a host, the SNAT rule would be:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` would be a public address for the VRA.

It is highly recommended to use the VRRP public address of the VRA, so you can differentiate between host and VRA public traffic.

Assume that `150.1.2.3` is the VRRP VRA address, and `150.1.2.5` is the real dp0bond1 address.

The stateful firewall applied on `dp0bond1 out` would be:

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

Note that the combination of Source NAT and firewall achieves the required design goal.

Ensure that the rules are appropriate for your design, and that no other rules would allow traffic that should be blocked.

## How do I protect the VRA itself with a zone-based firewall?
{: faq}

The VRA does not have a `local zone`.

You can utilize the Control Plane Policing (CPP) functionality instead as it is applied as a `local` firewall on loopback.

Note that this is a stateless firewall and you will need to explicitly allow the returning traffic of outbound sessions originating on the VRA itself.

## How do I restrict SSH and block connections coming from the internet?
{: faq}

It is considered a best practice to not allow SSH connections from the internet, and to use another means of accessing the private address, such as SSL VPN.

By default, the VRA accepts SSH on all interfaces.
In order to only listen for SSH connections on the private interface, the following configuration needs to be set:

```
set service ssh listen-address '10.1.2.3'
```

Keep in mind that the IP address needs be replaced with the address belonging to the VRA.
