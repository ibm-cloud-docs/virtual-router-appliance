---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# Upgrading the Vyatta 5400 and Reusing its IP Addresses
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

This option allows you to reuse your existing Vyatta 5400 device as an equivalent {{site.data.keyword.vra_full}} (VRA), and keep your associated IP addresses. The following procedures provide instructions for upgrading either a stand alone Vyatta 5400, or two Vyatta 5400 devices operating in a High Availability (HA) pair.
{: shortdesc}

## Upgrading a Stand Alone Vyatta 5400
{: #upgrading-a-stand-alone-vyatta-5400}

To upgrade a single Vyatta 5400 to a {{site.data.keyword.vra_full}}, perform the following procedure:

1. Ensure that you have backed up your 5400 and stored the data in two different locations. This includes SSH keys, SSL certs, scripts and any other files necessary to recover your current Vyatta 5400 configuration, if necessary.
2. Reload your Vyatta 5400 to a default {{site.data.keyword.vra_full}} using the instructions in [this topic](/docs/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os).

  An OS reload will generate a new password for the root and Vyatta user IDs.
  {: note}

4. Make a note of the new {{site.data.keyword.vra_full}} password.
5. Configure the newly reloaded VRA with your desired settings using [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).

## Upgrading a Vyatta 5400 High Availability Pair
{: #upgrading-a-vyatta-5400-high-availability-pair}

To upgrade two Vyatta 5400s in an HA pair, perform the following procedure:

1. Identify the Backup Vyatta 5400 device and reload it first as a {{site.data.keyword.vra_full}}, using the procedure above.

  Ideally, a functional HA configuration shows all interfaces as either BACKUP or MASTER on a particular node. When in doubt, use the `show vrrp` command to confirm.
  {: note}
2. Configure the newly reloaded VRA with your desired settings using [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
3. The Master Vyatta 5400 and the Backup {{site.data.keyword.vra_full}} will be in a VRRP pair. Change the current Backup {{site.data.keyword.vra_full}} to become the Master using the VRRP command `reset VRRP master`.

  This action will cause an outage in any existing sessions, and the sessions state will be listed as Lost. Any existing NAT session will also be reset.
  {: note}

5. Verify that the new Master {{site.data.keyword.vra_full}} is functioning as desired.
6. Perform the reload procedure above on the current Backup Vyatta 5400 to upgrade it to a VRA.
7. After the second reload, configure the Backup {{site.data.keyword.vra_full}} as desired, using [these instructions](/docs/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
