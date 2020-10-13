---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-22"

keywords: upgrade vyatta

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
{:important: .important}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Upgrading the OS
{: #upgrading-the-os}
{: help}
{: support}

Upgrading the VRA operating system can be performed with the command `add system image` on a local ISO file uploaded by our {{site.data.keyword.IBM}} Support team. A list of available Vyatta upgrade versions can be obtained using the {{site.data.keyword.IBM_notm}} Support case system.
{: shortdesc}

To begin the upgrade process, open an {{site.data.keyword.IBM_notm}} Support case requesting an upload of the latest stable ISO image to your system. You receive a ticket update from IBM Support (ACS-Security) indicating where the ISO file was uploaded. In the following example, it is in the directory `tmp`, but it is commonly uploaded to `/home/vyatta`.

The upgrade process illustrated in this example is for a single VRA. If you are using VRA in high availability mode, you must run the same upgrade command on both systems. Furthermore, it is recommended that you upgrade the `BACKUP` machine first, and verify that it is working properly. Then access the `MASTER` machine and fail it over using the `reset vrrp` command. Finally, upgrade the original `MASTER` after the `BACKUP` has taken control.
{: important}

To upgrade the VRA, perform the following procedure.

1. Run the command `add system image <Local ISO File>`.
2. Press **Enter** to accept the default name of the ISO image, or enter your own.
3. Choose whether to save the current configuration directory and configuration file.
4. Choose whether to save the SSH host keys from your current configuration.
5. Press **Enter** to accept the default system console, or enter your own.
6. Press **Enter** to accept the default console speed, or enter your own.
7. Enter the command `reboot` and then enter `Yes` to reboot the machine.
8. If you are using VRA in a high availability mode, run step 1 to step 7 again on the second machine.

The following example illustrates the upgrade process.

```
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
{:screen}
