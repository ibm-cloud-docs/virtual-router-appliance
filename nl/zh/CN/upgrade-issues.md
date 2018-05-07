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

# 升级问题
有时，成功升级并重新引导新版本的 Vyatta 操作系统后，可能会遇到无法发出用户命令的问题。

例如：

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

对于这种情况，问题不在升级本身。如果在升级过程中发生错误，那么发出 `add system image` 命令时会看到这些错误。在此，设备已经重新引导，但是现在该设备有一个新的空 `/home` 目录空间，并且配置中出现的任何用户都需要重新生成其主目录。错误的根源在于未能将所需的“dot 文件”正确复制到 `vyatta` 用户目录：

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

请注意，有三个文件的长度为零，因此说明这些文件不包含配置。如果在登录时未通过命令为 VRA 用户初始化环境，那么当前 shell 无法解释您发出的 Vyatta 命令。因此，您必须从其他来源获取旧的 dot 文件。

幸运的是，先前的 `home` 目录作为持久性目录仍然存在，允许您复制这些文件。为此，请转至 `/lib/live/mount/persistence/sda2/boot`，然后列出其中的目录：

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

下面专门列出了用于初始安装和当前运行的操作系统的 ISO。 

**注：**如果进行过多次升级，那么那些 ISO 也会显示在这里。

接下来，将使用先前装入的操作系统的目录更改为下一个目录，然后进入 VRA 主目录：

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

进入该目录后，可以查看 dot 文件并复制这些文件：

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

复制后，请注销，然后重新登录：

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
所有的命令都会再次起作用，然后您可以正常继续工作。

**注：**在操作系统升级过程中，HTTPS 证书 `/etc/lighttpd/server.pem` 也可能复制失败，这可能会导致高可用性 (HA) 配置无法同步。要解决此问题，除了复制上面列出的文件外，还请复制旧 `server.pem` 文件（发出 `su -` 以转至根级别，然后发出 `copy` 命令），然后发出 `restart https` 以重新启动 HTTPS `demon.m` 文件（及上面列出的文件）。
