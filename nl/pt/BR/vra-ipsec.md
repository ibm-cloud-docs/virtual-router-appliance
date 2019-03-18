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

# Configurando um túnel de IPsec que funciona com firewalls de zona
{: #setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls}

Nas versões anteriores do Virtual Router Appliance, os túneis IPsec que usam Policy Based Routing não funcionaram bem com firewalls de zona. Com a versão 18.01, há um novo conjunto de comandos que aborda esse problema usando "pontos de recurso virtual" para permitir o tráfego de túneis designados, em que o ponto de recurso age como uma interface que fornece um terminal para inclusão em uma configuração de política de zona.

Segue uma configuração de exemplo de duas máquinas com IPsec entre elas:

###Máquina A
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

Isso configura um túnel genérico que roteia o tráfego 172.16.x.x entre as duas máquinas. A Máquina B tem 172.16.100.1 como um endereço de loopback para fornecer um terminal com o qual testar, enquanto a Máquina A tem uma máquina virtual em uma VLAN roteada para fornecer tráfego de origem através do túnel. 

É possível ver o resultado aqui:

```
[root@acs-jmat-migserver ~]# ping -c 5 172.16.100.1 PING 172.16.100.1 (172.16.100.1) 56(84) bytes de dados.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.5 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.8 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.6 ms
--- 172.16.100.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 44.578/44.750/44.993/0.244 ms
```

Isso ilustra o tráfego bidirecional através desse túnel IPsec. Em seguida, é possível aplicar uma política de zona simples "permitir tudo" a todas as interfaces na Máquina A:

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
 
Em seguida, inclua políticas entre as três interfaces:

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

Depois de aplicado, o tráfego não mais fluirá, apesar de ser configurado como `allow all`.
