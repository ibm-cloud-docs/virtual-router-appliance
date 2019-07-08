---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, prefix, IPsec, rules

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

# プレフィックス・ベースの IPsec での NAT の使用
{: #using-nat-with-prefix-based-ipsec}

トピック[「IPsec およびゾーン・ファイアウォールに対する VFP インターフェースの構成」
](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)では、VFP インターフェースを作成して IPsec トンネルと共に使用するよう設定しました。

NAT ルールでも、インバウンドおよびアウトバウンドのインターフェース宣言でも、同じインターフェースを使用できますが、注意事項が 1 つあります。

以下に NAT ルールの例をいくつか示します。

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

上記の例は、同じ IP に対する標準的な、双方向で 1 対 1 のソースと宛先の NAT です。 ただし、NAT トラフィックが適切にトンネルを通過するようにするために、もう一方のエンド用に静的ルートが必要となります。

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

静的ルートを使用する理由は、IPsec デーモンが既にリモート・プレフィックス用にカーネル経路を作成しているためです。

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

`10.177.137.251` のソースで `172.16.100.2` に ping すると、トラフィックは `dp0bond1` 経由で外に出て、トンネルを通過できず、NAT ルールに適切に一致することはありません。 静的ルートは以下のようにこれを修正します。

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

これにより、トラフィックが `vfp0` を通過するための、より具体的な経路が作成されます。

この時点で、NAT は構成されたとおりに機能し、トラフィックはトンネルを通過して移動します。

NAT では、IPsec リモート・プレフィックスより小さい CIDR を使用し (同じサイズのものは使用できません)、`vfp0` 仮想インターフェース上へトラフィックを指定した経路が必要です。
{: tip}

すべての準備が整ったら、以下のように ping を行って検証します。

```
[root@acs-jmat-migserver ~]# ping 172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.2 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.3 ms
^C
--- 172.16.100.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 44.247/44.431/44.727/0.272 ms
 
vyatta@acs-jmat-migsim01:~$ show nat source translations
Pre-NAT                 Post-NAT                Prot    Timeout
10.177.137.251:7553     172.16.200.2:7553       icmp    48
```
