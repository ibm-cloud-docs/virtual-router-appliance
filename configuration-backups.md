---

copyright:
  years: 2017, 2020
lastupdated: "2020-07-30"

keywords: 

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

A Vyatta configuration can be a boot configuration file, or a file with a list of configuration commands. By default, the latest 20 completed commits are saved on the system in `/config/archive` as archived boot configuration files.
{: shortdesc}

You can take a backup of the current `config.boot` by switching to user `root` and copying the file to another location. You can also back up the current configuration commands for your Virtual Router Appliance by running the operational mode command `show configuration commands` and then saving the output. Both of these methods are considered a minimum backup for the configuration.

The following example generates the complete list of commands used to configure the system, and then redirects the list of commands into a file in the Vyatta home directory:

```sh
vyatta@gateway02:~$ show configuration commands > /home/vyatta/configcomm.bak-07-30-2020
vyatta@gateway02:~$ ls -alh | grep bak
-rw-r----- 1 vyatta users  21K Jul 30 01:10 configcomm.bak
-rw-r----- 1 vyatta users  21K Jul 30 01:12 configcomm.bak-07-30-2020
```

The next example creates a backup copy of the boot configuration file and places it in `/home/vyatta`:

```sh
vyatta@gateway02:~$ su
Password:
root@gateway02:/home/vyatta# cp /config/config.boot /home/vyatta/config.boot.bak-07-30-2020
root@gateway02:/home/vyatta# ls -alh | grep config.boot
-rw------- 1 root   root   12K Jul 30 01:36 config.boot.bak-07-30-2020
```

The following example uses the configuration mode's save command to backup:

```sh
vyatta@asloma-vra-5218-ha1:~$ configure

[edit]
vyatta@asloma-vra-5218-ha1# save /home/vyatta/config-05-14-2021.bak
Saving configuration to '/home/vyatta/config-05-14-2021.bak'...
Done
[edit]
```

A more complete backup, including log data, involves generating a technical support archive for the system:

```sh
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

You can then copy the generated archive file from the VRA to the storage device of your choice. The archive contains backups of the configuration information, home directories, and logging information.

As an example:

```sh
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Consider backing up any notes that you create while configuring the device at a central location accessible to all of your administration staff.
{: tip}

## Restoring a configuration
{: #restoring-a-configuration}
{: help}
{: support}

To restore a backup configuration from a `config.boot` file, copy the file to `/config/config.boot` and reboot. For example:

```sh
root@asloma-vra-5218-ha1:/home/vyatta# cp config.boot.bak-05-14-2021 /config/config.boot
root@asloma-vra-5218-ha1:/home/vyatta# reboot
```

Alternatively, to restore the configuration from a `save`, enter the configuration mode, load the backup configuration and commit:

```sh
vyatta@cicd-bm-vra2-sa0# save /home/vyatta/config.bak.05-14-2021
Saving configuration to '/home/vyatta/config.bak.05-14-2021'...
Done
[edit]
vyatta@cicd-bm-vra2-sa0# load /home/vyatta/config.bak.05-14-2021
Loading configuration from '/home/vyatta/config.bak.05-14-2021'...
Load complete.  Use 'commit' to make changes active.
[edit]
vyatta@cicd-bm-vra2-sa0# commit
```
