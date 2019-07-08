---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, setup, 5400

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
{:important: .important}

# Vyatta 5400에 NAT 규칙 설정
{: #setting-up-nat-rules-on-vyatta-5400}

이 주제에는 Vyatta에 사용된 NAT(Network Address Translation) 규칙의 예제가 포함되어 있습니다.

## 일대다 NAT 규칙(masquerade)
{: #one-to-many-nat-rule-masquerade-}

프롬프트에서 다음 명령을 입력하십시오.

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

`10.xxx.xxx.xxx` 네트워크에서 머신의 연결 요청은 bond1의 IP에 맵핑되고 아웃바운드로 이동할 때 연관된 임시 포트를 수신합니다. 해당 목적은 일대다 위장 규칙 수를 더 높게 지정하여 발생할 수 있는 더 낮은 NAT 규칙과 충돌하지 않도록 하는 것입니다.

기본 게이트웨이가 관리된 가상 LAN(VLAN)의 사설 IP 주소가 되도록 VRA를 통해 인터넷 트래픽을 전달하도록 서버를 구성해야 합니다. 예를 들어, `bond0.2254`의 경우 게이트웨이는 `10.52.69.201`입니다. 이는 인터넷 트래픽을 통과하는 서버의 게이트웨이 주소여야 합니다.
{: note}

다음 명령을 사용하여 NAT의 문제점을 해결하십시오. 

  '''
  run show nat source translations detail 
'''
  {: tip}

## 일대일 NAT 규칙
{: #one-to-one-nat-rule}

다음 명령은 일대일 NAT 규칙 설정 방법을 보여줍니다. 규칙 수가 위장 규칙보다 더 낮게 설정됩니다. 이로 인해 일대일 규칙이 일대다 규칙보다 우선됩니다.

일대일로 맵핑된 IP 주소는 위장할 수 없습니다. IP 인바운드를 변환하는 경우 트래픽이 두 방향으로 이동하도록 IP 아웃바운드를 변환해야 합니다.
{: tip}

다음 명령은 소스 및 대상 규칙에 사용됩니다. NAT 규칙 유형을 보려면 구성 모드에서 `show nat`를 입력하십시오.

  `run show nat source translations detail` 명령을 사용하여 NAT의 문제점을 해결하십시오.
  {: tip}

사용자가 구성 모드에 있는지 확인한 후 프롬프트에 다음 명령을 입력하십시오.

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

트래픽이 bond1의 IP `50.97.203.227`에서 들어오는 경우, 해당 IP는 IP `10.52.69.202`(정의된 인터페이스에 있음)에 맵핑됩니다. 트래픽이 `10.52.69.202`(정의된 인터페이스에 있음)의 IP를 사용하여 아웃바운드로 이동하는 경우 이는 IP `50.97.203.227`로 변환되고 인터페이스 bond1에서 아웃바운드로 처리됩니다.

일대일로 맵핑된 IP 주소는 위장할 수 없습니다. IP 인바운드를 변환하는 경우 트래픽이 두 방향으로 이동하도록 같은 IP 아웃바운드를 변환해야 합니다.
{: note}

## VRA를 통해 IP 범위 추가
{: #adding-ip-ranges-through-your-vra}

VRA 구성에 따라 특정 IBM© Cloud IP 주소를 허용하려고 할 수 있습니다.

새 vRouter 배치에는 `SERVICE-ALLOW`라고 하는 방화벽 규칙에 정의된 IBM Cloud의 서비스 네트워크 IP 주소가 포함됩니다.

다음은 `SERVICE-ALLOW`의 예제입니다. 이는 완전한 사설 IP 규칙 세트가 아닙니다.

```
~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~
```

방화벽 규칙을 정의하고 나면 방화벽 규칙을 적절하게 지정할 수 있습니다. 두 가지 예제는 다음과 같습니다.

구역에 적용: `set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

본드 인터페이스에 적용: `set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
