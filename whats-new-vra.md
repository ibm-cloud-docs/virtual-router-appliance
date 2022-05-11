---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-05"

keywords: updates, changes, additions

subcollection: virtual-router-appliance

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for IBM Cloud Virtual Router Appliance
{: #virtual-router-appliance-release-notes}

Find out about new and updated features in {{site.data.keyword.vra_full}} (VRA).
{: shortdesc}

## 21 August 2018
{: #virtual-router-appliance-aug2118}
{: release-note}

Brocade operating system version 18.x
:    Version 18.x of the Brocade OS is now available for the {{site.data.keyword.vra_full}}. Among other new features, this version provides remediation for the Spectre security breach.

:    New features of the 18.x VRA are discussed in the following topics:

    * [Setting up an IPsec tunnel that works with zone firewalls](/docs/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
    * [Configuring a VFP interface with IPsec and zone firewalls](/docs/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
    * [Using NAT with prefix based IPsec](/docs/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
    * [Troubleshooting your VFP Interface](/docs/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

:    If you are migrating from Vyatta 5400, the best way to upgrade to 18.x is through a full OS reload. For more information, see  [Upgrading the OS](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

:    Because there is no simple one-to-one mapping of functionality between Vyatta 5400 and the {{site.data.keyword.vra_full}}, creating a baseline configuration for the VRA is helpful. An IBM Partner, WanClouds, can help you with this process, and provide guidance on creating functionality similar to the Vyatta 5400 on your VRA.

:    For more information about common issues encountered during this upgrade process, see [Vyatta 5400 common migration issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
