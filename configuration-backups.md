---

copyright:
  years: 2017
lastupdated: "2020-07-30"

keywords: vra, backup, configuration

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
{:important: .important}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Backing up a configuration
{: #backing-up-a-configuration}
{: help}
{: support}

A Vyatta configuration can be a boot configuration file or a file with a list of configuration commands. The latest 20 completed commits are saved on the system by default in `/config/archive` as archived boot configuration files. 
{: shortdesc}

A backup can be taken of the current `config.boot` by switching to user `root` and copying the file to another location. The current configuration commands for your Virtual Router Appliance can also be backed up by running the operational mode command `show configuration commands` and then saving the output. Both of these methods are considered a minimum backup for the configuration.

The following example generates the complete list of commands used to configure the system and then redirects the list of commands into a file in the Vyatta home directory:

```
vyatta@gateway02:~$ show configuration commands > /home/vyatta/configcomm.bak-07-30-2020
vyatta@gateway02:~$ ls -alh | grep bak
-rw-r----- 1 vyatta users  21K Jul 30 01:10 configcomm.bak
-rw-r----- 1 vyatta users  21K Jul 30 01:12 configcomm.bak-07-30-2020
```

The following example creates a backup copy of the boot configuration file and places it into `/home/vyatta`:

```
vyatta@gateway02:~$ su
Password:
root@gateway02:/home/vyatta# cp /config/config.boot /home/vyatta/config.boot.bak-07-30-2020
root@gateway02:/home/vyatta# ls -alh | grep config.boot
-rw------- 1 root   root   12K Jul 30 01:36 config.boot.bak-07-30-2020
```

A more complete backup, including log data, involves generating a technical support archive for the system:

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

The generated archive file can then be copied from the VRA to the storage device of your choice. The archive contains backups of the configuration information, home directories and logging information, and is a much more complete backup of a system.

As an example:

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Consider backing up any notes that you create while configuring the device at a central location accessible to all of your administration staff.
