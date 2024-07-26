---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for {{site.data.keyword.cloud_notm}} Virtual Router Appliance
{: #faqs-for-ibm-virtual-router-appliance}

These frequently asked questions can help you when working with the {{site.data.keyword.vra_full}} (VRA).
{: shortdesc}

## What is a VRA?
{: #what}
{: faq}
{: support}

A {{site.data.keyword.vra_full}} (VRA) allows an {{site.data.keyword.cloud}} customer to selectively route private and public network traffic through a full-featured enterprise router with firewall, traffic shaping, policy-based routing, VPN, and a host of other features. All VRA features are customer-managed. VRA gives an {{site.data.keyword.cloud_notm}} customer a degree of control that is normally reserved for on-premises networks.

## What is a gateway appliance?
{: #gateway}
{: faq}
{: support}

With a gateway appliance fixture, you can use the web portal or API to choose network segments (VLANs) to route through a VRA. You can change VLAN selections at any time. The gateway appliance also handles VRA High Availability (HA), configuring a second VRA to take over if the first one fails.

## I sometimes see references to terms like "Vyatta" and "vRouter." How do they relate to VRA?
{: #vyatta-vrouter}
{: faq}

Vyatta was an open source, PC-based router software that became closed source. Today, "Vyatta" and "Vyatta OS" describe commercial software adaptations that are derived from that closed source project. IBM VRA incorporates elements of Vyatta OS, along with substantial feature and service enhancements available exclusively through {{site.data.keyword.cloud_notm}}.

"vRouter" was a short-lived rebranding of Vyatta by its then-owner. When seen in documentation, it can be considered synonymous with Vyatta.

## Is Vyatta 5400 still supported?
{: #5400}
{: faq}
{: support}

IBM no longer supports the Vyatta 5400 as of 31 March 2019.

## What improvements does the {{site.data.keyword.vra_full}} (Vyatta 5600) have over the Vyatta 5400?
{: #what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-}

Vyatta 5600 offers the following enhancements over the Vyatta 5400:

- Faster throughput, at up to 10 Gbps per CPU core
- Over 3X increased throughput per IPsec VPN session (Advanced Encryption Standards)
- Increased capacity for routers, flows, and concurrent connections
- Updated standards support, including Layer 2 Tunneling Protocol, Version 3 (L2TPv3), Internet Key Exchange, Version 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2), and 802.1Q tunneling (Q-in-Q) encapsulation

## What about the AT&T vRouter 5600 offering?
{: #vrouter-5600}
{: faq}

AT&T (formerly Brocade) announced the End-of-Life and End-of-Support of their Brocade vRouter 5600 offering. While the Brocade vRouter 5600 provides the underlying technology capability for the {{site.data.keyword.vra_full}}, this announcement does not apply to IBM customers. IBM customers continue to have support by using this new offering.

## How is VRA delivered?
{: #vra-delivered}
{: faq}

You can obtain a VRA by ordering a network gateway. You can choose a data center and a suitable VRA server, as well to specify whether you want to deploy an HA pair of VRAs. Servers, operating systems, and the gateway appliance fixture are all provisioned automatically. When the provisioning is complete, you can use the gateway appliance interface to route VLANs through the VRA. You can configure your VRA server directly by using SSH (Secure Shell) with the passwords that are provided in the Hardware Details section of the {{site.data.keyword.cloud_notm}} console.

## Is my password safe?
{: #password-safe}
{: faq}

Yes. All VRAs are assigned random passwords visible only to the account holder. Passwords are easily changed, as are SSH public keys and admin IP access restrictions.

## Can I get VRA without a gateway appliance?
{: #vra-without-ga}
{: faq}

Yes, but it can only manage traffic between the VRA's public and private interfaces. VLANs and HA require the gateway appliance fixture.

## Is all network traffic sent through the VRA?
{: #all-traffic-vra}
{: faq}

No. The gateway appliance allows you to select the private and public network segments (VLANs) that you want to route through the VRA. You can change and bypass VLAN selections at any time. VRA also allows you to define IP-based rules that apply to subnets or IP ranges. Such rules function only if the VLANs containing those subnets are routed through the VRA.

## Can a VRA or dedicated firewall prevent new server provisions?
{: #new-server}
{: faq}
{: support}

Yes. Whenever possible, you shouldn't lock down your network until you've populated it with the servers you plan to use.

IBM support is forbidden by policy to examine or alter VRA or dedicated firewall configuration without a customer's explicit involvement, so support cannot know that a VRA is responsible for stalled or failed server provisions.

It is the customer's responsibility to ensure that the VRA or firewall is configured to permit automated server provisions before the server order is placed. Provisions that are blocked by a customer-managed VRA or firewall are the customer's responsibility to resolve. Such provisioning delays are not subject to SLAs or credits. Ordered systems can be returned to inventory (after customer data is expunged) if the customer does not respond quickly.

Likewise, if a VRA or firewall is bypassed after an order is placed, it's still likely that the order will fail. There might be a narrow window during which automation retries are attempted. It is best that the entire provision process proceed without network interference.

## What firewall products does IBM offer?
{: #firewall-products}
{: faq}

To find a detailed comparison of all firewall products that are offered in {{site.data.keyword.cloud_notm}}, see [Exploring firewalls](/docs/fortigate-10g?topic=fortigate-10g-exploring-firewalls).

## Can a VRA confound customer support efforts?
{: #vra-support-efforts}
{: faq}

Yes, for the preceding reasons. VRA is a black box: VLANs go in, VLANs come out, and IBM Support doesn't know what customers are doing with packets in-between.

IBM Support always does its best, but with VRA and dedicated firewall: 

1. Customer privacy trumps connectivity.
2. Our standard support staff is not equipped to analyze poorly formed or highly complex VRA or firewall configurations.

As a first diagnostic step, IBM Support might require you to put your VRA or firewall VLANs in bypass mode. If, in this state, provisions that failed start going through, the issue is likely with your VRA or firewall configuration.

## What effect does VRA have on my network performance?
{: #effect-network-performance}
{: faq}

Keep in mind that even though they can't see you, a public cloud shares networks with other customers. True, best-case VRA throughput is determined by available network capacity at a point in time, plus the distance the data must travel.

These variables aside, VRA can forward 80 Gbps of unmodified traffic across multiple interfaces by using the rough formula that every 10 Gbps of throughput requires one full processor core (not including hyperthread). Current servers max out at 40 Gbps (2 x 10 Gbps public + 2 x 10 Gb private). As a result, a server with 8 or more cores has sufficient compute headroom to handle multiple common VRA features at near best-case network performance

## What do I do if I lose my VRA password?
{: #password}
{: faq}
{: support}

If you can access the system, set a new password by running the following command:

```sh
set system login user [account] authentication plaintext-password [password]
```

If you cannot access the system, you can restart the device and use the password recovery option on the GRUB menu to reset the root user password.

## What do I do if I am locked out of the firewall?
{: #locked-out}
{: faq}
{: support}

The `reboot at [time]` construct can be useful when testing potentially dangerous firewall rules.

If the rule works, then use the command `reboot cancel` to cancel the restart. If the rule locks out your access, simply wait for the scheduled restart to occur.

If you cannot access the system, then you might restart to recover access. Upon rebooting, the system reads the configuration file, which is unchanged by previous entries that were discarded.

If there is access by using IPMI, follow these steps to recover access:

1. Disable the offending rule by running:

	```sh
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Unhook the entire named rule set from the necessary interface by running:

	```sh
	delete interfaces dataplane [interface] firewall [type] [firewall name]
	commit
	```

Incorrect use of these commands can wipe out your interface configuration.
{: important}

## Why would I want to run two Vyatta devices in a High Availability (HA) pair?
{: #HA}
{: faq}
{: support}

Most cloud customers want HA services. This is so that your workload is hosted on at least two separate (hardware) machines, or even better, in two separate availability zones (think data centers), so that if one fails, the other is able to continue the service. If one machine fails, there is a failover to the other machine, which means that the service can keep running. This is what is referred to as an HA service — it’s almost always available.

## How can I enable root logins to the VRA?
{: #root-login}
{: faq}
{: support}

To enable root access through SSH, run the following command:

`set service ssh allow-root`

Allowing root access that uses SSH is considered unsafe. 
{: note}

An alternative to accessing a root shell is to either log in as another user and elevate to root locally with `su -`, or allow sudo commands to superusers. For example, to configure the Vyatta as a superuser:

`set system login vyatta level superuser`
