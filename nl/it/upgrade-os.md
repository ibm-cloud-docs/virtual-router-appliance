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

# Aggiorna il SO
L'aggiornamento del sistema operativo VRA può essere eseguito con il comando ``add system image`` in un file ISO locale caricato dal nostro team di supporto. È possibile ottenere un elenco di aggiornamenti Vyatta disponibili utilizzando il sistema di ticket di supporto IBM.

Per avviare il processo di aggiornamento, apri un ticket con il sistema di ticket di supporto di IBM richiedendo un aggiornamento e che sia caricata una nuova immagine ISO nel tuo sistema. Riceverai un'email dal supporto IBM che indica dove il file ISO è stato caricato. Nel seguente esempio, è nella directory ``tmp``.

**NOTA:** il processo di aggiornamento di seguito illustrato è per un solo VRA. Se stai utilizzando il VRA nella modalità ad elevata disponibilità, devi eseguire lo stesso comando di aggiornamento su entrambi i sistemi. Inoltre, si consiglia di aggiornare prima la macchina `BACKUP` e verificare che funzioni correttamente. Quindi accedi alla macchina `MASTER` ed effettua il failover utilizzando il comando `reset vrrp`. Infine, aggiorna la macchina originale `MASTER` una volta che `BACKUP` ha preso il controllo.

Per aggiornare il VRA, esegui la seguente procedura.

1. Esegui il comando ``add system image <Local ISO File>``.
2. Premi **Invio** per accettare il nome predefinito dell'immagine ISO o immettine uno tuo.
3. Scegli se salvare la directory di configurazione corrente e il file di configurazione.
4. Scegli se salvare le chiavi host SSH dalla tua configurazione corrente.
5. Premi **Invio** per accettare la console di sistema predefinita, o immettine una tua.
6. Premi **Invio** per accettare la velocità della console predefinita, o immettine una tua.
7. Immetti il comando `reboot` e quindi `Yes` per riavviare la macchina.
8. Se stai utilizzando il VRA in una modalità ad elevata disponibilità, riesegui dal passo 1 al 7 nella seconda macchina.

Il seguente esempio illustra il processo di aggiornamento.

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
