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

# 고가용성 구성 동기화
고가용성(HA) 쌍의 두 VRA(Virtual Router Appliance)는 두 디바이스가 유사한 방식으로 작동하도록 구성을 충분히 동기화해야 합니다. 이는 `configuration sync-maps`를 통해 수행되고 동기화할 구성의 부분을 선택할 수 있습니다. 한 시스템에서 변경하면 표시된 구성을 다른 디바이스로 푸시합니다.

**참고:** 이는 원격 디바이스에서 로컬 디바이스의 실행 구성을 동기화하고 저장합니다. 그러나 커미트 프로세스의 한 단계로 로컬 디바이스에 구성을 저장하지 않습니다. 

한 시스템에 고유한 구성을 다른 시스템에 동기화하지 않아야 합니다. 예를 들어 실제 IP 주소와 MAC을 동기화하지 않아야 합니다. `system config-sync` 구성 노드와 `service https` 노드는 동기화할 수 없습니다.

다음 예에서는 config-sync를 보여줍니다.

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

처음 두 행은 실제 sync-map을 작성합니다. 여기서 `security firewall`의 구성 스탠자가 `sync-map`에 설정됩니다. 그 결과 구성 노드에서의 변경사항이 원격 디바이스로 푸시됩니다. 그러나 `security user`의 변경사항은 규칙과 일치하지 않으므로 전송되지 않습니다. `sync-map`을 원하는 대로 특정하게 또는 일반적으로 작성할 수 있습니다.

다음 세 행은 원격 라우터의 `config-sync` 사용자와 비밀번호, IP 및 푸시할 sync-map을 지정합니다. `TEST`의 규칙과 일치하는 변경사항은 이 로그인 정보를 사용하여 `remote-router 192.168.1.22`로 이동합니다. `REST` 호출은 VRA API를 사용하여 수행되므로 HTTPS 서버가 원격 라우터에서 실행 중(차단 해제)이어야 합니다.

Config-sync는 변경사항을 커미트할 때마다 발생합니다. 원격 디바이스의 오류 메시지를 확인하십시오. 구성이 동기화되지 않은 경우 이를 다시 작동하도록 원격 디바이스에서 수정해야 합니다.

또한 `show config-sync difference` 명령을 사용하여 구성의 차이점을 확인할 수도 있습니다.
