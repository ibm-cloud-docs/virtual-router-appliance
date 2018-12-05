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

# Vyatta 5400 の一般的なマイグレーション問題
以下の表では、Vyatta 5400 デバイスから IBM 仮想ルーター・アプライアンスにマイグレーションした後に発生する可能性のある、一般的な問題または動作変更を説明しています。 問題に対処する回避策を含む場合もあります。

## ステートフル・ファイアウォールのインターフェース・ベースのグローバル・ステート・ポリシー

### 問題
リリース 5.1 から、ステートフル・ファイアウォールの「ステートポリシーの状態」を設定する際の動作が変更されました。 リリース 5.1 より前のバージョンでは、ステートフル・ファイアウォールの `state - global -state -policy` を設定すると、vRouter は、セッションの戻り通信の暗黙的な`許可`ルールを自動的に追加していました。

リリース 5.1 以降では、仮想ルーター・アプライアンスに`許可`ルール設定を追加する必要があります。 ステートフル設定は Vyatta 5400 デバイスのインターフェースのために機能し、VRA デバイスのプロトコルのために機能します。

### 回避策
`firewall-in` ルールが入口/内部インターフェースに適用された場合は、`Firewall-out` ルールが出口/外部インターフェースに適用される必要があります。 その他の場合、リターン・トラフィックは出口/外部インターフェースでドロップされます。        

## ファイアウォール・ルールでのステートイネーブル

### 問題
`global-state-policy` が構成されていない場合、この動作変更は影響を受けません。

また、`global-state-policy` ではなく、各ルールで `state enable` オプションを使用している場合、動作変更は影響を受けません。

## ゾーンベースのポリシー: ローカルゾーン処理

### 問題
zone-policy に割り当てる「ローカルゾーンの」pseudo-interface はありません。

### 回避策
この動作は、ゾーンベースのファイアウォールを物理インターフェースに適用し、interface-firewall をループバック・インターフェースに適用することでシミュレートできます。 ループバック・インターフェースのファイアウォールは、ルーターから入出力されるすべてのものをフィルターに掛けます。

以下に例を示します。

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## ファイアウォール、NAT、ルーティング、DNS の操作順序


### 問題
Masquerade Source NAT が IBM 仮想ルーター・アプライアンスにデプロイされるシナリオでは、ファイアウォールを使用してホストのインターネットへのアクセスを決定できません。これは、ポスト NAT アドレスが同じになるからです。


Vyatta 5400 デバイスでは、ファイアウォールが NAT より手前で行われ、ホストのインターネットへのアクセス制限が許可されるため、この操作は可能でした。

### 回避策
以下の新規のルーティング・スキームが VRA に必要です。
![routing dns](./images/routing-dns.png)

## ポリシー・ベースのルーティング・テーブル

### 問題
構成内の用語「Table」は、v5400 ポリシー・ベース・ルーティングではオプションですが、VRA では、アクションが `accept` の場合は、**「Table」**フィールドは必須です。VRA 構成でアクションが `drop` の場合は、「Table」フィールドはオプションです。


### 回避策
「Table Main」は Vyatta 5400 ポリシー・ベース・ルーティングでは使用可能なオプションです。VRA でこれに相当するものは「routing-instance default」です。

## トンネル・インターフェースのポリシー・ベース・ルーティング


### 問題
仮想ルーター・アプライアンスの PBR (ポリシー・ベース・ルーティング）ポリシーは、インバウンド・トラフィックについてデータ・プレーン・インターフェースに適用することができますが、ループバック、トンネル、ブリッジ、OpenVPN、VTI、および IP アンナンバード・インターフェースには適用できません。


### 回避策
現在この問題に対する回避策はありません。

## TCP-MSS

### 問題
IBM 仮想ルーター・アプライアンスは、nftables を使用しており、TCP-MSS をサポートしていません。


### 回避策
現在この問題に対する回避策はありません。

## OpenVPN

### 問題
OpenVPN が仮想ルーター・アプライアンスで `push-route` パラメーターの使用時に動作を開始しません。


### 回避策
`push-route` の代わりに `openvpn-option` パラメーターを使用してください。

## GRE/VTI over IPSEC + OSPF

### 問題
* VIF に複数のサブネットが構成されている場合、トラフィックは VRA のそれらのサブネットを超えて移動できません。
                                             
* InterVlan Routing が VRA で動作していません。

### 回避策
VIF インターフェース間のトラフィックを受け入れる、暗黙の許可ルールを使用します。


## IPSec

### 問題
IPSec (プレフィックスベース) が IN フィルターで動作しません。


### 回避策
IPSec (VTI BASED) を使用します。


## IPSEC 'match-none"


### 問題
Vyatta 5400 デバイスで、以下のファイアウォール・ルールが許可されています。


set firewall name allow rule 10 ipsec


ただし、IBM 仮想ルーター・アプライアンスでは IPSec がありません。


### 回避策
以下に VRA デバイスで使用可能な代替ルールを示します。


```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets
```

## IPSEC 'match-ipsec"

### 問題
Vyatta 5400 デバイスで、以下のファイアウォール・ルールが許可されています。


set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

ただし、IBM 仮想ルーター・アプライアンスでは IPSec がありません。


### 回避策
プロトコル `ESP` と `AH` (それぞれ IP プロトコル 50 と 51) を追加します。


`「アクション」`ルールは、以下に示すように`「accept」`または`「drop」`のいずれかになります。

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## DNAT を指定したサイト間 IPSEC


### 問題
IPSec (プレフィックスベース) が DNAT で動作しません。
                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1)
===S-S-IPsec=== (12.0.0.1)
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

上記のスニペットは、IPSec パケットが Vyatta 5400 で復号化された後の DNAT トランスレーションの小さなセットアップ例です。 この例では、2 つの vyatta があります (`vyatta1 (11.0.0.1)` と `vyatta2 (12.0.0.1)`)。IPsec ピアリングは `11.0.0.1` と `12.0.0.1` の間に確立されます。この場合、クライアントは `10.103.0.1` エンドツーエンドから送信される `172.16.1.245` をターゲットとしています。このシナリオの予期される動作は、パケットのヘッダーで宛先アドレス `172.16.1.245` が `10.71.68.245` に変換されることです。


初めに Vyatta 5400 デバイスがインバウンド IPSec で DNAT を実行中で、接続トラッキング・テーブルを使用してインターフェースを終了しトラフィックを正常に IPsec トンネルに返しました。

仮想ルーター・アプライアンスでは、この構成は同じように機能しません。 セッションは作成されますが、リターン・トラフィックは conntrack テーブルが DNAT の変更を元に戻した後に IPsec トンネルをバイパスします。VRA は次に、IPsec を暗号化せずにワイヤー上のパケットを送信します。 上流装置はこのトラフィックを想定していないため、ドロップする可能性が高くなります。エンドツーエンドの接続が失敗しますが、これは意図された動作です。

### 回避策
上記のネットワーキング・シナリオに対応するため、IBM は RFE を作成しました。 


RFE は現在査定中ですが、以下の回避策を推奨します。


**インターフェース構成コマンド**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN 構成コマンド**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT 構成コマンド**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**プロトコル構成コマンド**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR 構成コマンド**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### 問題
PPTP は仮想ルーター・アプライアンスでサポートされなくなりました。
                                                                                                                                                   

### 回避策
代わりに L2TP プロトコルを使用してください。


## IPSec 再始動のスクリプト


### 問題
VRRP 仮想アドレスが高可用性 VPN の IBM 仮想ルーター・アプライアンスに追加されるたびに、IPsec デーモンを再初期化する必要があります。これは、IPsec サービスは、IKE サービス・デーモンの初期化時に VRA 上に存在したアドレスへの接続のみを listen するためです。


VRRP を使用した VRA のペアで、初期化時にデバイス上に存在する VRRP 仮想アドレスをマスター・ルーターが現在所持していない場合、スタンバイ・ルーターはそのアドレスを所持していない可能性があります。 そのため、VRRP 状態遷移が発生した場合に IPsec デーモンを再初期化するためには、マスター・ルーターとバックアップ・ルーターで以下のコマンドを実行します。


```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## 最近のカウントと最近の時刻


### 問題

以下のルールは、アドレスを使用した SSH で SSH 接続を 30 秒ごとに 3 個に制限することを意図しています。

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

IBM 仮想ルーター・アプライアンスでは、このルールに以下の問題があります。


* recent count (最近のカウント) オプションと recent time (最近の時刻) オプションは非推奨になりました。


* 前述の問題により、このルールは予期されるとおり機能することができず、適用されるインターフェースに対するすべての SSH 接続をブロックします。


### 回避策
代わりに CPP を使用してください。

## set system conntrack の問題


### 問題

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## set system conntrack のタイムアウト


### 問題
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## 時間基準ファイアウォール


### 問題
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### 問題
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## 特定のアプリケーションまたはポートが S-S Ipsec VPN で壊れている


### 問題

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## ロギング動作での重大な変更点


### 問題
Vyatta 5400 デバイスと IBM 仮想ルーター・アプライアンス間で、セッションごとのロギングからパケットごとのロギングまで、ロギング動作に重大な変更点があります。


* セッション・ロギング: ステートフル・セッションの状態遷移を記録。

* パケット・ロギング: ルールに一致するすべてのパケットを記録。パケット・ロギングは「パケット・ユニット」のログ・ファイルに記録されるため、スループットの著しい減少とディスク容量の圧迫があります。


* vRouter のロギング機能は、ファイアウォール・アクティビティーの取り込みに使用することができます。あらゆるロギング機能と同様、特定の問題のトラブルシューティング実行時にのみこれを使用可能にし、できる限り早くこのロギングを使用不可にすべきです。


ファイアウォール / NAT セッションを管理するステートフル・ファイアウォールは、「セッション・ユニット」に書き込まれます。 セッション・ロギングの使用を推奨します。 各設定例を以下に示します。


**セッション / ロギング**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**パケット・ロギング・ファイアウォール**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>`
