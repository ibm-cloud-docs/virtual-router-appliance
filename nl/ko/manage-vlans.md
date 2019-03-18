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

# VLAN 관리
{: #managing-your-vlans}

[게이트웨이 어플라이언스 세부사항 화면](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)에서 다양한 조치를 수행할 수 있습니다.

## 게이트웨이 어플라이언스에 VLAN 연결

VLAN을 라우팅하려면 먼저 게이트웨이 어플라이언스에 연결해야 합니다. VLAN 연결은 나중에 게이트웨이 어플라이언스에 라우팅할 수 있도록 적합한 VLAN을 네트워크 게이트웨이에 연결하는 링크입니다. 연결 프로세스가 VLAN을 게이트웨이 어플라이언스에 자동으로 라우팅하지는 않습니다. VLAN은 게이트웨이로 라우팅되기 전에는 계속해서 프론트엔드 및 백엔드 고객 라우터를 사용합니다. 

VLAN은 한 번에 한 게이트웨이에만 연결할 수 있으며 방화벽이 있어서는 안 됩니다. 다음 프로시저를 수행하여 VLAN을 네트워크 게이트웨이에 연결하십시오.

1. 고객 포털에서 [게이트웨이 어플라이언스 세부사항 화면에 액세스](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)하십시오. 
2. **VLAN 연결** 드롭 다운 목록에서 원하는 VLAN을 선택하십시오.
3. **연결** 단추를 클릭하여 VLAN을 연결하십시오.

VLAN을 게이트웨이 어플라이언스에 연결하면 게이트웨이 어플라이언스 세부사항 화면의 연결된 VLAN 섹션에 표시됩니다. 이 섹션에서 VLAN을 게이트웨이에 라우팅하거나 게이트웨이에서 연결 해제할 수 있습니다. 위의 단계를 반복하여 언제든지 적합한 추가 VLAN을 게이트웨이 어플라이언스에 연결할 수 있습니다.

## 연결된 VLAN 라우팅

연결된 VLAN은 게이트웨이 어플라이언스에 링크되지만 VLAN 내외부로의 트래픽은 VLAN이 라우팅될 때까지 게이트웨이에 도달하지 않습니다. 연결된 VLAN을 라우팅하면 모든 프론트엔드 및 백엔드 트래픽이 고객 라우터와 달리 게이트웨이 어플라이언스를 통해 라우팅됩니다. 

다음 프로시저를 수행하여 연결된 VLAN을 라우팅하십시오.

1. 고객 포털에서 [게이트웨이 어플라이언스 세부사항 화면에 액세스](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)하십시오. 
2. 연결된 VLAN 섹션에서 원하는 VLAN을 찾으십시오.
3. 조치 드롭다운 메뉴에서 **VLAN 라우팅**을 선택하십시오.
4. **예**를 클릭하여 VLAN을 라우팅하십시오. 

VLAN을 라우팅하면 모든 프론트엔드 및 백엔드 트래픽이 고객 라우터에서 네트워크 게이트웨이로 이동합니다. 트래픽 및 게이트웨이 어플라이언스 자체와 관련된 추가 제어는 게이트웨이 관리 도구에 액세스하여 수행할 수 있습니다. [게이트웨이 어플라이언스를 우회하여](#bypass-gateway-appliance-routing-for-a-vlan) 언제든지 네트워크 게이트웨이를 통한 라우팅을 중단할 수 있습니다.

## VLAN에 대한 게이트웨이 어플라이언스 라우팅 우회

VLAN을 라우팅하면 모든 프론트엔드 및 백엔드 트래픽이 네트워크 게이트웨이를 통해 이동합니다. 트래픽이 프론트엔드 및 백엔드 고객 라우터(FCR 및 BCR)로 되돌아가도록 언제든지 게이트웨이 어플라이언스를 우회할 수 있습니다. 

VLAN을 우회하면 VLAN이 계속해서 네트워크 게이트웨이에 연결될 수 있습니다. VLAN이 게이트웨이 어플라이언스와 연결되지 않아야 하는 경우 [게이트웨이 어플라이언스에서 VLAN 연결 해제](#disassociate-a-vlan-from-a-gateway-appliance)를 참조하십시오. 

다음 프로시저를 수행하여 VLAN에 대한 게이트웨이 라우팅을 우회하십시오.

1. 고객 포털에서 [게이트웨이 어플라이언스 세부사항 화면에 액세스](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)하십시오. 
2. 연결된 VLAN 섹션에서 원하는 VLAN을 찾으십시오.
3. 조치 드롭다운 메뉴에서 **VLAN 우회**를 선택하십시오.
4. **예**를 클릭하여 게이트웨이를 우회하십시오. 

네트워크 게이트웨이를 우회하면 모든 프론트엔드 및 백엔드 트래픽이 VLAN과 연결된 FCR 및 BCR을 통해 라우팅됩니다. VLAN은 계속해서 게이트웨이 어플라이언스와 연결되며 언제든지 게이트웨이 어플라이언스로 다시 라우팅될 수 있습니다.

## 게이트웨이 어플라이언스에서 VLAN 연결 해제

[연결](#associate-a-vlan-to-a-gateway-appliance)을 통해 VLAN을 한 번에 한 게이트웨이 어플라이언스에 링크할 수 있습니다. 연결을 사용하면 VLAN을 언제든지 게이트웨이 어플라이언스에 라우팅할 수 있습니다. VLAN을 다른 게이트웨이 어플라이언스에 연결해야 하거나 VLAN을 게이트웨이에 연결하지 않아야 하는 경우 연결 해제가 필요합니다. 연결 해제는 VLAN에서 게이트웨이로의 "링크"를 제거하며, 이를 통해 필요한 경우 다른 게이트웨이로 연결할 수 있습니다. 

다음 프로시저를 수행하여 게이트웨이 어플라이언스에서 VLAN을 연결 해제하십시오.

1. 고객 포털에서 [게이트웨이 어플라이언스 세부사항 화면에 액세스](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)하십시오. 
2. 연결된 VLAN 섹션에서 원하는 VLAN을 찾으십시오.
3. **조치** 드롭다운 메뉴에서 **연결 해제**를 선택하십시오. 
4. **예**를 클릭하여 VLAN을 연결 해제하십시오. 

게이트웨이 어플라이언스에서 VLAN을 연결 해제하면 VLAN을 다른 게이트웨이에 연결할 수 있습니다. 또한 VLAN을 언제든지 게이트웨이 어플라이언스로 다시 연결할 수 있습니다. 게이트웨이 어플라이언스에서 VLAN을 연결 해제하면 VLAN의 트래픽을 게이트웨이를 통해 라우팅할 수 없습니다. VLAN을 라우팅하려면 먼저 게이트웨이 어플라이언스에 연결해야 합니다.

## 동일한 네트워크 인터페이스를 통해 여러 VLAN 라우팅
Virtual Router Appliance는 동일한 네트워크 인터페이스(`dp0bond0` 또는 `dp0bond1`)를 통해 여러 VLAN을 라우팅할 수 있습니다. 이는 스위치 포트를 트렁크 모드로 설정하고 디바이스에 가상 인터페이스(VIF)를 구성하여 수행할 수 있습니다.

예를 들어 다음과 같습니다. 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

위의 명령은 `dp0bond0` 인터페이스에 두 개의 가상 인터페이스를 작성합니다. 인터페이스 `dp0bond0.1432`는 VLAN 1432의 트래픽을 처리하고 인터페이스 `dp0bond0.1693`은 VLAN 1693의 트래픽을 처리합니다.

## 단일 VLAN에 여러 서브넷 추가

다음은 끝에 공용 VLAN(1451)의 샘플 서브넷(`159.8.67.96/28`)이 추가된 예제 구성입니다. 각 VIF(VLAN 인터페이스)의 주소는 BCR(Backend Customer Router) 또는 FCR(Frontend Customer Router)에서 라우팅됩니다. 두 Vyattas 사이의 VRRP/고가용성 통신에만 사용합니다. 

서브넷은 사용하지 않은 사설 IP 공간에서 선택할 수 있습니다. 결과적으로 `10.0.0.0/8`은 일반적으로 여기에서 제외됩니다. 아래 예에는 `192.168.0.0/16`의 서브넷을 선택하지만 `172.16.0.0/12`의 서브넷도 사용할 수 있습니다. 

`virtual-address`를 통해 새 서브넷을 구성해야 합니다. 대부분의 경우 서브넷의 게이트웨이 IP 주소를 구성해야 합니다. 그러면 VIF에 바인드된 게이트웨이 IP를 VRA 뒤의 새 서브넷에 설정된 베어메탈 또는 Virtual Server의 다음 게이트웨이 주소로 사용합니다. 

다음 예에서는 VRA에서 `159.8.67.98/28` 서브넷의 모든 트래픽을 관리할 수 있도록 VIF에 바인드되는 `159.8.67.97/28`을 보여줍니다.

```
set interfaces bonding dp0bond0 vif 1623 address '192.168.10.2/30'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 virtual-address '10.127.132.129/26'
set interfaces bonding dp0bond0 vif 1750 address '192.168.20.2/30'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 virtual-address '10.126.19.129/26'
set interfaces bonding dp0bond1 vif 788 address '192.168.150.2/30'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 virtual-address '159.8.106.129/28'
set interfaces bonding dp0bond1 vif 1451 address '192.168.200.2/30'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.67.97/28'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.86.49/29'
```
