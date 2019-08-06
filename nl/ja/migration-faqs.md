---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Vyatta 5400 のサポート終了に関する FAQ
{: #vyatta-5400-end-of-support-faqs}

以下は、Vyatta 5400 からマイグレーションする際のよくある質問です。

## Vyatta 5400 の「サポート終了」(EoS) はなぜ 2019 年 3 月 31 日なのですか?
{: faq}

2017 年 9 月、IBM のサポート・ライフサイクル・ポリシーに基づいて、従来の Vyatta 5400 の EoS が 2018 年 2 月 20 日と発表されました。これは、次期バージョンの {{site.data.keyword.vra_full}} (VRA) の一般出荷可能日 (GA) である 7 月の日付から 6 カ月後に当たります。

お客様のマイグレーション・スケジュールを考慮し、Vyatta 5400 の EoS 日付は 2019 年 3 月 31 日まで延長されました。Debian 7 ソフトウェアが Debian オープン・ソース・コミュニティーでサポートされなくなったため、AT&T によるベンダー・サポートを延長するための今後の計画はありません。

[ここをクリックして、](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)サポート終了の正式な発表をご確認ください。
{: important}

## 「サポート終了」は顧客にとってどのような意味がありますか。
{: faq}

サポート終了日以降、A&T はコード・パッチを提供したり、IBM からサポート・エスカレーションを受け入れたりすることがなくなります。

同様に、IBM Cloud サポートは、Vyatta 5400 デプロイメントにおける構成やネットワーキングの問題をトラブルシューティングしなくなります。サポートは、ハードウェア・レベルの要求 (ハード・ディスクや RAM など)、電源、およびアウト・オブ・バンド (IPMI) 接続に限定されます。

お客様は、{{site.data.keyword.vra_full}} (:VRA。Vyatta 5600 をベースとする) や Juniper vSRX などの代替ソリューションへのマイグレーション作業にすぐに着手することをお勧めします。[ここをクリックして](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)開始してください。

## 3 月 31 日より後に Vyatta 5400 を使用して IBM Cloud ワークロードをまだ実行している場合はどうなりますか?
{: faq}

Vyatta 5400 は 3 月 31 日後も機能します。ただし、Vyatta 5400 ソフトウェアの潜在的な脆弱性により、お客様のビジネス環境とアプリケーション環境は、潜在的なセキュリティーの脅威やその他のデータ改ざんの違反行為にさらされるおそれがあります。

ビジネス環境とアプリケーション環境に悪影響を与えるネットワークの問題が発生し、根本原因が Vyatta 5400 にあることを突き止めた場合、IBM からも AT&T からもサポートが提供されなくなるため、その問題は 5400 のオファリング・マネージャーにエスカレーションしてください。オファリング管理チームに連絡するには `nwom@us.ibm.com` までメールをお送りください。

## 基礎となるベアメタル・サーバーのハードウェアについては、引き続きサポートされますか?
{: faq}

ハードウェアの交換はサポートされていますが、問題が Vyatta OS に関連していることがトラブルシューティングにより示された場合は、サポートされるハードウェア・オファリングに直ちにマイグレーションするよう指示されます。[ここをクリックして](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)開始してください。

## Vyatta 5400 を所有する顧客として、2019 年 3 月 31 日までに何をする必要がありますか?
{: faq}

Vyatta 5400 をお持ちのお客様は、VRA (Vyatta 5600)、Juniper vSRX、または Fortigate Security Appliance (FSA) 10G のいずれかにマイグレーションする必要があります。VRA (Vyatta 5600) はまだ完全にサポートされています。IBM Cloud または AT&T による VRA の現時点でのサポート終了日または計画されたサポート終了日はありません。[ここをクリックして](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)開始してください。

  VRA と vSRX はお客様が管理するデバイスです。
　{: note}

## Vyatta 5400 から VRA、vSRX、または FSA 10G にマイグレーションする場合、IBM からのサポートはありますか?
{: faq}

Vyatta 5400 から VRA (5600) への構成移行サービスが引き続き使用可能です。

* 既存のお客様向けに IBM Cloud では、既存の Vyatta 5400 構成を {{site.data.keyword.vra_full}} (VRA)、Juniper vSRX、または Fortigate Security Appliance (FSA) 10G の各フォーマットにリファクタリングできるよう支援する無料のオファリングを提供しています。構成移行サービスのリクエストを送信するには、「`Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx. `」という件名のメールを nwom@us.ibm.com まで送信してください。

  件名内の `aaaaaaaa` には選択したアプリケーション ({{site.data.keyword.vra_full}} 、Juniper vSRX、または Fortigate Security Appliance (FSA) 10G) を、`xxxxxx` には固有のアカウント番号を必ず入力してください。
  {: note}

* この変換移行プロセスのパートナーである Wanclouds は、数百件のマイグレーションの取り組みに成功しています。Wanclouds では、既存の Vyatta 5400 を変換して、Vyatta 5600 プラットフォームで同様の機能を作成します。以下で説明するように、Wanclouds はそのサービスを 2 つの層で提供します。

  <img src="images/tiers.png" alt="図" style="width: 700px;"/>

[ここをクリックして](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)開始してください。

## Vyatta 5400 からマイグレーションする際に IBM ビジネス・パートナーから利用可能なその他の有料のマイグレーション・サービスはありますか?
{: faq}

Vyatta 5400 のマイグレーションに対する有償サポートを提供するビジネス・パートナーは複数存在します。詳しくは、[ここをクリックしてください](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)。

## Vyatta 5400 マイグレーションに関する質問を出すことができる、Vyatta 5400 オファリング管理サポートの窓口は IBM 内にありますか?
{: faq}

ご質問は IBM Vyatta 5400 および VRA ネットワーク・オファリング管理 (`nwom@us.ibm.com`) までお送りください。また、IBM Watson Cloud Platform ワークスペースで Slack を使用して (`#vyatta-migration` を指定) 問い合わせいただくともできます。

## このマイグレーションに役立つリソースは他にどのようなものがありますか?
{: faq}

詳しくは、以下の {{site.data.keyword.vra_full}} の資料リソースを参照してください。

  * [{{site.data.keyword.vra_full}} の概要](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [VRA について](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Vyatta 5400 のマイグレーションの概要](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
