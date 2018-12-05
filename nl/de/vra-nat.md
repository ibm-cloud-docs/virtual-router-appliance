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

# NAT mit präfixbasiertem IPsec verwenden
Im Abschnitt [VFP-Schnittstelle mit IPsec und zonenbasierten Firewalls konfigurieren](vra-vfp.html) haben wir eine VFP-Schnittstelle erstellt und für die Verwendung mit einem IPsec-Tunnel konfiguriert. 

Wir können dieselbe Schnittstelle in NAT-Regeln sowohl für die eingehende als auch für die ausgehende Schnittstellendeklaration mit einem zusätzlichen Vorbehalt verwenden.  

Im Folgenden finden Sie einige Beispiele für NAT-Regeln:

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

Das obige Beispiel ist eine bidirektionale 1:1-Standardquellen-und-Ziel-NAT für dieselben IPs. Um jedoch sicherzustellen, dass der NAT-Verkehr ordnungsgemäß durch den Tunnel erfolgt, benötigen Sie eine statische Route für das andere Ende:

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

Der Grund für eine statische Route ist, dass der IPsec-Dämon bereits eine Kernelroute für das ferne Präfix erstellt hat:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

Bei einer Überprüfung der Quelle `10.177.137.251` mit Ping zu `172.16.100.2` wird der Datenverkehr über `dp0bond1` abgesetzt, schlägt beim Transit des Tunnels fehl und stimmt niemals ordnungsgemäß mit den NAT-Regeln überein. Die statische Route behebt diesen Fehler wie folgt: 

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

Auf diese Weise wird eine spezifischere Route für den Datenverkehr über `vfp0` erstellt.  

Damit funktioniert NAT wie konfiguriert und der Datenverkehr erfolgt durch den Tunnel.  

**HINWEIS:** Bei NAT benötigen Sie eine Route mit einem CIDR, das kleiner als das ferne Präfix von IPsec ist (es darf nicht die gleiche Größe aufweisen) und auf Ihren Datenverkehr über die virtuelle Schnittstelle `vfp0` verweist. 

Nachdem alle Änderungen wirksam sind, können Sie das Pingsignal absetzen und alles überprüfen: 

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
