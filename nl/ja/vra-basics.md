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

# VRA の基本
VRA の構成は、SSH を介してリモート・コンソール・セッションを使用するか、または Web GUI にログインすることによって実行できます。 デフォルトでは、Web GUI はパブリック・インターネットからは利用できません。 Web GUI を使用可能にするには、最初に SSH を介してログインします。

**注:** VRA をシェルおよびインターフェースの外部で構成すると、予期しない結果が生じる可能性があるため、お勧めできません。

## SSH を使用したデバイスへのアクセス
Linux、BSD、Mac OSX など、ほとんどの UNIX ベースのオペレーティング・システムでは、デフォルトのインストール済み環境に OpenSSH クライアントが組み込まれています。 Windows のユーザーは、PuTTy などの SSH クライアントをダウンロードできます。

パブリック IP への SSH を無効にして、プライベート IP への SSH のみを許可することをお勧めします。 プライベート IP への接続では、クライアントがプライベート・ネットワークに接続されている必要があります。 カスタマー・ポータルで提供されるデフォルト VPN オプション (PPTP、SSL-VPN、および IPsec) の 1 つを使用するか、VRA に構成されたカスタム VPN ソリューションを使用することによって、ログインできます。

**「デバイスの詳細」**ページの Vyatta アカウントを使用して、SSH 経由でログインします。 ルート・パスワードも提供されますが、セキュリティー上の理由で、デフォルトでは root ログインは無効になっています。

`ssh vyatta@[IP address] `

**注:** root ログインは無効のままにすることをお勧めします。 Vyatta アカウントを使用してログインし、必要な場合にのみ root に切り替えてください。

Vyatta アカウントにログインしなくても済むように、デプロイメント中に SSH 鍵を提供することもできます。 SSH 鍵を使用して VRA にアクセスできることを確認した後、次のコマンドを実行して、ユーザー名とパスワードによる標準ログインを無効にできます。

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Web GUI を使用したデバイスへのアクセス

上記の SSH に関する指示に従って VRA にログインしてから、次のコマンドを実行して HTTPS サービスを有効にします。

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

これらのコマンドが完了した後、ブラウザーのアドレス・バーに `https://<ip.address>` を入力します。IP アドレスは VRA のアドレスに置き換えてください。 VRA の自己発行証明書に同意を求められることがあります。 これに同意し、プロンプトが出されたら、Vyatta 資格情報を使用して Web インターフェースにログインします。

## モード
**構成モード:** `configure` コマンドを使用して呼び出されます。このモードで、VRA システムの構成を実行します。

**操作モード:** VRA システムへのログイン時の初期モード。 このモードでは、`show` コマンドを実行して、システムの状態に関する情報を照会できます。 このモードからシステムを再始動することもできます。

VRA のシェルは、いくつかの操作モードを持つ、モーダル・インターフェースです。 1 次 (デフォルト) モードは**「操作」**で、これはログイン時に提供されるモードです。 **「操作」**モードでは、情報を表示したり、システムの現在の実行に影響を与えるコマンド (日付/時刻の設定やデバイスのリブートなど) を実行したりできます。

コマンド `configure` は、ユーザーを**「構成」**モードに入れます。このモードでは、構成を編集できます。 構成変更は即時に有効にはなりません。変更をコミットして保存する必要があります。 コマンドは入力されると構成バッファーに入ります。 必要なコマンドをすべて入力したら、`commit` コマンドを実行して、変更を有効にします。

コマンドを永続的に保存するには、`commit` コマンドの後で `save` コマンドを実行します。

操作モード・コマンドは、コマンドの先頭に `run` をつけることで、構成モードから実行できます。 以下に例を示します。


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

ハッシュ・マーク (`#`) は、構成モードを示します。 コマンドを `run` で開始することにより、操作コマンドが提供されることを VRA シェルに示します。 前の例は、コマンドの出力に対する「grep」機能も示しています。

## コマンドの探索

VRA コマンド・シェルには、タブの補完機能が含まれています。 どのコマンドが使用可能かを知りたい場合は、Tab キーを押すと、リストと短い説明が表示されます。 これは、シェル・プロンプトでも、コマンドの入力中でも機能します。 例えば、次のようにします。

```
$show log dns [Tab キーを押す]
可能な設定:
  dynamic    動的 DNS のログの表示
  forwarding DNS 転送のログの表示
```

## サンプル構成
構成はノードの階層型パターンでレイアウトされます。 次の静的ルート・ブロックを考えてみましょう。

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

これは、次のコマンドによって生成されます。

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

この例は、1 つのノード (静的) が複数の下位ノードを持つことができることを示しています。 `192.168.1.0/24` の経路を削除するには、コマンド `delete protocols static route 192.168.1.0/24` を使用します。 コマンドに `192.168.1.0/24` の入力がない場合は、両方の経路ノードに削除のマークが付けられます。

`commit` コマンドが発行されるまで、構成は実際には変更されないことに注意してください。 現在実行されている構成と、構成バッファー内にある変更を比較するには、`compare` コマンドを使用します。 構成バッファーをフラッシュするには、`discard` を使用します。

## ユーザーおよび役割ベースのアクセス制御 (RBAC)
ユーザー・アカウントは、次の 3 つのアクセス・レベルを使用して構成できます。

* 管理者
* オペレーター
* スーパーユーザー

オペレーター・レベルのユーザーは、`show` コマンドを実行して、システムの稼働状況を表示したり、`reset` コマンドを発行して、デバイスのサービスを再始動したりできます。 オペレーター・レベルの許可は、読み取り専用アクセスを暗黙指定するものではありません。

管理者レベルのユーザーは、デバイスのすべての構成および操作に対する全アクセス権限を持ちます。 管理者ユーザーは、実行中の構成の表示、デバイスの構成の設定変更、デバイスのリブート、およびデバイスのシャットダウンを行えます。

スーパーユーザー・レベルのユーザーは、管理者レベルの特権に加えて、`sudo` コマンドにより root 権限を使用してコマンドを実行できます。

ユーザーは、パスワード認証スタイルまたは公開鍵認証スタイル、あるいはその両方に構成できます。 公開鍵認証は SSH で使用され、これによりユーザーは自身のシステム上の鍵ファイルを使用したログインが可能となります。 パスワードを使用するオペレーター・ユーザーを作成するには、次のようにします。

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**注:** レベルが指定されていない場合、ユーザーは管理者レベルと見なされます。 この場合、ユーザー・パスワードは、構成ファイルに暗号化されて表示されます。

役割ベースのアクセス制御 (RBAC) は、許可ユーザーに対して構成の一部へのアクセス権限を制限する方式です。 RBAC を使用すると、管理者はユーザー・グループに対して、実行できるコマンドを制限するルールを定義できます。

RBAC を実行するには、アクセス制御管理 (ACM) ルール・セットに割り当てられるグループを作成し、グループにユーザーを追加し、グループをシステム内のパスに一致させるルール・セットを作成してから、グループに適用されるパスを許可または拒否するようにシステムを構成します。

デフォルトでは、仮想ルーター・アプライアンスには ACM ルール・セットが定義されておらず、ACM は無効になっています。 RBAC を使用して細分性のあるアクセス制御を提供する場合は、ACM を有効にし、ユーザー独自の定義済みルールに加えて、以下のデフォルト ACM ルールを追加する必要があります。

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

ACM ルールを有効にする前に、補足[資料](vra-docs.html)を参照してください。 不正確な ACM ルールを設定すると、デバイスがアクセス拒否されたり、システム作動不能エラーが発生したりする可能性があります。
