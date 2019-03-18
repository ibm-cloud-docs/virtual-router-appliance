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

# 備份配置
{: #backing-up-a-configuration}

當系統發生變更時，需要備份配置指令。這可以藉由執行作業模式指令 `show configuration commands` 然後儲存輸出（例如，從 SSH 階段作業複製和貼上）來達成。這將視為配置的最小備份。

更完整的備份包括產生系統的技術支援保存檔： 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

然後，產生的保存檔可以從 Virtual Router Appliance 複製到您選擇的儲存裝置。保存檔包含配置資訊、起始目錄及記載資訊等的備份。它是更完整的系統備份。 

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

請考慮將您在配置裝置時所建立的任何附註，備份到所有管理人員都可以存取的中央位置。
