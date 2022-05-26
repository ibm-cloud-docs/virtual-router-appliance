---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-25"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Resolving upgrade issues
{: #upgrade-issues}

There are several issues that you might experience after [upgrading](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) your VRA image. To troubleshoot an upgrade issue, start by connecting to the VRA using SSH if possible. Otherwise, connect to the IPMI remote management console. After you are connected, check the status of your interfaces, VRRP, and IPsec tunnels as necessary to find what isn't working. Are the main interfaces up? Is VRRP up? Are the IPsec tunnels up?

A connection to the IPMI requires a [classic SSL VPN](/docs/iaas-vpn?topic=iaas-vpn-getting-started) connection. 
{: tip}

The following commands are available for checking your VRA's general status and logs:

* `show version`
* `show vrrp`
* `show interfaces`
* `show vpn ike sa`
* `show vpn ipsec sa`
* `show log messages`
* `journalctl -f -a`

The `journalctl` commands are only available when `user-isolation` is disabled.
{: note}

When opening a [support case](/docs/virtual-router-appliance?topic=gateway-appliance-getting-help) for an upgrade issue, include the output of at least the first 3 previous commands along with the output of any other relevant commands and errors that show the issue. If your IPsec tunnels are down, then include the first 5 commands. You should also try to reset any down IPsec tunnels by using `reset vpn ipsec-peer <Peer IP> tunnel <tunnel number>`. You can also try restarting the entire IPsec daemon with `restart vpn`. 

To avoid prolonged downtime for stand-alone VRAs, try reverting to the previous working version by using the following instructions after you complete your basic troubleshooting and information gathering.

## Reverting to a previous version
{: #revert-version}

To revert your VRA to a previous version, perform one of the following actions, depending on your situation:

* If you can access your VRA by using the IPMI console or SSH, use the Vyatta CLI command to set a default-boot image and then reboot. This reboots your VRA to the chosen version. For example:

    ```sh
    vyatta@vyatta01:~$ set system image default-boot
    Possible completions:
      1801q.09052048
      1912q.09012155
      1912r.10190551
      2012d.06101417
      2012h.04211451
      2110c.03031901
      <Enter>         Execute the current command
      <text>          <No help text available>
    
    vyatta@vyatta01:~$ set system image default-boot 2012d.06101417
    Default boot image has been set to "2012d.06101417".
    You need to reboot the system to start the new default image.
    
    vyatta@vyatta01:~$ reboot
    Proceed with reboot? (Yes/No) [No] y
    
    Broadcast message from vyatta@vyatta01 (pts/1) (Mon May 23 11:07:03 20
    
    The system is going down for reboot
    ```

* If you cannot access your VRA by using SSH or the IPMI console, reboot the device, then press the Esc key during boot to reach the Grub boot menu. Select the version that you want to revert to. 

    You can also use the Grub boot menu for recovering a Vyatta user's password.
    {: tip}
    
    The following illustrations show the process on a Supermicro server by using the IPMI HTML5 console:
    
    Press Esc when the boot process shows this screen:
    ![image](https://media.github.ibm.com/user/84952/files/5bf80b80-da89-11ec-8006-1d362a5b1044){: class="center"}
    
    Doing so triggers the Grub boot menu:
    ![image](https://media.github.ibm.com/user/84952/files/6d411800-da89-11ec-9da7-10df1aeadda2){: class="center"}
    
    Select the desired version that you want to revert to:
    ![image](https://media.github.ibm.com/user/84952/files/729e6280-da89-11ec-81ee-001a60c3e942){: class="center"}

## If your VRA is in a factory reset state
{: #factory-reset}

If your VRA becomes inaccessible after an update, and your password does not work in the IPMI console, then it is likely that the device is in a factory reset state. There are at least two scenarios that can cause this issue.

First, it is possible that you selected **No** instead of **Yes** when you were prompted to save the configuration during the upgrade process. To fix this issue, log in as the user that you would have previously, then reboot. After that, use one of the two methods outlined in the previous section to revert to a former version. You can then perform the upgrade procedure again, this time making sure to select **Yes** to save the configuration.

The second possibility is that there is an old line in your configuration file that the version you are upgrading to does not support. This is generally a bug and should be reported by creating a [support case](/docs/virtual-router-appliance?topic=gateway-appliance-getting-help).

To fix this issue, log in as user `vyatta` with password `vyatta` from the IPMI, and find the latest `config.boot` file in your VRA file system by running the linux/bash `find` command. Then, run the `merge` command on that file to see what is causing the configuration problem during the upgrade process. 

The following example illustrates problems when updating from 1801ze to 1912f. Notice that the `find` command pulled 4 `config.boot` files that were archived in the file system even after the upgrade. To use the `find` command, use the `su` command to change your user to the root user.

```sh
vyatta@gateway02# find / -name config.boot 2>/dev/null | grep 1801ze
/lib/live/mount/persistence/sda2/boot/1801ze.01142008/persistence/rw/mnt/config/config.boot
/lib/live/mount/persistence/sda2/boot/1801ze.01142008/persistence/rw/mnt/config/archive/config.boot
/run/live/persistence/sda2/boot/1801ze.01142008/persistence/rw/mnt/config/config.boot
/run/live/persistence/sda2/boot/1801ze.01142008/persistence/rw/mnt/config/archive/config.boot
 
vyatta@gateway02# merge /lib/live/mount/persistence/sda2/boot/1801ze.01142008/persistence/rw/mnt/config/config.boot
```
{: screen}

Using Configure mode, run the `merge` or `load` command, specifying one of the latest `config.boot` files from the preceding example, then commit. The errors show the cause of the problem. Repeat this process and resolve all the issues until the commit is successful.

You can upgrade your other VRAs without having to repeat this process by making the same changes on those devices before you attempt an update. Fixing these issues before running the upgrade allows subsequent updates to work. 

Report all bugs that you find during this process by opening a [support case](/docs/virtual-router-appliance?topic=gateway-appliance-getting-help) so that the vendor can fix these issues in later releases.
{: important}

## Upgrading from 1801 to 1912
{: #1801-to-1912}

You might encounter these common issues when upgrading from 1801 to 1912:

Many Bash commands no longer work
:    By version 1912, a security feature called [user-isolation](https://docs.vyatta.com/en/release-notes/earlier-releases/earlier-releases/vnos-release-notes-1903/new-features#topic-6481){: external} was enabled by default, which creates a shell session where commands for accessing the underlying Debian system are limited. To fix this, commit the line `set system login user-isolation disable` in your configuration and then log in again.

Problems with time zones that contain 3 locations 
:    Time zones that contain 3 locations, such as America/Kentucky/Monticello, can cause a complete failure of your upgrade, which results in the VRA entering a factory reset condition. To fix this issue, set the time zone to one that has only 2 locations. 

:    This issue, [VRVDR-52825](/docs/virtual-router-appliance?topic=virtual-router-appliance-at-t-vyatta-5600-vrouter-software-patches#1912g), was fixed in version 1912g.
{: note}

Port ranges starting with 0 instead of 1
:    If you configure a port range to start with 0 instead of 1, it can cause a complete failure of your upgrade, which results in the VRA entering a factory reset condition. To fix this, change your port ranges to start with 1 instead of 0 and perform the upgrade process again. 

:    This issue, [VRVDR-52668](/docs/virtual-router-appliance?topic=virtual-router-appliance-at-t-vyatta-5600-vrouter-software-patches#1912g), was fixed in version 1912g.
{: note}

## Upgrading from 1912 to 2012
{: #Common-Issues-1912-to-2012}

You might encounter these common issues when upgrading from 1912 to 2012:

AES-NI must be enabled before updating to 2012
:    By default, some old BIOS versions from 2014 disable AES-NI. IPsec issues, as well as VRRP and config-sync issues, can be the result of AES-NI being disabled. To fix this, run a firmware update using a cloud portal for the VRA's system board BIOS before upgrading to 2012.

IPsec tunnels either do not initiate after a reboot or failover
:    The Vyatta 5400's `run-transition-scripts` command was deprecated as noted in 2012's [release notes](https://docs.vyatta.com/en/release-notes/earlier-releases/earlier-releases/vnos-release-notes-2012/features#topic-8059){: external}. Instead, use the `notify ipsec configuration` line in the VRA configuration file that replaced that command for the Vyatta 5600.

:    This issue can also be caused by AES-NI not being enabled, per the previous issue. 
{: tip}

:    This example illustrates the old version:

    ```sh
    set interfaces bonding dp0bond1 vrrp vrrp-group 1 run-transition-scripts backup /config/scripts/ipsec-stop
    set interfaces bonding dp0bond1 vrrp vrrp-group 1 run-transition-scripts fault /config/scripts/ipsec-stop
    set interfaces bonding dp0bond1 vrrp vrrp-group 1 run-transition-scripts master /config/scripts/ipsec-restart
    ```
    {: pre}

:    Switch to this type of configuration, making sure to match the correct outside interface and vrrp-group:

    ```sh
    set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
    ```
    {: pre}

Failures with multiple IPsec tunnels
:    VRA versions 2012h and earlier have a bug where IPsec tunnels fail when there are many of them. When this occurs, errors similar to the following occur in your IPsec and system logs:

    ```sh
    2022-04-28T10:15:28+0000 dataplane[4551]: CRYPTODEV: rte_cryptodev_pmd_allocate() line 726: Reached maximum number of crypto devices
    2022-04-28T10:15:28+0000 dataplane[4551]: CRYPTODEV: rte_cryptodev_pmd_create() line 110: Fail
    ```
    {: screen}
    
:    This issue was fixed in version 2012k. If you have more than 10 IPsec tunnels, wait for the release of 2012k. Currently, the exact number of tunnels that cause this issue is unknown.

## Vbash issues in older 5.2 Upgrades
{: #Vbash-Issue-5.2}

Occasionally, after a successful upgrade and reboot of a new version of the Vyatta OS, you might encounter a problem where you cannot issue user commands.

For example:

```sh
[jmathews@shelladmindal0101 ~]$ ssh 10.115.174.6 -l vyatta
Welcome to AT&T vRouter

Welcome to AT&T vRouter
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Last login: Fri Feb  2 12:42:45 2018 from 10.0.80.100
vyatta@acs-jmat-vyatta01:~$ show int
-vbash: show: command not found
```
{: screen}

In this case, the problem is not with the upgrade itself. If there were errors in that process, you would see them when you issued the `add system image` command. In this instance, the device was rebooted, but it now has a new and empty `/home` directory space, and any users that appear in the configuration need their home directories regenerated. The error stems from the failure to properly copy the needed "dotfiles" to the `vyatta` user directory:

```sh
vyatta@acs-jmat-vyatta01:~$ ls -la
total 16
drwxr-x--- 3 vyatta users 4096 Feb  2 12:44 .
drwxr-xr-x 1 root   root  4096 Feb  2 11:57 ..
-rw------- 1 vyatta users  456 Feb  2 12:44 .bash_history
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .bash_logout
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .bashrc
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .profile
drwxr-x--- 2 vyatta users 4096 Feb  2 11:57 .ssh
```
{: screen}

Note that three files are zero length, and thus have no configuration. Without the commands to initialize the environment for the VRA user on login, the current shell is unable to interpret the Vyatta commands you issue. As a result, you must obtain the old dotfiles from a different source.

Fortunately, the previous `home` directory still exists as a persistence directory, which allows you to copy the files over. To do so, go to `/lib/live/mount/persistence/sda2/boot` and list the directories there:

```sh
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```
{: screen}

The ISOs for the initial installation and the OS that you are currently running are featured here.

If you made more than one upgrade, those display here as well.
{: note}

Next, change directories using the previously loaded OS as the next directory, and go to the VRA home directory:

```sh
vyatta@acs-jmat-vyatta01:cd 5.2R5S3.06301309/persistence/rw/home/vyatta/
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ ls -la
total 343084
drwxr-x--- 3 vyatta users      4096 Jan 29 16:29 .
drwxr-xr-x 3 root   root       4096 Nov 20 05:05 ..
-rw------- 1 vyatta users     10537 Feb  2 11:55 .bash_history
-rw-r--r-- 1 vyatta users       220 Nov  5  2016 .bash_logout
-rw-r--r-- 1 vyatta users      3515 Nov  5  2016 .bashrc
-rw------- 1 vyatta users        53 Dec 25 10:45 .lesshst
-rw-r--r-- 1 vyatta users       675 Nov  5  2016 .profile
drwxr-x--- 3 vyatta users      4096 Jan  9 10:34 .ssh
-rw-r----- 1 vyatta users 351272960 Jan 26 14:23 vyatta-vrouter-5.2_20180126T1706-amd64.iso
```
{: screen}

From this directory, you can see the dotfiles and copy them over:

```sh
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .bashrc /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .profile /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .bash_logout /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cd /home/vyatta
vyatta@acs-jmat-vyatta01:~$ ls -la
total 28
drwxr-x--- 3 vyatta users 4096 Feb  2 12:44 .
drwxr-xr-x 1 root   root  4096 Feb  2 11:57 ..
-rw------- 1 vyatta users  456 Feb  2 12:44 .bash_history
-rw-r--r-- 1 vyatta users  220 Feb  2 12:56 .bash_logout
-rw-r--r-- 1 vyatta users 3515 Feb  2 12:56 .bashrc
-rw-r--r-- 1 vyatta users  675 Feb  2 12:56 .profile
drwxr-x--- 2 vyatta users 4096 Feb  2 11:57 .ssh
```
{: screen}

After the files are copied, log out, then log back in:

```sh
[jmathews@shelladmindal0101 ~]$ ssh 10.115.174.6 -l vyatta
Welcome to AT&T vRouter

Welcome to AT&T vRouter
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Last login: Fri Feb  2 12:57:29 2018 from 10.0.80.100
vyatta@acs-jmat-vyatta01:~$ show version
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Built on:     Fri Jan 26 17:06:52 UTC 2018
System type:  Intel 64bit
Boot via:     image
HW model:     PIO-518D-TLN4F-ST031
HW S/N:       S14073214705613
HW UUID:      00000000-0000-0000-0000-0CC47A07EF22
Uptime:       12:57:47 up 59 min,  1 user,  load average: 0.35, 0.27, 0.26
vyatta@acs-jmat-vyatta01:~$
```
{: screen}

All your commands now work again, and you can proceed normally.

The HTTPS certificate `/etc/lighttpd/server.pem` might also fail to copy during the OS upgrade process, which can cause High Availability (HA) configurations to fail to synchronize. To fix this problem, copy over the old `server.pem` file in addition to the files listed (issue `su -` to reach root level, then issue the `copy` command), then issue `restart https` to restart the HTTPS `demon.m` file (and the files listed previously).
{: important}
