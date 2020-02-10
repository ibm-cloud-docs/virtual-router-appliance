---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# Migration Overview
{: #migration-overview}

Legacy Vyatta 5400 customers should migrate to {{site.data.keyword.vra_full}} (VRA) as soon as possible, as support for Vyatta 5400 will be removed as of March 31, 2019. To read the full end of support announcement and for more information, click [here](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## How Upgrading Can Benefit You
{: #how-upgrading-can-benefit-you}

In addition to a variety of new features and capabilities that IBM's {{site.data.keyword.vra_full}} brings to the table, there are also a number of improvements it provides as well. Refer to our [FAQs](/docs/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) for more details.
{: shortdesc}

Upgrading to a new device will have a different configuration from the Vyatta 5400. As a result, a Vyatta 5400 configuration (file) will not transfer to your new appliance.
{: note}

## Migration Documentation
{: #migration-documentation}

To help you migrate from your Vyatta 5400, we have prepared the following documentation and support:

| Migration Documentation | Description |
| ------------- | ------------- |
| [Upgrading the Vyatta 5400 and Reusing its IP Address](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Instructions on upgrading your Vyatta 5400 to an equivalent {{site.data.keyword.vra_full}}, while reusing your Vyatta 5400 IP Addresses. |
| [Migrating a Vyatta 5400 to a Juniper vSRX or Fortigate Security Appliance (FSA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Instructions on migrating to either a Juniper vSRX or the Fortigate Security Appliance. This option will not allow you to reuse your existing Vyatta 5400 device or keep your associated IP addresses. |
| [Common Migration Issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Information on the most frequently encountered issues or behavior changes you may encounter after migrating from a Vyatta 5400 device to an {{site.data.keyword.vra_full}}. In many cases, it includes workarounds to address these issues. |

If you need to simply order a new {{site.data.keyword.vra_full}}, [click here](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started) for more details.
{: note}
