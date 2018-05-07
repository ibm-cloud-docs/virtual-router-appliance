---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 へのファイアウォール機能の追加 (ステートレスおよびステートフル)

各インターフェースにファイアウォール・ルール・セットを適用することが、Brocade 5400 vRouter (Vyatta) デバイス使用時のファイアウォールの 1 つの方法として挙げられます。各インターフェースには、In、Out、および Local という 3 つの適用可能なファイアウォール・インスタンスがあり、各インスタンスには、適用できるルールがあります。デフォルトのアクションは「ドロップ」で、ルール 1 から N までという方法で特定のトラフィックを適用できるようにするルールを使用します。組み合わせが決まるとすぐに、ファイアウォールはマッチング・ルールの特定のアクションを適用します。

以下の 3 つのファイアウォール・インスタンスのいずれの場合も、適用できるのは **1 つのみ**です。

* **IN:** ファイアウォールは、インターフェースに入るパケットと Brocade システムを通過するパケットをフィルタリングします。管理 (ping、モニタリングなど) のために、特定の SoftLayer IP 範囲がフロントエンドおよびバックエンドにアクセスすることを許可する必要があります。
* **OUT:** ファイアウォールは、インターフェースから出て行くパケットをフィルタリングします。これらのパケットは Brocade システムを通過している場合も、システムを発信元とする場合もあります。
* **LOCAL:** ファイアウォールは、システム・インターフェースを介して、Brocade vRouter システム自体を宛先とするパケットをフィルタリングします。制限されていない外部 IP アドレスから Brocade vRouter デバイスに着信するアクセス・ポートに対して制限を設定する必要があります。

以下のステップを使用して、Brocade 5400 vRouter のインターフェースへの Internet Control Message Protocol (ICMP) *(ping、IPv4 エコー応答メッセージ)* をオフにするサンプル・ファイアウォール・ルールを設定します (これはステートレス・アクションであり、ステートフル・アクションについては後述します)。

1\. コマンド・プロンプトに *show configuration* コマンドを入力して、設定されている構成を確認します。デバイスに設定したすべてのコマンドのリストが表示されます (マイグレーションすることを決定し、すべての構成を確認したい場合に便利です)。*set firewall all-ping 'enable'* コマンドがあることを確認してください。これは、デバイスで ICMP がまだ有効になっていることを示します。

2\. *configure* と入力します。

3\. *set firwall all-ping'disable'* と入力します。

4\. *commit* と入力します。

これで、Brocade 5400 vRouter デバイスに ping を試みても、応答を受け取らなくなります。

経路指定されたトラフィックにファイアウォール・ルールを割り当てるには、Brocade 5400 vRouter の**インターフェース**にルールを適用するか、**ゾーンを作成**して、ゾーンに適用する必要があります。

この例では、これまでに使用されていた VLAN に対してゾーンが作成されます。

**VLAN = ゾーン**

bond1 = dmz

bond1.2007 = prod (実働用)

bond0.2254 = private (開発用)

bond1.1280 = 将来の利用のために予約済み

bond1.1894 = 将来の利用のために予約済み

**ファイアウォール・ルールの作成**

ゾーンが実際に作成される前に、ゾーンに適用されるファイアウォール・ルールを作成することをお勧めします。ゾーンの前にルールを作成すると、ルールを直ちに適用できます。ゾーンを作成してからルールを作成すると、ルールを適用するためにゾーンに戻らなければなりません。

**注:** ゾーンとルール・セットの両方に、デフォルトのアクション・ステートメントがあります。ゾーン・ポリシーを使用する場合、デフォルトのアクションはゾーン・ポリシー・ステートメントによって設定され、ルール 10,000 によって表されます。ファイアウォール・ルール・セットのデフォルト・アクションは、すべてのトラフィックを**ドロップ**することであると覚えておくことも重要です。

以下のコマンドで次の操作が実行されます。

* どのパケットもドロップするデフォルト・アクションを含む **dmz2private** という名前のファイアウォール・ルールを作成します。
* **dmz2private** という名前のルールで、受け入れられたトラフィックと拒否されたトラフィックのロギングを有効にします。


1\. コマンド・プロンプトで *configure* と入力します。

2\. プロンプトで以下のコマンドを入力します。

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

次の一連のコマンドは、確立されたセッションのトラフィックが戻ることを **iptables** が許可できるようにするものです。デフォルトでは、**iptables** はこれを許可しないため、明示的なルールが必要になります。

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

3 番目のコマンド・セットは、TCP/UDP がポート22 (SSH のデフォルト) を通過できるようにします。

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

最後のコマンド・セットは、TCP/UDP がポート80 (HTTP のデフォルト) を通過できるようにします。

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. *commit* と入力して、終了時にすべてのルールが実行されるようにします。

4\. コマンド・プロンプトで *show firewall name dmz2private* と入力して、構成を表示します。

次に作成するファイアウォール・ルールが **dmz** ゾーンに適用されます。ルールは **public** という名前になります。プロンプトで以下のコマンドを入力します。

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**注:** ファイアウォール・ルールは、**prod** を介して **dmz** にアウトバウンドで流れる必要があります。ネットワーク・フローのトラブルシューティングには、コマンド *sudo tcpdump -i any host 10.52.69.202* を使用します。

**ゾーンの作成**

ゾーンは、インターフェースの論理表現です。以下のコマンドで次の操作が実行されます。

* ゾーンを作成し、このゾーンを宛先とするパケットをドロップするデフォルト・アクションを含む **dmz** という名前のポリシーを作成する。
* **bond1** インターフェースを使用するように **dmz** ポリシーを設定する。
* **bond1.2007** インターフェースを使用するように **prod** ポリシーを設定する。
* このゾーンを宛先とするパケットをドロップするデフォルト・アクションを含む **private** という名前のゾーン・ポリシーを作成する。
* **bond0.2254** インターフェースを使用するように **private** という名前のポリシーを設定する。

1\. プロンプトで以下のコマンドを入力します。

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. 以下のコマンドを使用して、ゾーンのファイアウォール・ポリシーを設定します。

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

特定のファイアウォール・ルールをゾーン・ポリシーに適用したくない場合は、そのルールを特定のインターフェースに適用できます。以下のコマンドを使用して、特定のルールをインターフェースに適用します。

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## ステートフル・ファイアウォール

*ステートフル*・ファイアウォールは以前に確認されたフローのテーブルを保持します。前のパケットとの関係に従ってパケットを受け入れるかドロップすることができます。原則として、アプリケーション・トラフィックが多い場所では、通常はステートフル・ファイアウォールが優先されます。 

<span style="text-decoration: underline">*Brocade 5400 vRouter は、デフォルト構成での接続の状態を追跡しません。ファイアウォールは、以下のいずれかの条件が満たされるまでステートレスです。*</span>

* ファイアウォールの構成、およびいずれかのルールで状態パラメーターが設定されている
* ファイアウォール状態ポリシーの構成
* NAT の構成
* 透過的 Web プロキシー・サービスの有効化
* WAN ロード・バランシング構成の有効化

## ステートレス・ファイアウォール

*ステートレス*・ファイアウォールは、すべてのパケットが分離しているものと見なします。IP または Transmission Control Protocols/User Datagram Protocol (TCP/UDP) ヘッダー内のソース・フィールドおよび宛先フィールドなどの、基本的なアクセス制御リスト(ACL) 基準のみに従って、パケットを受け入れるかドロップすることができます。ステートレス Brocade 5400 vRouter は接続情報を保管せず、前のフローとの間のすべてのパケットの関係を検索する必要はありません (これらはどちらも少量のメモリーと CPU 時間を消費します)。したがって、未加工転送のパフォーマンスは、ステートレス・システムで最高になります。Brocade では、ステートフルな状態に固有の機能を必要としない場合、最高のパフォーマンスを得るためにルーターをステートレスな状態に維持することをお勧めします。
