---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Vyatta 5400을 Juniper vSRX 또는 FSA(Fortigate Security Appliance) 10Gbps로 마이그레이션
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Vyatta 5400 디바이스를 {{site.data.keyword.vra_full}}로 업그레이드하는 것 외에도 시장에서 가장 비용 효율적이고 신뢰할 수 있는 안전한 옵션(예: Juniper vSRX 및 Fortigate Security Appliance)으로 마이그레이션할 수도 있습니다.
하지만 기존 Vyatta 5400 디바이스를 재사용하거나 연관된 IP 주소를 보존할 수는 없습니다. 

다음 프로시저에서는 독립형 Vyatta 5400 또는 고가용성(HA) 쌍으로 작동하는 두 개의 Vyatta 5400 디바이스를 Juniper vSRX 또는 FSA로 업그레이드하는 데 필요한 지시사항을 제공합니다. 

## 독립형 Vyatta 5400 업그레이드
{: #upgrading-a-stand-alone-vyatta-5400}

작동 중단 시간을 최소화하여 단일 Vyatta 5400을 Juniper vSRX 또는 FSA - 10G 어플라이언스로 업그레이드하려면 다음 프로시저를 수행하는 것이 좋습니다. 

1. 5400을 백업했고 데이터를 두 개의 다른 위치에 저장했는지 확인하십시오. 여기에는 SSH 키, SSL 인증서, 스크립트 및 현재 Vyatta 5400 구성을 복구하는 데 필요한 기타 파일(필요한 경우)이 포함됩니다. 이를 수행하려면 [이 지시사항](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)을 따르십시오. 

2. nwom@us.ibm.com에 문의하여 구성을 변환하도록 요청하십시오. 

3. [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 또는 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps)에 대한 지시사항을 따라 새 디바이스를 주문하십시오. 

  이는 고가용성 솔루션 주문을 고려할 수 있는 좋은 기회입니다.
  {: note}

4. nwom@us.ibm.com에서 수신한 구성을 새로 주문한 디바이스에 로드하십시오. 

5. VLAN을 새 디바이스로 마이그레이션할 시간을 스케줄하십시오. 

  이 단계를 수행하려면 잠시 작동 중단 시간이 필요합니다.
  {: note}

6. 새 디바이스가 제대로 작동하는지 테스트하여 확인하십시오. 

7. 지원 티켓을 열어 Vyatta 5400 디바이스를 취소하십시오. 

## Vyatta 5400 고가용성 쌍 업그레이드
{: #upgrading-a-vyatta-5400-high-availability-pair}

두 개의 Vyatta 5400을 하나의 HA 쌍으로 업그레이드하려면 다음 프로시저를 수행하십시오. 

1. 5400을 백업했고 데이터를 두 개의 다른 위치에 저장했는지 확인하십시오. 여기에는 SSH 키, SSL 인증서, 스크립트 및 현재 Vyatta 5400 구성을 복구하는 데 필요한 기타 파일(필요한 경우)이 포함됩니다. 이를 수행하려면 [이 지시사항](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration)을 따르십시오. 

2. nwom@us.ibm.com에 문의하여 구성을 변환하도록 요청하십시오. 

3. [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) 또는 [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps)에 대한 지시사항을 따라 새 디바이스를 주문하십시오. 

4. nwom@us.ibm.com에서 수신한 구성을 새로 주문한 디바이스에 로드하십시오. 

5. VLAN을 새 디바이스로 마이그레이션할 시간을 스케줄하십시오. 

  이 단계를 수행하려면 잠시 작동 중단 시간이 필요합니다.
  {: note}

6. 새 디바이스가 제대로 작동하는지 테스트하여 확인하십시오. 

7. 지원 티켓을 열어 Vyatta 5400 디바이스를 취소하십시오. 
