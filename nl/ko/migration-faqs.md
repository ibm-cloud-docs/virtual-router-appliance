---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Vyatta 5400 지원 종료 FAQ
{: #vyatta-5400-end-of-support-faqs}

Vyatta 5400에서 마이그레이션할 때 자주 질문되는 내용(FAQ)은 다음과 같습니다. 

## Vyatta 5400의 "지원 종료"(EoS) 날짜가 2019년 3월 31일인 이유는 무엇입니까? 
{: faq}

2017년 9월에 레거시 Vyatta 5400은 IBM의 지원에 대한 라이프사이클 정책에 따라 2018년 2월 20일(다음 버전인 {{site.data.keyword.vra_full}}(VRA)에 대한 GA(General Availability) 7월 날짜 이후 6개월)이 EoS 날짜라고 발표했습니다. 

고객 마이그레이션 타임라인을 존중하기 위해 Vyatta 5400 EoS 날짜가 2019년 3월 31일로 연장되었습니다. 이제 Debian 7 소프트웨어는 더 이상 Debian 오픈 소스 커뮤니티에서 지원하지 않기 때문에 AT&T로부터 공급업체 지원을 연장할 향후 계획이 없습니다. 

[여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)하여 공식적인 지원 종료 발표를 확인하십시오.
{: important}

## "지원 종료"가 고객인 저에게 미치는 영향은 무엇입니까? 
{: faq}

지원 종료 날짜가 경과하면 AT&T는 더 이상 코드 패치를 제공하지 않고 IBM으로부터의 지원 에스컬레이션을 승인하지 않습니다. 

마찬가지로 IBM Cloud 지원에서는 더 이상 Vyatta 5400 배치에 대한 구성 또는 네트워킹 문제를 해결하지 않습니다. 지원은 하드웨어 레벨 요청(하드 드라이브, RAM 등), 전원 및 대역 외(IPMI) 연결로 제한됩니다. 

고객이 즉각적으로 조치를 취하여 대체 솔루션(예: Vyatta 5600을 기반으로 하는 VRA({{site.data.keyword.vra_full}}) 또는 Juniper vSRX)으로 마이그레이션하도록 강력하게 권장합니다. [여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)하여 시작하십시오. 

## 3월 31일 이후 Vyatta 5400을 사용하여 IBM Cloud 워크로드를 계속 실행하면 어떻게 됩니까? 
{: faq}

Vyatta 5400은 3월 31일 이후에도 작동합니다. 하지만 Vyatta 5400 소프트웨어의 잠재적 취약점 때문에 비즈니스 및 애플리케이션 환경이 잠재적 보안 위협 및 기타 변조 위반에 노출될 수 있습니다. 

비즈니스 및 애플리케이션 환경을 중단시키는 네트워크 문제가 발생할 때 근본 원인이 Vyatta 5400으로 추적되는 경우에는 IBM 또는 AT&T로부터 더 이상 지원을 받을 수 없기 때문에 이 문제를 5400 오퍼링 관리자에게 보고하십시오. 이메일(`nwom@us.ibm.com`)을 통해 오퍼링 관리 팀에 문의할 수 있습니다. 

## 제 기본 Bare Metal Server 하드웨어는 어떻게 됩니까? 계속 지원됩니까? 
{: faq}

하드웨어 교체는 지원되지만 문제점 해결 시 문제점이 Vyatta OS와 관련되어 있다고 표시되는 경우에는 지원되는 하드웨어 오퍼링으로 즉시 마이그레이션하도록 지시됩니다. [여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)하여 시작하십시오. 

## Vyatta 5400을 소유한 고객인데 2019년 3월 31일까지 무엇을 해야 합니까? 
{: faq}

Vyatta 5400을 보유한 고객은 VRA(Vyatta 5600), Juniper vSRX 또는 FSA(Fortigate Security Appliance) 10G로 마이그레이션해야 합니다. VRA(Vyatta 5600)는 계속 완전히 지원됩니다. VRA의 경우에는 IBM Cloud 또는 AT&T로부터의 현재 또는 예상 지원 종료 날짜가 없습니다. [여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)하여 시작하십시오. 

  VRA 및 vSRX는 고객이 관리하는 디바이스입니다.
  {: note}

## Vyatta 5400에서 VRA, vSRX 또는 FSA 10G로 마이그레이션할 때 IBM으로부터 지원을 받을 수 있습니까? 
{: faq}

Vyatta 5400에서 VRA(5600)로 구성 변환 서비스는 계속 사용할 수 있습니다. 

* 기존 고객의 경우 IBM Cloud는 기존 Vyatta 5400 구성을 VRA({{site.data.keyword.vra_full}}), Juniper vSRX 또는 FSA(Fortigate Security Appliance) 10G 형식으로 리팩토링하는 것을 지원하기 위해 무료 오퍼링을 제공합니다. 구성 변환 서비스에 대한 요청을 제출하려면 `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx.`를 제목으로 사용하여 nwom@us.ibm.com에 이메일을 보내십시오. 

  제목 행에서 `aaaaaaaa` 대신 선택한 애플리케이션({{site.data.keyword.vra_full}}, Juniper vSRX 또는 FSA(Fortigate Security Appliance) 10G)을 삽입하고 `xxxxxx` 대신 구체적인 계정 번호를 삽입하십시오.
  {: note}

* 이 변환 구성 프로세스에서 파트너인 Wanclouds는 수백 번의 마이그레이션 작업을 완료했습니다. Wanclouds는 기존 Vyatta 5400을 변환하여 Vyatta 5600 플랫폼에서 유사한 기능을 작성합니다. Wanclouds는 아래에 설명된 두 계층으로 서비스를 제공합니다. 

  <img src="images/tiers.png" alt="그림" style="width: 700px;"/>

[여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)하여 시작하십시오. 

## Vyatta 5400에서 마이그레이션하기 위해 IBM 비즈니스 파트너가 제공하는 추가적인 유료 마이그레이션 서비스가 있습니까? 
{: faq}

Vyatta 5400 마이그레이션을 위한 유료 지원을 제공하는 여러 비즈니스 파트너가 있습니다. [여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)하여 자세한 정보를 확인하십시오. 

## Vyatta 5400 마이그레이션과 관련하여 문의할 수 있는 Vyatta 5400 오퍼링 관리 지원 담당자가 IBM에 있습니까? 
{: faq}

`nwom@us.ibm.com`으로 IBM Vyatta 5400 및 VRA 네트워크 오퍼링 관리 부서에 질문하십시오. IBM Watson 클라우드 플랫폼 작업공간에서 슬랙을 사용하여 문의할 수도 있습니다(`#vyatta-migration`). 

## 이 마이그레이션을 지원하기 위해 사용할 수 있는 추가 리소스는 무엇입니까? 
{: faq}

자세한 정보는 다음과 같은 {{site.data.keyword.vra_full}} 문서 리소스를 검토하십시오. 

  * [{{site.data.keyword.vra_full}} 시작하기](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [VRA 정보](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Vyatta 5400 마이그레이션 개요](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
