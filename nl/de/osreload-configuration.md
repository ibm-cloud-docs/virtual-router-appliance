---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Betriebssystem erneut laden
Beim Ausführen des Prozesses zum erneuten Laden des Betriebssystems im Kundenportal werden alle auf der betreffenden Einheit vorhandenen Konfigurationen gelöscht und die ursprüngliche Konfiguration wiederhergestellt. Alle Änderungen, die seit dem letzten Laden des Betriebssystems vorgenommen wurden, gehen verloren. Dies kann beispielsweise zu einem Konflikt auf der Maschine mit dem erneut geladenen Betriebssystem führen, wenn Änderungen an der VRRP-Gruppe auf einer anderen Maschine vorgenommen wurden.

Gehen Sie wie folgt vor, um Ihr Betriebssystem erneut zu laden:

1. Melden Sie sich mit Ihren eindeutigen Berechtigungsnachweisen beim [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){: new_window} an.
2. Wählen Sie in der Dropdown-Liste den Eintrag **Einheitenliste** aus.
3. Klicken Sie auf den Server, der erneut geladen werden soll.
4. Wählen Sie im Dropdown-Menü **Aktionen** in der linken oberen Ecke der Seite die Option **Betriebssystem erneut laden** aus.
5. Wählen Sie in der Anzeige für das erneute Laden des Betriebssystems alle Komponenten und Optionen aus, die beim erneuten Laden angewendet werden sollen. 
6. Klicken Sie auf die Schaltfläche **Vorherige Konfiguration erneut laden**, um das Popup-Fenster **Überprüfen** zu öffnen. Klicken Sie auf **Abbrechen**, um die Änderungen zu verwerfen und das Fenster zu schließen.
7. Überprüfen Sie, dass alle Details im Abschnitt 'Neue Konfiguration' korrekt sind. Klicken Sie auf **Weiter**, um das Popup-Fenster 'Bestätigen' zu öffnen.
8. Klicken Sie auf die Schaltfläche **Erneutes Laden des Betriebssystems bestätigen**, um die Aktion zu bestätigen und das erneute Laden des Betriebssystems zu starten. Klicken Sie auf **Abbrechen**, um die Aktion abzubrechen.

Nachdem das erneute Laden des Betriebssystems eingeleitet wurde, wird die Einheit offline geschaltet und das erneute Laden des Betriebssystems beginnt. Die für das erneute Laden des Betriebssystems erforderliche Zeit variiert entsprechend der aktuellen und der neuen Konfiguration der Einheit. Während der Durchführung des Konfigurationsprozesses wird in jedem Dialogfenster die Mindestzeit für das erneute Laden des Betriebssystems angezeigt. Der angezeigte Zeitrahmen ist ein vom System ermittelter Schätzwert. Wenn das erneute Laden länger als 24 Stunden dauert, verständigen Sie das Support-Team. Wenn die Einheit wieder online ist, arbeitet sie wie in der neuen Konfiguration für das erneute Laden des Betriebssystems angegeben. 

**HINWEIS:** Alle zuvor in der Einheit gespeicherten Daten gehen verloren, sie können jedoch wiederhergestellt werden, wenn vor dem erneuten Laden des Betriebssystems ein Backup der Einheit erstellt wurde. Daten, die nicht gesichert wurden, können nicht wiederhergestellt werden.
