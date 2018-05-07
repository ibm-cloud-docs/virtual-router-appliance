---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 업그레이드 문제
때때로, Vyatta OS의 새 버전을 업그레이드하고 다시 부팅한 후에 사용자 명령을 실행할 수 없는 문제가 발생할 수 있습니다. 

예를 들어 다음과 같습니다.

```
[jmathews@shelladmindal0101 ~]$ ssh 10.115.174.6 -l vyatta
Welcome to AT&T vRouter

Welcome to AT&T vRouter
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Last login: Fri Feb  2 12:42:45 2018 from 10.0.80.100
vyatta@acs-jmat-vyatta01:~$ show int
-vbash: show: command not found
```

이 경우, 업그레이드 자체에 문제는 없습니다. 해당 프로세스에 오류가 있는 경우 `add system image` 명령을 실행할 때 표시됩니다. 디바이스가 다시 부팅되었으나 이제 디바이스에는 비어 있는 새 `/home` 디렉토리 공간이 제공되었으며 구성에 표시된 모든 사용자는 다시 생성된 홈 디렉토리가 필요합니다. 오류는 필요한 "dotfiles"를 `vyatta` 사용자 디렉토리에 적절하게 복사하지 못해 발생합니다. 

```
vyatta@acs-jmat-vyatta01:~$ ls -la
total 16
drwxr-x--- 3 vyatta users 4096 Feb  2 12:44 .
drwxr-xr-x 1 root   root  4096 Feb  2 11:57 ..
-rw------- 1 vyatta users  456 Feb  2 12:44 .bash_history
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .bash_logout
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .bashrc
-rw-r--r-- 1 vyatta users    0 Feb  2 12:43 .profile
drwxr-x--- 2 vyatta users 4096 Feb  2 11:57 .ssh
```

세 가지 파일의 길이가 0이고 이로 인해 구성이 없습니다. 로그인 시 VRA 사용자용 환경을 초기화하기 위한 명령 없이, 현재 쉘은 사용자가 실행한 Vyatta 명령을 해석할 수 없습니다. 결과적으로, 다른 소스에서 이전 dotfiles를 가져와야 합니다. 

다행히 이전 `home` 디렉토리는 파일을 복사할 수 있는 지속성 디렉토리로 계속 존재합니다. 이를 수행하려면 `/lib/live/mount/persistence/sda2/boot`로 이동하여 디렉토리를 나열하십시오. 

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

여기서 초기 설치를 위한 ISO와 현재 실행 중인 OS가 작동합니다.  

**참고:** 하나 이상의 업그레이드를 수행한 경우에도 여기에 표시됩니다. 

이후, 다음 디렉토리로 이전에 로드된 OS를 사용하여 디렉토리를 변경한 후 VRA 홈 디렉토리로 이동하십시오. 

```
vyatta@acs-jmat-vyatta01:cd 5.2R5S3.06301309/persistence/rw/home/vyatta/
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ ls -la
total 343084
drwxr-x--- 3 vyatta users      4096 Jan 29 16:29 .
drwxr-xr-x 3 root   root       4096 Nov 20 05:05 ..
-rw------- 1 vyatta users     10537 Feb  2 11:55 .bash_history
-rw-r--r-- 1 vyatta users       220 Nov  5  2016 .bash_logout
-rw-r--r-- 1 vyatta users      3515 Nov  5  2016 .bashrc
-rw------- 1 vyatta users        53 Dec 25 10:45 .lesshst
-rw-r--r-- 1 vyatta users       675 Nov  5  2016 .profile
drwxr-x--- 3 vyatta users      4096 Jan  9 10:34 .ssh
-rw-r----- 1 vyatta users 351272960 Jan 26 14:23 vyatta-vrouter-5.2_20180126T1706-amd64.iso
```

내부에 들어가면 dotfiles를 볼 수 있고 복사할 수 있습니다. 

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .bashrc /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .profile /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cp .bash_logout /home/vyatta
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot/5.2R5S3.06301309/persistence/rw/home/vyatta$ cd /home/vyatta
vyatta@acs-jmat-vyatta01:~$ ls -la
total 28
drwxr-x--- 3 vyatta users 4096 Feb  2 12:44 .
drwxr-xr-x 1 root   root  4096 Feb  2 11:57 ..
-rw------- 1 vyatta users  456 Feb  2 12:44 .bash_history
-rw-r--r-- 1 vyatta users  220 Feb  2 12:56 .bash_logout
-rw-r--r-- 1 vyatta users 3515 Feb  2 12:56 .bashrc
-rw-r--r-- 1 vyatta users  675 Feb  2 12:56 .profile
drwxr-x--- 2 vyatta users 4096 Feb  2 11:57 .ssh
```

복사되면 로그아웃한 후 다시 로그인합니다. 

```
[jmathews@shelladmindal0101 ~]$ ssh 10.115.174.6 -l vyatta
Welcome to AT&T vRouter

Welcome to AT&T vRouter
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Last login: Fri Feb  2 12:57:29 2018 from 10.0.80.100
vyatta@acs-jmat-vyatta01:~$ show version
Version:      5.2R6S5
Description:  AT&T vRouter 5600 5.2R6S5
Built on:     Fri Jan 26 17:06:52 UTC 2018
System type:  Intel 64bit
Boot via:     image
HW model:     PIO-518D-TLN4F-ST031
HW S/N:       S14073214705613
HW UUID:      00000000-0000-0000-0000-0CC47A07EF22
Uptime:       12:57:47 up 59 min,  1 user,  load average: 0.35, 0.27, 0.26
vyatta@acs-jmat-vyatta01:~$
```
모든 명령이 다시 작동하고 정상적으로 진행될 수 있습니다. 

**참고:** HTTPS 인증서 `/etc/lighttpd/server.pem` 또한 OS 업그레이드 프로세스 중에 복사하는 데 실패하여 고가용성(HA) 구성이 동기화되지 않을 수 있습니다. 이 문제점을 수정하려면 이전 `server.pem` 파일과 위에 나열된 파일을 복사한 후(`su -`를 실행하여 루트 레벨에 도달한 후 `copy` 명령 실행) `restart https`를 실행하여 HTTPS `demon.m` 파일(및 위에 나열된 파일)을 재시작하십시오. 
