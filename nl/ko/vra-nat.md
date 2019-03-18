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

# 접두부 기반 IPsec이 포함된 NAT 사용
{: #using-nat-with-prefix-based-ipsec}

[IPsec 및 구역 방화벽으로 VFP 인터페이스 구성](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls) 주제에서, VFP 인터페이스를 작성하고 IPsec 터널과 함께 사용하도록 설정했습니다. 

한 가지 추가 제한사항으로 인바운드 및 아웃바운드 인터페이스 선언뿐만 아니라 NAT 규칙에서 동일한 인터페이스를 사용할 수 있습니다. 

다음은 몇 가지 예제 NAT 규칙입니다.

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

이전 예는 동일한 IP에 대한 양방향의 표준 일대일 소스 및 대상 NAT입니다. 그러나 NAT 트래픽이 적절하게 터널을 통과하도록 하기 위해, 다른 쪽 끝에 대한 정적 라우트가 필요합니다.

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

정적 라우트를 사용하는 이유는 IPsec 디먼이 원격 접두부에 대한 커널 라우트를 이미 작성했기 때문입니다.

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

`10.177.137.251`의 소스로 `172.16.100.2`에 ping을 실행하면, 트래픽이 `dp0bond1`을 통해 이동하여 터널로 이전하는 데 실패하고 NAT 규칙과 적절하게 일치하지 않습니다. 정적 라우트에서 이를 수정합니다.

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

이 명령은 `vfp0`을 통해 받는 트래픽에 대한 더 구체적인 라우트를 작성합니다. 

이 때 NAT는 구성된 대로 작동하며 터널을 통해 트래픽이 이동하게 됩니다. 

**참고:** NAT를 사용하면, `vfp0` 가상 인터페이스에서 트래픽을 가리키는 IPsec 원격 접두부보다 더 작은 CIDR(동일한 크기일 수 없음)을 사용하는 라우트가 필요합니다.

모든 사항이 준비되면 ping을 실행하여 확인할 수 있습니다.

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
