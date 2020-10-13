---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

keywords: updates, changes, additions

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


# Release notes for IBM Cloud Virtual Router Appliance
{: #release-notes-for-ibm-virtual-router-appliance}

Find out about new and updated features in {{site.data.keyword.vra_full}} (VRA).
{: shortdesc}

## August 2018
{: #august-2018}

### Brocade operating system version 18.x
{: #brocade-operating-system-version-18-x}

Version 18.x of the Brocade OS is now available for the {{site.data.keyword.vra_full}}. Among other new features, this version provides remediation for the Spectre security breach.

New features of the 18.x VRA are discussed in the following topics:

* [Setting up an IPsec tunnel that works with zone firewalls](/docs/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [Configuring a VFP interface with IPsec and zone firewalls](/docs/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [Using NAT with prefix based IPsec](/docs/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [Troubleshooting your VFP Interface](/docs/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

If you are migrating from Vyatta 5400, the best way to upgrade to 18.x is through a full OS reload. For more information, see  [Upgrading the OS](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

Because there is no simple one-to-one mapping of functionality between Vyatta 5400 and the {{site.data.keyword.vra_full}}, creating a baseline configuration for the VRA is helpful. An IBM Partner, WanClouds, can help you with this process, and provide guidance on creating functionality similar to the Vyatta 5400 on your VRA.

For more information about common issues encountered during this upgrade process, see [Vyatta 5400 common migration issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
