---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

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


# {{site.data.keyword.vra_full}} 시작하기
{: #getting-started}

{{site.data.keyword.vra_full}}(VRA)는 x86 베어메탈 서버용 최신 Vyatta 5600 운영 체제를 제공합니다. VRA는 고가용성(HA) 또는 독립형 구성으로 제공되며, 이를 통해 방화벽, 트래픽 쉐이핑, 정책 기반 라우팅, VPN 및 기타 기능이 포함된 모든 기능을 갖춘 엔터프라이즈 라우터를 통해 사설 및 공용 네트워크 트래픽을 선택적으로 라우팅할 수 있습니다. 

VRA 최소 서버 요구사항에서는 10Gbps의 네트워크 용량마다 8GB의 RAM 및 하나의 CPU 코어를 요구합니다. 예를 들어 듀얼 10Gbps 공용 및 사설 업링크가 있는 시스템은 최소 4개의 코어를 필요로 합니다. 또한 암호화를 사용하여 VPN 서비스를 설정하려는 경우 추가적인 코어를 추가할 수 있습니다. VPN 서비스에 대한 추가적인 코어를 추가하면 VRA가 데이터를 라우팅하고 동시에 암호화/복호화할 때 과부하로 인해 중단되지 않게 됩니다. 

## VRA({{site.data.keyword.vra_full}}) 주문
{: #order-vra}

VRA를 주문하려면 다음 프로시저를 수행하십시오. 

1. 브라우저의 [IBM Cloud UI 콘솔![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window}에서 게이트웨이 어플라이언스 페이지를 열고 계정에 로그인하십시오.  

  [IBM Cloud 카탈로그![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com)의 왼쪽 상단에서 탐색 메뉴를 선택한 후 **클래식 인프라 > 네트워크 > 게이트웨이 어플라이언스**를 선택하여 이 페이지로 이동할 수도 있습니다.
  {: tip}

2. **게이트웨이 공급업체** 섹션에서 **AT&T** 옵션을 선택하십시오(이 옵션이 선택되면 파란색 체크 표시가 단추에 표시됨). 동일한 단추의 드롭 다운에서 대역폭(20Gbps 또는 2Gbps)을 선택하십시오. 

  <img src="images/ordering_vra.png" alt="그림" style="width: 500px;"/>

3. **게이트웨이 어플라이언스** 섹션에서 **호스트 이름** 및 **도메인** 이름 정보를 입력하십시오. 이 필드는 이미 기본 정보로 채워져 있으므로 값이 올바른지 확인하십시오. 원하는 경우 **고가용성** 옵션을 선택한 후 원하는 데이터 센터 **위치**와 드롭 다운 메뉴에서 원하는 특정 **팟(Pod)**을 선택하십시오. 

  이미 연관된 VLAN이 있는 팟(Pod)만 여기에 표시됩니다. 나열되지 않은 팟(Pod)에 게이트웨이 어플라이언스를 프로비저닝하려면 먼저 VLAN을 작성하십시오.
  {: note}

4. **구성** 섹션에서 RAM 및 SSH 키(필요한 경우)를 선택하여 프로세서를 선택하십시오. 

  <img src="images/ordering_vra_2.png" alt="그림" style="width: 600px;"/>
  
  2단계에서 선택한 라이센스 버전을 기반으로 적절한 프로세서가 선택됩니다. 하지만 다른 RAM 구성을 선택할 수 있습니다.
  {: note}

5. **스토리지 디스크** 섹션에서 스토리지 요구사항을 충족하는 옵션을 선택하십시오.  

  RAID0 및 RAID1 옵션은 핫 스페어(기본 컴포넌트가 실패하는 즉시 서비스에 배치될 수 있는 백업 컴포넌트)로서 데이터 손실에 대해 추가적으로 보호하기 위해 사용할 수 있습니다.
  {: note}

  VRA당 최대 4개의 디스크가 있을 수 있습니다. RAID 구성은 미러링되기 때문에 RAID 구성을 사용하는 "디스크 크기"가 사용 가능한 디스크 크기입니다.
  {: note}

  상세한 로그를 생성하는 네트워크 진단을 실행하려면 기본 디스크 설정보다 많은 설정을 예약하십시오.
  {: tip}

6. **네트워크 인터페이스** 섹션에서 **업링크 포트 속도**를 선택하십시오. 기본 선택사항은 단일 인터페이스이지만 중복 및 사설 전용 옵션도 있습니다. 요구사항에 가장 적합한 항목을 선택하십시오. 

  네트워크 인터페이스 **추가 기능** 섹션에서는 필요한 경우 IPv6 주소를 선택할 수 있으며 추가적으로 포함된 기본 옵션을 표시합니다.  
  
8. 선택사항을 검토하고 서드파티 서비스 계약을 읽었음을 확인한 후 **작성**을 클릭하십시오. 주문은 자동으로 확인됩니다. 

주문이 승인되면 VRA({{site.data.keyword.vra_full}}) 프로비저닝이 자동으로 시작됩니다. 프로비저닝 프로세스가 완료되면 새 VRA가 게이트웨이 어플라이언스 목록 페이지에 표시됩니다. 게이트웨이 이름을 클릭하여 게이트웨이 세부사항 페이지를 여십시오. 디바이스의 IP 주소, 로그인 사용자 이름 및 비밀번호를 찾을 수 있습니다.  

  <img src="images/gateway_details.png" alt="그림" style="width: 500px;"/>

IBM Cloud 카탈로그에서 VRA를 주문하고 구성하고 나면 동일한 설정으로 디바이스 자체도 구성해야 한다는 점을 기억하십시오.
{: tip}

## VLAN 및 게이트웨이 어플라이언스의 역할
{: #vlans-and-the-gateway-appliance-s-role}

VLAN(가상 LAN)은 실제 네트워크를 여러 가상 세그먼트로 나누는 메커니즘입니다. 편의를 위해 선택된 여러 VLAN에서의 트래픽을 단일 네트워크 케이블을 통해 전달할 수 있으며, 일반적으로 이 프로세스를 "트렁킹"이라고 합니다.

VRA({{site.data.keyword.vra_full}})는 VRA 서버와 게이트웨이 어플라이언스 설비라는 두 개의 파트로 전달됩니다. 게이트웨이 어플라이언스는 VRA와 연결할 VLAN을 선택하기 위한 인터페이스(GUI 및 API)를 제공합니다. VLAN을 게이트웨이 어플라이언스에 연결하면 해당 VLAN과 모든 서브넷을 VRA에 다시 라우팅("또는 트렁킹")하여 필터링, 전달 및 보호를 제어할 수 있습니다. 게이트웨이 어플라이언스와 연관된 모든 VLAN의 경우 해당 VLAN은 VRA가 연결되는 스위치 포트에서 허용되며 해당 VLAN의 서브넷은 VRA의 공인 VRRP IP로 정적으로 라우팅되거나(서브넷이 공인 서브넷인 경우) VRA의 사설 VRRP IP로 정적으로 라우팅됩니다(서브넷이 사설 서브넷인 경우). 이 라우팅은 공용 트래픽과 개인용 트래픽 각각에 대해 FCR(Frontend Customer Router) 또는 BCR(Backend Customer Router)인 VRA가 뒤에 있는 라우터에서 수행됩니다.  

기본적으로 VRRP는 사용 안함으로 설정되므로 독립형 vyatta에서도 VLAN 트래픽이 작동하도록 VRRP를 사용으로 설정해야 합니다. 이는 연관된 VLAN의 서브넷이 VRA에 지정된 VRRP IP 또는 가상 주소로 라우팅된 결과입니다. 자세한 정보는 [VRRP 가상 IP(VIP) 주소](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses)를 참조하십시오.
{: important}

연관된 VLAN의 서버는 VRA({{site.data.keyword.vra_full}})를 통해서만 다른 VLAN에서 연결될 수 있습니다. VLAN을 무시하거나 연관 해제하는 경우가 아니면 VRA를 우회할 수 없습니다. 

기본적으로 새 게이트웨이 어플라이언스는 두 개의 제거 불가능한 "전이" VLAN과 연결할 수 있습니다(공용 및 사설에 하나씩). 이는 일반적으로 관리에 사용되고 VRA 명령으로 별도로 보안 설정할 수 있습니다.

전이 VLAN은 인터넷에서 격리되는 서버 또는 컨테이너와 같은 기타 장치를 유지하는 동안 트래픽을 라우팅할 수 있도록 방화벽 또는 로드 밸런서와 같은 네트워크 장치를 위한 것입니다.

이와 비교해 보면, "게이트웨이" VLAN은 서버 및 컨테이너와 같은 디바이스가 호스팅되는 위치입니다.

VRA는 게이트웨이 어플라이언스를 통해 VRA와 연결된 VLAN만 관리할 수 있습니다.

게이트웨이 어플라이언스 세부사항 페이지에서 VLAN을 관리하는 방법에 대한 정보는 [VLAN 관리](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans)를 참조하십시오.
