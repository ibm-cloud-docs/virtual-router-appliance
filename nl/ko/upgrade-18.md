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

# OS 버전 18.01에 대한 업그레이드 고려사항

이 파일에는 Brocade 5.X 운영 체제의 VRA(Vyatta 5600)를 Spectre 보안 위반에 대한 조치방안이 포함되어 있는 18.01 운영 체제로 업그레이드할 때 알아야 할 몇 가지 사항이 포함되어 있습니다. 이 업그레이드를 수행할 때 알고 있어야 할 몇 가지 문제가 있습니다.

## 이 주제를 읽어야 하는 사용자

* 현재 1801 운영 체제를 실행 중이지 않은 VRA를 사용하는 사용자. (현재 버전은 18.01g임)

* Virtual Router Appliance에 대해 새로운 18.01f 버전을 설치하려는 사용자(예를 들어, 17.2X에서 업그레이드하는 사용자).

* Vyatta 5400이 있는 경우 이 정보는 사용자에게 적용되지 않습니다.

## 이 정보가 필요한 이유

VRA의 버전 1801f에는 Spectre 취약성에 대한 보안 수정사항이 포함되어 있지만, 설치 프로그램 자체에 대한 변경사항을 작성해야 패치를 설치할 수 있습니다. 버전 1801C를 설치하기 위한 중간 단계가 필요합니다.

## 일반 업그레이드 프로시저
버전 18.01f로 업그레이드하려면, 먼저 VRA 설치 프로그램을 업데이트할 18.01C 패치를 다운로드하여 설치해야 합니다.

1. 이 위치에서 1801c 패치를 다운로드한 다음 [일반 프로시저](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)에 따라 설치하십시오.

2. 다시 부팅한 후에, 이 위치에서 18.01f 패치를 다운로드하고 [일반 프로시저](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)를 통해 설치하십시오.

이제 Spectre 보안 제한사항이 수정된 버전 18.01f가 실행 중입니다. 

## 전체 다시 로드 프로시저
대체 방법으로, 18.01f 레벨에서 VRA의 전체 다시 로드를 수행할 수도 있습니다.

*경고:* 이 프로시저를 통해 두 개 패치를 다운로드하여 설치하는 중간 단계를 생략할 수 있지만, ISO를 사용하는 전체 다시 로드 동안 모든 데이터가 손실됩니다.

1. 이 위치에서 18.01f ISO를 가져오십시오.
2. [일반 프로시저](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)에 따라 설치한 후 다시 부팅하십시오.

이제 Spectre 보안 제한사항이 수정된 버전 1801f가 실행 중입니다.

## 추가 안내

Vyatta 5400과 Virtual Router Appliance 간에는 단순한 일대일 기능 맵핑이 없으므로, VRA에 대한 기준선 구성을 작성하는 것이 도움이 됩니다. IBM© 파트너인 WanClouds는 이 프로세스를 지원할 수 있으며 VRA의 Vyatta 5400과 유사한 기능을 작성하는 데 대한 안내를 제공합니다. 

이 업그레이드 프로세스 중에 발생하는 일반적인 문제에 대한 자세한 정보는 [추가 문서](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)를 참조하십시오.
