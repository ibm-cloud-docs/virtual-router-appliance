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

# 设置使用区域防火墙的 IPsec 隧道
{: #setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls}

在先前版本的虚拟路由器设备中，使用基于策略的路由的 IPsec 隧道与区域防火墙无法很好地协作。在 V18.01 中，有一组新增的命令可解决此问题，这些命令使用“虚拟功能点”支持来自指定隧道的流量，其中功能点充当接口来提供要包含在区域策略配置中的端点。

两台机器之间使用 IPSec 的配置示例如下：

###机器 A
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

###机器 B
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

这将设置一个通用隧道，用于路由两台机器之间的 172.16.x.x 流量。机器 B 的 172.16.100.1 作为回送地址，以提供要用于测试的端点，而机器 A 在路由的 VLAN 上具有虚拟机，可在隧道中提供源流量。 

您可能会看到以下结果：

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

这说明了在此 IPSec 隧道中有双向流量。接下来，您可以对机器 A 上的所有接口应用简单的“全部允许”区域策略：

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
 
然后，在所有三个接口之间添加策略：

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

应用后，流量就不会再流动，虽然是设置为 `allow all`。
