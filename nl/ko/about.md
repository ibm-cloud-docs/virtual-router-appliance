---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# VRA 정보

IBM Virtual Router Appliance(VRA)는 **x86** 베어메탈 서버용 최신 Vyatta 5600 운영 체제를 제공합니다. 이는 고가용성(HA) 또는 독립형 구성으로 제공됩니다.

Virtual Router Appliance는 방화벽, 트래픽 쉐이핑, 정책 기반 라우팅, VPN 및 기타 여러 기능이 포함되어 있는 전체 기능을 갖춘 엔터프라이즈 라우터를 통해 사설 및 공용 네트워크 트래픽을 선택적으로 라우팅합니다. VRA는 손쉬운 구성을 통해 성능을 제공합니다. 일반 하드웨어 서버에서 실행되는 유지보수의 이점이 있습니다. VRA 하드웨어 어플라이언스는 다중 VLAN에 대한 라우팅 로드를 처리하도록 크기가 조정되며 중복 네트워크 링크 및 중복 RAID 어레이와 함께 주문 가능합니다. 모든 VRA 기능은 고객이 관리합니다.  

**대안:** FSA(FortiGate Security Appliance) 10Gbps는 단일 테넌트(전용)이며, AV(AntiVirus), IPS(Intrusion Prevention) 및 웹 필터링과 같은 차세대 기능이 포함되어 있는 처리량(10Gbps)이 높은 하드웨어 방화벽입니다. 유사한 목표를 달성하기 위해 VRA의 대안이 될 수 있습니다. 자세한 정보는 [FSA 문서](/docs/infrastructure/fortigate-10g/getting-started.html#getting-started)를 참조하십시오.

## 방화벽
외부 위협으로부터 환경을 보호하기 위해 Virtual Router Appliance를 방화벽으로 활용할 수 있습니다. 애플리케이션이 실행되는 포트에 대한 인바운드 또는 아웃바운드 네트워크 트래픽을 허용하거나 거부하기 위해 방화벽 규칙을 추가할 수 있으며, 고유한 네트워크 내의 트래픽을 필터링할 수 있습니다. 상태 저장 IPv4 및 IPv6 필터링을 수행하여 중요한 데이터를 보호하도록 Virtual Router Appliance를 구성할 수도 있습니다.

## 가상 사설망(VPN) 게이트웨이
Virtual Router Appliance를 네트워크 게이트웨이 디바이스로 프로비저닝하여 VPN 터널링을 통해 현장의 데이터 센터 또는 오피스를 IBM Cloud에 연결하십시오. 엔터프라이즈 데이터 센터 또는 오피스에서 IBM Cloud 네트워크로의 통신을 보호하기 위해 IPsec 사이트-투-사이트 VPN 터널을 사용할 수 있습니다. 기타 VPN 옵션은 원격 액세스 IPsec VPN(클라이언트-투-사이트), OpenVPN, GRE, L2TP 및 DMVPN입니다.

[Supplemental VRA documentation 섹션](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)에서 Brocade VPN Configurations 안내서를 참조하십시오.

## 네트워크 주소 변환(NAT)
Virtual Router Appliance에서는 소스 NAT를 사용하여 서버가 인터넷에 액세스할 수 있는 동안 공용 네트워크 인터페이스 없이 애플리케이션과 데이터베이스 서버를 프로비저닝할 수 있습니다. 또한 보안 향상을 위해 대상 NAT를 사용하여 서버를 게이트웨이 장치 뒤로 숨길 수도 있습니다.

## 엔터프라이즈급 라우팅

서로 다른 격리된 네트워크에 있는 다중 계층 애플리케이션의 경우 Virtual Router Appliance를 사용하면 이러한 네트워크 간의 연결을 보다 유연하게 구축할 수 있습니다. BGP를 사용하여 동적 라우팅을 설정할 수 있으며, 이를 통해 IBM Cloud 라우터에 고유 공인 IP 영역을 나타낼 수 있습니다. 또한 BGP는 다양한 터널과 직접 링크 솔루션을 사용하는 경우 사용자 정의 사설 네트워크 구성에 대한 추가 유연성도 제공합니다.

[Supplemental VRA documentation 섹션](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)에서 Brocade BGP Configurations 안내서를 참조하십시오.
