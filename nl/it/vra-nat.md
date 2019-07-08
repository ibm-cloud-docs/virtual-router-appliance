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

# Utilizzo di NAT con IPsec basato sui prefissi
{: #using-nat-with-prefix-based-ipsec}

Nell'argomento [Configurazione di un'interfaccia VFP con firewall zona e IPsec](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls), abbiamo creato un'interfaccia VFP e l'abbiamo configurata perché venisse utilizzata con un tunnel IPsec.

Possiamo utilizzare la stessa interfaccia nelle regole NAT, nonché la dichiarazione di interfaccia in entrata e in uscita, con un avvertimento aggiuntivo.

Ecco alcune regole NAT di esempio:

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

L'esempio precedente è una NAT di origine e di destinazione uno-a-uno bidirezionale standard per gli stessi IP. Per garantire però che il traffico NAT transiti per il tunnel in modo corretto, ti serve un instradamento statico per l'altra estremità:

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

Il motivo per cui utilizzare un instradamento statico consiste nel fatto che il daemon IPsec ha già creato un instradamento kernel per il prefisso remoto:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

Eseguendo il ping con un'origine di `10.177.137.251` a `172.16.100.2`, il traffico uscirà tramite `dp0bond1`, non riuscirà a transitare per il tunnel e non corrisponderà mai correttamente alle regole NAT. L'instradamento statico corregge ciò:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

Questo crea un instradamento più specifico che può essere seguito dal traffico attraverso `vfp0`.

A questo punto, NAT funzionerà come configurato e il traffico transiterà attraverso il tunnel.

NAT richiede un instradamento con un CIDR più piccolo del prefisso remoto IPsec (non può essere della stessa dimensione) che punta il tuo traffico sull'interfaccia virtuale `vfp0`.
{: tip}

Dopo che tutti gli elementi sono stati posizionati correttamente, puoi eseguire un ping e verificare:

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
