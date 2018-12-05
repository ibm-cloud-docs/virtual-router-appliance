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

# Configuración de una interfaz de VFP con IPsec y cortafuegos de zona
Cuando llega un datagrama de IPSec, se procesa a través de las reglas de cortafuegos y luego se desencapsula. El nuevo datagrama que se genera no está asociado con una interfaz en absoluto. Normalmente, esto no es un problema, y puede ir a la interfaz de destino, pero los cortafuegos de zona impedirán que el datagrama progrese. Cualquier datagrama que no provenga de una interfaz de una política de zona se descarta. Sin embargo, una interfaz de VFP informa al cortafuegos de zona que el datagrama ha salido de una interfaz, lo que permite aplicar las reglas. 

Para configurar una interfaz de VFP para que funcione con el tráfico IPsec, cree primero un punto de característica definiendo el VFP con una única IP:

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Luego añada el VFP al túnel:

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

A continuación, añada la interfaz a una zona (en este caso, la zona de INTERNET con dp0bond1):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

Ahora ejecute ping del servidor:

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

Se puede utilizar la misma interfaz de VFP en varios túneles, o crear diferentes VFP para otros túneles; simplemente asegúrese de que cualquier VFP adicional se encuentre en una zona y tenga reglas que permitan el tráfico para las tramas no encapsuladas.

También se pueden añadir VFP a su propia zona. Por ejemplo:

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

Aunque la interfaz de VFP es una interfaz "real" en el sentido de que se puede supervisar con mandatos como `tshark` y puede direccionar el tráfico directamente, no resulta útil hacerlo. Cualquier tráfico que llegue al otro extremo que no coincida con la política del túnel se descartará. No es tan versátil como lo sería una interfaz de VTI.
