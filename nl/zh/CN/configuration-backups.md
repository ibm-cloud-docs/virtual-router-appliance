---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: backup, configuration

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

# 备份配置
{: #backing-up-a-configuration}

对系统进行更改后，需要备份配置命令。这可以通过运行操作方式命令 `show configuration commands`，然后保存输出（例如，通过从 SSH 会话复制和粘贴）来完成。这将视为配置的最小备份。

更完整的备份涉及生成系统的技术支持归档：

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

然后，可以将生成的归档文件从虚拟路由器设备复制到所选的存储设备。该归档包含配置信息、主目录和日志记录信息的备份。这是完整得多的系统备份。

例如：

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

请考虑备份您在可供所有管理人员访问的中央位置配置设备时所创建的所有注释。
