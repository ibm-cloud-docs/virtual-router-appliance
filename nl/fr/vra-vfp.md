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

# Configuration d'une interface VFP avec IPsec et des pare-feux de zone
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

Lorsqu'un datagramme IPSec arrive, il est traité via les règles de pare-feu, puis désencapsulé. Le nouveau datagramme qui émerge n'est pas du tout associé à une interface. Normalement, ce n'est pas un problème, et il peut accéder à l'interface de destination, mais des pare-feux de zone empêcheront la progression du datagramme. Tout datagramme qui ne provient pas d'une interface dans une stratégie de zone est abandonné. Cependant, une interface VFP informe le pare-feu de zone que le datagramme ne provenait pas d'une interface, ce qui permet l'application des règles. 

Pour configurer une interface VFP afin qu'elle fonctionne avec du trafic IPsec, commencez par créer un point de fonction en définissant l'interface VFP avec une seule adresse IP :

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Ajoutez ensuite l'interface VFP au tunnel :

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Puis, ajoutez l'interface à une zone (en l'occurrence, la zone INTERNET avec dp0bond1) :

```
set security zone-policy zone INTERNET interface 'vfp0'
```

A présent, envoyez une commande PING au serveur :

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

Il est possible d'utiliser la même interface VFP sur plusieurs tunnels ou de créer différentes interfaces VFP pour d'autres tunnels, juste pour s'assurer que les interfaces VFP supplémentaires se trouvent dans une zone et sont associées à des règles qui permettent le trafic pour des trames désencapsulées.

Il est également possible d'ajouter des interfaces VFP à leur propre zone. Par exemple :

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

Même si l'interface VFP est une interface "réelle" qui peut être surveillée à l'aide de commandes, telles que `tshark`, et qui peut router le trafic directement, il n'est pas utile d'effectuer ces actions. Le trafic qui arrive à l'autre extrémité et qui ne correspond pas à la stratégie du tunnel sera supprimé. Une interface VFP n'est pas aussi polyvalent qu'une interface VTI.
