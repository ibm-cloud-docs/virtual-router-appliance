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

# Configurando uma interface VFP com IPsec e firewalls de zona
Quando um datagrama IPSec chega, ele é processado por meio de regras de firewall e, em seguida, desencapsulado. O novo datagrama que emerge não está associado a nenhuma interface. Normalmente, isso não é um problema e pode continuar para a interface de destino, mas os firewalls de zona evitarão que o datagrama progrida. Qualquer datagrama que não vier de uma interface em uma política de zona será descartado. Uma interface VFP, no entanto, informa ao firewall de zona que o datagrama veio realmente de uma interface, permitindo a aplicação de regras. 

Para configurar uma interface VFP para funcionar com tráfego de IPsec, primeiro crie um ponto de recurso definindo o VFP com um único IP:

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Em seguida, inclua o VFP no túnel:

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Em seguida, inclua a interface em uma zona (neste caso, a zona INTERNET com dp0bond1):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

Agora execute ping do servidor:

```
[root@acs-jmat-migserver ~]# ping -c 5 172.16.100.1 PING 172.16.100.1 (172.16.100.1) 56(84) bytes de dados.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.3 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.8 ms

--- 172.16.100.1 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 4006ms

rtt min/avg/max/mdev = 44.377/44.728/44.996/0.243 ms
```

É possível usar a mesma interface VFP em vários túneis ou criar VFPs diferentes para outros túneis, basta assegurar-se de que quaisquer VFPs adicionais estejam em uma zona e tenham regras que permitam o tráfego para os quadros não encapsulados.

Também é possível incluir VFPs em sua própria zona. Por exemplo:

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

Embora a interface VFP seja uma interface "real" em que pode ser monitorada com comandos como `tshark` e pode rotear o tráfego diretamente, não é útil fazer isso. Qualquer tráfego que chegar à outra extremidade que não corresponda à política do túnel será descartado. Não é tão versátil quanto uma interface VTI seria.
