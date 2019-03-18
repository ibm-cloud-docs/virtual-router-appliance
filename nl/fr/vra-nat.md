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

# Utilisation de NAT avec IPsec basé sur le préfixe
{: #using-nat-with-prefix-based-ipsec}

Dans la rubrique [Configuration d'une interface VFP avec IPsec et des pare-feux de zone](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls), nous avons créé une interface VFP et l'avons configurée pour être utilisée avec un tunnel IPsec. 

Nous pouvez utiliser la même interface dans des règles NAT, ainsi que la déclaration d'interface entrante et sortante, avec une restriction supplémentaire. 

Exemples de règles NAT :

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

L'exemple précédent illustre une conversion NAT cible et source un-à-un bidirectionnelle pour les mêmes adresses IP. Mais, pour s'assurer que le trafic NAT transite correctement par le tunnel, vous avez besoin d'une route statique pour l'autre extrémité :

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

La raison pour laquelle l'utilisation d'une route statique est nécessaire est liée au fait que le démon IPsec a déjà créé une route de noyau pour le préfixe distant :

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

Si une commande PING est exécutée entre `10.177.137.251` et `172.16.100.2`, le trafic sort par `dp0bond1`, ne passe pas par le tunnel et ne correspond jamais aux règles NAT. La route statique permet de remédier à cela :

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

Cela crée une route plus spécifique pour permettre au trafic de reprendre `vfp0`. 

A ce stade, la conversion NAT fonctionnera comme prévu et le trafic transitera par le tunnel. 

**Remarque :** avec la conversion NAT, vous avez besoin d'une route avec un routage CIDR plus petit que le préfixe distant IPsec (il ne peut pas être de la même taille) pointant votre trafic sur l'interface virtuelle `vfp0`.

Une fois que tout est en place, vous pouvez exécuter une commande PING et procéder à des vérifications :

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
