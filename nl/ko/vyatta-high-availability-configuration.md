---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 고가용성 구성
{: #vyatta-5400-high-availability-configuration}

Vyatta 고가용성은 VRRP(Virtual Routing Redundancy Protocol)의 사용을 통해 지원됩니다. 각 게이트웨이 그룹에는 두 가지 기본 VRRP IP 주소(사설 및 네트워크의 공용 측)가 있습니다. 

**참고:** 사설 전용 Vyatta에는 사설 VRRP만 있습니다. 

이 IP 주소는 게이트웨이 멤버와 연관된 VLAN의 모든 서브넷을 라우팅하기 위한 Softlayer 네트워크 인프라의 대상 IP입니다. 하나의 Vyatta만 VRRP IP를 동시에 실행되도록 하고, 게이트웨이 그룹의 다른 멤버는 VRRP IP를 관리적으로 작동 중단되도록 합니다.

"config-sync" 구성 명령이 포함된 두 개의 Vyatta 사이에 구성을 동기화할 수 있습니다. 이 구성을 통해 하나의 멤버는 특정 옵션의 구성을 그룹의 다른 Vyatta로 푸시하고, 선택적으로 이를 수행할 수 있습니다. 방화벽 규칙만, IPsec 구성만 또는 규칙 세트의 조합만 푸시할 수 있습니다. 

config-sync가 해당 인터페이스를 온라인으로 전환하여 즉각적으로 다른 Vyatta에 대한 변경사항을 커미트하므로 IP 주소 또는 다른 네트워크 구성을 푸시하지 않는 것이 좋습니다. 인터페이스 및 서비스를 동적으로 사용하려면 상태 전이 스크립트를 사용하여 장애 복구에 대한 이 조치를 수행해야 합니다. 또한 장애 복구를 좀 더 쉽게 관리할 수 있도록 연관된 VLAN에서 게이트웨이 IP에 대한 추가 VRRP IP 주소를 사용하는 것이 좋습니다.

두 머신의 기본 VRRP 구성은 다음 샘플 구성과 같이 표시됩니다.

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP에는 VRRP 통지를 보내기 전에 먼저 가상 인터페이스에 바인드될 실제 IP 주소가 필요합니다. 많은 경우, 기본 서브넷에서 IP만 추가할 수 있거나(단, 추가 프로비저닝과 충돌할 수 있음) 서버에 할당된 모든 기본 서브넷 IP가 있는 VLAN을 라우팅하려고 할 수 있습니다. 이를 해결하기 위해 서로 대화하는 데 사용할 수 있는 두 Vyatta에서 대역 외 IP 주소 쌍을 사용할 수 있습니다. 예를 들면, 다음과 같습니다.

첫 번째 Vyatta에서:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

두 번째 Vyatta에서:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

이런 경우 두 Vyatta에는 할당된 서브넷과 충돌하지 않는 고유한 IP가 있습니다. 원하는 대부분의 개인용 범위를 선택할 수 있으며, IPsec 터널을 통한 서브넷 범위 또는 Softlayer의 주소와 충돌하는 10.0.0.0/8 주소를 보유할 수 있는 기타 라우트와 충돌하지 않도록 소형 서브넷만 선택하면 문제가 없습니다.

또한 "sync-group" 이름도 추가하려고 할 수 있습니다. 모든 VRRP 주소는 같은 sync-group의 일부입니다. 하나의 인터페이스에서 실패가 발생하면 같은 sync-group의 모든 인터페이스에 장애 조치가 발생합니다. 실패가 발생하지 않으면 MASTER와 BACKUP 상태로 종료할 수 있습니다. bond0 및 bond1 원시 VLAN 구성에 같은 이름을 사용하십시오.

참고: bond0 및 bond1 VRRP 구성에는 rfc3768-compatibility의 행이 있을 수 있습니다. vif의 VRRP에는 필요하지 않고 bond0 및 bond1의 원시 VLAN에만 필요합니다.

새로 프로비저닝된 게이트웨이 쌍의 config-sync에는 최소 구성만 있습니다.


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

규칙을 추가하여 마이그레이션할 구성 분기를 결정해야 합니다. 예를 들어, 방화벽 및 ipsec 구성이 푸시되도록 하려면 다음 명령을 추가하십시오.


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

이러한 명령이 커미트되면 방화벽 또는 ipsec 구성에 대한 변경사항이 커미트 시 다른 Vyatta에 푸시됩니다.

**참고:** sync-group 및 sync-map은 두 가지 다른 구성입니다. sync-map은 구성 변경사항을 다른 Vyatta로 푸시하는 규칙을 위한 구성입니다. sync-group은 한 번에 하나씩 대신 그룹으로 장애 복구될 VRRP IP를 위한 구성입니다. 하나를 구성해도 다른 하나에 영향을 주지 않습니다.
