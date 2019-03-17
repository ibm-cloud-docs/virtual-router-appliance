---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Migrating a Vyatta 5400 to a Juniper vSRX or Fortigate Security Appliance (FSA) 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

In addition to being able to upgrade your Vyatta 5400 device to an IBM Virtual Router Appliance, you can also migrate to some of the most cost effective, reliable, and secure options on the market; the Juniper vSRX and the Fortigate Security Appliance.
However, you will not be able to reuse your existing Vyatta 5400 device or keep your associated IP addresses.

The following procedures provide instructions for upgrading either a stand alone Vyatta 5400, or two Vyatta 5400 devices operating in a High Availability (HA) pair, to either a Juniper vSRX or FSA.

## Upgrading a Stand Alone Vyatta 5400

To upgrade a single Vyatta 5400 to a Juniper vSRX or FSA - 10G Appliance with the least amount of down-time, we recommend you perform the following procedure.

1. Ensure that you have backed up your 5400 and stored the data in two different locations. This includes SSH keys, SSL certs, scripts and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contact nwom@us.ibm.com to request that your configuration be converted

3. Order the new device, following the instructions for either [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) or for [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

  This is a good opportunity to consider ordering a High Availability solution.
  {: note}

4. Load the configuration you received from nwom@us.ibm.com onto your newly ordered device.

5. Schedule time to migrate your VLANs to your new device.

  This step requires a brief downtime.
  {: note}

6. Test and confirm your new device is functioning properly.

7. Open a support ticket to cancel your Vyatta 5400 device.

## Upgrading a Vyatta 5400 High Availability Pair

To upgrade two Vyatta 5400s in an HA pair, perform the following procedure:

1. Ensure that you have backed up your 5400 and stored the data in two different locations. This includes SSH keys, SSL certs, scripts and any other files necessary to recover your current Vyatta 5400 configuration, if necessary. To do so, follow [these instructions](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contact nwom@us.ibm.com and request that your configuration be converted.

3. Order the new device, following the instructions for either [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) or for [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

4. Load the configuration you received from nwom@us.ibm.com onto your newly ordered device.

5. Schedule time to migrate your VLANs to your new device.

  This step requires a brief downtime.
  {: note}

6. Test and confirm your new device is functioning properly.

7. Open a support ticket to cancel your Vyatta 5400 devices.
