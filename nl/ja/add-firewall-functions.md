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

# {{site.data.keyword.vra_full}} へのファイアウォール機能の追加 (ステートレスおよびステートフル)
各インターフェースにファイアウォール・ルール・セットを適用することが、{{site.data.keyword.vra_full}} (VRA) 使用時のファイアウォールの 1 つの方法として挙げられます。 各インターフェースには、In、Out、および Local という 3 つの適用可能なファイアウォール・インスタンスがあり、各インスタンスには、適用できるルールがあります。 

デフォルトのアクションは「ドロップ」で、特定のトラフィックを許可するルールを、ルール 1 から N までというように適用していきます。一致するとすぐに、ファイアウォールは一致したルールの特定のアクションを適用します。

以下の 3 つのファイアウォール・インスタンスのいずれの場合も、適用できるのは 1 つのみです。

**IN:** ファイアウォールは、インターフェースに入ってきて、かつシステムを通過するパケットをフィルタリングします。 管理 (ping、モニタリングなど) のために、特定の IP 範囲がフロントエンドおよびバックエンドにアクセスすることを許可する必要があります。

**OUT:** ファイアウォールは、インターフェースから出ていくパケットをフィルタリングします。 これらのパケットは、VRA システムを通過している場合も、そのシステムを発信元とする場合もあります。

**LOCAL:** ファイアウォールは、システム・インターフェースを使用する、VRA システム自体を宛先とするパケットをフィルタリングします。 制限されていない外部 IP アドレスからデバイスに着信するアクセス・ポートに対して制限を設定する必要があります。

以下のステップを使用して、{{site.data.keyword.vra_full}} のインターフェースへの Internet Control Message Protocol (ICMP) (IPv4 エコー応答メッセージであるping) をオフにするサンプル・ファイアウォール・ルールを設定します (これはステートレス・アクションであり、ステートフル・アクションについては後述します)。

1. コマンド・プロンプトに `show configuration commands` と入力して、設定されている構成を確認します。 デバイスに設定したすべてのコマンドのリストが表示されます (マイグレーションすることを決定し、すべての構成を確認したい場合に便利です)。 `set firewall all-ping enable` コマンド があることを確認してください。これは、デバイスで ICMP がまだ有効になっていることを示します。

2. `configure` と入力します。

3. `set firwall all-ping disable` と入力します。

4. `commit` と入力します。

これで、VRA デバイスに ping を試みても、応答を受け取らなくなります。

経路指定されたトラフィックにファイアウォール・ルールを割り当てるには、VRA のインターフェースにルールを適用するか、ゾーンを作成して、ゾーンに適用する必要があります。

この例では、これまでに使用されていた VLAN に対してゾーンが作成されます。

 VLAN | ゾーン 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (実動用)
bond0.2254 | private (開発用)
bond1.1280 | 将来の利用のために予約済み
bond1.1894 | 将来の利用のために予約済み

## ファイアウォール・ルールの作成
ゾーンが実際に作成される前に、ゾーンに適用されるファイアウォール・ルールを作成することをお勧めします。 ゾーンの前にルールを作成すると、ルールを直ちに適用できます。ゾーンを作成してからルールを作成すると、ルールを適用するためにゾーンに戻ることになります。

以下のコマンドで次の操作が実行されます。

* どのパケットもドロップするデフォルト・アクションを含む `dmz2private` という名前のファイアウォール・ルールを作成します。
* `dmz2private` という名前のルールで、受け入れられたトラフィックと拒否されたトラフィックのロギングを有効にします。

1. コマンド・プロンプトで `configure` と入力します。

2. 以下のコマンドを入力します。

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. 次の一連のコマンドを入力して、確立されたセッションのトラフィックが戻ることを iptables が許可するようにします。 デフォルトでは、iptables はこれを許可しないため、明示的なルールが必要になります。

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. 以下のコマンドを入力して、TCP/UDP がポート22 (SSH のデフォルト) を通過できるようにします。
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. 以下のコマンドを入力して、TCP/UDP がポート80 (HTTP のデフォルト) を通過できるようにします。

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. `commit` と入力して、終了時にすべてのルールが実行されるようにします。

7. コマンド・プロンプトで `show firewall name dmz2private` と入力して、新しい構成を表示します。

8. プロンプトで以下のコマンドを入力して、dmz ゾーンに適用されるファイアウォール・ルールを作成します。 ルールは public という名前になります。 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## ゾーンの作成

ゾーンは、インターフェースの論理表現です。 このセクションでは、以下を実行します。

* ゾーンを作成し、このゾーンを宛先とするパケットをドロップするデフォルト・アクションを含む dmz という名前のポリシーを作成する。
* `bond1` インターフェースを使用するように dmz ポリシーを設定する。
* `bond1.2007` インターフェースを使用するように prod ポリシーを設定する。
* このゾーンを宛先とするパケットをドロップするデフォルト・アクションを含む `private` という名前のゾーン・ポリシーを作成する。
* `bond0.2254` インターフェースを使用するように `private` という名前のポリシーを設定する。

1. プロンプトで以下のコマンドを入力します。

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. 以下のコマンドを使用して、ゾーンのファイアウォール・ポリシーを設定します。

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
特定のファイアウォール・ルールをゾーン・ポリシーに適用したくない場合は、そのルールを特定のインターフェースに適用できます。 以下のコマンドを使用して、特定のルールをインターフェースに適用します。

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
