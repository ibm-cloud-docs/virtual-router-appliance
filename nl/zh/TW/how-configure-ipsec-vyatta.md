---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:important: .important}
{:tip: .tip}

# 在 Vyatta 5400 上配置 IPSec
{: #configuring-ipsec-on-vyatta-5400}

對於「網際網路安全通訊協定 (IPSec)」通道，Brocade 5400 vRouter (Vyatta) 裝置將被稱為「本端」。下列每一個指令都會執行不同的功能來配置 IPSec 網站至網站。請注意，此 IPSec 網站至網站範例示範 SoftLayer 公用網路上的通道；請使用 **bond0** 進行專用 IPSec 網站至網站連線。

1. 將 **interface bond1** 的用途「告知」通道：

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. 設定兩階段通道的第一個階段。下列指令將：

  * 建立稱為 **test** 的新 **ike** 群組，並使用 **dh-group** 作為金鑰交換類型。
  * 指定要使用的加密類型；如果未設定此項目，則裝置會使用 **aes128** 作為預設值
  * 使用雜湊函數 **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. 設定兩階段通道的第二個階段。下列指令將：

  * 停用完全秘密轉遞 (PFS)，因為並非所有裝置都可以使用它（指令中的 esp 是加密的第二個部分）。
  * 指定要使用的加密類型；如果未設定此項目，則裝置會使用 **aes128** 作為預設值
  * 使用 `has` 函數 **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. 設定 IPSec 網站至網站加密參數。下列指令將：

  * 指定遠端 IP，以及 IPSec 將使用預先共用的密碼
  * 使用遠端 IP 及秘密金鑰 TestPSK
  * 將通道的預設 **esp** 群組設為 TestESP
  * 「告知」IPSec 使用先前定義的 ike-group TestIKE<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. 建立 IPSec 通道的對映。根據資料中的範例，下列指令將：

  * 「告知」通道將遠端 IP 169.54.254.117 對映至 bond1 的本端 IP 位址 50.97.240.219
  * 只會將本端伺服器介面上子網路為 10.54.9.152/29 的 IP 位址遞送至遠端伺服器 169.54.254.117
  * 將通道 1 遠端資料流量 169.54.254.117 遞送至遠端子網路 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

下一步是設定遠端裝置，即 Brocade 5400 vRouter 6.6.5 R。

  * 使用剛才配置的裝置（以作業模式所配置），輸入指令來顯示配置指令。將會呈現用來設定裝置的指令清單。
  * 將指令複製到文字編輯器。用來設定本端裝置的指令將會用來設定遠端伺服器，但需要修改 IP 以指向 SoftLayer 上的 Brocade 5400 vRouter 6.6.5R 裝置。

先前使用的遠端配置如下。本端配置所需的變更會以粗體顯示。

遠端配置：

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret（已交換網站至網站對等 IP）*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117***（已交換本端 IP 位址）

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29***（尚未交換本端字首及遠端字首）

* 複製新的指令，並將其貼入遠端伺服器（務必處於配置模式），並鍵入 commit，然後儲存。
* 鍵入 `run show vpn ike sa`，以查看現在是否建立通道。

以下是所執行作業的總結：只將子網路 '10.54.9.152/29' 位在本端介面（bond1、50.97.240.219）的 IP 位址，遞送至遠端服務上的 192.168.1.2/32 子網路，而遠端服務位在 IP 位址為 169.54.254.117 的介面中。
