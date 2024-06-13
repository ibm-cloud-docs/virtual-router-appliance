---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 5400, eos

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Vyatta 5400 end of support FAQs
{: #vyatta-5400-end-of-support-faqs}

These frequently asked questions can help you when migrating from the Vyatta 5400 to the {{site.data.keyword.vra_full}}.
{: shortdesc}

## Why is the Vyatta 5400’s “End of Support” (EoS) on March 31, 2019?
{: #why-end-of-support}
{: faq}

In September 2017, the legacy Vyatta 5400 announced its EoS to be on February 20, 2018. Based on IBM’s lifecycle policy for support, EoS was six months after the General Availability (GA) date of {{site.data.keyword.vra_full}} (VRA).

To honor customer migration timelines, the Vyatta 5400 EoS date was extended to March 31, 2019. Because the Debian 7 software is now no longer supported by the Debian Open Source community, there are no future plans to extend vendor support from AT&T.

For more information, see the [End of Support Announcement](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## What does “End of Support” mean for me as a customer?
{: #meaning-eos}
{: faq}

After the End of Support date, AT&T no longer provides any code patches or accept support escalations from IBM.

Similarly, IBM Cloud Support no longer troubleshoots configuration or networking issues on Vyatta 5400 deployments.  Support is limited to hardware level requests (hard drive, RAM, and so on), power and Out of Band (IPMI) connectivity.

It is highly recommend that customers take immediate action to migrate onto an alternate solution, such as the {{site.data.keyword.vra_full}} (VRA; based on the Vyatta 5600) or Juniper vSRX.  See [Migrating Vyatta 5400](/docs/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) to get started.

## What happens if I'm still running my IBM Cloud workloads using a Vyatta 5400 after March 31?
{: #still-running-5400}
{: faq}

Your Vyatta 5400 continues to work after March 31. However, your business and application environments might be exposed to potential security threats and other tampering violations due to latent vulnerabilities in the Vyatta 5400 software.

If you encounter a network issue which takes down your business and application environment, and you trace the root cause to the Vyatta 5400, escalate this matter to our 5400 Offering Manager, because support is no longer be available from IBM or AT&T. You can reach the Offering Management team through email at `nwom@us.ibm.com`.

## What about my underlying Bare Metal Server hardware – is it still supported?
{: #bare-metal-hardware}
{: faq}

Hardware replacements are supported, but if trouble-shooting indicates that your problem is related to the Vyatta OS, you will be directed to migrate to a supported hardware offering immediately. See [Migrating Vyatta 5400](/docs/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) to get started.

## As a customer who owns a Vyatta 5400, what do I need to do by March 31, 2019?
{: #what-to-do}
{: faq}

Customers who have a Vyatta 5400 should migrate to either VRA (Vyatta 5600), Juniper vSRX, or Fortigate Security Appliance (FSA) 10G. The VRA (Vyatta 5600) is still fully supported. There is no current or projected end of support date for the VRA from either IBM Cloud or AT&T. See [Migrating Vyatta 5400](/docs/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) to get started.

   VRAs and vSRXs are customer managed devices.
   {: note}

## Is support available from IBM for migrating from the Vyatta 5400 to either the VRA, vSRX, or FSA 10G?
{: #available-support}
{: faq}

The Vyatta 5400 to VRA (5600) Configuration Conversion Service is still available:

* For existing customers, IBM Cloud is providing a no-cost offering to assist with re-factoring your existing Vyatta 5400 configuration into {{site.data.keyword.vra_full}} (VRA), Juniper vSRX, or Fortigate Security Appliance (FSA) 10G formats. To submit a request for the Configuration Conversion service, send an email to `nwom@us.ibm.com` with the subject: `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx.`.

   Be sure to insert your application choice in place of `aaaaaaaa` ({{site.data.keyword.vra_full}}, Juniper vSRX, or Fortigate Security Appliance (FSA) 10G), and your specific account number in place of `xxxxxx` in your subject line.
   {: note}

* Wanclouds, our partner in this conversion configuration process, has completed several hundred successful migration engagements. They transform your existing Vyatta 5400 to create similar functionality on the Vyatta 5600 Platform. They provide their services in two tiers:

   ![Tiers](images/tiers.png "Tiers"){: caption="Service tiers" caption-side="bottom"}

See [Migrating Vyatta 5400](/docs/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) to get started.

## Are there any additional paid migration services available from IBM Business Partners for migrating off the Vyatta 5400?
{: #additional-migration}
{: faq}

We have several business partners who provide paid support for Vyatta 5400 migrations. For more information, see the [End of Support Announcement](/docs/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).

## Is there a Vyatta 5400 Offering Management support contact in IBM where I can ask questions related to my Vyatta 5400 migration?
{: #5400-om}
{: faq}

Contact IBM Vyatta 5400 and VRA Network Offering Management with questions at `nwom@us.ibm.com`. You can also contact them using slack with the IBM Watson Cloud Platform workspace: `#vyatta-migration`

## What additional resources are available to help me with this migration?
{: #additional-migration-resources}
{: faq}

Review the following {{site.data.keyword.vra_full}} documentation resources for more information:

* [Getting Started with {{site.data.keyword.vra_full}}](/docs/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
* [About the VRA](/docs/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
* [Vyatta 5400 Migration Overview](/docs/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
