---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Vyatta での NAT ルールのセットアップ
このトピックでは、Vyatta で使用される NAT (ネットワーク・アドレス変換) ルールの例を示します。

## 1 対多 NAT ルール (マスカレード)

プロンプトに以下のコマンドを入力します。

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

`10.xxx.xxx.xxx` ネットワーク内のマシンからの接続要求は、bond1 上の IP にマップされ、発信される際にはそれに関連付けられている一時ポートを受け取ります。 1 対多マスカレード・ルール番号に大きな数を割り当てるのは、ユーザーが保有している可能性のある小さい番号の NAT ルールと競合しないようにするためです。

**注:** VRA を介してインターネット・トラフィックを渡すようにサーバーを構成して、デフォルト・ゲートウェイが管理対象仮想 LAN (VLAN) のプライベート IP アドレスであるようにする必要があります。 例えば、`bond0.2254` の場合、ゲートウェイは `10.52.69.201` です。 これは、インターネット・トラフィックを渡すサーバーのゲートウェイ・アドレスである必要があります。

**注:** NAT のトラブルシューティングには以下のコマンドを使用します。 

'''
run show nat source translations detail 
'''

## 1 対 1 NAT ルール

以下のコマンドは、1 対 1 NAT ルールのセットアップ方法を示しています。 ルール番号がマスカレード・ルールより小さくなるようセットアップされていることに注意してください。 これは、1 対 1 ルールが 1 対多ルールよりも優先されるようにするためです。

**注:** 1 対 1 でマップされる IP アドレスをマスカレードにすることはできません。 IP インバウンドを変換する場合、トラフィックが両方向になるためには、その IP アウトバウンドを変換する必要があります。

以下のコマンドは、ソースおよび宛先のルール用です。 NAT ルール・タイプを表示するには、構成モードで `show nat` と入力します。

**注:** NAT のトラブルシューティングには、コマンド `run show nat source translations detail` を使用します。 

構成モードであることを確認した後、プロンプトに以下のコマンドを入力します。

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

トラフィックが bond1 上の IP `50.97.203.227` に着信する場合、その IP は (定義された任意のインターフェース上の) IP `10.52.69.202` にマップされます。 トラフィックが (定義された任意のインターフェース上の) IP `10.52.69.202` で発信される場合、IP `50.97.203.227` に変換され、インターフェース bond1 上でアウトバウンドに進みます。

**注:** NAT のトラブルシューティングには、コマンド `run show nat source translations detail` を使用します。

## VRA を介した IP 範囲の追加

ご使用の VRA 構成によっては、特定の IBM Cloud IP アドレスを受け入れることができます。 

新しい vRouter デプロイメントには、`SERVICE-ALLOW` という名前のファイアウォール・ルールに定義された、IBM Cloud のサービス・ネットワーク IP アドレスが付属しています。

`SERVICE-ALLOW` の例を以下に示します。 これは、完全なプライベート IP ルール・セットではありません。

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

ファイアウォール・ルールの定義が終わったら、適切と思われる方法でそれらを割り当てることができます。 以下に 2 つの例を示します。 

ゾーンへの適用:

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

結合インターフェースへの適用:

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`

**注:**

* 1 対 1 でマップされる IP アドレスをマスカレードにすることはできません。 IP インバウンドを変換する場合、トラフィックが両方向になるためには、その同じ IP アウトバウンドを変換する必要があります。

* NAT のトラブルシューティングには、コマンド `run show nat source translations detail` を使用します。

