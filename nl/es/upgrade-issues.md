---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: upgrade, issues, troubleshooting

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

# Problemas de actualización
{: #upgrade-issues}

Ocasionalmente, después de una actualización correcta y del reinicio de una nueva versión de Vyatta OS, puede producirse un problema por el que no pueda emitir mandatos de usuario.

Por ejemplo:

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

En este caso, el problema no es con la propia actualización. Si hubiera errores en dicho proceso, los vería al emitir el mandato `add system image`. En este caso, el dispositivo se ha reiniciado, pero ahora tiene un nuevo espacio de directorio `/home` vacío, y es necesario regenerar los directorios de inicio de los usuarios que aparecen en la configuración. El error se deriva de no haber copiado correctamente los "dotfiles" necesarios en el directorio de usuario `vyatta`:

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

Tenga en cuenta que hay tres archivos de longitud cero, y por lo tanto no tienen configuración. Sin los mandatos para inicializar el entorno para el usuario de VRA en el inicio de sesión, el shell actual no puede interpretar los mandatos Vyatta que se emiten. Como consecuencia, debe obtener los dotfiles anteriores de un origen diferente.

Afortunadamente, el directorio `home` anterior todavía existe como directorio de persistencia, lo que le permite copiar los archivos. Para ello, vaya a `/lib/live/mount/persistence/sda2/boot` y liste los directorios allí:

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

Las ISO de la instalación inicial y el sistema operativo que se ejecuta actualmente aparecen aquí.

Si ha realizado más de una actualización, también se mostrarán aquí.
{: note}

A continuación, cambie los directorios utilizando el sistema operativo cargado anteriormente como directorio siguiente, y vaya al directorio inicial de VRA:

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

Una vez dentro, verá los dotfiles y podrá copiarlos:

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

Una vez copiados, cierre la sesión y después vuelva a iniciarla:

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
Todos los mandatos funcionarán de nuevo y podrá continuar con normalidad.

También puede fallar la copia del certificado HTTPS `/etc/lighttpd/server.pem` durante el proceso de actualización del sistema operativo, lo que puede provocar que no se puedan sincronizar las configuraciones de alta disponibilidad (HA). Para solucionar este problema, copie el archivo antiguo `server.pem` además de los archivos enumerados más arriba (emita `su -` para llegar al nivel raíz y, a continuación, emita el mandato `copy`) luego emita `restart https` para reiniciar el archivo `demon.m` HTTPS (y los archivos enumerados más arriba).
{: important}
