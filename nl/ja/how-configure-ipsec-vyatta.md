---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 での IPSec の構成

Brocade 5400 vRouter (Vyatta) デバイスは、インターネット・セキュリティー・プロトコル (IPSec) トンネルに関しては「ローカル」と見なされます。以下の各コマンドがそれぞれ異なる機能を実行して IPSec サイト間が構成されます。この IPSec サイト間の例は SoftLayer のパブリック・ネットワーク上のトンネルを示していることに注意してください。プライベート IPSec サイト間接続には **bond0** を使用してください。

1. **interface bond1:** の目的をトンネルに知らせます。

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. 2 フェーズ・トンネルの第 1 フェーズをセットアップします。以下のコマンドで次の操作が実行されます。

  * **test** という名前の新しい **IKE** グループを作成し、鍵交換タイプとして **DH グループ**を使用します。
  * 使用する暗号化のタイプを指定します。これがセットアップされていない場合、デバイスはデフォルトとして **aes128** を使用します。
  * ハッシュ関数 **sha-1** を使用します。<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. 2 フェーズ・トンネルの第 2 フェーズをセットアップします。 以下のコマンドで次の操作が実行されます。

  * Perfect Forward Secrecy (PFS) を使用不可にします。すべてのデバイスがこれを使用できるわけではないためです。(コマンド中の esp は、暗号化の 2 番目の部分です)
  * 使用する暗号化のタイプを指定します。これがセットアップされていない場合、デバイスはデフォルトとして **aes128** を使用します。
  * ハッシュ関数 **sha-1** を使用します。<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. IPSec サイト間暗号化パラメーターをセットアップします。 以下のコマンドで次の操作が実行されます。

  * リモート・サイド IP を指定し、IPSec が事前共有秘密を使用することを指定します。
  * リモート IP および秘密鍵 TestPSK を使用します。
  * トンネルのデフォルト **ESP** グループを TestESP に設定します。
  * 先に定義した IKE グループ TestIKE を使用するように IPSec に指示します。<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. IPSec トンネル用のマッピングを作成します。 以下のコマンドで実行する内容は、当資料の例に基づくと次のようになります。

  * リモート IP 169.54.254.117 を bond1 のローカル IP アドレス 50.97.240.219 にマップするようにトンネルに指示します。
  * ローカル・サーバー・インターフェースにあるサブネット 10.54.9.152/29 の IP アドレスのみをリモート・サーバー 169.54.254.117 にルーティングします。
  * トンネル 1 リモート・トラフィック 169.54.254.117 をリモート・サブネット 192.168.1.2/32 にルーティングします。<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

次のステップは、リモート・サイド・デバイス (Brocade 5400 vRouter 6.6.5 R) をセットアップすることです。

  * 構成したばかりの (オペレーション・モードで構成された) デバイスを使用して、構成コマンドを表示するコマンドを入力します。そのデバイスのセットアップに使用されたコマンドのリストが表示されます。
  * それらのコマンドをテキスト・エディターにコピーします。ローカル・デバイスをセットアップするために使用されたそれらのコマンドは、リモート・サーバーをセットアップするために、SoftLayer 上の Brocade 5400 vRouter 6.6.5R デバイスをポイントするように IP を変更して、使用されます。

前に使用されたリモート・サイド構成は次のとおりです。ローカル・サイド構成に必要な変更は、太字で示されています。

リモート・サイド構成:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (サイト間ピア IP が交換されています)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (ローカル IP アドレスが交換されています)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (ローカル・プレフィックスおよびリモート・プレフィックスは交換されていません)

* これらの新しいコマンドをコピーしてリモート・サーバー (必ず構成モードにしてください) に張り付け、commit と入力し、次に save と入力します。
* run show vpn ike sa と入力して、トンネルが確立されたかどうかを確認します。

実行内容を要約すると次のようになります。ローカル・インターフェース (bond1、50.97.240.219) にある、サブネット '10.54.9.152/29' の IP アドレスのみを、IP アドレス 169.54.254.117 のインターフェースにあるリモート・サービス上の 192.168.1.2/32 サブネットにのみルーティングします。
