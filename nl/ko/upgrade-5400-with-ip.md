---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# Vyatta 5400 업그레이드 및 해당 IP 주소 재사용
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

이 옵션을 사용하면 기존 Vyatta 5400 디바이스를 동등한 VRA({{site.data.keyword.vra_full}})로 재사용하고 연관된 IP 주소를 보존할 수 있습니다. 다음 프로시저에서는 독립형 Vyatta 5400 또는 고가용성(HA) 쌍으로 작동하는 두 개의 Vyatta 5400 디바이스를 업그레이드하는 것에 대한 지시사항을 제공합니다. 

## 독립형 Vyatta 5400 업그레이드
{: #upgrading-a-stand-alone-vyatta-5400}

단일 Vyatta 5400을 {{site.data.keyword.vra_full}}로 업그레이드하려면 다음 프로시저를 수행하십시오. 

1. 5400을 백업했고 데이터를 두 개의 다른 위치에 저장했는지 확인하십시오. 여기에는 SSH 키, SSL 인증서, 스크립트 및 현재 Vyatta 5400 구성을 복구하는 데 필요한 기타 파일(필요한 경우)이 포함됩니다. 
2. [이 주제](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os)에 있는 지시사항을 사용하여 Vyatta 5400을 기본 {{site.data.keyword.vra_full}}에 다시 로드하십시오. 

  OS를 다시 로드하면 루트 및 Vyatta 사용자 ID에 대한 새 비밀번호가 생성됩니다.
  {: note}

4. 새 {{site.data.keyword.vra_full}} 비밀번호를 기록해 두십시오. 
5. [이 지시사항](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)을 사용하여 원하는 설정으로 새로 다시 로드된 VRA를 구성하십시오. 

## Vyatta 5400 고가용성 쌍 업그레이드
{: #upgrading-a-vyatta-5400-high-availability-pair}

두 개의 Vyatta 5400을 하나의 HA 쌍으로 업그레이드하려면 다음 프로시저를 수행하십시오. 

1. 위 프로시저를 사용하여 백업 Vyatta 5400 디바이스를 식별한 후 먼저 {{site.data.keyword.vra_full}}로 다시 로드하십시오. 

  작동되는 HA 구성에 모든 인터페이스가 특정 노드에 백업 또는 마스터로 표시되는 것이 이상적입니다. 확실하지 않은 경우에는 `show vrrp` 명령을 사용하여 확인하십시오.
  {: note}
2. [이 지시사항](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)을 사용하여 원하는 설정으로 새로 다시 로드된 VRA를 구성하십시오. 
3. 마스터 Vyatta 5400 및 백업 {{site.data.keyword.vra_full}}가 하나의 VRRP 쌍이 됩니다. VRRP 명령 `reset VRRP master`를 사용하여 마스터가 되도록 현재 백업 {{site.data.keyword.vra_full}}를 변경하십시오. 

  이 조치를 수행하면 기존 세션에 가동 중단이 발생하고 세션 상태가 유실로 나열됩니다. 기존 NAT 세션도 재설정됩니다.
  {: note}

5. 새 마스터 {{site.data.keyword.vra_full}}가 원하는 대로 작동하는지 확인하십시오. 
6. 현재 백업 Vyatta 5400에서 위의 다시 로드 프로시저를 수행하여 이를 VRA로 업그레이드하십시오. 
7. 두 번째로 다시 로드한 후 [이 지시사항](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance)을 사용하여 백업 {{site.data.keyword.vra_full}}를 원하는 대로 구성하십시오. 
