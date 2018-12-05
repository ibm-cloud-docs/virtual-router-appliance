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

# Upgrade für Betriebssystem durchführen
Das Betriebssystem der VRA kann mit dem Befehl ``add system image`` und einer lokalen ISO-Datei aktualisiert werden, die vom Support-Team heruntergeladen wurde. Eine Liste der verfügbaren Vyatta-Upgradeversionen kann über das System für IBM Support-Tickets abgerufen werden.

Um den Upgradeprozess zu starten, öffnen Sie ein IBM Support-Ticket, um ein Upgrade und das Hochladen eines neuen ISO-Images auf Ihr System anzufordern. In einer E-Mail-Nachricht vom IBM Support werden Sie darüber informiert, an welche Speicherposition die ISO-Datei hochgeladen wurde. Im nachfolgenden Beispiel wird das Verzeichnis ``tmp`` verwendet.

**HINWEIS:** Der unten dargestellte Upgradeprozess gilt für eine einzelne VRA. Wenn Sie VRA im Hochverfügbarkeitsmodus verwenden, müssen Sie denselben Upgrade-Befehl auf beiden Systemen des HA-Paars ausführen. Außerdem wird empfohlen, zuerst das Sicherungssystem (`BACKUP`) zu aktualisieren und zu überprüfen, ob es ordnungsgemäß funktioniert. Greifen Sie anschließend auf das Mastersystem (`MASTER`) zu und starten Sie die Funktionsübernahme mit dem Befehl `reset vrrp`. Führen Sie schließlich das Upgrade des ursprünglichen Mastersystems (`MASTER`) durch, nachdem das Sicherungssystem (`BACKUP`) die Steuerung übernommen hat.

Gehen Sie wie folgt vor, um ein Upgrade für das VRA durchzuführen.

1. Führen Sie den Befehl ``add system image <Local ISO File>``.
2. Drücken Sie die **Eingabetaste**, um den Standardnamen für das ISO-Image beizubehalten, oder geben Sie einen anderen Namen ein.
3. Wählen Sie aus, ob das aktuelle Konfigurationsverzeichnis und die Konfigurationsdatei gespeichert werden sollen.
4. Wählen Sie aus, ob die SSH-Hostschlüssel aus Ihrer aktuellen Konfiguration gespeichert werden sollen.
5. Drücken Sie die **Eingabetaste**, um die Standardsystemkonsole beizubehalten, oder geben Sie eine andere Konsole ein.
6. Drücken Sie die **Eingabetaste**, um die Standardgeschwindigkeit für die Konsole beizubehalten, oder geben Sie eine andere Geschwindigkeit ein.
7. Geben Sie den Befehl `reboot` und anschließend `Yes` ein, um die Maschine erneut zu starten.
8. Wenn Sie VRA im Hochverfügbarkeitsmodus verwenden, wiederholen Sie die Schritte 1 bis 7 für die zweite Maschine.

Das nachfolgende Beispiel veranschaulicht den Ugradeprozess.

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
