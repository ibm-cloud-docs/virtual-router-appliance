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

# 액세스 및 구성
VRA는 SSH를 통한 원격 콘솔을 사용하거나 웹 GUI에 로그인하여 구성할 수 있습니다. 기본적으로 웹 GUI는 공용 인터넷에서 사용할 수 없습니다. 웹 GUI를 사용하려면 먼저 SSH를 통해 로그인하십시오.

**참고:** 쉘과 인터페이스 외부에 VRA를 구성하면 예기치 않은 결과가 발생할 수 있으므로 권장되지 않습니다.

## SSH를 사용하여 디바이스에 액세스
Linux, BSD 및 Mac OSX와 같은 대부분의 Unix 기반 운영 체제에는 기본 설치에 포함된 OpenSSH 클라이언트가 있습니다. Windows 사용자는 PuTTy와 같은 SSH 클라이언트를 다운로드할 수 있습니다.

공용 IP에는 SSH를 사용하지 않고 사설 IP에만 SSH를 허용할 것을 권장합니다. 사설 IP에 대한 연결에는 사설 네트워크에 연결된 클라이언트가 필요합니다. 고객 포털에 제공된 기본 VPN 옵션(PPTP, SSL-VPN 및 IPsec) 중 하나를 사용하거나 VRA에 구성된 사용자 정의 VPN 솔루션을 사용하여 로그인할 수 있습니다.

**디바이스 세부사항** 페이지에서 Vyatta 계정을 사용하여 SSH를 통해 로그인하십시오. 루트 비밀번호도 제공되지만 루트 로그인은 보안 상의 이유로 기본적으로 사용 안함으로 설정됩니다.

`ssh vyatta@[IP address] `

**참고:** 루트 로그인은 계속해서 사용 안함으로 설정하는 것이 좋습니다. Vyatta 계정을 사용하여 로그인하고 필요한 경우에만 루트로 승격시키십시오.

로그인하는 데 Vyatta 계정이 필요하지 않도록 배치 중에 SSH 키도 제공될 수 있습니다. SSH 키를 사용하여 VRA에 액세스할 수 있는지 확인한 후에 다음 명령을 실행하여 표준 사용자/비밀번호 로그인을 사용 안함으로 설정할 수 있습니다.

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## 웹 GUI를 사용하여 디바이스에 액세스

위의 SSH 지시사항을 따라 VRA에 로그인한 후 다음 명령을 실행하여 HTTPS 서비스를 사용으로 설정하십시오.

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

이 명령을 완료한 후 브라우저의 주소 표시줄에 `https://<ip.address>`를 입력하십시오. 여기서 IP 주소를 VRA의 IP 주소로 바꾸십시오. VRA의 자체 발행 인증서를 승인하도록 요청받을 수 있습니다. 이를 수행한 다음 프롬프트가 표시되면 Vyatta 인증 정보로 웹 인터페이스에 로그인하십시오.

## 모드
**구성 모드:** `configure` 명령을 사용하여 호출하면 이 모드에서 VRA 시스템의 구성이 수행됩니다.

**운영 모드:** VRA 시스템에 로그인할 때의 초기 모드입니다. 이 모드에서 `show` 명령을 실행하여 시스템 상태에 대한 정보를 조회할 수 있습니다. 또한 이 모드에서 시스템을 다시 시작할 수도 있습니다.

VRA의 쉘은 여러 가지의 조작 모드가 있는 모달 인터페이스입니다. 1차/기본 모드는 **운영**이고 이 모드가 로그인할 때 표시되는 모드입니다. **운영** 모드에서는 모드에서는 정보를 보고 날짜/시간 설정 또는 디바이스 다시 부팅과 같이 시스템의 현재 실행에 영향을 주는 명령을 실행할 수 있습니다.

`configure` 명령을 수행하면 사용자가 **구성** 모드가 되며 여기서 구성 편집을 수행할 수 있습니다. 구성 변경사항은 즉시 적용되지 않습니다. 변경사항을 커미트하고 저장해야 합니다. 명령을 입력하면 구성 버퍼로 이동합니다. 필요한 모든 명령을 입력한 후 `commit` 명령을 실행하여 변경사항을 적용하십시오.

명령을 영구적으로 저장하려면 `commit` 명령 뒤에 `save` 명령을 실행하십시오.

`run`으로 명령을 시작하여 구성 모드에서 운영 모드 명령을 실행할 수 있습니다. 예를 들어 다음과 같습니다.


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

해시 마크(`#`)는 구성 모드를 표시합니다. `run`으로 명령을 시작하면 운영 명령이 표시되는 VRA 쉘을 표시합니다. 이전 예에서도 명령 출력에 대한 "grep" 기능을 표시합니다.

## 명령 탐색

VRA 명령 쉘에는 탭 완료 기능이 포함됩니다. 사용 가능한 명령을 알아보려면 Tab 키를 눌러 목록 및 간단한 설명을 보십시오. 이는 쉘 프롬프트에서와 명령 입력 중에 모두 작동합니다. 예를 들어 다음과 같습니다.

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## 샘플 구성
구성은 노드의 계층 구조 패턴으로 배치됩니다. 이 정적 라우트 블록을 고려해 보십시오.

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

다음이 명령에서 생성됩니다.

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

이 예는 노드(정적)가 여러 하위 노드를 가질 수 있음을 보여줍니다. `192.168.1.0/24`에 대한 라우트를 제거하기 위해 `delete protocols static route 192.168.1.0/24` 명령이 사용됩니다. `192.168.1.0/24`가 명령에서 제외되면 두 라우트 노드가 모드 삭제되도록 표시됩니다.

`commit` 명령이 실행될 때까지는 구성이 실제로 변경되지 않음을 주의하십시오. 현재 실행되는 구성을 구성 버퍼에서 표시되는 변경사항과 비교하려면 `compare` 명령을 사용하십시오. 구성 버퍼를 비우려면 `discard`를 사용하십시오.

## 사용자 및 역할 기반 액세스 제어(RBAC)
사용자 계정을 다음 세 가지 액세스 레벨로 구성할 수 있습니다.

* 관리자
* 운영자
* 수퍼유저

운영자 레벨 사용자는 `show` 명령을 실행하여 시스템의 실행 상태를 보고 `reset` 명령을 실행하여 디바이스에서 서비스를 다시 시작할 수 있습니다. 운영자 레벨 권한은 읽기 전용 액세스를 의미하지 않습니다.

관리자 레벨 사용자는 디바이스의 모든 구성 및 조작에 대한 전체 액세스 권한을 가집니다. 관리자 사용자는 실행되는 구성을 보고 디바이스의 구성 설정을 변경하며 디바이스를 다시 부팅하고 디바이스를 종료할 수 있습니다.

수퍼유저 레벨 사용자는 관리자 레벨 권한 뿐 아니라 `sudo` 명령을 통해 루트 권한으로 명령을 실행할 수 있습니다.

사용자를 비밀번호 또는 공개 키 인증 스타일 또는 둘 다에 대해 구성할 수 있습니다. 공개 키 인증을 SSH와 함께 사용하고 사용자가 키 파일을 사용하여 시스템에 로그인할 수 있습니다. 비밀번호로 운영자 사용자를 작성하려면 다음을 수행하십시오.

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**참고:** 레벨을 지정하지 않으면 사용자가 관리자 레벨로 간주됩니다. 이 경우 사용자 비밀번호는 구성 파일에서 암호화된 것으로 표시됩니다.

역할 기반 액세스 제어(RBAC)는 구성 일부에 대한 액세스를 권한 부여된 사용자로 제한하는 방법입니다. RBAC를 사용하면 관리자가 실행할 수 있는 명령을 제한하는 사용자 그룹에 대한 역할을 정의할 수 있습니다.

RBAC는 액세스 제어 관리(ACM) 규칙 세트에 지정된 그룹을 정의하고, 사용자를 그룹에 추가하고, 시스템의 경로와 그룹이 일치하는 규칙 세트를 작성하고, 그룹에 적용되는 경로를 허용하거나 거부하도록 시스템을 구성하여 수행됩니다.

기본적으로 Virtual Router Appliance에 정의된 ACM 규칙 세트가 없으며 ACM은 사용 안함으로 설정됩니다. RBAC를 사용하여 세분화된 액세스 제어를 제공하려면 ACM을 사용으로 설정하고 사용자 소유의 정의된 규칙 외에 다음 기본 ACM 규칙을 추가해야 합니다.

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

ACM 규칙을 사용으로 설정하기 전에 보충 [문서](vra-docs.html)를 참조하십시오. 올바르지 않은 ACM 규칙을 사용하면 액세스 거부나 시스템 고장이 발생할 수 있습니다.
