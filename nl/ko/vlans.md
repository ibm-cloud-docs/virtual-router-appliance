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

# VLAN 라우팅
{: #routing-your-vlans}

Virtual Router Appliance는 동일한 네트워크 인터페이스(`dp0bond0` 또는 `dp0bond1`)를 통해 여러 VLAN을 라우팅할 수 있습니다. 이는 스위치 포트를 트렁크 모드로 설정하고 디바이스에 가상 인터페이스(VIF)를 구성하여 수행할 수 있습니다.

예를 들어 다음과 같습니다.

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

위의 명령은 `dp0bond0` 인터페이스에 두 개의 가상 인터페이스를 작성합니다. 인터페이스 `dp0bond0.1432`는 VLAN 1432의 트래픽을 처리하고 인터페이스 `dp0bond0.1693`은 VLAN 1693의 트래픽을 처리합니다.
