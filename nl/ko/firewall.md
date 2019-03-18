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

# IBM Firewalls 관리
{: #manage-your-ibm-firewalls}

VRA(Virtual Router Appliance)에는 디바이스를 통해 라우팅된 VLAN을 보호하기 위해 방화벽 규칙을 처리하는 기능이 있습니다. VRA의 방화벽은 두 단계로 나눌 수 있습니다.

1. 하나 이상의 규칙 세트 정의
2. 인터페이스 또는 구역에 규칙 세트 적용. 구역은 하나 이상의 네트워크 인터페이스로 구성됩니다.

규칙이 원하는 대로 제대로 작동하는지와 새 규칙이 디바이스에 대한 관리 액세스를 제한하지 않는지 확인하기 위해 방화벽 규칙 작성 후 규칙을 테스트하는 것이 중요합니다.

`dp0bond1` 인터페이스에서 규칙을 조작하는 동안 `dp0bond0`을 사용하여 디바이스에 연결하는 것이 좋습니다. IPMI(Intelligent Platform Management Interface)를 사용하여 콘솔에 연결하는 것도 선택사항입니다.

## Stateless 대 Stateful
기본적으로 방화벽은 Stateless으로 설정되지만 필요한 경우 Stateful으로 구성할 수 있습니다. Stateless 방화벽은 양방향 트래픽에 대한 규칙이 필요한 반면에 Stateful 방화벽은 연결을 추적하고 허용된 플로우의 리턴 트래픽을 자동으로 허용합니다. Stateful 방화벽을 구성하려면 Stateful으로 운영할 규칙을 지정해야 합니다.

`tcp`, `udp` 또는 `icmp` 트래픽의 'Stateful' 추적을 사용으로 설정하려면 다음 명령을 실행하십시오.

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

`global-state-policy` 명령은 해당 프로토콜을 명시적으로 설정하는 방화벽 규칙과 일치된 트래픽의 상태만 추적합니다. 예를 들어 다음과 같습니다.

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

`GLOBAL_STATELESS`가 `protocol tcp`를 지정하지 않으므로 `global-state-policy tcp` 명령은 이 규칙에 적용되지 않습니다. 

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

이 경우, `protocol tcp`가 명시적으로 정의됩니다. `global-state-policy tcp` 명령은 `GLOBAL_STATEFUL_TCP`의 규칙 1과 일치하는 트래픽의 Stateful 추적을 사용합니다.


개별 방화벽을 'Stateful'으로 구성하려면 다음을 수행하십시오.

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
이를 통해 `global-state-policy` 명령의 존재와 상관 없이 Stateful으로 추적될 수 있고 `TEST`의 규칙 1과 일치하는 모든 트래픽의 Stateful 추적이 사용 가능합니다. 

## 지원된 Stateful 추적용 ALG
FTP와 같은 일부 프로토콜은 일반 Stateful 방화벽 오퍼레이션이 추적할 수 있는 좀 더 복잡한 세션을 활용합니다. 
Stateful로 관리될 이 프로토콜을 사용하는 사전 구성된 모듈이 있습니다.
각 프로토콜의 성공적인 사용을 위해 ALG 모듈이 필요하지 않는 한 ALG 모듈을 사용하지 않는 것이 좋습니다.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## 방화벽 규칙 세트
방화벽 규칙은 여러 인터페이스에 규칙을 쉽게 적용할 수 있도록 이름 지정된 세트로 그룹화됩니다. 각 규칙 세트에는 연관된 기본 조치가 있습니다. 다음 예를 고려하십시오. 
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

규칙 세트 `ALLOW_LEGACY`에 두 개의 규칙이 정의되어 있습니다. 첫 번째 규칙은 `network-group1`이라는 주소 그룹에서 오는 트래픽을 삭제합니다. 두 번째 규칙은 `network-group2`라는 주소 그룹의 Telnet 포트(`tcp/23`)를 대상으로 하는 트래픽을 버리고 로깅합니다. 기본 조치는 다른 모든 조치가 허용됨을 표시합니다.

## 데이터 센터 액세스 허용
IBM©은 데이터 센터에서 실행되는 시스템에 서비스와 지원을 제공하는 여러 IP 서브넷을 제공합니다. 예를 들어 DNS 분석기 서비스는 `10.0.80.11` 및 `10.0.80.12`에서 실행됩니다. 다른 서브넷은 프로비저닝 및 지원에 사용됩니다. 이 데이터 센터에서 사용한 IP 범위는 [이 주제](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges)에서 찾을 수 있습니다.

`accept` 조치로 방화벽 규칙 세트를 시작할 때 적합한 `SERVICE-ALLOW` 규칙을 배치하여 데이터 센터 액세스를 허용할 수 있습니다. 규칙 세트를 적용해야 하는 위치는 구현되는 라우팅과 방화벽 설계에 따라 다릅니다.

작업의 중복을 최소화하는 위치에 방화벽 규칙을 배치하는 것이 좋습니다. 예를 들어 백엔드 서브넷을 `dp0bond0`에 인바운드하도록 허용하는 것은 백엔드 서브넷을 VLAN 가상 인터페이스로 아웃바운드하도록 허용하는 것보다 효과적이지 않습니다.

### 인터페이스별 방화벽 규칙
VRA에 방화벽을 구성하는 한 가지 방법은 각 인터페이스에 방화벽 규칙 세트를 적용하는 것입니다. 이 경우 인터페이스는 데이터플레인 인터페이스(`dp0s0`) 또는 가상 인터페이스(`dp0bond0.303`)일 수 있습니다. 각 인터페이스에는 세 가지 방화벽을 지정할 수 있습니다.

`in` - 이 방화벽은 인터페이스를 통해 들어오는 패킷에 대해 검사됩니다. 이러한 패킷은 VRA를 통과하거나 VRA가 목적지가 될 수 있습니다.

`out` - 이 방화벽은 인터페이스를 통해 나가는 패킷에 대해 검사됩니다. 이러한 패킷은 VRA를 통과하거나 VRA에서 생성될 수 있습니다.

`local` - 이 방화벽은 VRA의 직접적인 대상이 되는 패킷에 대해 검사됩니다.

인터페이스는 각 방향에서 여러 규칙 세트를 적용할 수 있습니다. 규칙 세트는 구성 순서 대로 적용됩니다. 인터페이스별 방화벽을 사용하여 VRA 디바이스에서 생성된 트래픽을 방화벽으로 차단할 수는 없습니다.

예를 들어 `ALLOW_LEGACY` 규칙 세트를 `bp0s1` 인터페이스의 `in` 옵션으로 지정하려면 다음 구성 명령을 사용합니다. 

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## CPP(Control Plane Policing)
CPP(Control Plane Policing)는 원하는 인터페이스에 지정되는 방화벽 정책을 구성하도록 허용하고 VRA에 들어오는 패킷에 대해 이러한 정책을 적용하여 Virtual Router Appliance에 대한 공격으로부터 보호합니다.

CPP는 데이터플레인 인터페이스 또는 루프백 인터페이스와 같은 모든 유형의 VRA 인터페이스에 지정되는 방화벽 정책에 `local` 키워드가 사용될 때 구현됩니다. VRA를 통과하는 패킷에 적용되는 방화벽과 달리 제어 플레인을 나가고 들어오는 트래픽에 대한 방화벽 규칙의 기본 조치는 `Allow`입니다. 기본 동작을 원하지 않는 경우 명시적 삭제 규칙을 추가해야 합니다.

VRA는 기본 CPP 규칙 세트를 템플리트로 제공합니다. 다음을 실행하여 이를 사용자 구성에 병합할 수 있습니다. 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

이 규칙 세트가 병합되면 `CPP`라는 이름의 새 방화벽 규칙 세트가 루프백 인터페이스에 추가되고 적용됩니다. 사용자 환경에 맞게 이 규칙 세트를 수정하는 것이 좋습니다.

CPP 규칙이 Stateful일 수 없으며 유입 트래픽에만 적용됩니다.

## 구역 방화벽
Virtual Router Appliance의 다른 방화벽 개념은 구역 기반 방화벽입니다. 구역 기반 방화벽 조작에서는 인터페이스가 구역에 지정되고(인터페이스당 한 구역만 지정) 방화벽 규칙 세트는 구역 내의 모든 인터페이스가 동일한 보안 레벨을 가지고 자유롭게 라우팅할 수 있다는 개념으로 구역 간의 경계에 지정됩니다. 트래픽은 한 구역에서 다른 구역으로 전달될 때만 심사됩니다. 구역은 자기 구역으로 들어오는 명시적으로 허용되지 않는 트래픽을 삭제합니다.

인터페이스는 구역에 속하거나 인터페이스별 방화벽 구성을 가질 수 있습니다. 인터페이스는 둘 다 할 수는 없습니다.

세 부서가 있고 각각의 부서에 자체 VLAN이 있는 다음 오피스 시나리오를 생각해 보십시오. 

* 부서 A - VLAN 10 및 20(인터페이스 dp0bond1.10 및 dp0bond1.20)
* 부서 B - VLAN 30 및 40(인터페이스 dp0bond1.30 및 dp0bond1.40)
* 부서 C - VLAN 50(인터페이스 dp0bond1.50)

구역을 각 부서에 작성하고 해당 부서의 인터페이스를 구역에 추가할 수 있습니다. 다음 예에서 이에 대해 설명합니다.
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

`commit` 명령은 각 구역을 인터페이스로 채우고 기본 삭제 규칙은 외부에서 구역으로 들어오려고 시도하는 트래픽을 버립니다. 예에서 VLAN 10 및 20은 동일한 구역(`DEPARTMENTA`)에 있기 때문에서 트래픽을 전달할 수 있지만 VLAN 30은 다른 구역(`DEPARTMENTB`)에 있기 때문에 VLAN 10 및 VLAN 30은 트래픽을 전달할 수 없습니다.

각 구역 내의 인터페이스는 자유롭게 트래픽을 전달할 수 있으며 구역 간의 상호작용에 대한 규칙을 정의할 수 있습니다. 한 구역에서 다른 구역으로 나간다는 관점으로 규칙 세트가 구성됩니다. 

다음 명령은 규칙을 구성하는 방법을 보여줍니다.

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

이 명령은 DEPARTMENTC에서 DEPARTMENTB로의 전이를 `ALLOW_PING`이라는 규칙 세트와 연관시킵니다. DEPARTMENTC 구역에서 DEPARTMENTB 구역으로 들어오는 트래픽이 이 규칙 세트에 대해 검사됩니다.

구역 DEPARTMENTC에서 구역 DEPARTMENTB로 이동하는 이 지정은 역방향에 대한 명령을 작성하지 않는다는 것을 알고 있어야 합니다. 구역 DEPARTMENTB에서 구역 DEPARTMENTC로의 트래픽을 허용하는 규칙이 없는 경우 트래픽(ICMP 응답)은 DEPARTMENTC의 호스트로 돌아가지 않습니다.

`ALLOW_PING`은 DEPARTMENTB 구역의 인터페이스(dp0bond1.30 및 dp0bond1.40)에서 `out` 방화벽으로 적용됩니다. 이는 구역 정책으로 설치되므로, 규칙 세트에 대해 소스 구역의 인터페이스(dp0bond1.50)에서 생성되는 트래픽만 확인합니다.

## 세션 및 패킷 로깅
VRA는 두 가지 유형의 로깅을 지원합니다.

1. 세션 로깅.  ``security firewall session-log`` 명령을 사용하여 방화벽 세션 로깅을 구성합니다.
  
	UDP, ICMP 및 모든 비TCP 플로우의 경우 세션은 플로우 수명 동안 네 가지 상태로 전이됩니다. 각 전이에서 메시지를 로깅하도록 VRA를 구성할 수 있습니다. TCP에는 더 많은 수의 상태 전이가 있으며 각각의 상태 전이를 로깅하도록 구성할 수 있습니다.  

2. 패킷당 로깅. 방화벽 또는 NAT 규칙에 키워드 ``log``를 포함시켜 규칙과 일치하는 모든 네트워크 패킷을 로깅합니다.

	패킷당 로깅은 패킷 전달 경로에서 발생하며 많은 수의 출력을 생성합니다. 이는 VRA의 처리량을 현저하게 줄일 수 있으며 로그 파일에 사용되는 디스크 공간을 대폭 늘릴 수 있습니다. 디버깅을 위해서만 패킷당 로깅을 사용하는 것이 좋습니다. 모든 운영을 위해서는 Stateful 세션 로깅을 사용해야 합니다.
