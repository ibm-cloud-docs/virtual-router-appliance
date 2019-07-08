---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: updates, changes, additions

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


# IBM Virtual Router Appliance의 최신 업데이트
{: #recent-updates-for-ibm-virtual-router-appliance}

IBM© Virtual Router Appliance(VRA)의 업데이트된 새로운 기능에 대해 살펴봅니다.

## 2018년 8월
{: #august-2018}

### Brocade 운영 체제 버전 18.x
{: #brocade-operating-system-version-18-x}

이제 Brocade OS의 버전 18.x를 Virtual Router Appliance에 사용할 수 있습니다. 다른 새 기능들 중에 이 버전은 Spectre 보안 위반에 대한 조치방안을 제공합니다.

18.x VRA의 새 기능은 다음 주제에서 논의합니다.

* [구역 방화벽에 대해 작업하는 IPsec 터널 설정](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [IPsec 및 구역 방화벽을 사용하여 VPF 인터페이스 구성](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [접두부 기반 IPSec이 포함된 NAT 사용](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [VFP 인터페이스 문제점 해결](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

Vyatta 5400에서 마이그레이션하는 경우, 18.x로 업그레이드하는 가장 좋은 방법은 전체 OS를 다시 로드하는 [일반 프로시저](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)를 이용하는 것입니다.

Vyatta 5400과 Virtual Router Appliance 간에는 단순한 일대일 기능 맵핑이 없으므로, VRA에 대한 기준선 구성을 작성하는 것이 도움이 됩니다. IBM 파트너인 WanClouds는 이 프로세스를 지원할 수 있으며 VRA의 Vyatta 5400과 유사한 기능을 작성하는 데 대한 안내를 제공합니다.

이 업그레이드 프로세스 중에 발생하는 일반적인 문제에 대한 자세한 정보는 [추가 문서](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)를 참조하십시오.
