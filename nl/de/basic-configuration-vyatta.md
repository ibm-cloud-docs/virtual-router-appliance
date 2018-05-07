---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Basiskonfiguration von Vyatta 5400

Gehen Sie wie folgt vor, um Vyatta 5400 zu konfigurieren.

Das öffentliche virtuelle LAN (VLAN) 1224, das jetzt zugeordnet und weitergeleitet wird, muss konfiguriert werden, bevor es Datenverkehr übergeben kann. Zwei Einzelinformationen sind erforderlich, um die Konfiguration auszuführen: 

  * Die öffentliche Seite der Brocade 5400 vRouter Appliance
  * Das Standardgateway 1224

Beachten Sie, dass es die öffentliche Seite des VLANs ist, auf der sich die Rechenoption befindet und nicht das öffentliche VLAN der Brocade 5400 vRouter Appliance. 

## Im SoftLayer-Portal

1\. Suchen Sie im SoftLayer-Webportal unter **Geräte** Verbindung zur Registerkarte **Konfiguration** für das Gerät Brocade 5400 vRouter. Vorausgesetzt wird, dass auf Ihrem Gerät <span style="text-decoration: underline">eth1=bond1</span> ist. 

2\. Wählen Sie im Webportal **Netz > IP-Verwaltung > VLANs** aus, um das Standardgateway für VLAN 1224 zu finden.

3\. Klicken Sie auf Ihr VLAN in der Liste und klicken Sie dann auf das **Teilnetz**, in dem Sie Ihr Gateway sehen. Notieren Sie die Details zur CIDR-Maske (Classless Inter-Domain Routing) nach dem Schrägstrich.  

## In der GUI von Vyatta

4\. Klicken Sie im Dashboard auf die Registerkarte **Konfiguration**. 

5\. Erweitern Sie **Schnittstellen > Bonding > bond1** auf der linken Seite der Anzeige: Heben Sie **vif** hervor.

6\. Geben Sie den "Namen" des VLANs im Feld **vif** ein (in unserem Fall 1224) und klicken Sie auf die Schaltfläche **Erstellen**. Der Prozess dauert einige Sekunden. 

7\. Wählen Sie den neu erstellten Eintrag **vif 1224** unter dem Twistie **vif** aus. 

8\. Markieren Sie das Feld unter **dhcp** und geben Sie die IP-Adresse des Standardgateways ein. Maskieren Sie die CIDR-Notation des VLANs, das Sie über den Brocade 5400 vRouter übergeben möchten (zum Beispiel VLAN 1224).

9\. Klicken Sie auf die Schaltfläche **Festlegen** und dann auf **Commit**.

10\. Klicken Sie in der mittleren Menüleiste auf **Speichern**. Andernfalls kehrt die Konfiguration beim nächsten Neustart des Systems zur Standardeinstellung zurück. 

**HINWEIS:** Ein Rollback der Konfiguration kann sehr sinnvoll sein, wenn Sie Ihre Konfiguration beim Testen von Änderungen beschädigt haben. Solange die Änderungen nicht gespeichert wurden, können Sie den Server vom Webportal aus erneut starten und so die vorherige Konfiguration wiederherstellen. 

11\. Klicken Sie auf die Registerkarte für Statistik und öffnen Sie die neue Schnittstelle,  um den Datenverkehr zu überprüfen und zu überwachen. 

## Konfigurieren Sie das private VLAN mithilfe der CLI

Es gibt zwei Befehlsmodi in der CLI (Command Line Interface): 'operational' und 'configuration'. 

Der Betriebsmodus (operational) bietet Zugriff auf die Betriebsbefehle für: 

  * Anzeigen und Löschen von Daten
  * Aktivieren und Inaktivieren der Fehlerbehebung
  * Konfigurieren von Terminaleinstellungen
  * Laden und Speichern der Konfiguration
  * Neustarten des Systems

Der Konfigurationsmodus (configuration) bietet Zugriff auf Befehle für: 

  * Erstellen
  * Ändern
  * Löschen
  * Commit durchführen
  * Konfigurationsdaten anzeigen
  * Navigieren durch die Konfigurationshirarchie

Wenn Sie sich beim System anmelden, befindet sich das System im Betriebsmodus. Sie müssen in den Konfigurationsmodus wechseln, um auf die Befehle zugreifen zu können. 

**HINWEIS:** Sie können an der Eingabeaufforderung erkennen, in welchem Modus ('operational' oder 'configuration') Sie sich gerade befinden. Sie befinden sich im Betriebsmodus (operational), wenn die Eingabeaufforderung das Doppelkreuz (#) ist. Sie befinden sich im Konfigurationsmodus (configuration), wenn die Eingabeaufforderung ein Dollarzeichen ($) ist.

Gehen Sie wie folgt vor, um das private VLAN mit der CLI zu konfigurieren. Notieren Sie die Werte, die für die Konfiguration des VLAN benötigt werden: 

  * VLAN-Name des VLANs, das über das Gerät Brocade 5400 vRouter weitergeleitet wird (2254)
  * Gateway und Maske (CIDR-Format) des VLANs, das weitergeleitet werden soll (10.52.69.201/29)
  * Privater Verbindungsname des Geräts Brocade 5400 vRouter (bond0)

1. SSH in Ihrem Brocade 5400 vRouter (öffentliche oder private IP-Adresse) mit **vyatta** als **Benutzername**. Geben Sie das Kennwort ein, wenn Sie dazu aufgefordert werden. 

   **HINWEIS:** Sie sollten einen neuen Benutzer innerhalb von Brocade 5400 vRouter erstellen und den Standarderstbenutzer `vyatta` inaktivieren.

2. Virtuelle Schnittstelle konfigurieren: 

  * Geben Sie in der Eingabeaufforderung *configure* ein, um in den Konfigurationsmodus zu wechseln. 
  * Geben Sie in der Eingabeaufforderung *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* ein, um die virtuelle Schnittstelle festzulegen. 
  * Geben Sie in der Eingabeaufforderung *commit* ein, um ein Commit für die Einstellungen durchzuführen. 
  * Geben Sie *save* ein, um die Einstellungen zu speichern. 
  * Geben Sie *exit* ein, um in den Betriebsmodus zurückzukehren. 

3\. Geben Sie *show interfaces* ein, um die Einstellungen zu prüfen, für die gerade ein Commit durchgeführt wurde. 

4\. Leiten Sie alle verbliebenen VLANs über das Gerät Brocade 5400 vRouter weiter. 
