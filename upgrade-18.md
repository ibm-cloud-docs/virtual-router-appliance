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

# Upgrade Considerations for OS Version 18.01

This file contains a list of things to know when you're upgrading the VRA (Vyatta 5600) from the Brocade 5.X operating system to 18.01 operating system, which includes remediation for the Spectre security breach. There are several issues to be aware of when performing this upgrade.

## Who needs to read this topic

* Anybody with a current VRA that is not running the 1801 operating system. (Current version is 18.01g)

* Anybody looking to install a new 18.01f version for a Virtual Router Appliance (for example, anyone upgrading from 17.2X).

* If you have a Vyatta 5400, this information does not apply to you.

## Why you need this information

Version 1801f of the VRA contains a security fix for the Spectre vulnerability, however a change to the installer itself must be made before the patch can be installed. An intermediate step to install version 1801C is required.

## Normal upgrade procedure
To upgrade to version 18.01f, you must first download and install the 18.01C patch to update the VRA installer:

1. Download the 1801c patch from this location, then follow the [normal procedure](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) to install it.

2. Once you reboot, then download the 18.01f patch from this location, and install it using the [normal procedure](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

You should now be running version 18.01f, with the Spectre security limitation fixed.

## Full reload procedure
As an alternative, you can also do a full reload of VRA at the 18.01f level:

*WARNING:* This procedure allows you to skip the intermediary step of downloading and installing two patches, but you will lose all your data during a full reload using the ISO.

1. Get the 18.01f ISO from this location.
2. Follow the [normal procedure](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) to install it and reboot.

You should now be running version 1801f, with the Spectre security limitation fixed.

## Additional guidance

Because there is no simple one-to-one mapping of functionality between Vyatta 5400 and the Virtual Router Appliance, creating a baseline configuration for the VRA is helpful. An IBM© Partner, WanClouds, can help you with this process, and provide guidance on creating functionality similar to the Vyatta 5400 on your VRA.

For more information about common issues encountered in this upgrade proces, please refer to our [additional documentation](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
