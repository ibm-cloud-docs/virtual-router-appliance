---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: 5400, vyatta, migrate, fsa, Fortigate

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Migrating a Vyatta 5400 to a Juniper vSRX or Fortigate Security Appliance (FSA) 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

In addition to being able to upgrade your Vyatta 5400 device to an {{site.data.keyword.vra_full}}, you can also migrate to some of the most cost effective, reliable, and secure options on the market; the Juniper vSRX and the Fortigate Security Appliance. However, you will not be able to reuse your existing Vyatta 5400 device or keep your associated IP addresses.
{: shortdesc}

The following procedures provide instructions for upgrading either a stand alone Vyatta 5400, or two Vyatta 5400 devices operating in a High Availability (HA) pair, to either a Juniper vSRX or FSA.

## Upgrading a Stand Alone Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

To upgrade a single Vyatta 5400 to a Juniper vSRX or FSA - 10G Appliance with the least amount of down-time, we recommend you perform the following procedure.

1. Ensure that you have backed up your 5400 and stored the data in two different locations. This includes SSH keys, SSL certs, scripts and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Order the new device, following the instructions for either [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started) or for [FSA -10G](/docs/fortigate-10g?topic=fortigate-10g-getting-started). 

  It is a recommended best practice to order a High Availability solution.
  {: tip}

3. Load the applicable configuration you onto your newly ordered device.

4. Schedule time to migrate your VLANs to your new device.

  This step requires a brief downtime.
  {: note}

5. Test and confirm that your new device is functioning properly.

6. Open a support case to cancel your Vyatta 5400 device.

## Upgrading a Vyatta 5400 High Availability Pair
{: #upgrading-a-vyatta-5400-high-availability-pair}

To upgrade two Vyatta 5400s in an HA pair, perform the following procedure:

1. Ensure that you have backed up your 5400 and stored the data in two different locations. This includes SSH keys, SSL certs, scripts and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Order the new device, following the instructions for either [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started) or for [FSA-10G](/docs/fortigate-10g?topic=fortigate-10g-getting-started). 

3. Load the applicable configuration onto your newly ordered device.

4. Schedule time to migrate your VLANs to your new device.

  This step requires a brief downtime.
  {: note}

5. Test and confirm that your new device is functioning properly.

6. Open a support case to cancel your Vyatta 5400 devices.
