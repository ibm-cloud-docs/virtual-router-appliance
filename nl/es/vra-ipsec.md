---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ipsec, firewall, configure, policy

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

# Configuración de un túnel IPsec que funciones con cortafuegos de zona
{: #setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls}

En versiones anteriores de Virtual Router Appliance, los túneles IPsec que utilizan direccionamiento basado en políticas no funcionaban bien con los cortafuegos de zona. Con la versión 18.01, hay un nuevo conjunto de mandatos que se ocupa de este problema, utilizando "puntos de características virtuales" para habilitar el tráfico procedente de túneles designados, donde el punto de característica actúa como una interfaz que proporciona un punto final para incluir en una configuración de política de zona.

A continuación se muestra una configuración de ejemplo de dos máquinas con IPsec entre ellas:

###Máquina A
{: #machine-a}

```
vyatta@acs-jmat-migsim01:~$ show configuration commands | grep ipsec
set security vpn ipsec esp-group ESP01 pfs 'enable'
set security vpn ipsec esp-group ESP01 proposal 1 encryption 'aes256'
set security vpn ipsec esp-group ESP01 proposal 1 hash 'sha2_512'
set security vpn ipsec ike-group IKE01 proposal 1 dh-group '2'
set security vpn ipsec ike-group IKE01 proposal 1 encryption 'aes256'
set security vpn ipsec ike-group IKE01 proposal 1 hash 'sha2_512'
set security vpn ipsec site-to-site peer 50.23.177.59 authentication pre-shared-secret '********'
set security vpn ipsec site-to-site peer 50.23.177.59 default-esp-group 'ESP01'
set security vpn ipsec site-to-site peer 50.23.177.59 ike-group 'IKE01'
set security vpn ipsec site-to-site peer 50.23.177.59 local-address '169.47.243.43'
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 local prefix '172.16.200.1/30'
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 remote prefix '172.16.100.1/30'
```

###Máquina B
{: #machine-b}

```
vyatta@acs-jmat-1801-1a:~$ show configuration commands | grep ipsec
set security vpn ipsec esp-group ESP01 pfs 'enable'
set security vpn ipsec esp-group ESP01 proposal 1 encryption 'aes256'
set security vpn ipsec esp-group ESP01 proposal 1 hash 'sha2_512'
set security vpn ipsec ike-group IKE01 proposal 1 dh-group '2'
set security vpn ipsec ike-group IKE01 proposal 1 encryption 'aes256'
set security vpn ipsec ike-group IKE01 proposal 1 hash 'sha2_512'
set security vpn ipsec site-to-site peer 169.47.243.43 authentication pre-shared-secret 'iamsecret'
set security vpn ipsec site-to-site peer 169.47.243.43 default-esp-group 'ESP01'
set security vpn ipsec site-to-site peer 169.47.243.43 ike-group 'IKE01'
set security vpn ipsec site-to-site peer 169.47.243.43 local-address '50.23.177.59'
set security vpn ipsec site-to-site peer 169.47.243.43 tunnel 1 local prefix '172.16.100.1/30'
set security vpn ipsec site-to-site peer 169.47.243.43 tunnel 1 remote prefix '172.16.200.1/30'
```

Esto configura un túnel genérico que direcciona el tráfico 172.16.x.x entre las dos máquinas. La Máquina B tiene 172.16.100.1 como dirección de bucle de retorno para proporcionar un punto final con el que realizar pruebas, mientras que la Máquina A tiene una máquina virtual en una VLAN direccionada para proporcionar el tráfico de origen del túnel.

Puede ver los resultados aquí:

```
[root@acs-jmat-migserver ~]# ping -c 5 172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.5 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.8 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.6 ms
--- 172.16.100.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 44.578/44.750/44.993/0.244 ms
```

Esto ilustra el tráfico bidireccional a través de este túnel IPsec. A continuación, puede aplicar una simple política de zona "allow all" a todas las interfaces de la Máquina A:

```
set security firewall name ALLOWALL default-action 'drop'
set security firewall name ALLOWALL rule 10 action 'accept'
set security firewall name ALLOWALL rule 10 protocol 'tcp'
set security firewall name ALLOWALL rule 10 state 'enable'
set security firewall name ALLOWALL rule 20 action 'accept'
set security firewall name ALLOWALL rule 20 protocol 'icmp'
set security firewall name ALLOWALL rule 20 state 'enable'
set security firewall name ALLOWALL rule 30 action 'accept'
set security firewall name ALLOWALL rule 30 protocol 'udp'
set security firewall name ALLOWALL rule 30 state 'enable'
```

A continuación, añada políticas entre las tres interfaces:

```
set security zone-policy zone INTERNET interface 'dp0bond1'
set security zone-policy zone INTERNET to PRIVATE firewall 'ALLOWALL'
set security zone-policy zone INTERNET to SERVERS firewall 'ALLOWALL'
set security zone-policy zone PRIVATE interface 'dp0bond0'
set security zone-policy zone PRIVATE to INTERNET firewall 'ALLOWALL'
set security zone-policy zone PRIVATE to SERVERS firewall 'ALLOWALL'
set security zone-policy zone SERVERS interface 'dp0bond0.1341'
set security zone-policy zone SERVERS to INTERNET firewall 'ALLOWALL'
set security zone-policy zone SERVERS to PRIVATE firewall 'ALLOWALL'
```

Una vez aplicado, el tráfico dejará de fluir, a pesar de que se haya establecido en `allow all`.
