---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# マイグレーションの概要
{: #migration-overview}

Vyatta 5400 は、2019 年 3 月 31 日時点でサポート対象外となるため、従来の Vyatta 5400 のお客様は、できるだけ早く IBM© Virtual Router Appliance (VRA) にマイグレーションする必要があります。サポート終了に関する発表の全文およびその詳細については、[ここ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)をクリックしてください。
{: important}

## アップグレードによるメリット
{: #how-upgrading-can-benefit-you}

IBM の Virtual Router Appliance では、さまざまな新しいフィーチャーおよび機能が導入されたと同時に、いくつかの機能改善も行われています。詳しくは、[FAQ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) を参照してください。

新しいデバイスにアップグレードすると、Vyatta 5400 とは異なる構成になります。そのため、Vyatta 5400 構成 (ファイル) は新しいアプライアンスには転送されません。
{: note}

## マイグレーションの資料
{: #migration-documentation}

Vyatta 5400 からのマイグレーションを支援するために、以下の資料およびサポートが用意されています。

| マイグレーションの資料 | 説明 |
| ------------- | ------------- |
| [Vyatta 5400 のアップグレードおよびその IP アドレスの再利用](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Vyatta 5400 の IP アドレスを再利用しながら、Vyatta 5400 を同等の IBM Virtual Router Appliance にアップグレードする方法についての説明。 |
| [Vyatta 5400 から Juniper vSRX または Fortigate Security Appliance (FSA) へのマイグレーション](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Juniper vSRX または Fortigate Security Appliance へのマイグレーションに関する説明。 このオプションでは、既存の Vyatta 5400 デバイスを再利用したり、関連付けられた IP アドレスを保持したりすることはできません。|
| [一般的なマイグレーション問題](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Vyatta 5400 デバイスから IBM Virtual Router Appliance へのマイグレーション後に生じる可能性のある、最も頻繁に発生する問題または動作変更に関する情報。多くの場合、問題に対処するための回避策が含まれています。|

新しい Virtual Router Appliance を注文するだけでよい場合は、[ここをクリックして](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)詳細を確認してください。
{: note}
