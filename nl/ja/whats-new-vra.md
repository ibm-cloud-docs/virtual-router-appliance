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


# IBM 仮想ルーター・アプライアンスの最新の更新
{: #recent-updates-for-ibm-virtual-router-appliance}

以下では、IBM© 仮想ルーター・アプライアンス (Virtual Router Appliance: VRA) の新機能および更新された機能について説明します。

## 2018 年 8 月
### Brocade オペレーティング・システム バージョン 18.x
仮想ルーター・アプライアンスでは、Brocade OS のバージョン 18.x が使用できるようになりました。 様々な新機能に加え、このバージョンは、Spectre セキュリティー・ブリーチの修復を提供します。 

18.x VRA の新機能については、以下のトピックで説明します。

* [ゾーン・ファイアウォールで機能する IPsec トンネルのセットアップ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [IPsec およびゾーン・ファイアウォールに対する VFP インターフェースの構成](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [プレフィックス・ベースの IPsec での NAT の使用](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [VFP インターフェースのトラブルシューティング](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

Vyatta 5400 から移行する場合、18.x にアップグレードする最善の方法は、[通常の手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)、すなわち OS の完全な再ロードです。

Vyatta 5400 と仮想ルーター・アプライアンスとの間では、機能を 1 対 1 で単純にマッピングできないので、VRA のベースライン構成を作成すると有用です。 IBM パートナーである WanClouds はこのプロセスのサポートを提供し、Vyatta 5400 の機能に類似した機能を VRA で作成するためのガイダンスを提供しています。

このアップグレード・プロセスで発生する可能性のある問題の詳細については、[追加資料](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)を参照してください。


