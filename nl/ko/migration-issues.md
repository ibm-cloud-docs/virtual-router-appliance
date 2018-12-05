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

# Vyatta 5400 일반 마이그레이션 문제
다음 표에서는 Vyatta 5400 디바이스를 IBM Virtual Router Appliance로 마이그레이션한 후에 발생할 수 있는 일반적인 문제나 동작 변경사항에 대해 설명합니다. 어떤 경우에는 문제에 대한 임시 해결책도 있습니다.

## 상태 저장 방화벽에 대한 인터페이스 기반 글로벌 상태 정책

### 문제
릴리스 5.1에서 stateful 방화벽에 대한 "상태 정책 상태"를 설정할 때의 동작이 변경되었습니다. 릴리스 5.1 이전 버전에서는 상태 저장 방화벽의 `state - global -state -policy`를 설정하는 경우 vRouter가 세션의 자동 반환 통신에 대한 암묵적인 `Allow` 규칙을 자동으로 추가했습니다.

릴리스 5.1 이상에서는 `Allow` 규칙 설정을 Virtual Router Appliance에 추가해야 합니다. 상태 저장 설정은 Vyatta 5400 디바이스의 인터페이스에 대해 그리고 VRA 디바이스의 프로토콜에 대해 작동합니다.

### 임시 해결책
`firewall-in` 규칙이 유입/내부 인터페이스에 적용되는 경우 `Firewall-out` 규칙이 유출/외부 인터페이스에 적용되어야 합니다. 그렇지 않으면 반환 트래픽이 유출/외부 인터페이스에서 삭제됩니다.        

## 방화벽 규칙에서 상태 저장 사용

### 문제
`global-state-policy`가 구성되지 않으면 이 동작 변경사항이 적용되지 않습니다.

또한 각 규칙에서 `global-state-policy` 대신 `state enable` 옵션을 사용하는 경우 동작 변경사항이 적용되지 않습니다.

## 구역 기반 정책: 로컬 구역 처리

### 문제
구역 정책에 지정할 "로컬 구역" 유사 인터페이스가 없습니다.

### 임시 해결책
구역 기반 방화벽을 실제 인터페이스에 적용하고 인터페이스-방화벽을 루프백 인터페이스에 적용하여 이 동작을 시뮬레이션할 수 있습니다. 루프백 인터페이스의 방화벽은 라우터에서 유입되고 유출되는 모든 항목을 필터링합니다.

예를 들어 다음과 같습니다.

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## 방화벽, NAT, 라우팅 및 DNS에 대한 오퍼레이션 순서

### 문제
Masquerade Source NAT가 IBM Virtual Router Appliance에 배치되는 시나리오에서는 방화벽을 사용하여 인터넷에 대한 호스트의 액세스를 판별할 수 없습니다. 이는 NAT 이후 주소가 같기 때문입니다.

Vyatta 5400 디바이스의 경우 방화벽이 NAT 전에 수행되었기 때문에 이 조작이 가능했으며, 이 조작을 통해 호스트가 인터넷에 액세스하는 것을 제한할 수 있습니다.

### 임시 해결책
VRA에 새 라우팅 스킴이 필요합니다. ![라우팅 dns](./images/routing-dns.png)

## 정책 기반 라우팅 테이블

### 문제
configs의 단어 "Table"은 v5400 정책 기반 라우팅에서 선택사항이지만 VRA의 경우 조치가 `승인`되면 **테이블** 필드는 필수입니다. 조치가 VRA config에서 `drop`되는 경우에는 테이블 필드는 선택사항입니다. 

### 임시 해결책
"Table Main"은 Vyatta 5400 정책 기반 라우팅에서 사용할 수 있는 옵션입니다. VRA에서는 "routing-instance default"와 같습니다.

## 터널 인터페이스의 정책 기반 라우팅

### 문제
Virtual Router Appliance PBR(정책 기반 라우팅)에서 정책을 인바운드 트래픽에 대한 데이터 플레인 인터페이스에 적용할 수 있지만, 루프백, 터널, 브릿지, OpenVPN, VTI 및 IP 번호가 지정되지 않은 인터페이스에는 적용할 수 없습니다. 

### 임시 해결책
현재 이 문제에 대한 임시 해결책은 없습니다. 

## TCP-MSS

### 문제
IBM Virtual Router Appliance는 nftables를 사용하며 TCP-MSS는 지원하지 않습니다.

### 임시 해결책
현재 이 문제에 대한 임시 해결책은 없습니다. 

## OpenVPN

### 문제
Virtual Router Appliance에서 `push-route` 매개변수를 사용하는 경우 OpenVPN이 작동하지 않습니다. 

### 임시 해결책
`push-route` 대신에 `openvpn-option` 매개변수를 사용하십시오. 

## IPSEC + OSPF를 통한 GRE/VTI

### 문제
* VIF에 여러 서브넷이 구성되어 있는 경우 VRA의 해당 서브넷 간에 트래픽이 이동할 수 없습니다.                                              
* InterVlan Routing이 VRA에서 작동하지 않습니다.

### 임시 해결책
Implicit 허용 규칙을 사용하여 VIF 인터페이스 간의 트래픽을 승인하십시오. 

## IPSec

### 문제
IPSec(접두부 기반)이 IN 필터와 작동하지 않습니다. 

### 임시 해결책
IPSec(VTI 기반)을 사용하십시오.

## IPSEC 'match-none"

### 문제
Vyatta 5400 디바이스를 사용하면, 다음 방화벽 규칙이 허용됩니다. 

set firewall name allow rule 10 ipsec

그러나 IBM Virtual Router Appliance에서는 IPSec이 없습니다.

### 임시 해결책
VRA 디바이스에 대한 가능한 대체 규칙: 

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### 문제
Vyatta 5400 디바이스를 사용하면, 다음 방화벽 규칙이 허용됩니다. 

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

그러나 IBM Virtual Router Appliance에서는 IPSec이 없습니다.

### 임시 해결책
프로토콜 `ESP` 및 `AH`를 추가하십시오(각각 IP 프로토콜 50 및 51).

아래 표시된 대로 `action` 규칙은 `accept` 또는 `drop`일 수 있습니다. 

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## DNAT으로 사이트-투-사이트 IPSEC

### 문제
IPSec(접두부 기반)이 DNAT와 작동하지 않습니다.                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1)
===S-S-IPsec=== (12.0.0.1)
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

위의 스니펫은 IPSec 패킷이 Vyatta 5400에서 복호화된 후 DNAT 변환을 위한 간단한 설정 예입니다. 이 예제에는 `vyatta1 (11.0.0.1)` 및 `vyatta2 (12.0.0.1)`의 두 개의 vyatta가 있습니다. IPsec 피어링은 `11.0.0.1` 및 `12.0.0.1` 사이에 구축됩니다. 이 경우 클라이언트는 `10.103.0.1`에서 시작하여 `172.16.1.245`를 대상으로 합니다(엔드-투-엔드). 이 시나리오의 예상 동작은 목적지 주소 `172.16.1.245`가 패킷 헤더에서 `10.71.68.245`로 변환되는 것입니다. 

초기에 Vyatta 5400 디바이스는 인바운드 IPSec에서 DNAT를 수행하여 인터페이스를 종료하고 연결 추적 테이블을 사용하여 트래픽을 IPsec 터널로 정상적으로 반환했습니다.

Virtual Router Appliance에서는 이 구성이 동일하게 작동하지 않습니다. 세션은 작성되지만 conntrack 테이블이 DNAT 변경사항을 되돌린 후에는 반환 트래픽이 IPsec 터널을 우회합니다.그런 다음 VRA는 IPsec 암호화 없이 유선으로 패킷을 전송합니다. 업스트림 디바이스는 이 트래픽을 예상하지 않으며 대부분 삭제합니다.엔드-투-엔드 연결이 끊어진 상태에서 이는 의도된 동작입니다.

### 임시 해결책
위의 네트워킹 시나리오를 수용하기 위해 IBM은 RFE를 작성했습니다. 

해당 RFE가 현재 평가되고 있으며, 그 동안 다음 임시 해결책을 권장합니다.

**인터페이스 구성 명령**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN 구성 명령**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT 구성 명령**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**프로토콜 구성 명령**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR 구성 명령**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### 문제
PPTP는 더 이상 Virtual Router Appliance에서 지원되지 않습니다.                                                                                                                                                   

### 임시 해결책
대신 L2TP 프로토콜을 사용하십시오. 

## IPSec을 다시 시작하기 위한 스크립트

### 문제
VRRP 가상 주소가 고가용성 VPN의 IBM Virtual Router Appliance에 추가될 때마다 IPsec 디먼을 다시 초기화해야 합니다. 이는 IPsec 서비스가 IKE 서비스 디먼이 초기화될 때 VRA에 표시되는 주소에 대한 연결만 청취하기 때문입니다.

VRRP가 있는 VRA 쌍의 경우 마스터 라우터에 VRRP 가상 주소가 없으면 초기화하는 동안 디바이스에 있는 VRRP 가상 주소가 대기 라우터에 없을 수 있습니다. 따라서 VRRP 상태 전이가 발생할 때 IPsec 디먼을 다시 초기화하려면 마스터 및 백업 라우터에서 다음 명령을 실행하십시오.

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## 최근 개수 및 최근 시간

### 문제

다음 규칙은 임의의 주소를 사용하는 SSH에 대해 30초마다 SSH 연결을 3개로 제한하기 위한 것입니다.

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

IBM Virtual Router Appliance에서 이 규칙에는 다음과 같은 문제가 있습니다.

* 최근 개수 및 최근 시간에 대한 옵션은 더 이상 사용되지 않습니다. 

* 이전 문제로 인해, 규칙이 예상대로 작동할 수 없으며, 적용된 인터페이스에 대한 모든 SSH 연결을 차단합니다. 

### 임시 해결책
대신 CPP를 사용하십시오. 

## set system conntrack 문제

### 문제

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## set system conntrack 제한시간

### 문제
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## 시간 기반 방화벽

### 문제
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### 문제
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## S-S Ipsec VPN에서 중단된 특정 애플리케이션 또는 포트

### 문제

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
가능한 완료:
   <Enter> 현재 명령 실행
   port    임의의 TCP 또는 UDP 포트
   prefix  원격 IPv4 또는 IPv6 접두부                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## 로깅 동작의 중요한 변경사항

### 문제
Vyatta 5400 디바이스와 IBM Virtual Router Appliance 간의 로깅 동작에 중요한 변경사항이 있습니다. 세션당 로깅에서 패킷당 로깅으로 변경되었습니다. 

* 세션 로깅: 상태 저장 세션 상태 전이를 기록합니다. 

* 패킷 로깅: 규칙과 일치하는 모든 패킷을 기록합니다. 패킷 로깅이 로그 파일에서 "패킷 단위"로 기록되므로, 처리량과 디스크 용량에 대한 부담이 현저하게 감소합니다. 

* vRouter의 로깅 기능을 사용하여 방화벽 활동을 캡처할 수 있습니다. 로깅 기능과 마찬가지로 특정한 문제를 해결하는 경우에만 이 기능을 사용하고, 가능한 한 빨리 로깅을 사용 안함으로 설정해야 합니다.

방화벽/NAT 세션을 관리하는 상태 저장 방화벽은 "세션 단위"로 기록됩니다. 이 경우 세션 로깅을 사용할 것을 권장합니다. 각 설정 예는 아래에 설명되어 있습니다.

**세션/로깅**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**패킷 로깅 방화벽**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>`
