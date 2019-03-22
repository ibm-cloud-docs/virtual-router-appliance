---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: troubleshooting, vfp, problems, nat, dnat, gre

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

# Troubleshooting Your VFP Interface
{: #troubleshooting-your-vfp-interface}

This topic contains troubleshooting information for VFP interfaces.

* A VFP interface is not a "real" interface, such as `dp0bond0` (or even a VIF or TUN). It is a placeholder for the firewall and NAT processes to hang an interface so they can process traffic properly. You can still route traffic over a VFP like a regular interface, but `tshark` and other monitor commands will reveal no traffic.
* With NAT, you must use a more specific subnet range to get traffic routed to the VFP, rather than the kernel route that is created by IPsec. If a static route is not set, then the kernel route will be followed. You can test this using `show ip route x.x.x.x`.
* DNAT should be processed properly coming out of the VFP, but returning traffic still needs a static route set. Look for non-NAT traffic heading out of the IPsec interface, `dp0bond1` or `dp0bond0` (or any interface using IPsec traffic).
* Using routing protocols over a VFP is untested.
* Using a GRE tunnel over a VFP is also untested.
