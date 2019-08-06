---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# Vyatta 5400 のアップグレードおよびその IP アドレスの再利用
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

このオプションを使用すると、既存の Vyatta 5400 デバイスを同等の {{site.data.keyword.vra_full}} (VRA) として再利用し、関連付けられた IP アドレスを保持することができます。以下の手順では、スタンドアロン Vyatta 5400 デバイス、または高可用性 (HA) ペアで作動する 2 つの Vyatta 5400 デバイスをアップグレードする方法について説明します。

## スタンドアロン Vyatta 5400 のアップグレード
{: #upgrading-a-stand-alone-vyatta-5400}

単一の Vyatta 5400 を {{site.data.keyword.vra_full}} にアップグレードするには、以下の手順を実行します。

1. ご使用の 5400 をバックアップし、そのデータを 2 つの異なる場所に保管したことを確認します。これには、必要に応じて SSH 鍵、SSL 証明書、スクリプト、および現在の Vyatta 5400 構成のリカバリーに必要なその他のあらゆるファイルが含まれます。
2. [このトピック](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os)の説明に従って、Vyatta 5400 をデフォルトの {{site.data.keyword.vra_full}} に再ロードします。

  OS の再ロードにより、root ユーザー ID と Vyatta ユーザー ID の新規パスワードが生成されます。{: note}

4. 新しい {{site.data.keyword.vra_full}} のパスワードをメモします。
5. [このページの手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)を使用して、新規に再ロードされた VRA を必要な設定で構成します。

## Vyatta 5400 高可用性ペアのアップグレード
{: #upgrading-a-vyatta-5400-high-availability-pair}

HA ペア内の 2 つの Vyatta 5400 をアップグレードするには、以下の手順を実行します。

1. バックアップ Vyatta 5400 デバイスを特定し、上記の手順を使用して、そのデバイスを {{site.data.keyword.vra_full}} として最初に再ロードします。

  機能 HA 構成では、すべてのインターフェースが特定のノード上で BACKUP または MASTER として示されるのが理想的です。判別できない場合は、`show vrrp` コマンドを使用して確認してください。 {: note}
2. [このページの手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)を使用して、新規に再ロードされた VRA を必要な設定で構成します。
3. マスター Vyatta 5400 とバックアップ {{site.data.keyword.vra_full}} は VRRP ペアになります。VRRP コマンド `reset VRRP master` を使用して、現在のバックアップ {{site.data.keyword.vra_full}} がマスターになるように変更します。

  このアクションにより、既存のセッションで停止が発生し、セッション状態が「Lost」と示されます。既存の NAT セッションもすべてリセットされます。{: note}

5. 新しいマスター {{site.data.keyword.vra_full}} が適切に機能していることを確認します。
6. 現在のバックアップ Vyatta 5400 で上記の再ロード手順を実行して、VRA にアップグレードします。
7. 2 回目に再ロードした後、[このページの手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)を使用して、必要に応じてバックアップ {{site.data.keyword.vra_full}} を構成します。
