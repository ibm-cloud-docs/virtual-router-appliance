---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400에 방화벽 기능 추가(상태 비저장 및 상태 저장)

각 인터페이스에 방화벽 규칙 세트를 적용하는 것은 Brocade 5400 vRouter(Vyatta) 디바이스를 사용할 때 방화벽 설정의 한 방법입니다. 각 인터페이스에는 세 가지 방화벽 인스턴스(In, Out 및 Local)가 있으며 각 인스턴스에는 적용할 수 있는 규칙이 있습니다. 기본 조치는 삭제로, 특정 트래픽이 규칙 1부터 N까지의 방식으로 적용될 특정 트래픽을 허용하는 규칙이 포함됩니다. 일치되면 방화벽은 일치 규칙의 특정 조치를 적용합니다. 

세 가지 방화벽 인스턴스는 다음과 같습니다(단, **하나만** 적용됨). 

* **IN:** 방화벽은 인터페이스에 들어오고 Brocade 시스템을 통과하는 패킷을 필터링합니다. 특정 SoftLayer IP 범위를 허용하여 관리(Ping, 모니터링 등)를 위해 프론트 엔드 및 백엔드에 액세스할 수 있어야 합니다.
* **OUT:** 방화벽은 인터페이스를 나가는 패킷을 필터링합니다. 이러한 패킷은 Brocade 시스템을 통과하거나 시스템에서 생성될 수 있습니다. 
* **LOCAL:** 방화벽은 시스템 인터페이스를 통해 Brocade vRoute 시스템 자체를 대상으로 하는 패킷을 필터링합니다. 제한되지 않는 외부 IP 주소로 Brocade vRouter 디바이스에 들어오는 액세스 포트에 대한 제한을 설정해야 합니다.

ICMP(Internet Control Message Protocol)*(Ping - IPv4 에코 응답 메시지)*를 끄기 위해 예제 방화벽 규칙을 Brocade 5400 vRouter의 인터페이스에 설정하려면 다음 단계를 수행하십시오(즉, 상태 비저장 조치이며 상태 저장 조치는 나중에 검토됨). 

1\. 설정된 구성을 확인하려면 명령 프롬프트에 *show configuration* 명령을 입력하십시오. 디바이스에 설정한 모든 명령의 목록이 표시됩니다(마이그레이션을 결정하고 모든 구성을 표시하려는 경우 유용할 수 있음). ICMP가 계속해서 디바이스에 사용되고 있음을 나타내는 *set firewall all-ping 'enable'* 명령에 유의하십시오. 

2\. *configure*를 입력하십시오.

3\. *set firwall all-ping 'disable'*을 입력하십시오.

4\. *commit*를 입력하십시오.

지금 Brocade 5400 vRouter 디바이스에 대한 ping을 실행하려는 경우 더 이상 응답을 받을 수 없게 됩니다. 

방화벽 규칙을 라우팅된 트래픽에 지정하려면 규칙이 Brocade 5400 vRouter의 **인터페이스** 또는**구역 작성**에 적용된 후 구역에 적용되어야 합니다. 

이 예제에서는 지금까지 사용된 VLAN에 대한 구역이 작성됩니다.

**VLAN = 구역**

bond1 = dmz

bond1.2007 = prod(프로덕션용)

bond0.2254 = private(개발용)

bond1.1280 = 차후 사용을 위해 예약됨

bond1.1894 = 차후 사용을 위해 예약됨

**방화벽 규칙 작성**

구역을 실제로 작성하기 전에 구역에 적용될 방화벽 규칙을 작성하는 것이 좋습니다. 구역 작성 전에 규칙을 작성하면 즉시 규칙을 적용할 수 있으며(구역 작성 후 규칙 작성과 비교 시) 그런 다음 규칙 애플리케이션을 위해 구역으로 다시 돌아가야 합니다. 

**참고:** 구역 및 규칙 세트 모두 기본 조치 명령문이 있습니다. 구역-정책을 사용하는 경우 기본 조치는 zone-policy 명령문으로 설정되고 규칙 10,000으로 표시됩니다. 또한 방화벽 규칙 세트의 기본 조치가 모든 트래픽을 **삭제**하는 것임을 기억하십시오. 

명령은 다음을 수행합니다. 

* 기본 조치가 포함된 **dmz2private**라는 방화벽 규칙을 작성하여 패킷을 삭제합니다. 
* **dmz2private**이라는 규칙에 대해 허용된 트래픽 및 거부된 트래픽의 로깅을 사용합니다.


1\. 명령 프롬프트에서 *configure*를 입력하십시오.

2\. 프롬프트에서 다음 명령을 입력하십시오.

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

다음 명령 세트는 리턴할 확립된 세션의 트래픽을 허용하도록 **iptables**를 사용으로 설정하는 것입니다. 기본적으로, **iptables**는 명시적 규칙이 필수라는 이유로 이를 허용하지 않습니다. 

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

세 번째 명령 세트를 입력하면 TCP/UDP가 포트 22(SSH의 기본값)를 통과합니다. 

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

마지막 명령 세트를 입력하면 TCP/UDP가 포트 80(HTTP의 기본값)을 통과합니다. 

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. 종료 시 모든 규칙이 적용되었는지 확인하려면 *commit*를 입력하십시오. 

4\. 명령 프롬프트에서 *show firewall name dmz2private*를 입력하여 구성을 확인하십시오.

작성한 다음 방화벽 규칙은 **dmz** 구역에 적용됩니다. 규칙은 **public**으로 이름이 지정됩니다. 프롬프트에서 다음 명령을 입력하십시오.

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**참고:** 방화벽 규칙은 **prod**에서 **dmz**를 통해 아웃바운드를 플로우해야 합니다. 네트워크 플로우에 대한 문제를 해결하려면 *sudo tcpdump -i any host 10.52.69.202* 명령을 사용하십시오. 

**구역 작성**

구역은 인터페이스의 논리적 표시입니다. 명령은 다음을 수행합니다. 

* 기본 조치가 포함된 구역 및 **dmz**라고 하는 정책을 작성하여 이 구역을 대상으로 하는 패킷을 삭제합니다.
* **dmz** 정책을 설정하여 **bond1** 인터페이스를 사용합니다.
* **prod** 정책을 설정하여 **bond1.2007** 인터페이스를 사용합니다.
* 기본 조치가 포함된 **private**이라고 하는 구역 정책을 작성하여 이 구역을 대상으로 하는 패킷을 삭제합니다.
* **private**이라고 하는 정책을 설정하여 **bond0.2254** 인터페이스를 사용합니다.

1\. 프롬프트에서 다음 명령을 입력하십시오.

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. 구역에 방화벽 정책을 설정하려면 다음 명령을 사용하십시오. 

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

구역 정책에 방화벽을 적용하지 않으려는 경우 특정 인터페이스에 방화벽 규칙을 적용할 수 있습니다. 인터페이스에 규칙을 적용하려면 다음 명령을 사용하십시오.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## 상태 저장 방화벽

*상태 저장* 방화벽은 이전에 표시된 플로우의 테이블을 유지하고 이전 패킷과의 관계에 따라 패킷을 허용하거나 삭제할 수 있습니다. 일반적인 규칙으로, 애플리케이션 트래픽이 많이 발생하는 위치에서 상태 저장 방화벽이 보통 선호됩니다.  

<span style="text-decoration: underline">*Brocade 5400 vRouter는 기본 구성으로 연결 상태를 추적하지 않습니다. 방화벽은 다음 조건 중 하나가 충족될 때까지 상태 비저장입니다.*</span>

* 방화벽의 구성 및 상태 매개변수가 있는 규칙
* 방화벽 상태-정책의 구성
* NAT의 구성
* 투명 웹 프록시 서비스의 사용
* WAN 로드-밸런싱 구성의 사용

## 상태 비저장 방화벽

*상태 비저장* 방화벽은 독립적으로 모든 소켓을 고려합니다. 패킷은 IP 또는 TCP/UDP(Transmission Control Protocols/User Datagram Protocol) 헤더의 소스 및 대상 필드와 같이 기본 액세스 제어 목록(ACL)만 따라 허용되거나 삭제될 수 있습니다. 상태 비저장 Brocade 5400 vRouter는 연결 정보를 저장하지 않고 모든 패킷에 대해 이전 플로우와의 관계를 검색해야 하는 요구사항이 없으며, 둘 모두 약간의 메모리와 짧은 CPU 시간을 사용합니다. 그러므로 원시 전달 성능은 상태 비저장 시스템에서 극대화됩니다. Brocade는 상태 저장에 특정한 기능이 필요하지 않은 경우 최상의 성능을 위해 라우터를 상태 비저장 상태로 유지합니다. 
