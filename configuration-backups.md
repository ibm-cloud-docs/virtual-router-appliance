---

copyright:
  years: 2017
lastupdated: "2017-10-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Backup a Configuration
The configuration commands need to be backed up when there is a change to the system. This can be accomplished by running the operational mode command `show configuration commands` and then saving the output (for example by copying and pasting from the SSH session). This would be considered a minimum backup for the configuration.

A more complete backup involves generating a technical support archive for the system: 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

The generated archive file can then be copied from the Virtual Router Appliance to the storage device of your choice. The archive contains backups of the configuration information, home directories and logging information. It is a much more complete backup of a system. 

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