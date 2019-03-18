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
{:faq: data-hd-content-type='faq'}

# IBM Virtual Router Appliance의 기술 FAQ
{: #technical-faqs-for-ibm-virtual-router-appliance}

다음 자주 묻는 질문은 IBM© Virtual Router Appliance(VRA)의 구성과 Vyatta 5400에서 VRA로의 마이그레이션을 다룹니다.

## 사설 VLAN에 있는 호스트에서 인터넷 연결 트래픽을 허용할 수 있는 방법은 무엇입니까?
{:faq}

이 트래픽은 공용 소스 IP를 확보해야 하므로 소스 NAT는 공용 VRA가 포함된 사설 IP를 위장해야 합니다.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

위의 구성은 사설 `10.0.0.0/8` 네트워크의 서버에서 생성된 트래픽의 SNAT만 수행합니다.

이를 통해 인터넷 라우트 가능 소스 주소가 이미 있는 패킷을 방해하지 않게 됩니다.

## 인터넷 연결 트래픽을 필터링하고 특정 프로토콜/대상만 허용하는 방법은 무엇입니까?
{:faq}

이는 소스 NAT와 방화벽을 결합해야 하는 경우의 일반적인 질문입니다.

규칙 세트를 설계할 때 VRA의 오퍼레이션 순서에 유의하십시오.

즉, 방화벽 규칙은 SNAT *뒤에* 적용됩니다.

방화벽에서 특정 SNAT 플로우를 제외한 모든 출력 트래픽을 차단하기 위해 SNAT로 필터링 로직을 이동해야 할 수 있습니다.

예를 들어, 호스트의 HTTPS 인터넷 연결 트래픽만 허용하기 위한 SNAT 규칙은 다음과 같습니다.

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3`은 VRA의 공용 주소입니다. 

VRA의 VRRP 공용 주소 사용이 강력하게 권장되므로 호스트와 VRA 공용 트래픽을 구분할 수 있습니다.

`150.1.2.3`이 VRRP VRA 주소이고 `150.1.2.5`가 실제 dp0bond1 주소라고 가정하십시오.

`dp0bond1 out`에 적용된 Stateful 방화벽은 다음과 같습니다.

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

소스 NAT와 방화벽의 결합은 필요한 설계 목표를 달성합니다. 

규칙이 사용자의 설계에 적합하고 기타 규칙이 차단되어야 하는 트래픽을 허용하지 않는지 확인하십시오. 

## 구역 기반 방화벽을 통해 VRA 자체를 보호할 수 있는 방법은 무엇입니까?
{:faq}

VRA에는 `local zone`이 없습니다.

loopback에서 `local` 방화벽으로 적용되므로 대신 CPP(Control Plane Policing) 기능을 활용할 수 있습니다.

이는 Stateless 방화벽이며 VRA 자체에서 생성된 아웃바운드 세션의 리턴하는 트래픽을 명시적으로 허용해야 합니다.

## SSH를 제한하고 인터넷에서 수신되는 연결을 제한하는 방법은 무엇입니까?
{:faq}

인터넷의 SSH 연결을 허용하지 않고 또 다른 사설 주소에 대한 액세스 수단(예: SSL VPN)을 사용하는 것이 우수 사례로 간주됩니다.

기본적으로, 모든 인터페이스에서 VRA는 SSH를 허용합니다.
사설 인터페이스에서 SSH 연결만 청취하려면 다음 구성이 설정되어야 합니다.

```
set service ssh listen-address '10.1.2.3'
```

IP 주소가 VRA에 속하는 주소로 대체되어야 한다는 점에 유의하십시오.
