---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

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

# Vyatta 5400 から Juniper vSRX または Fortigate Security Appliance (FSA) 10Gbps へのマイグレーション
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Vyatta 5400 デバイスを IBM Virtual Router Appliance にアップグレードできることに加えて、費用効果、信頼性、および安全性の高い Juniper vSRX や Fortigate Security Appliance にマイグレーションすることも可能です。ただし、既存の Vyatta 5400 デバイスを再利用したり、関連付けられた IP アドレスを保持したりすることはできません。

以下の手順では、スタンドアロン Vyatta 5400 デバイス、または高可用性 (HA) ペアで作動する 2 つの Vyatta 5400 デバイスを Juniper vSRX または FSA にアップグレードする方法について説明します。

## スタンドアロン Vyatta 5400 のアップグレード
{: #upgrading-a-stand-alone-vyatta-5400}

ダウン時間を最も短くしながら Juniper vSRX アプライアンスまたは FSA - 10G アプライアンスに単一の Vyatta 5400 をアップグレードするには、以下の手順を実行することをお勧めします。

1. ご使用の 5400 をバックアップし、そのデータを 2 つの異なる場所に保管したことを確認します。これには、必要に応じて SSH 鍵、SSL 証明書、スクリプト、および現在の Vyatta 5400 構成のリカバリーに必要なその他のあらゆるファイルが含まれます。これを行う方法については、[このページの手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)を参照してください。

2. nwom@us.ibm.com に連絡して、構成の変換を要請します。

3. 新しいデバイスを注文します。その際、[Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) または [FSA-10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps)用の説明に従います。

  この機会に、高可用性ソリューションを注文することも検討してください。
  {: note}

4. nwom@us.ibm.com から受け取った構成を、新しく注文したデバイスにロードします。

5. VLAN を新規デバイスにマイグレーションするための時間をスケジュールします。

  この手順で行う場合は、ダウン時間は短くなります。
  {: note}

6. 新規デバイスが正しく機能していることをテストして確認します。

7. サポート・チケットをオープンして Vyatta 5400 デバイスを取り消します。

## Vyatta 5400 高可用性ペアのアップグレード
{: #upgrading-a-vyatta-5400-high-availability-pair}

HA ペア内の 2 つの Vyatta 5400 をアップグレードするには、以下の手順を実行します。

1. ご使用の 5400 をバックアップし、そのデータを 2 つの異なる場所に保管したことを確認します。これには、必要に応じて SSH 鍵、SSL 証明書、スクリプト、および現在の Vyatta 5400 構成のリカバリーに必要なその他のあらゆるファイルが含まれます。これを行う方法については、[このページの手順](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)を参照してください。

2. nwom@us.ibm.com に連絡して、構成の変換を要請します。

3. 新しいデバイスを注文します。その際、[Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) または [FSA-10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps)用の説明に従います。

4. nwom@us.ibm.com から受け取った構成を、新しく注文したデバイスにロードします。

5. VLAN を新規デバイスにマイグレーションするための時間をスケジュールします。

  この手順で行う場合は、ダウン時間は短くなります。
  {: note}

6. 新規デバイスが正しく機能していることをテストして確認します。

7. サポート・チケットをオープンして Vyatta 5400 デバイスを取り消します。
