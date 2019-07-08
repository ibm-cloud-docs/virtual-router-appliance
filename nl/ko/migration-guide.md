---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# 마이그레이션 개요
{: #migration-overview}

2019년 3월 31일부로 Vyatta 5400에 대한 지원이 중단되므로 레거시 Vyatta 5400 고객은 가능한 빨리 IBM© Virtual Router Appliance(VRA)로 마이그레이션해야 합니다. 전체 지원 종료 발표를 읽고 자세한 정보를 확인하려면 [여기](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement)를 클릭하십시오.
{: important}

## 업그레이드가 제공하는 혜택
{: #how-upgrading-can-benefit-you}

IBM Virtual Router Appliance가 테이블에 제공하는 다양한 새로운 기능 외에도 많은 개선사항이 있습니다. 자세한 내용은 [FAQ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-)를 참조하십시오. 

새 디바이스로 업그레이드하면 Vyatta 5400과 다른 구성을 가지게 됩니다. 따라서 Vyatta 5400 구성(파일)이 새 어플라이언스에 전송되지 않습니다.
{: note}

## 마이그레이션 문서
{: #migration-documentation}

Vyatta 5400에서 마이그레이션하는 데 도움이 되도록 다음과 같은 문서 및 지원이 준비되어 있습니다. 

| 마이그레이션 문서 |설명 |
| ------------- | ------------- |
| [Vyatta 5400 업그레이드 및 해당 IP 주소 재사용](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) |Vyatta 5400 IP 주소를 재사용하면서 Vyatta 5400을 동등한 IBM Virtual Router Appliance로 업그레이드하는 것에 대한 지시사항입니다. |
| [Vyatta 5400을 Juniper vSRX 또는 FSA(Fortigate Security Appliance)로 마이그레이션](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Juniper vSRX 또는 Fortigate Security Appliance로 마이그레이션하는 것에 대한 지시사항입니다. 이 옵션은 기존 Vyatta 5400 디바이스를 재사용하거나 연관된 IP 주소를 보존하도록 허용하지 않습니다. |
| [일반적인 마이그레이션 문제](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues) | Vyatta 5400 디바이스에서 IBM Virtual Router Appliance로 마이그레이션한 후 발생할 수 있는 가장 자주 발생하는 문제 또는 동작 변경사항에 대한 정보입니다. 많은 경우 이 문제를 해결하는 데 필요한 임시 해결책도 포함되어 있습니다. |

단순히 새 Virtual Router Appliance를 주문해야 하는 경우 [여기를 클릭](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)하여 자세한 내용을 확인하십시오.
{: note}
