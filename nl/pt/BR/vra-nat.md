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

# Usando a NAT com o IPsec baseado em prefixo
{: #using-nat-with-prefix-based-ipsec}

No tópico [Configurando uma interface VFP com IPsec e firewalls de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls), criamos uma interface VFP e a configuramos para ser usada com um túnel IPsec. 

É possível usar a mesma interface nas regras NAT, bem como a declaração de interface de entrada e saída, com uma advertência adicional. 

Aqui estão algumas regras NAT de exemplo:

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

O exemplo anterior é uma NAT de origem e de destino um para um bidirecional padrão para os mesmos IPs. Mas, para assegurar que o tráfego NAT passe pelo túnel adequadamente, é necessária uma rota estática para a outra extremidade:

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

O motivo do uso de uma rota estática é porque o daemon do IPsec já criou uma rota de kernel para o prefixo remoto:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

Ao executar ping com uma origem de `10.177.137.251` para `172.16.100.2`, o tráfego sairá por meio de `dp0bond1`, falhará ao transitar pelo túnel e nunca corresponderá adequadamente às regras NAT. A rota estática corrige isto:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

Isso cria uma rota mais específica para o tráfego passar por meio de `vfp0`. 

Nesse ponto, a NAT funcionará conforme configurado e o tráfego viajará pelo túnel. 

**NOTA:** com a NAT, é necessária uma rota com um CIDR menor que o prefixo remoto de IPsec (não pode ter o mesmo tamanho) apontando seu tráfego sobre a interface virtual `vfp0`.

Quando tudo estiver em seu lugar, será possível executar ping e verificar:

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
