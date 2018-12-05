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

# 보안 설정된 경계 작성
네트워크 격리의 기본적인 측면은 보안 설정된 경계의 설정입니다. 보안 경계는 IBM Cloud에서 호스틍되는 고객 자산에 대한 공용 인터넷 간의 트래픽을 제어합니다. 보안 설정된 경계는 VPN(Virtual Private Network) 터널 및 IBM Cloud Direct Link를 사용하여 고객 기업과의 직접 연결을 사용으로 설정합니다. 

보안 설정된 경계는 SPS(Secure Perimeter Segment)를 사용하여 보안 경계 안에 있는 자산 간의 네트워크를 분리합니다. 이 분리에는 액세스 제어 및 세그먼트 간의 서비스 트래픽 격리를 포함하여 몇 가지 이점이 있습니다. SPS(Secure Perimeter Segment)는 SPS로 들어오고 나가는 트래픽을 관리하기 위해 프론트 엔드 VLAN과 백엔드 VLAN의 두 개 VLAN(Virtual Local Area Network) 및 VLAN에 연결된 VRA로 구성됩니다. 보안 경계는 여러 SPS(Secure Perimeter Segment)를 포함할 수 있습니다(예를 들어, 고가용성 목적을 위해). 

## 보안 설정된 경계 설정

다음은 보안 설정된 경계를 설정하기 위한 단계입니다. 이러한 단계의 자세한 설명은 [IBM Cloud에서 보안 경계 설정](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter)을 참조하십시오.

1. 보안 경계에 관련된 인프라 컴포넌트 및 클라우드 서비스에 액세스하는 데 필요한 인프라 및 IBM Cloud에서 권한을 구성하십시오. 
2. 공용 VLAN 및 방화벽을 통해 공용 인터넷의 외부 경계를 작성하십시오. 이 외부 경계가 SPS를 격리하는 데 사용됩니다. 
3. 해당 경계 안에 SPS를 배치하십시오. 
4. 공용 VLAN과 SPS를 연결하기 위한 게이트웨이를 설정하십시오.
5. 구성 VRA(Virtual Router Appliance)를 작성하십시오.
6. 하나 이상의 SPS(Secured Perimeter Segment)를 작성하고 구성하십시오. 
7. 프론트 엔드 VLAN을 작성하십시오.
8. 백엔드 VLAN을 작성하십시오.
9. Vyatta 쌍을 작성하십시오.
10. Vyatta를 구성하십시오.
11. VLANS를 게이트웨이와 연관시키십시오.
12. 공용 VLAN에 대해 게이트웨이 인터페이스를 구성하십시오.
13. Vyatta 마스터의 게이트웨이를 구성하십시오.
14. Vyatta 백업의 게이트웨이를 구성하십시오.
15. 사설 VLAN에 대해 게이트웨이 인터페이스를 구성하십시오.
16. Vyatta 마스터의 게이트웨이를 구성하십시오.
17. Vyatta 백업의 게이트웨이를 구성하십시오.
18. 네트워크 튜닝을 사용으로 설정하십시오.
19. 방화벽 규칙을 설정하십시오.
20. Kubernetes 클러스터를 구성하십시오.
21. 사용자 제공 IP를 구성하십시오(선택사항).
22. Kubernetes 클러스터를 배치하십시오. 

## IBM Cloud의 전용 솔루션
컴퓨팅 격리 및 데이터 암호화와 함께 보안 설정된 경계는 퍼블릭 IBM Cloud의 완전한 전용 솔루션에 도움을 줍니다. 클라우드 전용 솔루션 패턴에 대한 설명은 [전용 솔루션 패턴 구현](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/)을 참조하십시오. 
