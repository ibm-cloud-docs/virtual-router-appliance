---

copyright:
  years: 2017, 2025
lastupdated: "2025-04-10"

keywords: 5400, vyatta, migrate, fsa, Fortigate

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Migrating a Vyatta 5400 to a Juniper vSRX or Fortigate Security Appliance (FSA) 10 Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

In addition to being able to upgrade your Vyatta 5400 device to an {{site.data.keyword.vra_full}}, you can also upgrade to the Juniper vSRX and the Fortigate Security Appliance. However, you cannot reuse your existing Vyatta 5400 device or keep your associated IP addresses.
{: shortdesc}

The following procedures provide instructions for upgrading either a stand-alone Vyatta 5400, or two Vyatta 5400 devices in a High Availability (HA) pair, to either a Juniper vSRX or FSA.

## Upgrading a stand-alone Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400-migration}

To upgrade a single Vyatta 5400 to a Juniper vSRX or FSA - 10G Appliance with the least amount of downtime, perform the following procedure:

1. Ensure that you backup your 5400 and stored the data in two different locations. This data includes SSH keys, SSL certs, scripts, and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Order the new device, following the instructions for either [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started-vsrx) or for [FSA -10G](/docs/fortigate-10g?topic=fortigate-10g-getting-started). 

   It is a recommended best practice to order a High Availability solution.
   {: tip}

3. Load the applicable configuration you onto your newly ordered device.

4. Schedule a time to migrate your VLANs to your new device.

   This step requires a brief downtime.
   {: note}

5. Test and confirm that your new device is functioning properly.

6. Open a support case to cancel your Vyatta 5400 device.

## Upgrading a Vyatta 5400 High Availability (HA) pair
{: #upgrading-a-vyatta-5400-high-availability-pair-migration}

To upgrade two Vyatta 5400s in an HA pair, perform the following procedure:

1. Ensure that you back up your 5400 and stored the data in two different locations. This data includes SSH keys, SSL certs, scripts, and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Order the new device, following the instructions for either [Juniper vSRX](/docs/vsrx?topic=vsrx-getting-started-vsrx) or for [FSA-10G](/docs/fortigate-10g?topic=fortigate-10g-getting-started). 

3. Load the applicable configuration onto your newly ordered device.

4. Schedule a time to migrate your VLANs to your new device.

   This step requires a brief downtime.
   {: note}

5. Test and confirm that your new device is functioning properly.

6. Open a support case to cancel your Vyatta 5400 devices.
