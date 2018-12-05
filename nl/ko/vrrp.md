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

# HA 및 VRRP
VRA(Virtual Router Appliance)는 VRRP(Virtual Router Redundancy Protocol)를 고가용성 프로토콜로 지원합니다. 디바이스 배치는 활성/수동 방식으로 수행되며 여기서 한 시스템은 마스터이고 다른 시스템은 백업이 됩니다. 두 시스템의 모든 인터페이스는 동일한 "sync-group"의 멤버이므로 한 인터페이스에 결함이 있는 경우 동일한 그룹의 다른 인터페이스에도 결함이 발생하며 디바이스가 마스터가 되지 않습니다. 현재 백업은 마스터가 더 이상 킵얼라이브/하트비트 메시지를 브로드캐스트하지 않음을 발견하고 VRRP 가상 IP의 제어를 가정하여 마스터가 됩니다.

VRRP는 게이트웨이를 프로비저닝할 때 가장 중요한 구성 파트입니다. 고가용성 기능은 하트비트 메시지에 따라 다르므로 메시지가 차단되지 않는지 확인하는 것이 중요합니다.

## VRRP 가상 IP(VIP) 주소

VRRP 가상 IP 또는 VIP는 장애 복구가 발생할 때 마스터에서 복구 디바이스로 변경된 유동 IP 주소입니다. VRA를 배치하면 공인 및 사설 본딩 네트워크 연결과 각 인터페이스에 지정된 실제 IP를 가지게 됩니다. VIP는 디바이스가 독립형인지 또는 HA 쌍인지 여부에 상관 없이 두 인터페이스 모두에 지정됩니다. VRA와 연관된 VLAN의 서브넷에 대상 IP가 있는 트래픽은 이러한 VRRP VIP에 직접 전송됩니다.

게이트웨이 그룹에 대한 VRRP 가상 IP 주소를 변경해도 안되고 VRRP 인터페이스를 사용 안함으로 설정해도 안 됩니다. 이러한 IP 주소는 VLAN이 연결될 때 트래픽이 게이트웨이로 라우트되는 수단입니다. IP 주소가 없는 경우 트래픽을 softLayer BCR/FCR에서 게이트웨이로 전달할 수 없습니다.

## VRRP 그룹

VRRP 그룹은 그룹의 1차 또는 “마스터” 인터페이스에 대한 중복성을 제공하는 인터페이스 또는 가상 인터페이스의 클러스터로 구성됩니다. 그룹의 각 인터페이스는 일반적으로 별도의 라우터에 있습니다. 중속성은 각 시스템의 VRRP 프로세스에 의해 관리됩니다. VRRP 그룹에는 고유한 숫자 식별자가 있고 최대 20개의 가상 IP 주소를 지정할 수 있습니다.  

VRRP 그룹 ID는 softlayer에 의해 지정되고 이를 변경할 수 없습니다. 새 게이트웨이 그룹이 처음으로 FCR(Front Customer Router)/BCR(Backend Customer Router) 뒤에서 프로비저닝되면 VRRP 그룹 1을 받습니다. 후속 게이트웨이 그룹 프로비저닝은 충돌이 발생하지 않도록 이 값을 늘려서 다음 그룹이 그룹 2, 그룹 3 등으로 설정됩니다. 그런 다음 SoftLayer 프로비저닝 프로세스에 의해 계산되고 지정됩니다. 이 값을 변경하면 다른 활성 그룹과 충돌이 발생하고 마스터/마스터 경합으로 인해 두 게이트웨이 그룹이 모두 가동 중단될 수 있습니다.

이전 구성에서 마이그레이션하는 경우 VRRP 그룹 ID가 정적으로 지정되지 않았는지 확인하기 위해 구성 코드를 두 번 검사하는 것이 좋습니다.

VRRP 그룹 ID는 SoftLayer 데이터베이스에서 지속되므로 OS 다시 로드 또는 업그레이드 중에 동일한 그룹 ID 값이 사용됩니다. OS를 다시 로드하는 동안 사용자 수정 VRRP 그룹 ID를 시스템 지정 값으로 겹쳐씁니다.  

## VRRP 우선순위

게이트웨이 그룹의 첫 번째 시스템은 254의 우선순위를 가지며 이 값은 다음으로 프로비저닝된 디바이스에 대해 감소됩니다. 우선순위를 255로 설정해서는 안 됩니다. 이는 VIP "주소 소유자"를 정의하고 시스템이 실행되는 활성 마스터와 다른 구성으로 온라인 상태가 될 때 예상치 못한 동작이 발생할 수 있습니다.

## VRRP 선점

새로운 디바이스 또는 OS를 다시 로드하는 과정의 디바이스가 클러스터를 인계하지 않도록 선점은 모든 VRRP 인터페이스에서 항상 사용 안함으로 설정되여야 합니다.

## VRRP 통지 간격

여전히 서비스 중임을 알리기 위해 마스터 인터페이스 또는 VIF는 VRRP에 대한 IANA 지정 멀티캐스트 주소(IPv4의 경우 `224.0.0.18`, IPv6의 경우 `FF02:0:0:0:0:0:0:12`)를 사용하여 “통지”라는 “하트비트” 패킷을 LAN 세그먼트로 보냅니다.

기본적으로 간격은 1로 설정됩니다. 이 값은 증가할 수 있지만 5보다 큰 값을 설정하는 것은 좋지 않습니다. 사용량이 많은 네트워크에서는 모든 VRRP 알림이 마스터에서 백업 디바이스로 도착하는 데 1초보다 길게 걸릴 수 있으므로 트래픽이 많은 네트워크의 경우 값 2를 사용해야 합니다.

## VRRP 동기화(sync-group)

VRRP 동기화 그룹(“sync group”)의 인터페이스는 다음과 같이 동기화됩니다. 그룹의 한 인터페이스가 백업으로 장애 복구되면 그룹의 모든 인터페이스가 백업으로 장애 복구됩니다. 예를 들어 많은 경우에 마스터 라우터의 한 인터페이스가 실패하면 전체 라우터가 백업 라우터로 장애 복구됩니다.

이 값은 VRRP 그룹과 다릅니다. 이는 해당 그룹의 인터페이스가 결함을 등록하는 경우 디바이스에서 장애 복구되는 인터페이스를 정의합니다. 모든 인터페이스가 동일한 sync-group에 속하는 것이 좋지만 일부 인터페이스는 마스터가 되고 게이트웨이 IP를 가지며 다른 인터페이스는 백업이 되고 트래픽이 더 이상 올바르게 전달되지 않습니다. 활성/활성 구성은 지원되지 않습니다.

## VRRP rfc-compatibility

rfc-compatibility는 인터페이스에서 VRRP에 대한 RFC 3768 MAC 주소를 사용 또는 사용 안함으로 설정합니다. 이는 기본 인터페이스에서 사용되어야 하지만 구성된 VIF에서는 사용되지 않아야 합니다. 일부 가상 스위치(대부분 vmware)에서는 이를 사용하는 데 문제가 있으며 이로 인해 트래픽을 삭제하고 하이퍼바이저 호스트 시스템에서 게이트웨이 IP로 전송하지 않습니다. 이 설정만 그대로 두고 모든 VIF의 설정을 구성하지 마십시오.

## VRRP 인증
VRRP 인증에 대한 비밀번호를 설정하는 경우 인증 유형도 정의해야 합니다. 비밀번호를 설정하고 인증 유형을 정의하지 않은 경우 구성을 커미트할 때 시스템에서 오류가 발생합니다.

이와 유사하게 VRRP 인증 유형을 삭제하지 않고 VRRP 비밀번호를 삭제할 수 없습니다. 이렇게 하면 구성을 커미트할 때 시스템에서 오류가 발생합니다. VRRP 인증 비밀번호와 인증 유형을 둘 다 삭제하면 VRRP 인증을 사용할 수 없습니다.

IETF는 VRRP 버전 3에 인증을 사용하지 않기로 결정했습니다. 자세한 정보는 RFC 5798 VRRP를 참조하십시오.

## 버전 2/3 지원
VRA는 VRRP 버전 2(기본값)와 버전 3 프로토콜을 모두 지원합니다. 버전 2에서는 IPv4와 IPv6을 함께 사용할 수 없습니다. 그러나 버전 3에서는 IPv4와 IPv6을 동시에 사용할 수 있습니다.

## VRRP에서의 고가용성 VPN
VRA는 VRRP와 함께 한 쌍의 Virtual Router Appliance를 사용하여 하나의 IPsec 터널을 통해 연결을 유지보수하는 기능을 제공합니다. 한 라우터가 실패하거나 유지보수를 위해 작동이 중지되는 경우 새 VRRP 마스터 라우터가 로컬과 원격 네트워크 간의 IPsec 연결을 복원합니다.

VRRP를 사용한 고가용성 VPN을 구성하는 경우 VRRP 가상 주소가 VRA 인터페이스에 추가될 때마다 IPsec 디먼을 다시 초기화해야 합니다. IPsec 서비스는 IKE(Internet Key Exchange) 서비스 디먼이 초기화될 때 VRA에 표시되는 주소에 대한 연결만 청취하기 때문입니다.

마스터 및 백업 라우터에서 다음 명령을 실행하여 장애 복구가 발생하면 다음 예에서와 같이 VIP가 전달된 후 IPsec 디먼이 새 마스터 디바이스에서 다시 시작되도록 하십시오.

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## VRRP에서의 고가용성 방화벽

두 개의 디바이스가 HA 쌍인 경우 마스터 디바이스가 다른 디바이스에서의 액세스를 차단하지 않도록 주의해야 합니다. config-sync가 작동하려면 두 디바이스 간에 포트 443을 허용해야 하고 VRRP는 멀티캐스트 범위 224.0.0.0/24를 포함하여 보내고 받을 수 있어야 합니다.

## VRRP와 연관된 VLAN 서브넷

VRRP를 사용한 VIF 구성에는 가상 IP 주소(서브넷에서 첫 번째로 사용 가능한 IP, 해당 서브넷의 모든 항목이 라우트되는 게이트웨이 IP) 및 두 디바이스의 VIF에 대한 실제 인터페이스 IP 주소가 필요합니다. 사용 가능한 IP를 유지하려면 실제 인터페이스 IP가 192.168.0.0/30과 같은 대역 외 범위를 사용하는 것이 좋습니다. 여기서 한 디바이스는 .1의 주소를 가지며 다른 디바이스는 .2의 주소를 가지게 됩니다. 다중 VRRP 가상 IP를 가질 수 있지만 각 VIF에 하나의 실제 주소만 필요합니다.

다음은 두 개의 사설 VLAN 및 세 개의 서브넷이 포함된 VRRP 구성의 예제입니다.

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* vrrp sync-group은 vrrp 그룹과 다릅니다. sync-group에 속하는 인터페이스가 상태를 변경하면 동일한 sync-group의 기타 모든 멤버가 동일한 상태로 전이됩니다.
* VLAN 인터페이스(VIF)의 vrrp-group 수는 원시 인터페이스 또는 기타 VLAN의 vrrp-group 수와 같을 필요가 없습니다. 그러나 VLAN 10에서와 같이 하나의 vrrp-group에서 동일한 VLAN의 모든 가상 주소를 유지하는 것이 좋습니다.
* 원시 VLAN의 실제 인터페이스 주소(예: dp0bond1: 169.110.20.28/29)는 항상 VIP와 동일한 서브넷에 있지 않습니다(169.110.21.26/29).

## 수동 VRRP 장애 복구
vrrp 장애 복구를 강제 실행해야 하는 경우 현재 VRRP 마스터인 디바이스에서 다음을 실행하여 수행할 수 있습니다.

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

그룹 ID는 원시 인터페이스의 vrrp 그룹 ID이고, 위에 언급된 대로 쌍에서 다를 수 있습니다.

## 연결 동기화
두 개의 VRA 디바이스가 HA 쌍인 경우 두 디바이스 간의 상태 저장 연결을 추적하는 것이 유용할 수 있습니다. 이렇게 하면 장애 복구가 발생하는 경우 실패한 디바이스에 있는 모든 연결의 현재 상태가 백업 디바이스로 복제되므로 현재 활성 세션(예: SSL 연결)을 처음부터 다시 빌드할 필요가 없습니다. 처음부터 다시 빌드하는 경우 사용자에게 혼란을 줄 수 있습니다.

이를 연결 추적 동기화라고 합니다.

이를 구성하려면 연결 추적 정보를 보내기 위해 사용하는 인터페이스인 장애 복구 방법과 원격 피어의 IP를 선언해야 합니다.

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

다른 VRA는 동일한 구성이지만 `remote-peer`는 다릅니다.

다른 인터페이스에 많은 연결이 들어오면 링크가 포화 상태가 될 수 있고 선언된 링크에서 다른 트래픽과 경쟁하게 됩니다.

## VRRP 시작 지연 기능
Vyatta OS 버전 1801p 이상에는 새 `vrrp` 명령이 포함되어 있습니다. 

`vrrp`는 가상 라우터에 대한 책임을 LAN의 VRRP 라우터 중 하나에 동적으로 지정하는 선택(election) 프로토콜을 지정합니다. 가상 라우터와 연관된 IPv4 또는 IPv6 주소를 제어하는 VRRP 라우터를 마스터라고 부르며, 이러한 IPv4 또는 IPv6 주소로 전송된 패킷을 전달합니다. 선택(election) 프로세스는 마스터를 사용할 수 없게 되는 경우 전달 책임에 대해 동적 장애 복구 기능을 제공합니다. 모든 프로토콜 메시징은 IPv4 또는 IPv6 멀티캐스트 데이터그램을 사용하여 수행됩니다. 결과적으로, 프로토콜은 IPv4/IPv6 멀티캐스트를 지원하는 다양한 멀티액세스 LAN 기술을 통해 작동할 수 있습니다. 

네트워크 트래픽을 최소화하기 위해, 각 가상 라우터의 마스터만 주기적 VRRP 광고 메시지를 보냅니다. 우선순위가 더 높은 경우를 제외하고 백업 라우터는 마스터를 선점하려고 시도하지 않습니다. 더 선호하는 경로가 제공되지 않는 한, 서비스 중단이 발생하지 않습니다. 관리 면에서, 모든 선점 시도를 금지하는 것도 가능합니다. 마스터가 제공되지 않는 경우에는 짧은 지연 후에 최우선순위 백업이 마스터로 상태 전이되며, 최소한의 서비스 인터럽트만으로 가상 라우터 책임의 상태 전이를 제어할 수 있습니다. 

**참고:** IBM Cloud 프로비저닝된 배치에서, 상태 지연 값은 기본값으로 설정됩니다. 장애 복구 및 고가용성 메소드를 테스트할 때 사용자 재량으로 이 값을 변경하려고 할 수 있습니다. 


### 선점 대 비선점

`vrrp` 프로토콜은 마스터 역할을 수행할 우수 피어 등 네트워크에서 어떤 VRRP 피어가 더 높은 우선순위를 갖는지 결정하는 로직을 정의합니다. 기본 구성을 사용하여, VRRP이 선점을 수행하는 데 사용되며 이는 네트워크에서 새로운 더 높은 우선순위 피어가 마스터 역할의 장애 복구를 강제 실행하게 됨을 의미합니다. 

선점이 사용 안함으로 설정되는 경우 기존의 더 낮은 우선순위의 피어가 더 이상 네트워크에서 사용 가능하지 않으면 더 높은 우선순위의 피어가 마스터 역할의 장애 복구만 강제 실행합니다. 더 높은 우선순위 피어가 피어 자체 또는 해당 네트워크 연결 중 하나에 대한 신뢰성 문제로 인해 주기적으로 문제가 발생하기 시작하는 상황에 더 잘 대처하므로, 선점을 사용 안함으로 설정하는 것이 때때로 실제 사례에서 유용합니다. 또한 네트워크 통합을 완료하지 못한 새로운 더 높은 우선순위의 피어에 너무 일찍 장애 복구를 실행하지 않도록 방지하는 데 유용합니다. 

### 선점의 제한사항 및 가정

VRRP 피어가 선점을 사용하지 않도록 구성된 경우에는 일부 사례에서 선점이 발생한 것처럼 “보일”수 있지만, 실제는 표준 VRRP 장애 복구입니다. 위에 설명된 대로, VRRP는 현재 선택된 마스터 라우터의 가용성을 확인하기 위한 수단으로 IP 멀티캐스트 데이터그램을 활용합니다. VRRP 피어의 실패를 감지하는 계층 3 프로토콜이므로, VRRP 및 계층 1에서 2의 네트워크 스택이 준비되고 통합될 때가지 VRRP에서 장애 복구 발견이 지연됩니다. 일부의 경우 VRRP를 실행 중인 네트워크 인터페이스에서 인터페이스가 작동되는 프로토콜을 확인할 수 있지만, 스패닝 트리 또는 본딩과 같은 다른 기본 서비스가 완료되지 않았을 수 있습니다. 결과적으로, 피어 간의 IP 연결을 설정할 수 없습니다. 이 경우 네트워크에 있는 현재 마스터 피어로부터 VRRP 메시지를 감지할 수 없으므로, 새로운 더 높은 우선순위 피어의 VRRP가 마스터가 됩니다. 통합 후에, 짧은 기간 동안 두 개의 마스터 VRRP 피어가 존재하게 되어 VRRP의 듀얼 마스터 로직이 실행됩니다. 더 높은 우선순위의 피어가 마스터로 남게 되고 더 낮은 우선순위의 피어는 백업 피어가 됩니다. 이 시나리오는 “비선점” 기능에 대한 실패를 입증한 것처럼“보일”수 있습니다. 

### 시작 지연 기능

다른 원인이 되는 요소뿐만 아니라 인터페이스 작동 이벤트 중에 네트워크 스택의 낮은 레벨 통합이 지연되는 것과 연관된 문제를 해결하기 위해, "Startup Delay"라는 새로운 기능이 1801p 패치에 도입되었습니다. 이 기능은 "다시 로드"한 머신의 VRRP 상태를 사전 정의된 지연 후에 INIT 상태로 유지되도록 해주며, 이 지연은 네트워크 운영자가 구성할 수 있습니다. 이 지연 값의 유연성을 통해 네트워크 운영자는 실제 환경에 따라 네트워크 및 디바이스의 특성을 사용자 정의할 수 있습니다. 

### 명령 세부사항

```
vrrp {
start-delay <0-600>
}
```

`start-delay`는 0(기본값)과 600초 사이의 값을 가질 수 있습니다.

### 예제 구성

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
