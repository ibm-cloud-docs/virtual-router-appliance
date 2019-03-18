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

# Probleme bei der Aktualisierung
{: #upgrade-issues}

Gelegentlich tritt nach einer erfolgreichen Aktualisierung und einem Warmstart einer neuen Version des Vyatta-Betriebssystems das Problem auf, dass Sie keine Benutzerbefehle eingeben können.

Beispiel:

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

In diesem Fall handelt es sich nicht um ein Problem der Aktualisierung selbst. Falls es in diesem Prozess Fehler gegeben hätte, würden Sie diese sehen, wenn Sie den Befehl `add system image` eingeben. In diesem Fall wurde die Einheit neu gestartet und hat einen neuen und leeren Speicherbereich im Ausgangsverzeichnis `/home`. Jeder Benutzer in der Konfiguration muss seine Ausgangsverzeichnisse neu generieren. Die Fehlerursache ist ein Fehler beim Kopieren der erforderlichen "dotfiles" in das `vyatta`-Benutzerverzeichnis:

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

Beachten Sie, dass drei Dateien die Länge null aufweisen und folglich über keine Konfiguration verfügen. Ohne die Befehle zum Initialisieren der Umgebung für den VRA-Benutzer bei der Anmeldung kann die aktuelle Shell die von Ihnen ausgegebenen Vyatta-Befehle nicht interpretieren. Folglich müssen Sie die alten Dateien (dotfiles) von einer anderen Quelle abrufen.

Glücklicherweise ist das bisherige Verzeichnis `home` als Persistenzverzeichnis noch vorhanden und Sie können die Dateien von dort kopieren. Wechseln Sie in das Verzeichnis `/lib/live/mount/persistence/sda2/boot` und listen Sie die Verzeichnisse dort auf:

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

Die ISOs für die Erstinstallation und Ihr aktuell aktives Betriebssystem werden hier aufgeführt. 

**HINWEIS:** Wenn Sie mehr als eine Aktualisierung durchgeführt haben, dann werden hier alle Aktualisierungen angezeigt.

Als nächstes wechseln Sie das Verzeichnis und das zuvor geladene Betriebssystem wird Ihr nächstes Verzeichnis. Danach wechseln Sie in das VRA-Ausgangsverzeichnis.

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

In diesem Verzeichnis sehen Sie die gesuchten Dateien und kopieren diese:

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

Nach dem Kopieren melden Sie sich ab und dann erneut wieder an:

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
Alle Befehle funktionieren wieder und Sie können Ihre Arbeit normal fortsetzen.

**HINWEIS:** Das HTTPS-Zertifikat `/etc/lighttpd/server.pem` kann während des Aktualisierungsprozesses für das Betriebssystem möglicherweise nicht kopieren. Dies kann dazu führen, dass die Hochverfügbarkeitskonfiguration (HA) nicht synchronisiert werden kann. Um dieses Problem zu beheben, kopieren Sie zusätzlich zu den oben aufgelisteten Dateien die alte Datei `server.pem`. (Geben Sie `su -` ein, um auf die Stammverzeichnisebene zu gelangen. Geben Sie dann den Befehl `copy` ein). Anschließend geben Sie `restart https` ein, um die HTTPS-Datei `demon.m` (und die oben aufgelisteten Dateien) erneut zu starten.
