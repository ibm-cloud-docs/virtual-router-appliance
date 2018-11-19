---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# FAQs
The following are frequently asked questions when working with the IBM Virtual Router Appliance (VRA).

## What is a VRA? 
{:faq}

A Virtual Router Appliance (VRA) allows an IBM Cloud customer to selectively route private and public network traffic through a full-featured enterprise router with firewall, traffic shaping, policy-based routing, VPN, and a host of other features. All VRA features are customer-managed. VRA gives an IBM Cloud customer a degree of control normally reserved for on-premise networks.

## What is a Gateway Appliance? 
{:faq}

A Gateway Appliance fixture lets you use the Web portal or API to choose network segments (VLANs) to route through a VRA. You can change VLAN selections at any time. The Gateway Appliance also handles VRA high-availability (HA), configuring a second VRA to take over if the first one fails.

## I sometimes see references to terms like "Vyatta" and "vRouter." How do they relate to VRA?
{:faq}

Vyatta was open source, PC-based router software that was acquired in full and transitioned to closed source. Today, "Vyatta" and "Vyatta OS" describe commercial software adaptations derived from that closed source project. IBM VRA incorporates elements of Vyatta OS, along with substantial feature and service enhancements available exclusively through IBM Cloud.

"vRouter" was a short-lived rebranding of Vyatta by its then-owner. When seen in documentation, it can be considered synonymous with Vyatta.

## Is Vyatta 5400 still supported?
{:faq}

IBM will no longer support Vyatta 5400 as of March 31, 2019.

## What improvements does the Virtual Router Appliance (Vyatta 5600) have over the Vyatta 5400?
{:faq}

Vyatta 5600 offers the following enhancements over the Vyatta 5400:

- Faster throughput, at up to 10Gbps per CPU core
- Over 3X increased throughput per IPsec VPN session (Advanced Encryption Standards)
- Increased capacity for routers, flows, and concurrent connections
- Updated standards support, including Layer 2 Tunneling Protocol, Version 3 (L2TPv3), Internet Key Exchange, Version 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2), and 802.1Q tunneling (Q-in-Q) encapsulation

## What about the AT&T vRouter 5600 offering?
{:faq}

AT&T (formerly Brocade) has announced the End-of-Life and End-of-Support of their Brocade vRouter 5600 offering. While the Brocade vRouter 5600 provides the underlying technology capability for the IBM Virtual Router Appliance, this announcement does not apply to IBM customers. IBM customers will continue to have support using this new offering.

## How is VRA delivered? 
{:faq}

You obtain a VRA by ordering a Network Gateway. This streamlined process lets you choose a data center and a suitable VRA server, as well as whether you want to deploy an HA pair of VRAs. Servers, operating systems, and the Gateway Appliance fixture are all provisioned automatically. When the provisioning is complete, you can use the Gateway Appliance interface to route VLANs through the VRA. You can configure your VRA server directly by using SSH (secure shell) with the passwords provided in the Hardware Details section of the Customer Portal.

## Is my password safe? 
{:faq}

Yes. All VRAs are assigned random passwords visible only to the account holder. Passwords are easily changed, as are SSH public keys and admin IP access restrictions.

## Can I get VRA without a Gateway Appliance? 
{:faq}

Yes, but it can only manage traffic between the VRA's public and private interfaces. VLANs and HA require the Gateway Appliance fixture.

## Is all network traffic sent through the VRA? 
{:faq}

No. The Gateway Appliance lets you select the private and public network segments (VLANs) you want to route through the VRA. You may change and bypass VLAN selections at any time. VRA also lets you define IP-based rules that apply to subnets or IP ranges. Such rules function only if the VLANs containing those subnets are routed through the VRA.

## Can a VRA or dedicated firewall prevent new server provisions? 
{:faq}

Yes. Whenever possible, you shouldn't lock down your network until you've populated it with the servers you plan to use.

IBM support is forbidden by policy to examine or alter VRA or dedicated firewall configuration without a customer's explicit involvement, so in most cases support cannot know that a VRA is responsible for stalled or failed server provisions.

It is the customer's responsibility to ensure that the VRA or firewall is configured to permit automated server provisions before the server order is placed. Provisions that are blocked by a customer-managed VRA or firewall are the customer's responsibility to resolve. Such provisioning delays are not subject to SLAs or credits. Ordered systems may be returned to inventory (after customer data is expunged) if the customer does not respond quickly.

Likewise, if a VRA/firewall is bypassed after an order is placed, it's still likely that the order will fail. There may be a narrow window during which automation retries will be attempted. It is best that the entire provision process proceed without network interference.

## What firewall products does IBM offer?
{:faq}

You can find a detailed comparison of all firewall products offered in the IBM Cloud by reviewing this [topic ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/infrastructure/fortigate-10g/explore-firewalls.html#explore-firewalls){: new_window}. 

## Can a VRA confound customer support efforts? 
{:faq}

Yes, for the reasons described above. VRA is a "black box:" VLANs go in, VLANs come out and IBM has no idea what customers are doing with packets in-between.

Support always does its best, but with VRA and dedicated firewall: a) customer privacy trumps connectivity, and b) our standard support staff is not equipped to analyze poorly-formed or highly complex VRA/firewall configurations.

As a first diagnostic step, we may require you to put your VRA or Firewall VLANs in bypass. If, in this state, provisions that had failed start going through, we must assume the issue lies with your VRA/firewall configuration.

## What effect will VRA have on my network performance? 
{:faq}

Keep in mind that even though they can't see you, a public cloud shares networks with other customers. True best-case VRA throughput is determined by available network capacity at a point in time, plus the distance the data must travel.

These variables aside, VRA is capable of forwarding 80 Gbps of unmodified traffic across multiple interfaces, using the rough formula that every 10 Gbps of throughput requires one full processor core (not including hyperthread). Given that current servers max out at 40 Gbps (2 x 10 Gbps public + 2 x 10 Gb private), a server with 8 or more cores should have sufficient compute headroom to handle multiple common VRA features at near best-case network performance

## What do I do if I lose my VRA password?
{:faq}

If there is access to the system, set a new password by running the following command:

```
set system login user [account] authentication plaintext-password [password]  
```

If there is no access to the system, you can reboot the device and use the password recovery option on the GRUB menu for resetting the root user password.

## What do I do if I have been locked out of the firewall?
{:faq}

The `reboot at [time]` construct can be useful when testing potentially dangerous firewall rules.

If the rule works then use the command `reboot cancel` to cancel the reboot. If the rule locks out your access, simply wait for the scheduled reboot to occur.

If there is no access to the system, then a reboot may be used to recover access. Upon rebooting, the system will read the configuration file which would be unchanged by previous entries that were discarded.

If there is access using IPMI, you can perform the following actions to recover access:

1. Disable the offending rule by running:

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Unhook the entire named rule set from the necessary interface by running:

	```
	delete interfaces dataplane [interface] firewall [type] [firewall name]
	commit
	```

**NOTE:** Incorrect use of these commands can wipe out your interface configuration.

## How can I enable root logins to the VRA?
{:faq}

To enable root access through SSH, run the following command:

`set service ssh allow-root`

Note that allowing root access using SSH is considered unsafe. An alternative to access a root shell is to either login as another user and elevate to root locally with `su -`, or by allowing sudo commands to 'superusers'. 

For example, to configure the vyatta as a superuser:

`set system login vyatta level superuser`