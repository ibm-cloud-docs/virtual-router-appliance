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

# 구역 방화벽에 대해 작업하는 IPsec 터널 설정
{: #setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls}

{{site.data.keyword.vra_full}}의 이전 버전에서, 정책 기본 라우팅을 사용하는 IPsec 터널은 구역 방화벽과 잘 작동하지 않았습니다. 버전 18.01에는, 이 문제를 해결하는 새로운 명령 세트가 있으며 이러한 명령은 지정된 터널에서 트래픽을 사용으로 설정하기 위해 "가상 기능 지점"을 사용하며, 여기에서 기능 지점은 구역 정책 구성에 포함할 엔드포인트를 제공하는 인터페이스의 역할을 합니다.

두 개 시스템 간의 IPsec을 사용하는 두 시스템의 예제 구성은 다음과 같습니다.

###시스템 A
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

###시스템 B
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

두 개 시스템 사이에 172.16.x.x 트래픽을 라우팅하는 일반 터널을 설정합니다. 시스템 B에 테스트할 엔드포인트를 제공하기 위해 루프백 주소로 172,16.100.1이 있는 반면, 시스템 A에는 터널에서 소스 트래픽을 제공하기 위해 라우팅된 VLAN에 가상 머신이 있습니다.

여기서 결과를 볼 수 있습니다. 

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

이 IPsec 터널 전체에서 양방향 트래픽을 보여줍니다. 다음으로, 단순 "allow all" 구역 정책을 시스템 A의 모든 인터페이스에 적용할 수 있습니다.

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

그런 다음 세 개 모든 인터페이스 사이에 정책을 추가하십시오.

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

적용되고 나면 `allow all`로 설정되어 있어도 트래픽이 더 이상 이동하지 않습니다. .
