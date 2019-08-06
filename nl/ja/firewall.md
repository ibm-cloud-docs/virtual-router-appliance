---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: firewall, manage, stateless, stateful, alg, firewall, rules, CPP, Logging

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

# IBM ファイアウォールの管理
{: #manage-your-ibm-firewalls}

{{site.data.keyword.vra_full}} (VRA) は、デバイスを経由して経路指定される VLAN を保護するファイアウォールのルールを処理することができます。 VRA 内のファイアウォールは、次の 2 つのステップに分けることができます。

1. 1 つ以上のルール・セットの定義。
2. インターフェースまたはゾーンへのルール・セットの適用。 1 つのゾーンは 1 つ以上のネットワーク・インターフェースで構成されます。

ファイアウォールのルールが意図したとおりに機能し、新しいルールがデバイスへの管理アクセスを制限しないようにするために、ルールを作成後にテストすることが重要です。

`dp0bond1` インターフェースでルールを操作中に `dp0bond0` を使用してデバイスに接続することをお勧めします。 Intelligent Platform Management Interface (IPMI) を使用してコンソールに接続することもオプションです。

## ステートレスとステートフル
{: #stateless-vs-stateful}

デフォルトでは、ファイアウォールはステートレスですが、必要な場合はステートフルとして構成することができます。 ステートレス・ファイアウォールは両方向のトラフィックのルールを必要としますが、ステートフル・ファイアウォールは接続を追跡し、受け入れられたフローの戻りトラフィックを自動的に許可します。 ステートフル・ファイアウォールを構成するには、どのルールをステートフルに操作するかを指示する必要があります。

`tcp`、`udp`、または `icmp` の各トラフィックの「ステートフル」トラッキングを有効にするには、以下のコマンドを実行します。

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

`global-state-policy` コマンドは、対応するプロトコルを明示的に設定するファイアウォール・ルールに一致したトラフィックの状態のみを追跡することに注意してください。 以下に例を示します。

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

`GLOBAL_STATELESS` が `protocol tcp` を指定していないため、`global-state-policy tcp` コマンドはこのルールに適用されません。

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

この場合、`protocol tcp` が明示的に定義されます。 `global-state-policy tcp` コマンドは、`GLOBAL_STATEFUL_TCP` のルール 1 に一致するトラフィックのステートフル・トラッキングを有効にします。


個々のファイアウォール・ルールを「ステートフル」にするには、以下のようにします。

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
これにより、`global-state-policy` コマンドが存在するかどうかに関係なく、ステートフルな追跡が可能な、`TEST` のルール 1 に一致するすべてのトラフィックのステートフル・トラッキングが有効になります。

## 補助されるステートフル・トラッキング用の ALG
{: #alg-for-assisted-stateful-tracking}

FTP などのいくつかのプロトコルは、通常のステートフル・ファイアウォール操作が追跡できる、より複雑なセッションを利用します。
これらのプロトコルをステートフルに管理できるようにする、事前構成されたモジュールがあります。

これらの ALG モジュールは、それぞれのプロトコルを正常に使用するために必要でない限り、使用不可にすることをお勧めします。

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## ファイアウォールのルール・セット
{: #firewall-rule-sets}

ファイアウォールのルールは、複数のインターフェースにルールを簡単に適用させるために、指定されたセットにグループ化されます。 各ルール・セットには、デフォルトのアクションが関連付けられています。 以下の例を考慮してください。
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1
set security firewall name ALLOW_LEGACY rule 2 action drop
set security firewall name ALLOW_LEGACY rule 2 destination port 23
set security firewall name ALLOW_LEGACY rule 2 log
set security firewall name ALLOW_LEGACY rule 2 protocol tcp
set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

ルール・セット `ALLOW_LEGACY` では、2 つのルールが定義されています。 最初のルールは、`network-group1` という名前のアドレス・グループから送信されるトラフィックをすべてドロップします。 2 番目のルールでは、`network-group2` というアドレス・グループから Telnet ポート (`tcp/23`) に送信されるトラフィックを破棄してログを記録します。 default-action は、その他すべてのことが受け入れられることを示します。

## データ・センターへのアクセスを許可
{: #allowing-data-center-access}

IBM© は、データ・センター内で実行中のシステムに対してサービスおよびサポートを実現するために、複数の IP サブネットを提供します。 例えば、DNS リゾルバー・サービスは `10.0.80.11` と `10.0.80.12` で実行中です。 他のサブネットはプロビジョニングとサポート中に使用されます。 データ・センターで使用されている IP 範囲は、[このトピック](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges)に記載されています。

ファイアウォールのルール・セットの初めに `accept` のアクションを指定して、適切な `SERVICE-ALLOW` ルールを設定することで、データ・センターへのアクセスを可能にすることができます。 ルール・セットが適用される必要がある場所は、実装されているルーティングとファイアウォール設計により異なります。

ファイアウォール・ルールを作業の重複が最小になるロケーションに設定することをお勧めします。 例えば、`dp0bond0` でのバックエンド・サブネットのインバウンドを許可すると、各 VLAN 仮想インターフェースに対してバックエンド・サブネットのアウトバウンドを許可するよりも動作が少なくなります。

### インターフェースごとのファイアウォール・ルール
{: #per-interface-firewall-rules}

VRA にファイアウォールを構成する 1 つの方法として、ファイアウォールのルール・セットを各インターフェースに適用する方法があります。 この場合、インターフェースはデータ・プレーン・インターフェース (`dp0s0`) または仮想インターフェース (`dp0bond0.303`) にすることができます。 各インターフェースには 3 個のファイアウォール割り当てがあります。

`in` - ファイアウォールは、インターフェースを介して入るパケットに対して検査されます。 これらのパケットは、VRA を通過、または VRA を宛先とすることができます。

`out` - ファイアウォールは、インターフェースを介して出ていくパケットに対して検査されます。 これらのパケットは、VRA を通過、または VRA を送信元とすることができます。

`local` - ファイアウォールは、VRA を直接宛先とするパケットに対して検査されます。

1 つのインターフェースには、各方向の複数のルール・セットを適用することができます。 これらは構成された順序で適用されます。 インターフェースごとのファイアウォールを複数使用して VRA デバイスから発信されるトラフィックにファイアウォールを適用することはできないことに注意してください。

例として、`ALLOW_LEGACY` ルール・セットを `bp0s1` インターフェースの `in` オプションに割り当てるには、次の構成コマンドを使用します。

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
{: #control-plane-policing-cpp-}

Control Plane Policing (CPP) は、対象のインターフェースに割り当てられるファイアウォール・ポリシーの構成をユーザーに許可し、このポリシーを VRA に入るパケットに適用することで、{{site.data.keyword.vra_full}} を攻撃から保護します。

CPP は、キーワード `local` が各種の VRA インターフェース (データ・プレーン・インターフェースやループバックなど) に割り当てられたファイアウォール・ポリシーに使用された場合に実装されます。 VRA を通るパケットに適用されるファイアウォール・ルールとは異なり、コントロール・プレーンに入るかまたはコントロール・プレーンから出るトラフィックのファイアウォール・ルールのデフォルト・アクションは `Allow` です。 デフォルトの動作が望ましくない場合は、ユーザーは明示ドロップ・ルールを追加する必要があります

VRA は基本的な CPP ルール・セットをテンプレートとして提供します。 以下を実行することによって、それを構成にマージできます。

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

このルール・セットがマージされた後、`CPP` という名前の新規ファイアウォール・ルール・セットが追加され、ループバック・インターフェースに適用されます。 ご使用の環境に合わせてこのルール・セットを修正することをお勧めします。

CPP ルールはステートフルであってはならず、進入トラフィックのみに適用されることに注意してください。

## ゾーン・ファイアウォール
{: #zone-firewalling}

{{site.data.keyword.vra_full}} 内のもう 1 つのファイアウォールの概念は、ゾーン・ベースのファイアウォールです。 ゾーン・ベースのファイアウォール操作では、インターフェースがゾーン (インターフェース当たり 1 つのゾーンのみ) に割り当てられ、ゾーン内のすべてのインターフェースのセキュリティー・レベルが同じで、自由に経路指定できるというアイデアで、ゾーン間の境界に、ファイアウォール・ルール・セットが割り当てられます。 トラフィックは、1 つのゾーンから別のゾーンへ渡されるときにのみ詳しく検査されます。 ゾーンは、明示的に許可されていないトラフィックが入ってくると、これらをすべてドロップします。

1 つのインターフェースが 1 つのゾーンに属するか、インターフェースごとのファイアウォール構成とすることができます。1 つのインターフェースがこの両方を実現することはできません。

3 つの部門を持つ以下のオフィス・シナリオを考えてみてください。それぞれの部門には独自の VLAN があります。

* 部門 A - VLAN 10 および 20 (インターフェース dp0bond1.10 および dp0bond1.20)
* 部門 B - VLANs 30 および 40 (インターフェース dp0bond1.30 および dp0bond1.40)
* 部門 C - VLAN 50 (インターフェース dp0bond1.50)

ゾーンは各部門用に作成することができ、その部門用のインターフェースをゾーンに追加することができます。 この点を以下の例に示します。
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20 
set security zone-policy zone DEPARTMENTB interface dp0bond1.30 
set security zone-policy zone DEPARTMENTB interface dp0bond1.40 
set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

`commit`  コマンドは、各ゾーンをインターフェー
スとして取り込み、デフォルトのドロップ・ルールは外部からゾーンに入ろうとするトラフィックをすべて破棄します。 例として、VLAN 10 と 20 は、同じゾーン内にあるので (`DEPARTMENTA`) トラフィックを渡すことができますが、VLAN 10 と VLAN 30 の場合は、VLAN 30 が別のゾーン (`DEPARTMENTB`) 内にあるので、トラフィックを渡すことができません。

各ゾーン内のインターフェースは自由にトラフィックを渡すことができ、ルールはゾーン間の相互作用に対して定義することができます。 ルール・セットは、あるゾーンから別のゾーンへの移動の観点から構成されます。

次のコマンドは、ルールを構成する方法の一例を示しています。

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING`

このコマンドは、`ALLOW_PING` という名前のルール・セットに DEPARTMENTC から DEPARTMENTB への通過を関連付けます。 DEPARTMENTC ゾーンから DEPARTMENTB ゾーンに入るトラフィックは、このルール・セットに対して検査されます。

ゾーン DEPARTMENTC からゾーン DEPARTMENTB へ入るこの割り当てでは、その逆の処理については記述していないことを理解することが重要です。 ゾーン DEPARTMENTB からゾーン DEPARTMENTC へのトラフィックを許可するルールがない場合、トラフィック (ICMP 応答) は DEPARTMENTC 内のホストに戻りません。

`ALLOW_PING` は、DEPARTMENTB ゾーンのインターフェース (dp0bond1.30 および dp0bond1.40) に `out` ファイアウォールとして適用されます。 これはゾーン・ポリシーによってインストールされるため、ソース・ゾーンのインターフェース (dp0bond1.50) から発信されるトラフィックのみがルール・セットに対して検査されます。

## セッションとパケットのロギング
{: #session-and-packet-logging}

VRA は次の 2 つのタイプのロギングをサポートします。

1. セッションのロギング。  ファイアウォール・セッション・ロギングを構成するには、``security firewall session-log`` コマンドを使用します。

	UDP、ICMP、およびすべての非 TCP フローの場合、セッションはフローの存続期間中に 4 つの状態に遷移します。 各遷移ごとに、メッセージをログに記録するように VRA を構成することができます。 TCP は 状態遷移の数が多く、それぞれをログに記録するように構成することができます。  ファイアウォールのセッション・ログをセットアップする方法の例を以下に示します。

```
set security firewall session-log icmp established
set security firewall session-log tcp established
set security firewall session-log udp established
```

2. パケットごとのロギング。 ファイアウォールまたは NAT ルールにキーワード ``log`` を含めて、ルールに一致する各ネットワーク・パケットをログに記録します。

	パケットごとのロギングは、パケット転送パスで実行され、大量の出力が生成されます。 パケット単位のロギングは、VRA のスループットを大幅に低下させ、パフォーマンスの問題を引き起こし、ログ・ファイル用に使用されるディスク・スペースを劇的に増加させる可能性があることに注意することがきわめて重要です。パケット単位のロギングの使用は、デバッグ目的にのみお勧めします。操作目的の場合は常に、パケット単位のロギングではなく、ステートフル・セッション・ロギングを使用する必要があります。{: important}
