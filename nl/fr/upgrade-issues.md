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

# Problèmes liés à la mise à niveau
Après une mise à niveau et un redémarrage d'une nouvelle version du système d'exploitation Vyatta, il se peut que vous ne puissiez pas exécuter des commandes utilisateur. 

Par exemple :

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

Dans ce cas, le problème n'est pas lié à la mise à niveau proprement dite. Si des erreurs s'étaient produites lors de ce processus, vous les auriez vues lors de l'exécution de la commande `add system image`. Ici, l'unité a été redémarrée, mais, désormais, elle comporte un nouvel espace de répertoire `/home` vide et les répertoires de base de tous les utilisateurs qui apparaissent dans la configuration devront être régénérés. L'erreur est liée à l'échec de la copie des fichiers "dotfile" requis dans le répertoire utilisateur `vyatta` :

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

Notez que trois fichiers ont une longueur égale à zéro, et par conséquent, ils n'ont aucune configuration. Sans les commandes d'initialisation de l'environnement pour l'utilisateur VRA lors de la connexion, l'interpréteur de commandes en cours ne peut pas interpréter les commandes Vyatta que vous émettez. Par conséquent, vous devez vous procurer les anciens fichiers "dotfile" à partir d'une autre source. 

Heureusement, l'ancien répertoire `home` existe encore en tant que répertoire de persistance, ce qui vous permet de recopier les fichiers. Pour ce faire, accédez à `/lib/live/mount/persistence/sda2/boot` et répertoriez les répertoires :

```
vyatta@acs-jmat-vyatta01:/lib/live/mount/persistence/sda2/boot$ ls -la
total 20
drwxr-xr-x 5 root root 4096 Feb  2 11:54 .
drwxr-xr-x 4 root root 4096 Nov 20 05:00 ..
drwxr-xr-x 4 root root 4096 Jan 23 11:30 5.2R5S3.06301309
drwxr-xr-x 4 root root 4096 Feb  2 11:54 5.2R6S5.01261706
drwxr-xr-x 5 root root 4096 Feb  2 11:54 grub
```

Les images ISO pour l'installation initiale et votre système d'exploitation en cours d'exécution sont présentés ici.  

**REMARQUE :** si vous avez effectué plus d'une mise à niveau, ces mises à niveau apparaîtront également ici. 

Ensuite, changez de répertoire en utilisant le système d'exploitation précédemment chargé comme répertoire suivant, puis accédez au répertoire de base VRA :

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

Une fois dans le répertoire, vous pouvoir voir les fichiers "dotfile" et les recopier :

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

Une fois les fichiers copiés, déconnectez-vous, puis reconnectez-vous :

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
Toutes vos commandes seront de nouveau opérationnelles et vous pourrez poursuivre normalement. 

**REMARQUE :** la copie du certificat HTTPS `/etc/lighttpd/server.pem` peut également échouer durant le processus de mise à niveau de système d'exploitation et provoquer éventuellement l'échec de la synchronisation des configurations à haute disponibilité. Pour résoudre ce problème, recopiez l'ancien fichier `server.pem` en plus des fichiers répertoriés ci-dessus (exécutez `su -` pour atteindre le niveau racine, puis exécutez la commande `copy`), puis exécutez la commande `restart https` pour redémarrer le fichier `demon.m` HTTPS (et les fichiers répertoriés ci-dessus). 
