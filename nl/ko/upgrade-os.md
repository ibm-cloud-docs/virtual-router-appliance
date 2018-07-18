---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# OS 업그레이드
지원 팀에서 업로드한 로컬 ISO 파일에서 ``add system image`` 명령을 사용하여 VRA 운영 체제를 업그레이드할 수 있습니다. IBM 지원 티켓 시스템에서 사용 가능한 Vyatta 업그레이드 버전 목록을 얻을 수 있습니다.

업그레이드 프로세스를 시작하려면 IBM 지원 티켓 시스템에서 업그레이드하고 새 ISO 이미지를 시스템에 업로드하도록 요청하는 티켓을 여십시오. IBM 지원 센터에서 ISO 파일이 업로드된 위치를 나타내는 이메일을 받습니다. 아래 예에서 이 파일은 ``tmp``에 있습니다.

**참고:** 아래 설명된 업그레이드 프로세스는 단일 VRA의 프로세스입니다. VRA를 고가용성 모드로 사용하는 경우 두 시스템 모두에서 동일한 업그레이드 명령을 실행해야 합니다. 또한 먼저 `BACKUP` 시스템을 업그레이드하고 제대로 작업되는지 확인하는 것이 좋습니다. 그런 다음 `MASTER` 시스템에 액세스한 후 `reset vrrp` 명령을 사용하여 장애 복구하십시오. 마지막으로 `BACKUP`으로 제어가 돌아오면 원래 `MASTER`를 업그레이드하십시오.

VRA를 업그레이드하려면 다음 프로시저를 수행하십시오.

1. ``add system image <Local ISO File>`` 명령을 실행하십시오.
2. **Enter**를 눌러 ISO 이미지의 기본 이름을 허용하거나 사용자 지정 이름을 입력하십시오.
3. 현재 구성 디렉토리 및 구성 파일을 저장할지 여부를 선택하십시오.
4. 현재 구성에서 SSH 호스트 키를 저장할지 여부를 선택하십시오.
5. **Enter**를 눌러 기본 시스템 콘솔을 허용하거나 사용자 지정 콘솔을 입력하십시오.
6. **Enter**를 눌러 기본 콘솔 속도를 허용하거나 사용자 지정 속도를 입력하십시오.
7. `reboot` 명령을 입력하고 `Yes`를 입력하여 시스템을 다시 부팅하십시오.
8. VRA를 고가용성 모드로 사용하는 경우 두 번째 시스템에서 1단계 ~ 7단계를 다시 실행하십시오.

아래 예는 업그레이드 프로세스를 보여줍니다.

```
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Welcome to the Brocade vRouter image installer.
Checking MD5 checksums of files on the ISO image...OK.
What would you like to name this image? [5.2R5S4.08091424]: 5.2R5S4
This image will be named: 5.2R5S4
Would you like to save the current configuration
directory and config file? (Yes/No) [Yes]:
Saving current configuration...
Would you like to save the SSH host keys from your
current configuration? (Yes/No) [Yes]:
Saving SSH keys...
Copying squashfs image...
Copying kernel and initrd images...
Copying saved configuration to config partition.
Copying saved SSH host keys.
Enter the desired system console [tty0]:
Enter the console speed [38400]:
Running post-install script...
Done.

vyatta@vra01:~$ show system image
The system currently has the following image(s) installed:

   1: 5.2R5S3.06301309 (running image)
   2: 5.2R5S4 (default boot)

vyatta@vra01:~$ reboot
Proceed with reboot? (Yes/No) [No] y
The system is going down for reboot NOW!

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```
