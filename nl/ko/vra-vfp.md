---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vfp, ipsec, firewall

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

# IPsec 및 구역 방화벽을 사용하여 VPF 인터페이스 구성
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

IPSec 데이터그램에 도달하면 방화벽 규칙을 통해 처리된 후 캡슐화가 해제됩니다. 새로 알려진 데이터그램은 인터페이스와 전혀 연관되어 있지 않습니다. 일반적으로, 문제가 되지 않으며 대상 인터페이스로 계속할 수 있지만, 구역 방화벽에서 데이터그램이 진행되는 것을 막습니다. 구역 정책의 인터페이스에서 발생하지 않는 데이터그램은 삭제됩니다. 그러나 VFP 인터페이스는 데이터그램이 인터페이스에서 발생하지 않았다고 구역 방화벽에 알리고, 규칙을 적용하도록 허용합니다.

IPsec 트래픽에 대해 작업하도록 VFP 인터페이스를 구성하려면, 먼저 단일 IP를 사용하여 VFP를 정의함으로써 기능 지점을 작성하십시오.

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

그런 다음 터널에 VFP를 추가하십시오.

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

다음으로, 인터페이스를 구역에 추가하십시오(이 경우 dp0bond1이 포함된 INTERNET 구역).

```
set security zone-policy zone INTERNET interface 'vfp0'
```

이제 서버에 대해 ping을 실행하십시오.

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

여러 터널에서 동일한 VFP 인터페이스를 사용하거나 다른 터널에 다른 VFP를 작성할 수 있으며, 단지 추가 VFP가 구역에 있으며 캡슐화되지 않은 프레임에 대해 트래픽을 허용하는 규칙을 포함하는지만 확인하십시오.

해당 구역에 VFP를 추가할 수도 있습니다. 예를 들어 다음과 같습니다.

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

VFP 인터페이스는 `tshark`와 같은 명령으로 모니터할 수 있으며 직접 트래픽을 라우팅할 수 있다는 점에서 "실제" 인터페이스이지만, 그렇게 하는 데에는 유용하지 않습니다. 터널의 정책과 일치하지 않는 다른 쪽 끝에 도달하는 트래픽은 삭제됩니다. VTI 인터페이스만큼 다재다능하지 않습니다.
