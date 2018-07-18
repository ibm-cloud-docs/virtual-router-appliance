---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HA と VRRP
仮想ルーター・アプライアンス (VRA) では、高可用性プロトコルとして Virtual Router Redundancy Protocol (VRRP) をサポートしています。 デバイスのデプロイメントは、アクティブ/パッシブ方式で行われます。つまり、一方のマシンがマスターになり、他方がバックアップになります。 両方のマシン上のすべてのインターフェースが同じ「同期グループ」のメンバーになるため、1 つのインターフェースに障害が発生すると、同じグループ内の他のインターフェースにも障害が起こり、そのデバイスはマスターであることを停止します。 現在のバックアップは、マスターがキープアライブ/ハートビート・メッセージをブロードキャストしなくなったことを検出し、VRRP 仮想 IP の制御を引き受けて、マスターになります。

VRRP は、ゲートウェイのプロビジョニング時の構成の最も重要な部分です。 高可用性機能は、ハートビート・メッセージに依存するため、メッセージがブロックされないようにすることが重要です。

## VRRP 仮想 IP (VIP) アドレス

VRRP 仮想 IP (VIP) は、フェイルオーバーの発生時にマスター・デバイスからバックアップ・デバイスに変更される浮動 IP アドレスです。 VRA は、デプロイすると、パブリックおよびプライベートの結合されたネットワーク接続を持ち、各インターフェースに実 IP が割り当てられます。 デバイスがスタンドアロンであるか HA ペアであるかに関係なく、VIP も両方のインターフェースに割り当てられます。 トラフィックの宛先 IP が VRA に関連付けられた VLAN のサブネットにある場合、トラフィックはこれらの VRRP VIP に直接送信されます。

ゲートウェイ・グループの VRRP 仮想 IP アドレスは変更してはならず、VRRP インターフェースも無効にしてはなりません。 これらの IP アドレスは、VLAN が関連付けられているときにトラフィックをゲートウェイに経路指定する手段です。 IP アドレスが存在しないと、トラフィックを softLayer BCR/FCR からゲートウェイ自体に転送できません。

## VRRP グループ

VRRP グループは、グループ内の 1 次 (つまり「マスター」) インターフェースの冗長性を提供する、インターフェースまたは仮想インターフェースのクラスターで構成されます。 グループ内の各インターフェースは通常、別個のルーター上に存在します。 冗長性は、各システムの VRRP プロセスによって管理されます。 VRRP グループには、固有の数値 ID があり、最大 20 個の仮想 IP アドレスを割り当てることができます。  

VRRP グループ ID は、SoftLayer によって割り当てられるもので、変更してはなりません。 Front Customer Router (FCR)/Backend Customer Router (BCR) の背後で初めて新規ゲートウェイ・グループがプロビジョンされた場合、そのグループは VRRP グループ 1 になります。 ゲートウェイ・グループの後続のプロビジョンでは、競合が発生しないようにするためにこの値が増分され、次のグループはグループ 2、その次はグループ 3 というようになります。 次に、SoftLayer プロビジョニング・プロセスによって計算され、割り当てられます。 この値を変更すると、他のアクティブ・グループと衝突して、マスター/マスターの競合が発生するリスクがあり、その結果、両方のゲートウェイ・グループが停止する可能性があります。

前の構成からマイグレーションする場合は、構成コードを入念にチェックして、VRRP グループ ID が静的に割り当てられていないことを確認することをお勧めします。

VRRP グループ ID は、SoftLayer データベース内に保持されるため、OS の再ロード時やアップグレード時には同じグループ ID 値が使用されます。 ユーザーが変更した VRRP グループ ID は、OS の再ロード中にシステム割り当て値で上書きされます。  

## VRRP 優先度

ゲートウェイ・グループの最初のマシンの優先度は 254 になります。この値は、次にプロビジョンされるデバイスでは減分されます。 決して優先度を 255 に設定しないでください。この値は VIP 「アドレス所有者」を定義するため、実行中のアクティブ・マスターとは異なる構成を使用してマシンをオンラインにすると、意図しない動作が生じることがあります。

## VRRP 優先使用

優先使用は、すべての VRRP インターフェースで常に無効に設定し、新規デバイスまたは OS 再ロード中のデバイスが、クラスターの引き継ぎを試みないようにしてください。

## VRRP 通知間隔

マスター・インターフェースまたは VIF は、まだサービス中であることをシグナル通知するために、IANA 割り当ての VRRP 用のマルチキャスト・アドレス (IPv4 の場合は `224.0.0.18`、IPv6 の場合は `FF02:0:0:0:0:0:0:12`) を使用して、「アドバタイズメント」と呼ばれる「ハートビート」パケットを LAN セグメントに送信します。

デフォルトでは、この間隔は 1 に設定されます。この値は増やすことができますが、5 を超える値に設定するのはお勧めできません。トラフィック量の多いネットワークでは、すべての VRRP 通知がマスターからバックアップ・デバイスに到着するまでに 1 秒よりかなり長い時間かかる場合があります。そのため、そのようなネットワークでは値 2 を使用してください。

## VRRP 同期 (同期グループ)

VRRP 同期グループ (「同期グループ」) 内のインターフェースは、グループ内のいずれかのインターフェースがバックアップにフェイルオーバーした場合、そのグループ内のすべてのインターフェースがバックアップにフェイルオーバーするように同期化されています。 例えば、多くの場合、マスター・ルーター上の 1 つのインターフェースに障害が起こると、ルーター全体がバックアップ・ルーターにフェイルオーバーします。

この値は、VRRP グループの場合とは異なり、そのグループ内のいずれかのインターフェースが障害を検知した場合にデバイス上のどのインターフェースがフェイルオーバーするのかを定義します。 すべてのインターフェースが同じ同期グループに属するようにすることをお勧めします。そうしないと、一部のインターフェースはマスターとなってゲートウェイ IP を持ち、他のインターフェースはバックアップとなるため、トラフィックが正常に転送されなくなります。 アクティブ/アクティブ構成はサポートされません。

## VRRP rfc-互換性

rfc-互換性は、インターフェース上の VRRP の RFC 3768 MAC アドレスを有効または無効にします。 これは、ネイティブ・インターフェースでは有効にする必要がありますが、構成された VIF では有効にしてはなりません。 一部の仮想スイッチ (大抵は、vmware) では、これが有効になっていると問題が生じ、トラフィックが除去されて、ハイパーバイザー・ホスト・マシンからゲートウェイ IP に送信されなくなります。 この設定はそのままにして、どの VIF にもこの設定を構成しないでください。

## VRRP 認証
VRRP 認証用のパスワードを設定する場合、認証タイプも定義する必要があります。 パスワードが設定されていて、認証タイプが定義されていない場合、構成をコミットしようとすると、システムはエラーを生成します。

同様に、VRRP パスワードを削除するときは、必ず VRRP 認証タイプも削除する必要があります。 パスワードだけを削除した場合、構成をコミットしようとすると、システムはエラーを生成します。 VRRP 認証パスワードと認証タイプの両方を削除すると、VRRP 認証は無効になります。

IETF は、VRRP バージョン 3 では認証を使用しないことに決定しました。詳しくは、RFC 5798 VRRP を参照してください。

## バージョン 2/3 サポート
VRA は、VRRP バージョン 2 (デフォルト) とバージョン 3 の両方のプロトコルをサポートします。 バージョン 2 では、IPv4 と IPv6 を一緒に使用できません。 しかし、バージョン 3 では、IPv4 と IPv6 を同時に使用できます。

## VRRP を使用した高可用性 VPN
VRA は、VRRP を使用する仮想ルーター・アプライアンスのペアを使用して、1 つの IPsec トンネルを介して接続を維持する機能を提供します。 1 つのルーターで障害が発生した場合または保守のためのサービス停止が発生した場合、新規 VRRP マスター・ルーターが、ローカル・ネットワークとリモート・ネットワーク間の IPsec 接続を復元します。

VRRP を使用して高可用性 VPN を構成する場合、VRRP 仮想アドレスが VRA インターフェースに追加されるたびに、IPsec デーモンを再初期化する必要があります。IPsec サービスは、Internet Key Exchange (IKE) サービス・デーモンの初期化時に VRA 上に存在したアドレスへの接続のみを listen するためです。

マスター・ルーターとバックアップ・ルーターで以下のコマンドを実行して、フェイルオーバーが発生した場合、以下の例に示されるように、VIP の移行後に新規マスター・デバイス上で IPsec デーモンが再始動されるようにします。

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec`

## VRRP を使用した高可用性ファイアウォール

2 つのデバイスが HA ペアになっている場合、マスター・デバイスでは、他方のデバイスからのアクセスをブロックしないように注意する必要があります。 構成同期が機能するように両方のデバイス間でポート 443 が許可され、マルチキャスト範囲 224.0.0.0/24 を含めて、VRRP の送受信が許可されている必要があります。

## VRRP を使用する関連 VLAN サブネット

VRRP を使用する VIF 構成では、仮想 IP アドレス (サブネット内の最初の使用可能 IP、そのサブネット内のすべての経路指定先であるゲートウェイ IP) が必要であり、さらに両方のデバイス上の VIF の実インターフェース IP アドレスも必要です。 使用可能な IP を節約するために、実インターフェース IP では 192.168.0.0/30 などのアウト・オブ・バンド範囲を使用することをお勧めします。ここでは、一方のデバイスは .1 アドレスを使用し、他方のデバイスは .2 アドレスを使用します。 複数の VRRP 仮想 IP を使用できますが、各 VIF で必要な実アドレスは 1 つだけです。

以下に、2 つのプライベート VLAN と 3 つのサブネットを持つ VRRP 構成の例を示します。

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* VRRP 同期グループと VRRP グループは違うものです。 同期グループ内のいずれかのインターフェースが状態を変更すると、同じ同期グループの他のすべてのメンバーは同じ状態に遷移します。 
* グループ内の VLAN インターフェース (VIF) の VRRP グループ番号は、ネイティブ・インターフェースのいずれかまたは他の VLAN のいずれかと同じである必要はありません。 ただし、VLAN 10 でのように、同じ VLAN のすべての仮想アドレスを 1 つの VRRP グループに保持することを強くお勧めします。
* ネイティブ VLAN 上の実際のインターフェース・アドレス (例えば、dp0bond1: 169.110.20.28/29) は、それらの VIP (169.110.21.26/29) と同じサブネットにあるとは限りません。 

## 手動 VRRP フェイルオーバー
VRRP フェイルオーバーを強制する必要がある場合、現在 VRRP マスターであるデバイスで以下を実行することでそれを行うことができます。

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

グループ ID は、ネイティブ・インターフェースの VRRP グループ ID であり、前述のとおり、ご使用のペアでは異なる可能性があります。

## 接続の同期
2 つの VRA デバイスが HA ペアになっている場合、2 つのデバイス間のステートフル接続をトラッキングすると役立つことがあります。フェイルオーバーが発生した場合、障害が発生したデバイス上のすべての接続の現行状態がバックアップ・デバイスに複製されるため、現在アクティブなセッション (SSL 接続など) を最初から再作成する必要がなく、ユーザーは中断を経験せずに済みます。

これは、接続トラッキング同期と呼ばれます。

これを構成するには、以下のように、使用するフェイルオーバー方式、接続トラッキング情報の送信に使用するインターフェース、およびリモート・ピアの IP を宣言する必要があります。

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

もう一方の VRA も同じ構成になりますが、`remote-peer` が異なります。

他のインターフェースに着信する接続が多数ある場合、これによりリンクが飽和状態になる可能性があり、宣言されたリンク上で他のトラフィックとの競合が生じることに注意してください。
