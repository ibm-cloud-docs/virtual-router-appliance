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

# IPsec およびゾーン・ファイアウォールに対する VFP インターフェースの構成
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

IPSec データグラムは、到着すると、ファイアウォール・ルールによって処理され、その後カプセル化解除されます。 こうして形成される新しいデータグラムは、インターフェースとは全く関連がありません。 通常、これは問題ではなく、宛先インターフェースに進むことができますが、ゾーン・ファイアウォールがデータグラムの進行を妨げます。 ゾーン・ポリシー内のインターフェースから送信されたものではないデータグラムは、すべてドロップされます。 ただし、VFP インターフェースは、そのデータグラムがインターフェースから送信されたものではないことをゾーン・ファイアウォールに通知し、これによりルールが適用されます。 

IPsec トラフィックを処理するよう VFP インターフェースを構成するには、単一の IP を使用して VFP を定義することで、まずフィーチャー・ポイントを作成します。

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

次に、以下のように VFP をトンネルに追加します。

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

次に、インターフェースをゾーンに追加します (この場合、dp0bond1 を使用した INTERNET ゾーン):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

この時点で、サーバーを ping します。

```
[root@acs-jmat-migserver ~]# ping -c 5  172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.3 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.8 ms

--- 172.16.100.1 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 4006ms

rtt min/avg/max/mdev = 44.377/44.728/44.996/0.243 ms
```

複数のトンネルで同じ VFP インターフェースを使用することも、他のトンネル用に別の VFP を作成することもできます。ただ、追加の VFP が 1 つのゾーン内にあり、カプセル化解除されたフレームのトラフィックを許可するルールを持つようにしてください。

VFP を独自のゾーンに追加することも可能です。 以下に例を示します。

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

VFP インターフェースは、`tshark` のようなコマンドでモニターが可能で、トラフィックを直接経路指定できるという点で、「実」インターフェースであると言えますが、それを行うことは有用ではありません。 もう一方のエンドに着信するトラフィックで、トンネルのポリシーと一致しないものは、すべてドロップされます。 このインターフェースは、VTI インターフェースほど汎用性はありません。
