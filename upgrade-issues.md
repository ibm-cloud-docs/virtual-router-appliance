---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Upgrade Issues
Occasionally, after a successful upgrade and reboot of a new version of the Vyatta OS, you may encounter an issue where you are unable to issue user commands.

For example:

```
[jmathews@shelladmindal0101 ~]$ ssh 10.115.174.6 -l vyatta
Welcome to AT&T vRouter

Welcome to AT&T vRouter
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Last login: Fri Feb  2 12:42:45 2018 from 10.0.80.100
vyatta@acs-jmat-vyatta01:~$ show int
-vbash: show: command not found
```

In this case, the problem is not with the upgrade itself. If there were errors in that process, you would see them when you issued the `add system image` command. Here the device has been rebooted, but it now has a new, and empty `/home` directory space, and any users that appear in the configuration will need their home directories regenerated. The error stems from the failure to properly copy the needed "dotfiles" to the `vyatta` user directory:

```
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

Note that three files are zero length, and thus have no configuration. Without the commands to initialize the environment for the VRA user on login, the current shell is unable to interpret the Vyatta commands you issue. As a result, you must obtain the old dotfiles from a different source.

Fortunately, the previous `home` directory still exists as a persistence directory, allowing you to copy the files over. To do so, go to `/lib/live/mount/persistence/sda2/boot` and list out the directories there:

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

The ISOs for the initial installation and your currently running OS are featured here. 

**NOTE:** If you made more than one upgrade, then those will display here as well.

Next, change directories using the previously loaded OS as the next directory, and go into the VRA home directory:

```
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

Once inside, you can see the dotfiles and copy them over:

```
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

Once copied, logout, and log back in:

```
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

All your commands will work again, and you can proceed normally.
