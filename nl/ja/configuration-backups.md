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

# 構成のバックアップ
{: #backing-up-a-configuration}

構成コマンドは、システムが変更された場合にバックアップする必要があります。 これは、操作モード・コマンド `show configuration commands` を実行してから出力を (例えば、SSH セッションからコピーして貼り付けることによって) 保存することによって行えます。 これは、構成の最小バックアップと見なされます。

より詳細なバックアップを作成するには、以下のようにして、システムのテクニカル・サポート・アーカイブを生成する必要があります。

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

生成されたアーカイブ・ファイルは、この後 {{site.data.keyword.vra_full}} から任意のストレージ・デバイスにコピーできます。 アーカイブには、構成情報、ホーム・ディレクトリー、およびロギング情報のバックアップが含まれています。 これは、システムのより詳細なバックアップです。

次に例を挙げます。

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

すべての管理スタッフがアクセスできる中央の場所でデバイスを構成するときに作成したメモをバックアップすることを検討してください。
