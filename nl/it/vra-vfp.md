---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vfp, ipsec, firewall

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Configurazione di un'interfaccia VFP con firewall zona e IPsec
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

Quando arriva un datagramma IPsec, viene elaborato tramite le regole del firewall e quindi deincapsulato. Il nuovo datagramma risultante non è affatto associato a un'interfaccia. Normalmente, questo non è un problema e può proseguire all'interfaccia di destinazione, ma i firewall zona impediranno al datagramma di andare avanti. Qualsiasi datagramma che non proviene da un'interfaccia in una politica di zona viene eliminato. Un'interfaccia VFP, tuttavia, informa il firewall zona che il datagramma proveniva da un'interfaccia, il che consente l'applicazione delle regole.

Per configurare un'interfaccia VFP per gestire il traffico IPsec, crea prima un punto funzione definendo il VFP con un singolo IP:

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Aggiungi quindi il VFP al tunnel:

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Aggiungi quindi l'interfaccia a una zona (in questo caso, la zona INTERNET con dp0bond1):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

Ora esegui il ping del server:

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

È possibile utilizzare la stessa interfaccia VFP su più tunnel o creare diversi VFP per altri tunnel; assicurati solo che eventuali VFP aggiuntivi si trovino in una zona e abbiano delle regole che consentono il traffico per i frame non incapsulati.

È anche possibile aggiungere i VFP alla propria zona. Ad esempio:

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

Sebbene l'interfaccia VFP sia un'interfaccia "reale" in quanto può essere monitorata con comandi come `tshark` e può instradare il traffico direttamente, non è utile farlo. Tutto il traffico che arriva sull'altra estremità che non corrisponde alla politica del tunnel verrà eliminato. Non è versatile come sarebbe un'interfaccia VTI-
