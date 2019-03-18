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

# 在 Vyatta 5400 上設定 NAT 規則
{: #setting-up-nat-rules-on-vyatta-5400}

本主題包含 Vyatta 上所使用的「網址轉換 (NAT)」規則範例。

## 一對多 NAT 規則（假冒）

在提示中，輸入下列指令：

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

來自 `10.xxx.xxx.xxx` 網路中機器的連線要求會對映至 bond1 上的 IP，並在往外時收到關聯的暫時埠。它的目的是指派較高的一對多假冒規則號碼，因此不會與您可能具有的較低 NAT 規則發生衝突。

**附註：**您必須配置伺服器透過 VRA 傳遞其網際網路資料流量，讓其預設閘道是受管理虛擬 LAN (VLAN) 的「專用 IP 位址」。例如，對於 `bond0.2254`，閘道是 `10.52.69.201`。這應該是傳遞網際網路資料流量之伺服器的閘道位址。

**附註：**使用下列指令，以協助疑難排解 NAT： 

'''
run show nat source translations detail 
'''

## 一對一 NAT 規則

下面的指令顯示如何設定一對一 NAT 規則。請注意，規則號碼設定的低於假冒規則。因此，一對一規則的優先順序高於一對多規則。

**附註：**無法假冒透過一對一對映的 IP 位址。如果您轉換 IP 入埠，則必須轉換該 IP 出埠，以接受雙向資料流量。

下列指令適用於來源及目的地規則。在配置模式中鍵入 `show nat`，以查看 NAT 規則類型。

**附註：**使用下列指令，以協助疑難排解 NAT：`run show nat source translations detail`。 

確定您處於配置模式之後，請在提示中輸入下列指令：

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

如果資料流量是從 bond1 的 IP `50.97.203.227` 流入，則會將該 IP 對映至 IP `10.52.69.202`（在定義的任何介面上）。如果資料流量使用 IP `10.52.69.202`（在定義的任何介面上）流出，則會將它轉換為 IP `50.97.203.227`，並繼續在介面 bond1 上往外流出。

**附註：**無法假冒透過一對一對映的 IP 位址。如果您轉換 IP 入埠，則必須轉換這個相同的 IP 出埠，以接受其雙向資料流量。


## 透過 VRA 新增 IP 範圍

根據 VRA 配置，您可能會想要接受特定的 IBM© Cloud IP 位址。 

新的 vRouter 部署隨附 IBM Cloud 中稱為 `SERVICE-ALLOW` 的防火牆規則中所定義的服務網路 IP 位址。

下列是 `SERVICE-ALLOW` 範例。這不是完整的專用 IP 規則集。

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

在定義防火牆規則之後，您現在可以按照您認為合適的方式指派它們。下面列出兩個範例。 

套用至區域：

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

套用至結合介面：

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
