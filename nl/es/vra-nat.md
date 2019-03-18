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

# Utilización de NAT con IPsec basado en prefijo
{: #using-nat-with-prefix-based-ipsec}

En el tema [Configuración de una interfaz de VFP con IPsec y cortafuegos de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls) hemos creado una interfaz VFP y la hemos configurado para que se utilice con un túnel IPsec. 

Podemos utilizar la misma interfaz en reglas NAT, así como la declaración de interfaz de entrada y de salida, con una advertencia adicional. 

A continuación se muestran algunas reglas NAT de ejemplo:

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

El ejemplo anterior constituye un NAT de origen y de destino bidireccional de uno a uno para las mismas IP. Pero, para asegurarse de que el tráfico de NAT pasa por el túnel correctamente, necesita una ruta estática para el otro extremo:

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

El motivo de utilizar una ruta estática es que el daemon IPsec ya ha creado una ruta de kernel para el prefijo remoto:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

Si se ejecuta ping con el origen `10.177.137.251` a `172.16.100.2`, el tráfico circulará a través de `dp0bond1`, no podrá pasar por el túnel y nunca cumplirá correctamente con las reglas NAT. La ruta estática lo soluciona:

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

Esto crea una ruta más específica para que el tráfico utilice `vfp0`. 

En este punto NAT funcionará según la configuración y el tráfico se desplazará a través del túnel. 

**NOTA:** con NAT, necesita una ruta con un CIDR menor que el prefijo remoto de IPsec (no puede ser del mismo tamaño) que apunte al tráfico sobre la interfaz virtual `vfp0`.

Una vez especificado todo esto, puede ejecutar ping y verificar:

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
