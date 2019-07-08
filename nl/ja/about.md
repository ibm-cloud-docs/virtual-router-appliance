---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

keywords: vra, about, firewall, VPN, NAT, Routing

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

# VRA について
{: #about-the-vra}

IBM© Virtual Router Appliance (VRA) は、**x86** ベアメタル・サーバーに対応した最新の Vyatta 5600 オペレーティング・システムを提供します。 高可用性 (HA) 構成またはスタンドアロン構成として提供されます。

Virtual Router Appliance を使用すると、ファイアウォール、トラフィック・シェーピング、ポリシーベースの経路指定、 VPN、および他のフィーチャーを備えたフル装備のエンタープライズ・ルーターを介して、プライベートおよびパブリックのネットワーク・トラフィックを選択的に経路指定できます。 VRA は容易な構成でパフォーマンスを提供します。 また、通常のハードウェア・サーバーで実行する場合のメンテナンスでの利点を実現します。 VRA ハードウェア・アプライアンスは、複数の VLAN に対応するルーティング・ロードを処理するようにサイズ変更され、冗長ネットワーク・リンクおよび冗長 RAID アレイと共に注文することができます。 すべての VRA フィーチャーはお客様により管理されます。

**代替案:** FortiGate Security Appliance (FSA) 10 Gbps は、AntiVirus (AV)、Intrusion Prevention (IPS)、および Web フィルタリングなど、次世代の機能を備えた、単一テナント (専用) でハイ・スループット (10 Gbps) のハードウェア・ファイアウォールです。 同様の目的を達成するために、VRA に代わるものとすることができます。 詳しくは、[FSA の資料](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started)を参照してください。

## ファイアウォール
{: #firewall}

外部の脅威から環境を保護するために、Virtual Router Appliance をファイアウォールとして利用できます。 アプリケーションが実行されているポートへのインバウンドまたはアウトバウンドのネットワーク・トラフィックを許可または拒否するファイアウォール・ルールを追加できます。また、独自のネットワーク内でトラフィックをフィルタリングすることができます。 Virtual Router Appliance は、重要なデータを保護するために、ステートフル IPv4 および IPv6 フィルター操作を実行するように構成することもできます。

## 仮想プライベート・ネットワーク (VPN) ゲートウェイ
{: #virtual-private-network-vpn-gateway}

Virtual Router Appliance をネットワーク・ゲートウェイ・デバイスとしてプロビジョンすることにより、VPN トンネリングを使用して、オンサイト・データ・センターまたはオフィスを IBM Cloud に接続します。 IPsec サイト間 VPN トンネルを使用して、エンタープライズ・データ・センターまたはオフィスから IBM Cloud ネットワークへのセキュアな通信を行うことができます。 その他の VPN オプションは、リモート・アクセス IPsec VPN (クライアントからサイト)、OpenVPN、GRE、L2TP、および DMVPN です。

[『Supplemental VRA documentation』セクション](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation)内の Brocade VPN 構成ガイドを参照してください。

## ネットワーク・アドレス変換 (NAT)
{: #network-address-translation-nat-}

ご使用のサーバーが送信元 NAT を使用したインターネット・アクセスを依然として許可している場合でも、Virtual Router Appliance を使用して、パブリック・ネットワーク・インターフェースなしでアプリケーション・サーバーとデータベース・サーバーをプロビジョンすることができます。 拡張セキュリティーのために、宛先 NAT を使用してゲートウェイ・デバイスの背後にあるサーバーを非表示にすることもできます。

## エンタープライズ・レベルの経路指定
{: #enterprise-grade-routing}

異なる分離ネットワーク上の多階層アプリケーションの場合、Virtual Router Appliance は、これらのネットワーク間に接続を構築するための柔軟な方法を提供します。 BGP を使用して動的ルーティングをセットアップできます。これにより、IBM Cloud ルーター上の独自のパブリック IP スペースを公表できます。 また、BGP により、さまざまなトンネルおよび直接リンク・ソリューションを使用する場合に、より柔軟にカスタム・プライベート・ネットワーク構成を行うことができます。

[『Supplemental VRA documentation』セクション](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation)内の Brocade BGP 構成ガイドを参照してください。
