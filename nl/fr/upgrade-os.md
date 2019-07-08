---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: upgrade, os

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

# Mise à niveau du système d'exploitation
{: #upgrading-the-os}

La mise à niveau du système d'exploitation de VRA peut être effectuée avec la commande ``add system image`` sur un fichier ISO local téléchargé par notre équipe de support. Une liste des versions de mise à niveau Vyatta peut être obtenue à l'aide du système de tickets de demande de service IBM©.

Pour commencer le processus de mise à niveau, ouvrez un ticket dans le système de tickets d'IBM support pour demander une mise à niveau et le téléchargement d'une nouvelle image ISO sur votre système. Vous recevrez un e-mail du support IBM indiquant que le fichier ISO a été téléchargé. Dans l'exemple ci-dessous, il se trouve dans le répertoire ``tmp``.

Le processus de mise à niveau illustré ci-dessous s'applique à un seul dispositif VRA. Si vous utilisez VRA en mode haute disponibilité, vous devez exécuter la même commande de mise à niveau sur les deux systèmes. En outre, nous vous recommandons de mettre d'abord à niveau la machine de secours (`BACKUP`) et de vérifier si elle fonctionne correctement. Accédez ensuite à la machine principale (`MASTER`) et faites-la basculer sur l'autre machine à l'aide de la commande `reset vrrp`. Mettez ensuite à niveau la machine `MASTER` d'origine lorsque la machine de secours `BACKUP` a pris la relève.
{: important}

Pour mettre à niveau le dispositif VRA, procédez comme suit :

1. Exécutez la commande ``add system image <Local ISO File>``. 
2. Appuyez sur la touche **Entrée** pour accepter le nom par défaut de l'image ISO ou entrez le nom de votre choix.
3. Choisissez si vous voulez sauvegarder le répertoire de configuration et le fichier de configuration en cours.
4. Choisissez si vous voulez sauvegarder les clés d'hôte SSH de votre configuration en cours.
5. Appuyez sur la touche **Entrée** pour accepter la console système par défaut, ou choisissez la console de votre choix.
6. Appuyez sur la touche **Entrée** pour accepter la valeur par défaut de la vitesse de la console, ou entrez une valeur de votre choix.
7. Entrez la commande `reboot`, puis tapez `Yes` pour redémarrer la machine.
8. Si vous utilisez VRA en mode haute disponibilité, exécutez à nouveau les étapes 1 à 7 sur la deuxième machine.

L'exemple ci-dessous illustre le processus de mise à niveau.

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
