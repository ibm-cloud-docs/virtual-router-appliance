---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-22"

keywords: upgrade vyatta

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading the OS
{: #upgrading-the-os}
{: help}
{: support}

You can upgrade the VRA operating system with the command `add system image` on a local ISO file uploaded by our IBM Cloud Security Support team. The version that is provided upon provisioning is [2012g](https://docs.vyatta.com/en/release-notes/vnos-release-notes-2012/patch-release-notes-2012g) from January of 2022. You should update immediately after provisioning or a reload. A list of available Vyatta upgrade versions can be obtained by reviewing [Ciena Vyatta NOS software patches](/docs/virtual-router-appliance?topic=virtual-router-appliance-ciena-vyatta-5600-vrouter-software-patches) and requesting the version's ISO file using the IBM Cloud Support case system. The latest version considered stable by IBM Cloud is 2012p, and the latest available version that has been vetted by support is 2204g. No 2308 versions are recommended at this time due to a VLAN breaking bug, as well as a bug that crashes SSH when a listen IP is set.
{: shortdesc}

To begin the upgrade process, open an IBM Cloud Support case requesting an upload of the latest stable ISO image to your system. You will receive a case update from IBM Cloud Security Support indicating the ISO file upload location on the Vyatta. In the following example, it is in the directory `tmp`, but it is commonly uploaded to `/home/vyatta`.

The upgrade process illustrated in the example below is for a single VRA. If you are using VRA in high availability mode, you must run the same upgrade command on both systems. Furthermore, before updating in HA, it is recommended that you verify config-sync is synced and then issue a reboot on the `BACKUP` machine, to make sure that it can reboot without issue, and verify that it is working properly. Please also make sure that the `BACKUP` has all necessary VLANs, interfaces and configurations to be able to handle the production traffic when the failover is pushed. Then, perform the update on the `BACKUP` and validate it comes back up without issue. Then, access the `MASTER` machine and fail it over using the `reset vrrp master interface <interface name> group <group number>` command. If the `BACKUP` takes over as the `MASTER` while config-sync is out of sync, this can cause the old `BACKUP` to sync with the old `MASTER` and potentially delete large amounts of configurations off of the old `MASTER`. Outages can and do occur if the configurations as well as config-sync are not properly managed on both `MASTER` and `BACKUP`. Finally, upgrade the original `MASTER` after the `BACKUP` has taken control. Optionally, fail back over to the original `MASTER`.
{: important}

Review [Resolving upgrade issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrade-issues) for a list of common issues that can happen when updating between versions, and review the specific Vyatta NOS version's [release notes](https://docs.vyatta.com/en/release-notes/release-notes) for information on known issues and deprecated commands as published by Ciena.
{: tip}

## Upgrade procedure
{: #upgrade-procedure}

To upgrade the VRA, perform the following procedure:

1. Run the command `add system image <Local ISO File>`.
2. Press **Enter** to accept the default name of the ISO image, or enter your own.
3. Select **Yes** to save the current configuration directory and configuration file. 

    Choosing **No** will erase your configuration.
    {: important}
    
5. Choose whether to save the SSH host keys from your current configuration.
6. Press **Enter** to accept the default system console, or enter your own.
7. Press **Enter** to accept the default console speed, or enter your own.
8. Enter the command `reboot` and then enter `Yes` to reboot the machine.
9. If you are using VRA in a high availability mode, run step 1 to step 7 again on the second machine.

The following example illustrates the upgrade process.

```sh
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Welcome to the Brocade vRouter image installer.
Checking MD5 checksums of files on the ISO image...OK.
What would you like to name this image? [5.2R5S4.08091424]: 5.2R5S4
This image will be named: 5.2R5S4
Would you like to save the current configuration
directory and config file? (Yes/No) [Yes]:
Saving current configuration...
Would you like to save the SSH host keys from your
current configuration? (Yes/No) [Yes]:
Saving SSH keys...
Copying squashfs image...
Copying kernel and initrd images...
Copying saved configuration to config partition.
Copying saved SSH host keys.
Enter the desired system console [tty0]:
Enter the console speed [38400]:
Running post-install script...
Done.

vyatta@vra01:~$ show system image
The system currently has the following image(s) installed:

   1: 5.2R5S3.06301309 (running image)
   2: 5.2R5S4 (default boot)

vyatta@vra01:~$ reboot
Proceed with reboot? (Yes/No) [No] y
The system is going down for reboot NOW!

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```
{: screen}
