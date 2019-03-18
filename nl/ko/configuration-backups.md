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

# 구성 백업
{: #backing-up-a-configuration}

시스템에 변경사항이 있는 경우 구성 명령을 백업해야 합니다. 운영 모드 명령 `show configuration commands`를 실행한 후 출력을 저장하여(예를 들어 SSH 세션에서 복사하여 붙여넣기) 이를 수행할 수 있습니다. 이는 구성에 대한 최소 백업으로 간주됩니다.

보다 완전한 백업에는 시스템에 대한 기술 지원 아카이브 생성이 포함됩니다. 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

생성된 아카이브 파일을 Virtual Router Appliance에서 선택한 스토리지 디바이스로 복사할 수 있습니다. 아카이브에는 구성 정보, 홈 디렉토리 및 로깅 정보의 백업이 포함됩니다. 이는 시스템의 보다 완전한 백업입니다. 

에를 들어 다음과 같습니다.

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

모든 관리 담당자가 액세스할 수 있는 중앙 위치에서 디바이스를 구성하는 동안 작성하는 모든 참고사항을 백업하는 것이 좋습니다.
