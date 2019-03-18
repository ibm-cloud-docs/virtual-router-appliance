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

# VFP-Schnittstelle mit IPsec und zonenbasierten Firewalls konfigurieren
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

wenn ein IPSec-Datagramm eingeht, wird es durch die Firewallregeln verarbeitet und anschließend wird die Einbindung aufgehoben. Das neu entstehende Datagramm ist keiner Schnittstelle zugeordnet. Normalerweise ist dies kein Problem und das Datagramm kann bis an die Zielschnittstelle gelangen, zonenbasierte Firewalls verhindern jedoch, dass das Datagramm weiter verarbeitet wird. Jedes Datagramm, das nicht von einer Schnittstelle in der Zonenrichtlinie stammt, wird gelöscht. Eine VFP-Schnittstelle hingegeben informiert die zonenbasierte Firewall darüber, dass das Datagramm nicht von einer Schnittstelle stammt, sodass Regeln angewendet werden können. 

Um eine VFP-Schnittstelle für die Arbeit mit dem IPsec-Datenverkehr zu konfigurieren, müssen Sie zuerst einen Funktionspunkt erstellen, indem Sie VFP mit einer einzelnen IP definieren:

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Danach fügen Sie VFP zum Tunnel hinzu:

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Als nächstes fügen Sie die Schnittstelle einer Zone hinzu (in diesem Fall der Zone INTERNET mit dp0bond1):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

Jetzt überprüfen Sie den Server mit dem Pingsignal:

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

Sie können dieselbe VFP-Schnittstelle in mehreren Tunnels verwenden oder unterschiedliche VFPs für andere Tunnel erstellen. Stellen Sie aber sicher, dass alle weiteren VFPs sich in einer Zone befinden und über Regeln verfügen, die Datenverkehr für nicht eingebundene Frames zulassen.

Es ist ferner möglich, VFPs ihrer eigenen Zone hinzuzufügen. Beispiel:

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

Obwohl die VFP-Schnittstelle eine "reale" Schnittstelle ist, in der eine Überwachung mithilfe von Befehlen wie `tshark` erfolgen und Datenverkehr direkt weitergeleitet werden kann, ist es nicht empfehlenswert, das zu tun. Jeder Datenverkehr, der am anderen Ende ankommt und nicht der Regel des Tunnels entspricht, wird gelöscht. Diese Schnittstelle ist nicht so vielseitig wie eine VTI-Schnittstelle.
