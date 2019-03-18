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

# OS バージョン 18.01 のアップグレードに関する考慮事項

このファイルには、VRA (Vyatta 5600) を Brocade 5.X オペレーティング・システムから 18.01 オペレーティング・システムにアップグレードする際に知っておくべき事項 (Spectre セキュリティー・ブリーチの修復を含む) のリストが含まれています。 このアップグレードを実行する際には、認識しておくべき問題がいくつかあります。

## 本トピックの対象読者

* 1801 オペレーティング・システムを実行していない現行 VRA のユーザー。 (現行バージョンは 18.01g)

* 仮想ルーター・アプライアンス用に新しい 18.01f バージョンをインストールしようとしているユーザー (例えば 17.2X からアップグレードするユーザーなど)。

* この情報は、Vyatta 5400 をお持ちのお客様には該当しません。

## この情報が必要な理由

VRA のバージョン 1801f には、Spectre の脆弱性に対するセキュリティー修正が含まれています。ただし、パッチをインストールする前に、インストーラー自体に変更を加える必要があります。 バージョン 1801C をインストールするための中間ステップが必要です。

## 通常のアップグレード手順
バージョン 18.01f にアップグレードするには、まず VRA インストーラーを更新するための 18.01C パッチをダウンロードしてインストールする必要があります。

1. このロケーションから 1801c パッチをダウンロードし、[通常の手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)に従ってインストールします。

2. リブート後、このロケーションから 18.01f パッチをダウンロードし、[通常の手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)を使用してインストールします。

これで、Spectre のセキュリティー制約が修正されたバージョン 18.01f が実行されている状態になりました。

## 完全再ロードの手順
代替方法として、18.01f レベルで VRA の完全再ロードを行うこともできます。

*警告:* この手順では、2 つのパッチをダウンロードしてインストールする中間ステップをスキップできます。ただし、ISO を使用した完全再ロード中に、お客様のデータはすべて失われます。

1. このロケーションから 18.01f ISO を取得します。
2. [通常の手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)に従ってインストールし、リブートします。

これで、Spectre のセキュリティー制約が修正されたバージョン 1801f が実行されている状態になりました。

## 追加のガイダンス

Vyatta 5400 と仮想ルーター・アプライアンスとの間では、機能を 1 対 1 で単純にマッピングできないので、VRA のベースライン構成を作成すると有用です。 IBM© パートナーである WanClouds はこのプロセスのサポートを提供し、Vyatta 5400 の機能に類似した機能を VRA で作成するためのガイダンスを提供しています。

このアップグレード・プロセスにおいて発生する可能性のある一般的な問題については、当社の [追加資料](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)を参照してください。
