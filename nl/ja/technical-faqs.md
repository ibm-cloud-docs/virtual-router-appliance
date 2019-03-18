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
{:faq: data-hd-content-type='faq'}

# IBM 仮想ルーター・アプライアンスの技術的な FAQ
{: #technical-faqs-for-ibm-virtual-router-appliance}

以下のよくある質問は、IBM© 仮想ルーター・アプライアンス (VRA) の構成と、Vyatta 5400 から VRA へのマイグレーションに関するものです。

## プライベート VLAN 上にあるホストからインターネットへのトラフィックを許可するにはどうすればよいですか？
{:faq}

このトラフィックは、パブリック・ソース IP を取得する必要があります。したがって、ソース NAT は、プライベート IP を VRA のパブリック IP でマスカレードする必要があります。

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

上記の構成は、プライベート `10.0.0.0/8` ネットワーク内のサーバーが発信元のトラフィックからの SNAT のみを実行します。

これにより、インターネット転送可能なソース・アドレスを既に持っているパケットを阻害しないことが保証されます。

## インターネットへのトラフィックをフィルタリングし、特定のプロトコル/宛先のみを許可するにはどうすればよいですか?
{:faq}

これは、ソース NAT とファイアウォールを結合する必要がある場合によくある質問です。

ルール・セットを設計するときには、VRA での操作の順序に留意してください。

簡単に言うと、ファイアウォール・ルールは SNAT の*後に*適用されます。

すべての発信トラフィックをファイアウォールでブロックするが、特定の SNAT フローを許可するには、フィルタリング・ロジックを SNAT に移動する必要があります。

例えば、あるホストでのインターネットへの HTTPS トラフィックのみを許可するには、SNAT 規則は次のようになります。

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` は、VRA のパブリック・アドレスです。 

ホストと VRA のパブリック・トラフィックを区別できるように、VRA の VRRP パブリック・アドレスを使用することを強くお勧めします。

`150.1.2.3` が VRRP VRA アドレスであり、`150.1.2.5` が実際の dp0bond1 アドレスであると想定します。

この場合、`dp0bond1 out` に適用されるステートフル・ファイアウォールは次のようになります。

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

必要な設計目標はソース NAT とファイアウォールの組み合わせによって達成されます。 

必ず、ルールが設計に合うものであること、および、ブロックする必要があるトラフィックを許可するような他のルールがないようにしてください。 

## ゾーン・ベースのファイアウォールで VRA 自体を保護するにはどうすればよいですか?
{:faq}

VRA には `local zone` はありません。

Control Plane Policing (CPP) 機能は `local` ファイアウォールとしてループバックに適用されるため、代わりにこの機能を利用できます。

これはステートレス・ファイアウォールであり、VRA 自体が発信元であるアウトバウンド・セッションのトラフィックを返すことを明示的に許可する必要があることに注意してください。

## SSH を制限し、インターネットからの接続をブロックするにはどうすればよいですか？
{:faq}

インターネットからの SSH 接続を許可しないこと、および、プライベート・アドレスへの別のアクセス手段 (SSL VPN など) を使用することが、ベスト・プラクティスであると考えられます。

デフォルトでは、VRA はすべてのインターフェースで SSH を受け入れます。
プライベート・インターフェースでのみ SSH 接続を listen するには、以下の構成を設定する必要があります。

```
set service ssh listen-address '10.1.2.3'
```

IP アドレスは VRA に属しているアドレスで置き換える必要があることに注意してください。
