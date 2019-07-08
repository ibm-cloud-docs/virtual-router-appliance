---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-4-17"

keywords: 5600, security, fixes, patches, brocade, os

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

# AT&T Vyatta 5600 vRouter 소프트웨어 패치
{: #at-t-vyatta-5600-vrouter-software-patches}

**기준: 2019년 6월 6일**

이 문서는 Vyatta Network OS 5600의 현재 지원되는 버전에 대한 패치를 나열합니다. 버전 5.2 이하를 사용하는 경우, 패치는 S 수를 사용하여 이름이 지정됩니다. 버전 17.1 이상의 경우, 패치는 "i", "o", "l" 및 "x"를 제외하고 소문자로 이름이 지정됩니다.

여러 CVE 수가 단일 업데이트에서 지정되는 경우 가장 높은 CVSS 점수가 나열됩니다.

## 1801z
{: #1801z}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-46941 | 낮음| SNAT 세션을 보유한 트래픽이 리턴 시 Stateless ZBF를 사용하여 필터링됨 |
| VRVDR-46659 | 높음 | mtu 9000을 가진 I350 intfs가 1808*에서 1903a로 업그레이드 시 u/D 상태로 멈춰 있음 |
| VRVDR-46623 | 낮음 | 방화벽 '설명'에 둘 이상의 단어가 포함된 경우 커미트 시 방화벽 설명에서 오류를 로그함 |
| VRVDR-46549 | 심각 | "show ip route routing-instance <name> variance" 명령에서 쉘 인젝션 권한 상승/샌드박스 이스케이프 |
| VRVDR-46389 | 높음 | (다시) 부팅 후 적용된 경우 BGP 구성 변경사항이 적용되지 않을 수 있음 |
| VRVDR-45949 | 낮음 | 특정 키가 아닌 필드가 구성된 경우 전송된 모든 샘플에 대해 넷플로우에서 NOTICE 로그를 생성함 |
| VRVDR-43169 | 낮음 | configd C 기반 API를 호출하지만 오류 구조체를 제공하지 않을 때마다 로깅하는 것이 더 이상 유용하지 않음 |
| VRVDR-41225 | 낮음 | 인터페이스 설명을 구성할 때 모든 공백이 줄 바꾸기로 처리됨 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-46824 |해당사항 없음 | DSA-4440-1 | CVE-2018-5743, CVE-2018-5745, CVE-2019-6465: Debian DSA-4440-1 : bind9 - 보안 업데이트 |
| VRVDR-46603 | 5.3 | DSA-4435-1 | CVE-2019-7317: Debian DSA-4435-1 : libpng1.6 - 보안 업데이트 |
| VRVDR-46425 |해당사항 없음 | DSA-4433-1 | CVE-2019-8320, CVE-2019-8321, CVE-2019-8322, CVE-2019-8323, CVE-2019-8324, CVE-2019-8325: Debian DSA-4433-1 : ruby2.3 - 보안 업데이트 |
| VRVDR-46350 | 9.1 | DSA-4431-1 | CVE-2019-3855, CVE-2019-3856, CVE-2019-3857, CVE-2019-3858, CVE-2019-3859, CVE-2019-3860, CVE-2019-3861, CVE-2019-3862, CVE-2019-3863: Debian DSA-4431-1 : libssh2 - 보안 업데이트 |

## 1801y
{: #1801y}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-46029 | 높음 | 단순 텍스트 비밀번호 또는 AH 유형을 사용한 VRRP 인증이 제대로 작동하지 않음 |
| VRVDR-45864 | 심각 | vyatta-techsupport remote copy에서 쉘 인젝션 권한 상승/샌드박스 이스케이프 |
| VRVDR-45748 | 높음 | zmsg_popstr에 대한 검사가 누락되어 NULL 포인터가 리턴되어 connsync가 데이터 플레인과 충돌함 |
| VRVDR-45740 | 낮음 | 'generate tech-support archive'는 모든 기존 아카이브를 집계하지 않아야 함 |
| VRVDR-45720 | 높음 | 단일 라우터에서만 start_delay가 사용된 경우 vrrp가 패킷을 기다리며 멈춰 있음 |
| VRVDR-45655 | 심각 | VRRP 장애 복구 수행 시 "rte_mbuf_raw_alloc에 패닉 발생"|
| VRVDR-45059 | 높음 | sip_expire_session_request에 널 deref |
| VRVDR-41419 | 높음 | 정적 분석 데이터 플레인 수정사항 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-46139 | 7.0 | DSA-4428-1 | CVE-2019-3842: Debian DSA-4428-1 : systemd - 보안 업데이트 |
| VRVDR-46087 |해당사항 없음 | DSA-4425-1 | CVE-2019-5953: Debian DSA-4425-1 : wget - 보안 업데이트 |
| VRVDR-45897 | 7.5 | DSA-4416-1 | CVE-2019-5716, CVE-2019-5717, CVE-2019-5718, CVE-2019-5719, CVE-2019-9208, CVE-2019-9209, CVE-2019-9214: Debian DSA-4416-1 : wireshark - 보안 업데이트 |
| VRVDR-45553 | 5.9 | DSA-4400-1 | CVE-2019-1559: Debian DSA-4400-1 : openssl1.0 - 보안 업데이트 |
| VRVDR-45549 | 6.5 | DSA-4397-1 | CVE-2019-3824: Debian DSA-4397-1 : ldb - 보안 업데이트 |
| VRVDR-45347 | 6.8  | DSA-4387-1 | CVE-2018-20685, CVE-2019-6109, CVE-2019-6111: Debian DSA-4387-1 : openssh - 보안 업데이트 |

**해결된 보안 취약점**

다음과 같은 명령은 이 패치부터 더 이상 사용되지 않으며 더 이상 사용할 수 없습니다.
  • policy route pbr <name> rule <rule-number> application name <name> 
  • policy route pbr <name> rule <rule-number> application type <type> 
  • policy qos name <policy-name> shaper class <class-id> match <match-name> application name <name> 
  • policy qos name <policy-name> shaper class <class-id> match <match-name> application type <type> 
  • security application firewall name <name> rule <rule-number> name <app-name> 

위 명령을 실행하면 "This feature is disabled."라는 오류 메시지가 표시됩니다.  

## 1801w
{: #1801w}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-45672 | 심각 | /opt/vyatta/etc/config/ipsec.d/rsakeys/localhost.key에 있는 RSA 개인 키의 권한이 잘못됨 |
| VRVDR-45591 | 심각 | 인터페이스 IP MTU 변경사항이 Intel x710 NICs에 적용되지 않음 |
| VRVDR-45466 | 낮음 | PXE 부팅을 통해 구성이 로드될 때 IPv6 주소가 축약되지 않아 config-sync 문제가 발생함 |
| VRVDR-45414 | 낮음 | Vyatta-cpu-shield가 시작하는 데 실패하여 2소켓 시스템의 다양한 코어에 대해 `OSError:[Errno 22] Invalid argument`를 생성함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-45253 | 7.5 | DSA-4375-1 | CVE-2019-3813: Debian DSA 4375-1: spice - 보안 업데이트 |
| VRVDR-44922 | 7.5 | DSA-4355-1 | CVE-2018-0732, CVE-2018-0734, CVE-2018-0737, CVE-2018-5407: Debian DSA-4355-1 : openssl1.0 - 보안 업데이트 |
| VRVDR-43936 | 7.5 | DSA-4309-1 | CVE-2018-17540: Debian DSA-4309-1 : strongswan - 보안 업데이트 |

## 1801v
{: #1801v}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-45175 | 심각 | VRF가 구성된 경우 Rsyslogd 코어 덤프 |
| VRVDR-45057 | 심각 | 처음 시작된 후 A/D 상태인 IPsec VTI 터널, IPsec SA는 작동 상태 유지 |
| VRVDR-44985 | 높음 | DNAT 및 입력 방화벽 로깅 / 운영 순서 |
| VRVDR-44944 | 심각 | vyatta-config-vti.pl: 안전하지 않은 임시 파일 사용 |
| VRVDR-44941 | 낮음| 간략한 VTI 인터페이스 플랩 이후 커널에서 정적 라우트 누락 |
| VRVDR-44914 | 심각 | HA 쌍의 두 멤버 모두에서 RPC ALG 충돌 |
| VRVDR-44668 | 높음 | 프로덕션 트래픽 플로우-모니터링을 통해 보고 넷플로우 통계 지연 및 중지 |
| VRVDR-44667 | 낮음 | 'show flow-monitoring' 실행 간 인터페이스 순서가 일치하지 않음 |
| VRVDR-44657 | 높음 | 터널이 작동하는 동안 IKEv1 키 재입력 충돌로 인해 VTI 인터페이스가 작동 해제 상태 유지 |
| VRVDR-44560 | 높음| ip_gre 드라이버를 가리키는 여러 rcu_sched CPU 중단 |
| VRVDR-44517 | 낮음 | rte_ipv6_fragment_packet의 패닉으로 데이터 영역 충돌 |
| VRVDR-44282 | 높음 | /32 마스크를 포함하고 포함하지 않는 주소가 모두 주소 그룹에 있으면 /32 마스크를 삭제하는 데 문제 발생 |
| VRVDR-44278 | 낮음 | "show address-group all ipv4 optimal"에서 출력을 생성하지 않음 |
| VRVDR-44239 | 높음 | 'all' 프로토콜이 필요한 경우 프로토콜 드롭 다운의 Web GUI 상세 정보를 향상시키도록 요청 |
| VRVDR-44076 | 높음 | 플로우-모니터링의 메모리 누수로 인해 데이터 플레인 세그먼트-결함 및 가동 중단 |
| VRVDR-44007 | 심각 | npf_dataplane_session_establish의 데이터 플레인 분석 방식 결함 |
| VRVDR-43909 | 낮음| Connsync로 인해 "restart vrrp" 후에 인터페이스가 작동 중지됨 |
| VRVDR-42679 | 높음 | syslog - zactor_is에서 충돌 발생 |
| VRVDR-42020 | 높음 | RIB에서 동일한 라우트를 반복 추가 |
| VRVDR-18095 | 낮음 | 플로우 모니터링 상태는 'show tech-support'의 일부로 캡처되지 않음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-45148 |해당사항 없음 | DSA-4371-1 | CVE-2019-3462: Debian DSA-4371-1 – apt security update |
| VRVDR-45043 | 8.8 | DSA-4369-1 | CVE-2018-19961, CVE-2018-19962, CVE-2018- 19965, CVE-2018-19966, CVE-2018-19967: DSA 4369-1 - Xen security update |
| VRVDR-45042 |해당사항 없음 | DSA-4368-1 | CVE-2019-6250: Debian DSA-4368-1 : zeromq3 - security update |
| VRVDR-45035 |해당사항 없음 | DSA-4367-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - security update |
| VRVDR-44956 | 7.5 | DSA-4359-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - security updateCVE-2018-12086, CVE-2018-18225, CVE-2018- 18226, CVE-2018-18227, CVE-2018-19622, CVE- 2018-19623, CVE-2018-19624, CVE-2018-19625, CVE-2018-19626, CVE-2018-19627, CVE-2018- 19628: Debian DSA-4359-1 : wireshark - security update |
| VRVDR-44747 |해당사항 없음 | DSA-4350-1 | CVE-2018-19788: Debian DSA-4350-1 : policykit-1 - security update |
| VRVDR-44634 | 8.8 | DSA-4349-1 | CVE-2017-11613, CVE-2017-17095, CVE-2018- 10963, CVE-2018-15209, CVE-2018-16335, CVE- 2018-17101, CVE-2018-18557, CVE-2018-5784, CVE-2018-7456, CVE-2018-8905:Debian DSA-4349- 1 : tiff - security update |
| VRVDR-44633 | 7.5 | DSA-4348-1| CVE-2018-0732, CVE-2018-0734, CVE-2018-0735, CVE-2018-0737, CVE-2018-5407: Debian DSA-4348- 1 : openssl - security update |
| VRVDR-44611 | 9.8 | DSA-4347-1| CVE-2018-18311, CVE-2018-18312, CVE-2018- 18313, CVE-2018-18314: Debian DSA-4347-1 : perl - security update |
| VRVDR-44348| 9.8 | DSA-4338-1 | CVE-2018-10839, CVE-2018-17962, CVE-2018- 17963: Debian DSA-4338-1: qemu security update |
| VRVDR-43264| 5.6 | DSA-4274-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4274- 1: xen security update |

## 1801u
{: #1801u}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-44406 | 심각 | 동일한 VIF에 여러 서브넷이 있으면 5400 성능과 비교할 때 낮은 비율의 전이 트래픽이 관찰됨 |
| VRVDR-44253 | 낮음 | 다시 부팅하고 나면 연결 인터페이스의 MSS 클램핑이 작동을 중지함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-44277 |해당사항 없음 | DSA-4332-1 | CVE-2018-16395, CVE-2018-16396: Debian DSA-4332-1 : ruby2.3 - security update |
| VRVDR-44276 |해당사항 없음 | DSA-4331-1 | CVE-2018-16839, CVE-2018-16842: Debian DSA-4331-1 : curl - security update |

## 1801t
{: #1801t}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-44172 | 긴급 | mss-clamp 테스트에서 "인터페이스 [openvpn]이 유효하지 않음" 오류가 보고됨 |
| VRVDR-43969 | 낮음 | Vyatta 18.x GUI에서 잘못된 상태 확인 메모리 사용을 보고함 |
| VRVDR-43847  | 높음 | 본딩 인터페이스에서 TCP 대화에 대한 처리량이 느림 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-43842 |해당사항 없음 | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – security update |

## 1801s
{: #1801s}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-44041 | 높음 | SNMP ifDescr oid 느린 응답 시간 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-44074| 9.1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – security update|
| VRVDR-44054 | 8.8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – security update |
| VRVDR-44038 |해당사항 없음 | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – security update |
| VRVDR-44033 |해당사항 없음 | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – security update |
| VRVDR-43922 | 7.8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – security update |
| VRVDR-43908 | 9.8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - security update |
| VRVDR-43884 | 7.5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - security update |

## 1801r
{: #1801r}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-43738 | 높음 | SNAT 세션을 통해 리턴된 ICMP 도달 불가능 패킷은 전달되지 않음 |
| VRVDR-43538 | 높음 | 본딩 인터페이스에서 오버사이즈 오류 수신 |
| VRVDR-43519 | 높음 | 구성 표시 없이 Vyatta-keepalived 실행 중 |
| VRVDR-43517 | 높음 | VFP/정책 기반 IPsec의 엔드포인트가 vRouter 자체에 상주하는 경우 트래픽이 실패함 |
| VRVDR-43477 | 높음 | IPsec VPN 구성을 커미트하면 다음 경고가 리턴됩니다. “경고: [VPN 토글 net.ipv4.conf.intf.disable_policy] 수행할 수 없음, 수신 오류 코드 65280 |
| VRVDR-43379 | 낮음 | NAT 통계가 올바르지 않게 표시됨 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-43837 | 7.5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1: libarchive-zip-perl –security update |
| VRVDR-43693 |해당사항 없음 | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1: mgetty –security update |
| VRVDR-43578 |해당사항 없음 | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1: curl -security update |
| VRVDR-43326 |해당사항 없음 | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1: openssh -security update |
| VRVDR-43198 |해당사항 없음 | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1: linux security update (FragmentSmack) |
| VRVDR-43110 |해당사항 없음 | DSA-4265-1 | Debian DSA-4265-1 : xml-security-c -security update |
| VRVDR-43057 |해당사항 없음 | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : libmspack -security update |
| VRVDR-43026 | 9.8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 -security updateVRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 :fuse -security update |

## 1801q
{: #1801q}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-43531 | 높음 | 1801p에서 부팅 시 약 40초 이내 커널 패닉이 발생함 |
| VRVDR-43104 | 심각 | IPsec이 사용으로 설정되어 있는 경우 DHCP 네트워크에서 Fake Gratuitous ARP 발생 |
| VRVDR-41531 | 높음 | 바인딩 해제 이후 IPsec에서 VFP 인터페이스를 사용하려고 계속 시도함 |
| VRVDR-43157 | 낮음 | 터널이 바운스되는 경우 SNMP 트랩이 올바르게 생성되지 않습니다. |
| VRVDR-43114 | 심각 | 다시 부팅 시, 해당 피어보다 높은 우선순위를 갖는 HA 쌍의 라우터는 고유 “preempt false” 구성을 이행하지 않으며 부팅 다음에 바로 마스터가 됨 |
| VRVDR-42826 | 낮음 | remote-id가 “0.0.0.0”인 피어 협상은 사전 공유된 키 불일치로 인해 실패함 |
| VRVDR-42774 | 심각| 매우 높은 비율로 X710(i40e) 드라이버에서 플로우 제어 프레임 전송 |
| VRVDR-42635 | 낮음 | BGP 재분배 라우트 맵 정책 변경사항이 적용되지 않음 |
| VRVDR-42620 | 낮음 | Vyatta-ike-sa-daemon에서 터널이 가동되는 것으로 표시되는 동안 “명령 실패: establishing CHILD_SA passthrough-peer” 오류를 처리함 |
| VRVDR-42483 | 낮음 | TACACS 인증 실패 |
| VRVDR-42283 | 높음 | vif 인터페이스 ip가 삭제되면 모든 인터페이스에 대한 VRRP 상태가 FAULT로 변경됨 |
| VRVDR-42244 | 낮음 | 플로우 모니터링은 1000개 샘플만 콜렉터로 내보냄 |
| VRVDR-42114 | 심각 | HTTPS 서비스는 TLSv1을 노출해서는 안 됨 |
| VRVDR-41829 | 높음 | 시스템이 SIP ALG soak 테스트에 반응하지 않게 될 때까지 데이터플레인 코어 덤프 발생 |
| VRVR-41683 | 긴급 | VRF를 통해 학습된 DNS 이름 서버 주소가 일관되게 인식되지 않음 |
| VRVDR-41628 | 낮음 | 라우터-통지의 라우트/접두부가 커널 및 데이터 플레인에서 활성 상태이지만 RIB에 의해 무시됨 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-43288 | 5.6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – Linux security update |
| VRVDR-43111 |해당사항 없음 | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – Linux security update |

## 1801n
{: #1801n}

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-42588 | 낮음 | 실수로 시스템 로그에서 누출된 민감한 라우팅 프로토콜 구성 |
| VRVDR-42566 | 심각 | 17.2.0h에서 1801m으로 업그레이드한 다음 날 두 HA 멤버 모두에서 다시 부팅이 여러 번 발생함 |
| VRVDR-42490 | 높음 | VRRP 상태 전이 이후 곧 VTI-IPSEC IKE SA 실패 |
| VRVDR-42335 | 높음 | IPSEC: remote-id “hostname” 동작이 5400에서 5600으로 변경 |
| VRVDR-42264 | 심각 | SIT 터널에서 연결 없음 – “kernel: sit: non-ECT from 0.0.0.0 with TOS=0xd” |
| VRVDR-41957 | 낮음 | 양방향 NAT 처리된 패킷이 GRE에 대해 너무 커서 ICMP Type 3 Code 4 리턴에 실패함 |
| VRVDR-40283 | 높음 | 구성 변경사항으로 인해 많은 로그 메시지가 생성됨 |
| VRVDR-39773 | 높음 | BGP vrrp 장애 복구 명령으로 라우트 맵을 사용하면 모든 접두부가 철회될 수 있음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-42505 |해당사항 없음 | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - security update |
| VRVDR-42427 |해당사항 없음 | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - security update |
| VRVDR-42383 |해당사항 없음 | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - security update |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – security update |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – security update |

## 5.2R6S12
{: #5-2R6S12}

2018년 6월 21일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-42084 | 긴급 | nat/ipsec 구성이 다시 적용되는 경우 Vfp 인터페이스가 “show dataplane route”에 “non-dataplane interface”로 표시됨 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR-42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |
| VRVDR-41797 |8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – security update |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update (Spectre) |

## 1801m
{: #1801m}

2018년 6월 15일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-42256 | 심각 | 최근 설정된 CHILD_SA가 삭제되는 경우 아웃바운드 트래픽 없음 |
| VRVDR-42084 | 긴급 | PB IPsec 터널에 대한 VFP 인터페이스로 링크되는 NAT 세션은 라우터가 이를 수행하도록 구성된 경우에도 라우터에 도착하는 패킷에 대해 작성되지 않음 |
| VRVDR-42018 | 낮음 | “restart vpn”이 실행되는 경우 “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” 오류 발생함 |
| VRVDR-42017 | 낮음 | “show vpn ipsec sa”가 VRRP 백업에서 실행 중인 경우, “ConnectionRefusedError” 오류가 vyatta-op-vpn- ipsec-vici 라인 563과 관련하여 발생함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |

## 5.2R6S11
{: #5-2R6S11}

2018년 6월 11일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-42109 | 심각 | 단 1개의 ICMP 응답 패킷만 5.2R6S7의 SNAT+FW와 함께 수신함 |
| VRVDR-42084 | 긴급 | PB IPsec 터널에 대한 VFP 인터페이스로 링크되는 NAT 세션은 라우터가 이를 수행하도록 구성된 경우에도 라우터에 도착하는 패킷에 대해 작성되지 않음 |
| VRVDR-42027 | 높음 | 올바르지 않은 입력 ifIndex를 사용하는 SFLOW |
| VRVDR-41558 | 높음 | 패킷 추적에 보고된 시간소인이 실제 시간 및 시스템 시계와 일치하지 않음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – security update |
| VRVDR- 42013 |해당사항 없음 | DSA-4210-1 | CVE-2018-3639: speculative execution, variant 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – security update |
| VRVDR- 41946 |해당사항 없음 | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – security update |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – security update |

## 1801k
{: #1801k}

2018년 6월 8일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-42084 | 긴급 | PB IPsec 터널에 대한 VFP 인터페이스로 링크되는 NAT 세션은 라우터가 이를 수행하도록 구성된 경우에도 라우터에 도착하는 패킷에 대해 작성되지 않음 |
| VRVDR-41944 | 높음 | VRRP에서 일부 VTI 터널을 장애 복구한 후 “vpn restart” 또는 피어 재설정이 실행될 때까지 재설정에 실패함 |
| VRVDR-41906 | 높음 | ICMP type 3 scode 4 메시지가 잘못된 소스 IP에서 발송되었으므로 PMTU 발견에 실패함 |
| VRVDR-41558 | 높음 | 패킷 추적에 보고된 시간소인이 실제 시간 및 시스템 시계와 일치하지 않음 |
| VRVDR-41469 | 높음 | 하나의 인터페이스 링크 작동 안함 – 본드에서 트래픽을 전달하지 않음 |
| VRVDR-41420 | 높음 | active-backup 모드가 LACP로 변경된 LACP 본딩 상태/링크 “u/D” |
| VRVDR-41313 | 심각 | IPsec – VTI 인터페이스 불안정 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – security update |
| VRVDR- 42013 |해당사항 없음 | DSA-4210-1 | CVE-2018-3639: Speculative execution, variant 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – security update |
| VRVDR- 41946 |해당사항 없음 | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – security update |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – security update |

## 1801j
{: #1801j}

2018년 5월 18일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41481 | 낮음 | 본드 인터페이스의 VRRP에서 VRRP 통지를 발송하지 않음 |
| VRVDR-39863 | 높음 | 고객이 연관된 GRE로 라우팅 인스턴스를 제거하고 터널 로컬 주소가 VRRP의 일부인 경우 VRRP가 장애 복구됨 |
| VRVDR-27018 | 심각 | 구성 파일 실행은 글로벌로 읽기 가능함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update |

## 5.2R6S10
{: #5-2R6S10}

2018년 5월 17일에 릴리스됨

**해결된 문제**
| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41543 | 높음 | “\” 백슬래시가 구성 설명에 사용되는 경우 “update config-sync”에서 오류를 보고함
| VRVDR-27018 | 심각 | 구성 파일 실행은 글로벌로 읽기 가능함 |

## 1801h
{: #1801h}

2018년 5월 11일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41664 | 심각 | 데이터플레인에서 MTU 크기의 ESP 패킷을 삭제함 |
| VRVDR-41536 | 낮음 | dns 포워딩이 사용으로 설정되면 4개를 초과하는 정적 호스트 항목이 추가되는 경우 Dnsmasq 서비스 start-init 한계에 도달함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7.8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux security update |

## 5.2R6S
{: #5-2R6S}

2018년 5월 8일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40803 | 낮음 | VIF 인터페이스가 다시 부팅 이후 “show vrrp” 출력에 표시되지 않음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |

## 1801g
{: #1801g}

2018년 5월 4일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41620 | 높음 | vTI 인터페이스 트래픽에서 새 vIF가 추가된 후 발송 트래픽을 중지함 |
| VRVDR-40965 | 높음 | 데이터 플레인 충돌 이후 본딩이 복구되지 않음 |

## 1801f
{: #1801f}

2018년 4월 23일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41537 | 낮음 | 1801d의 IPsec 터널에서 Ping이 작동하지 않음 |
| VRVDR-41283 | 낮음 | 구성에 정적 라우트가 사용 안함으로 설정되어 있는 경우 부팅하는 동안 Configd에서 정적 라우트 처리를 중지함 |
| VRVDR-41266 | 높음 | VRF로 정적 라우트 누수가 발생하면 다시 부팅 이후에 mGRE 터널에서 트래픽이 전송되지 않음 |
| VRVDR-41255 | 높음 | 슬레이브가 중단되는 경우 마스터 링크 상태가 해당 사항을 반영하려면 60초 이상 소요됨 |
| VRVDR-41252 | 높음 | 구역 정책에 바인드되지 않은 VTI를 사용하면, 구역 규칙의 커미트 순서에 따라 삭제 규칙이 무시됨 |
| VRVDR-41221 | 심각 | vRouter를 1801b에서 1801c 및 1801d로 업그레이드하는 경우 실패율이 10%에 해당됨 |
| VRVDR-40967 | 높음 | IPv6 포워딩을 사용 안함으로 설정하면 IPv4 패킷에서 오는 VTI의 라우팅이 금지됨 |
| VRVDR-40858 | 높음 | MTU 1428을 표시하는 VTI 인터페이스는 TCP PMTU 문제를 발생시킴 |
| VRVDR-40857 | 심각 | Vhost-bridge는 특정 길이의 인터페이스 이름으로 태그가 지정된 VLAN에 대해 작동하지 않음 |
| VRVDR-40803 | 낮음 | VIF 인터페이스가 다시 부팅 이후 “show vrrp” 출력에 표시되지 않음 |
| VRVDR-40644 | 높음 | IKEv1: QUICK_MODE 재전송이 올바르게 처리되지 않음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |
| VRVDR- 41331 | 6.5 | DSA-4158-1 | CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – security update
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update |
| VRVDR- 41215 | 6.1 | CVE-2018-1059 | CVE-2018-1059 – DPDK vhost out of bound host memory access from VM guests |

## 5.2R6S8
{: #5-2R6S8}

2018년 4월 16일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-41283 | 낮음 | 구성에 정적 라우트가 사용 안함으로 설정되어 있는 경우 부팅하는 동안 Configd에서 정적 라우트 처리를 중지함 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update

## 1801e
{: #1801e}

2018년 3월 28일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-39985 | 낮음 | GRE 터널 MTU보다 큰 TCP DF 패킷이 필요한 ICMP 단편화를 리턴하지 않고 삭제됨 |
| VRVDR-41088 | 심각 | 확장된(4바이트) ASN이 부호 없는 유형으로 내부적으로 표시되지 않음 |
| VRVDR-40988 | 심각 | 특정 수의 인터페이스와 사용되는 경우 Vhost가 시작되지 않음 |
| VRVDR-40927 | 심각 | DNAT: SIP 200 OK의 SDP는 183 응답 다음에 나오는 경우 변환되지 않음 |
| VRVDR-40920 | 높음 | listen-address로 127.0.0.1을 사용하면 snmpd가 시작되지 않음 |
| VRVDR-40920 | 심각 | ARP는 본딩 SR-IOV 인터페이스에서 작동하지 않음 |
| VRVDR-40294 | 높음 | 본딩 그룹에서 슬레이브가 제거되고 나면 데이터플레인에서 이전 큐를 복원하지 않음 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 41172 |해당사항 없음 | DSA-4140-1 | DSA 4140-1: libvorbis security update |

## 5.2R6S7
{: #5-2R6S7}

2018년 5월 15일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-38801 | 높음 | IPsec VTI를 통해 수신된 다중 세그먼트 패킷으로 인해 본드 인터페이스가 중단됨

## 5.2R6S6
{: #5-2R6S6}

2018년 5월 12일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40281 | 높음 | 5.2에서 최신 버전으로 업그레이드한 후 운영 모드에서 “vbash: show: command not found” 오류가 발생함 |
| VRVDR-40135 | 높음 | VIF 인터페이스 브릿지 포트에서 스패닝 트리 패킷이 수신되지 않음 |
| VRVDR-39991 | 높음 | Stateful 방화벽이 동일한 인터페이스에 있는 두 서브넷 간의 패킷을 삭제함 |
| VRVDR-36481 | 높음 | 5.2R4에서 17.1.0/5.2R3로 업그레이드/다운그레이드하는 경우 다음이 표시됨: /opt/vyatta/sbin/vyatta-install-image.functions: line 372: is_onie_boot: command not found |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8.8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – security update |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5717 / Spectre, aka. Variant #2 |

## 1801d
{: #1801d}

2018년 5월 8일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40940 | 높음 | NAT/방화벽과 관련된 데이터 플레인 충돌 |
| VRVDR-40886 | 높음 | 규칙의 다른 여러 구성과 “icmp name <value>”을 결합하면 방화벽이 로드되지 않음 |
| VRVDR-39879 | 높음 |점보 프레임에 대한 본딩 구성에 실패 |

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – 보안 업데이트 |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5715 / Spectre, aka variant #2 |

## 1801c
{: #1801c}

2018년 5월 7일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40281 | 높음 | 5.2에서 최신 버전으로 업그레이드한 후 운영 모드에서 “-vbash: show: command not found” 오류가 발생함 |

## 1801b
{: #1801b}

2018년 2월 21일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40622 | 높음 | IP 주소를 DHCP 서버에서 확보한 경우 Cloud-init 이미지를 올바르게 감지하는 데 실패함 |
| VRVDR-40613 | 심각 | 실제 링크 중 하나가 중단되는 경우 본드 인터페이스가 작동하지 않음 |
| VRVDR-40328 | 높음 | Cloud-init 이미지를 부팅하는 데 시간이 오래 소요됨 |

## 1801a
{: #1801a}

2018년 2월 7일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-40324 | 높음 | 로드 평균은 본딩 인터페이스를 사용하는 라우터에서 로드 없이 1.0을 초과함 |

## 5.2R6S5
{: #5-2R6S5}

2018년 1월 19일에 릴리스됨

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5.6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – security update (Meltdown) |
| VRVDR- 38265 | 8.8 | DSA-3970-1 | CVE-2017-1 |

## 5.2R6S4
{; #5-2R6S4}

2017년 12월 15일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-39529 | 높음 | DHCP 서버 장애 복구에서 데이터베이스를 동기화하지 않음 |
| VRVDR-39399 | 심각 | vrrp/다중 인터페이스 플랩/세그먼트 결함 표시에서 Vyatta가 네트워크 상태 FAULT를 삭제함 |
| VRVDR-39112 | 높음 | ZBF와 일치하는 DNAT 트래픽에서 플로우의 첫 번째 패킷만 삭제함 |
| VRVDR-38075 | 낮음 | 응답자에서 “restart vpn”이 발행되는 경우 개시자가 연결을 재설정하지 않음 |
| VRVDR-37934 | 심각 | aggregate-address summary-only가 구성되거나/정적 라우트가 누락된 경우 BGPd 충돌함 |
| VRVDR-37717 | 낮음 | 버전 출력에서 hard-enf “Description” 및 “License” 필드의 이름 바꾸기 |
| VRVDR-37689 | 높음 | 높은 NIC PF 인터럽트 비율 |
| VRVDR-37633 | 심각 | 활동 지속 정지 |

## 5.2R6S3
{: #5-2R6S3}

2017년 12월 4일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-39207 | 심각 | 본드 VIF 인터페이스에서 ARP 실패 |


## 5.2R6S2
{: #5-2R6S2}

2017년 11월 2일에 릴리스됨

**해결된 문제**

| 문제 번호 | 우선순위 |요약 |
| --- | --- | --- |
| VRVDR-39177 | 높음 | OpenVPN 서버 domain-name 옵션이 –push dhcp-option과 함께 적용되지 않음 |
| VRVDR-39129 | 심각 | OpenVPN 서버 push-route 매개변수로 인해 OpenVPN을 시작하는 데 실패 |

## 5.2R6S1
{: #5.2R6S1}

2017년 10월 12일에 릴리스됨

**해결된 보안 취약점**

| 문제 번호 | CVSS 점수 | 보안 권고문 |요약 |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9.8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- security update |

여기에 포함된 정보는 AT&T에서 제공하는 오퍼, 약정, 표시 또는 보증이 아니며 변경될 수 있습니다. 서면으로 작성된 계약에 명시된 경우를 제외하고 AT&T 회사 외부에서 사용 또는 공개할 수 없습니다.

© 2018 AT&T Intellectual Property. All rights reserved. AT&T 및 Globe 로고는 AT&T Intellectual Property의 등록상표입니다. 기타 상표는 해당 상표 소유자의 재산입니다.
