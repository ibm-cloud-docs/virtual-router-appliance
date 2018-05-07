---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Virtual Router Appliance에 방화벽 기능 추가(상태 비저장 및 상태 저장)
각 인터페이스에 방화벽 규칙 세트를 적용하는 것은 VRA(Virtual Router Appliance)를 사용할 때 방화벽 설정의 한 방법입니다. 각 인터페이스에는 세 가지 방화벽 인스턴스(In, Out 및 Local)가 있으며 각 인스턴스에는 적용할 수 있는 규칙이 있습니다.  

기본 조치는 삭제로, 특정 트래픽이 규칙 1부터 N까지의 방식으로 적용될 특정 트래픽을 허용하는 규칙이 포함됩니다. 일치되면 방화벽은 일치 규칙의 특정 조치를 적용합니다. 

세 가지 방화벽 인스턴스는 다음과 같습니다(단, 하나만 적용됨). 

**IN:** 방화벽은 인터페이스에 들어오고 시스템을 통과하는 패킷을 필터링합니다. 특정 IP 범위를 허용하여 관리(Ping, 모니터링 등)를 위해 프론트 엔드 및 백엔드에 액세스할 수 있어야 합니다.

**OUT:** 방화벽은 인터페이스를 나가는 패킷을 필터링합니다. 세 가지 패킷은 VRA 시스템을 통과하거나 시스템에서 시작할 수 있습니다.

**LOCAL:** 방화벽은 시스템 인터페이스를 사용하여 VRA 시스템 자체를 대상으로 하는 패킷을 필터링합니다. 제한되지 않는 외부 IP 주소에서 디바이스로 들어오는 액세스 포트에 대한 제한을 설정해야 합니다.

ICMP(Internet Control Message Protocol)(Ping - IPv4 에코 응답 메시지)를 끄기 위해 예제 방화벽 규칙을 Virtual Router Appliance의 인터페이스에 설정하려면 다음 단계를 수행하십시오(즉, 상태 비저장 조치이며 상태 저장 조치는 나중에 검토됨). 

1. 설정된 구성을 확인하려면 명령 프롬프트에 `show configuration commands`를 입력하십시오. 디바이스에 설정한 모든 명령의 목록이 표시됩니다(마이그레이션을 결정하고 모든 구성을 표시하려는 경우 유용할 수 있음). ICMP가 계속해서 디바이스에 사용되고 있음을 나타내는 `set firewall all-ping enable` 명령에 유의하십시오. 

2. `configure`를 입력하십시오.

3. `set firwall all-ping disable`을 입력하십시오.

4. `commit`를 입력하십시오.

지금 VRA 디바이스에 대한 ping을 실행하려는 경우 더 이상 응답을 받을 수 없게 됩니다. 

방화벽 규칙을 라우팅된 트래픽에 지정하려면 규칙이 VRA의 인터페이스에 적용되거나 구역을 작성한 후 구역에 적용되어야 합니다. 

이 예제에서는 지금까지 사용된 VLAN에 대한 구역이 작성됩니다.

 VLAN | 구역 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod(프로덕션용)
bond0.2254 | private(개발용)
bond1.1280 | 차후 사용을 위해 예약됨
bond1.1894 | 차후 사용을 위해 예약됨

## 방화벽 규칙 작성
구역을 실제로 작성하기 전에 구역에 적용될 방화벽 규칙을 작성하는 것이 좋습니다. 구역 작성 전에 규칙을 작성하면 즉시 규칙을 적용할 수 있으며(구역 작성 후 규칙 작성과 비교 시) 그런 다음 규칙 애플리케이션을 위해 구역으로 다시 돌아갑니다. 

명령은 다음을 수행합니다. 

* 기본 조치가 포함된 `dmz2private`이라는 방화벽 규칙을 작성하여 패킷을 삭제합니다. 
* `dmz2private`이라는 규칙에 대해 허용된 트래픽 및 거부된 트래픽의 로깅을 사용합니다.

1. 명령 프롬프트에서 `configure`를 입력하십시오.

2. 다음 명령을 입력하십시오.

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. 다음 명령 세트를 입력하면 iptables는 리턴할 확립된 세션의 트래픽을 허용합니다. 기본적으로, iptables는 명시적 규칙이 필수라는 이유로 이를 허용하지 않습니다. 

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. 다음 명령을 입력하면 TCP/UDP가 포트 22(SSH의 기본값)를 통과합니다. 
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. 다음 명령을 입력하면 TCP/UDP가 포트 80(HTTP의 기본값)을 통과합니다. 

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. 종료 시 모든 규칙이 적용되었는지 확인하려면 `commit`를 입력하십시오. 

7. 명령 프롬프트에서 `show firewall name dmz2private`를 입력하여 새 구성을 확인하십시오.

8. 프롬프트에서 다음 명령을 입력하여 dmz 구역에 적용될 방화벽 규칙을 작성하십시오. 규칙은 public으로 이름이 지정됩니다.  

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## 구역 작성

구역은 인터페이스의 논리적 표시입니다. 이 섹션에서 다음을 수행하십시오. 

* 기본 조치가 포함된 구역 및 dmz라고 하는 정책을 작성하여 이 구역을 대상으로 하는 패킷을 삭제하십시오. 
* dmz 정책을 설정하여 `bond1` 인터페이스를 사용하십시오. 
* prod 정책을 설정하여 `bond1.2007` 인터페이스를 사용하십시오. 
* 기본 조치가 포함된 `private`이라고 하는 구역 정책을 작성하여 이 구역을 대상으로 하는 패킷을 삭제하십시오. 
* `private`이라고 하는 정책을 설정하여 `bond0.2254` 인터페이스를 사용하십시오. 

1. 프롬프트에서 다음 명령을 입력하십시오.

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. 구역에 방화벽 정책을 설정하려면 다음 명령을 사용하십시오. 

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
구역 정책에 방화벽을 적용하지 않으려는 경우 특정 인터페이스에 방화벽 규칙을 적용할 수 있습니다. 인터페이스에 규칙을 적용하려면 다음 명령을 사용하십시오.

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
