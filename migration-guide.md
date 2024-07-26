---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Migration overview
{: #migration-overview}

Legacy Vyatta 5400 customers should migrate to {{site.data.keyword.vra_full}} (VRA) as soon as possible, because support for Vyatta 5400 was removed as of March 31, 2019. For more information, see the [Vyatta 5400 end of support announcement](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## How upgrading can benefit you
{: #how-upgrading-can-benefit-you}

In addition to a variety of new features and capabilities that IBM's {{site.data.keyword.vra_full}} brings to the table, there are a number of improvements it provides. See the [FAQs](/docs/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) for more details.
{: shortdesc}

Upgrading to a new device has a different configuration from the Vyatta 5400. As a result, a Vyatta 5400 configuration (file) cannot transfer to your new appliance.
{: note}

## Migration documentation
{: #migration-documentation}

To help you migrate from your Vyatta 5400, see the following documentation and support:

| Migration Documentation | Description |
| ------------- | ------------- |
| [Upgrading the Vyatta 5400 and reusing its IP address](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Instructions on upgrading your Vyatta 5400 to an equivalent {{site.data.keyword.vra_full}}, while reusing your Vyatta 5400 IP Addresses. |
| [Migrating a Vyatta 5400 to a Juniper vSRX or Fortigate Security Appliance (FSA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Instructions on migrating to either a Juniper vSRX or the Fortigate Security Appliance. This option does not allow you to reuse your existing Vyatta 5400 device or keep your associated IP addresses. |
| [Common migration issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Information on the most frequently encountered issues or behavior changes you may encounter after migrating from a Vyatta 5400 device to an {{site.data.keyword.vra_full}}. In many cases, it includes workarounds to address these issues. |
{: caption="Migration documentation and support" caption-side="bottom"}

To order a new {{site.data.keyword.vra_full}}, see [Getting started with IBM Cloud Virtual Router Appliance (VRA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started-vra) for more details.
{: note}
