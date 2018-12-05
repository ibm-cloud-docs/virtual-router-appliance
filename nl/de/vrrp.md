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

# HA und VRRP
Virtual Router Appliance (VRA) unterstützt Virtual Router Redundancy Protocol (VRRP) als Protokoll für Hochverfügbarkeit. Die Bereitstellung von Einheiten folgt der Aktiv/Passiv-Methode, bei der eine Maschine als Mastersystem fungiert und die andere als Backupsystem. Alle Schnittstellen auf beiden Maschinen sind Mitglieder derselben Synchronisationsgruppe (sync-group), d. h. wenn eine Schnittstelle ausfällt, fallen die übrigen Schnittstellen in derselben Gruppe ebenfalls aus und die Einheit wird nicht länger als Master verwendet. Sobald das aktuelle Backupsystem erkennt, dass vom Mastersystem keine Keepalive- bzw. Heartbeatnachrichten mehr gesendet werden, übernimmt es die Steuerung der virtuellen VRRP-IPs und wird damit zum Mastersystem.

VRRP ist der wichtigste Bestandteil der Konfiguration beim Bereitstellen von Gateways. Die Funktion für Hochverfügbarkeit ist von den Heartbeatnachrichten abhängig, d. h. diese Nachrichten dürfen keinesfalls blockiert werden.

## Virtuelle IP-Adressen (VIP-Adressen) für VRRP

Die virtuelle IP-Adresse oder VIP für VRRP ist eine variable IP-Adresse, die von der Mastereinheit zur Backupeinheit wechselt, wenn die Funktionsübernahme stattfindet. Beim Bereitstellen einer VRA werden zugehörige Netzverbindungen für öffentliche und private Daten sowie echte IP-Adressen für beide Schnittstellen zugewiesen. Außerdem wird jeder der beiden Schnittstellen eine VIP zugewiesen (unabhängig davon, ob sie eigenständig ist oder zu einem HA-Paar gehört). Datenverkehr mit einer Ziel-IP in VLAN-Teilnetzen, die der VRA zugeordnet sind, wird direkt an die betreffenden VRRP-VIPs gesendet.

Die virtuellen VRRP-IP-Adressen für Gateway-Gruppen sollten nicht geändert werden und die VRRP-Schnittstelle sollte nicht inaktiviert werden. Diese IP-Adressen dienen als Methode für die Weiterleitung des Datenverkehrs zum Gateway, wenn ein VLAN zugeordnet ist. Wenn die IP-Adresse nicht vorhanden ist, kann der Datenverkehr vom SoftLayer-BCR bzw. -FCR nicht zum Gateway weitergeleitet werden.

## VRRP-Gruppe

Eine VRRP-Gruppe besteht aus einem Cluster mit Schnittstellen oder virtuellen Schnittstellen, die Redundanz für eine primäre Schnittstelle (Masterschnittstelle) in der Gruppe bereitstellen. Normalerweise befindet sich jede Schnittstelle der Gruppe auf einem separaten Router. Die Redundanz wird von dem VRRP-Prozess auf jedem System verwaltet. Die VRRP-Gruppe verfügt über eine eindeutige numerische Kennung, der bis zu 20 virtuelle IP-Adressen zugewiesen werden können.  

Die VRRP-Gruppen-ID wird von SoftLayer zugewiesen und sollte nicht geändert werden. Wenn eine neue Gateway-Gruppe zum ersten Mal hinter einem Front-End-Kundenrouter (FCR) oder Back-End-Kundenrouter (BCR) bereitgestellt wird, wird ihr die VRRP-Gruppennummer 1 zugewiesen. Für jede weitere bereitgestellte Gateway-Gruppe wird diese Nummer erhöht, um Konflikte zu vermeiden, d. h. die nächste Gruppe ist Gruppe 2, danach folgt Gruppe 3 usw. Die Berechnung und Zuweisung erfolgt durch den SoftLayer-Bereitstellungsprozess. Das Ändern dieses Wertes kann zu Konflikten mit anderen aktiven Gruppen führen und in der Folge einen Master/Master-Konflikt sowie eine Betriebsunterbrechung für beide Gateway-Gruppen verursachen.

Bei der Migration einer vorherigen Konfiguration sollten Sie Ihren Konfigurationscode dahingehend überprüfen, dass keine statische VRRP-Gruppen-ID zugewiesen wird.

Die VRRP-Gruppen-ID wird in der SoftLayer-Datenbank dauerhaft festgelegt, d. h. beim erneuten Laden des Betriebssystems oder bei einem Upgrade wird derselbe Gruppen-ID-Wert verwendet. Beim erneuten Laden des Betriebssystems wird jede vom Benutzer geänderte VRRP-Gruppen-ID durch den vom System zugewiesenen Wert überschrieben.  

## VRRP-Priorität

Der ersten Maschine in einer Gateway-Gruppe wird die Priorität 254 zugeordnet. Dieser Wert wird beim Bereitstellen jeder weiteren Einheit verringert. Verwenden Sie auf keinen Fall den Prioritätswert 255. Dieser Wert definiert den "Eigentümer" der VIP-Adresse und kann zu unbeabsichtigtem Verhalten führen, wenn die Maschine mit einer Konfiguration online geschaltet wird, die nicht mit dem aktiven Mastersystem übereinstimmt.

## Präemptive Verarbeitung in VRRP

Die präemptive Verarbeitung sollte in allen VRRP-Schnittstellen stets inaktiviert sein, damit eine neue Einheit oder eine Einheit, deren Betriebssystem erneut geladen wird, nicht versucht, den Cluster zu übernehmen.

## VRRP-Heartbeatintervall

Die Masterschnittstelle oder virtuelle Schnittstelle signalisiert dem LAN-Segment durch das Senden von Heartbeatpaketen (advertisements), dass sie weiterhin aktiv ist, und verwendet dabei die von IANA für das VRRP zugewiesenen Multicast-Adressen (`224.0.0.18` für IPv4 und `FF02:0:0:0:0:0:0:12` für IPv6).

Das Sendeintervall ist standardmäßig auf 1 gesetzt. Dieser Wert kann erhöht werden, sollte jedoch den Wert 5 nicht überschreiten. In einem stark frequentierten Netz kann es deutlich länger als eine Sekunde dauern, bis alle VRRP-Nachrichten vom Mastersystem bei der Backupeinheit eintreffen, d. h. für Netze mit umfangreichem Datenverkehr sollte der Wert 2 angegeben werden.

## VRRP-Synchronisation (sync-group)

Die Synchronisation der Schnittstellen in einer VRRP-Synchronisationsgruppe (sync group) ist wie folgt ausgelegt: Wenn das Backupsystem die Funktion einer Schnittstelle der Gruppe übernimmt, werden auch die Funktionen aller anderen Schnittstellen der Gruppe vom Backupsystem übernommen. Beispiel: Wenn eine Schnittstelle in einem Master-Router ausfällt, wird in den meisten Fällen die Funktionalität des gesamten Routers von einem Backup-Router übernommen.

Dieser Wert stimmt nicht mit der VRRP-Gruppe überein. Er definiert, für welche Schnittstellen einer Einheit die Funktionsübernahme durchgeführt wird, wenn bei einer Schnittstelle in der betreffenden Gruppe ein Fehler festgestellt wird. Es wird empfohlen, alle Schnittstellen derselben Synchronisationsgruppe (sync-group) zuzuweisen. Andernfalls werden manche Schnittstellen als Master eingestuft und mit Gateway-IPs versehen, während andere als Backup eingestuft werden. Dies führt dazu, dass der Datenverkehr nicht mehr korrekt weitergeleitet wird. Aktiv/Aktiv-Konfigurationen werden nicht unterstützt.

## RFC-Kompatibilität für VRRP

Die RFC-Kompatibilität aktiviert bzw. inaktiviert RFC 3768-MAC-Adressen für VRRP in einer Schnittstelle. Diese sollten in nativen Schnittstellen aktiviert und in den konfigurierten virtuellen Schnittstellen inaktiviert sein. Bei manchen virtuellen Switches (insbesondere VMware) treten Probleme auf, wenn diese Adressen aktiviert sind. Dadurch geht Datenverkehr von der Hypervisor-Hostmaschine verloren und wird nicht an die Gateway-IP gesendet. Lassen Sie diese Einstellung unverändert und verzichten Sie auf das Konfigurieren der Einstellung für virtuelle Schnittstellen.

## VRRP-Authentifizierung
Wenn ein Kennwort für die VRRP-Authentifizierung festgelegt ist, muss auch der Authentifizierungstyp definiert werden. Wenn das Kennwort festgelegt ist und kein Authentifizierungstyp definiert wird, gibt das System einen Fehler aus, wenn Sie versuchen, die Konfiguration festzuschreiben.

Umgekehrt kann das VRRP-Kennwort nur gelöscht werden, wenn auch der VRRP-Authentifizierungstyp gelöscht wird. Andernfalls gibt das System einen Fehler aus, wenn Sie versuchen, die Konfiguration festzuschreiben. Wenn Sie sowohl das VRRP-Authentifizierungskennwort als auch den Authentifizierungstyp löschen, ist die VRRP-Authentifizierung inaktiviert.

Die IETF hat entschieden, dass für VRRP Version 3 keine Authentifizierung verwendet werden darf (weitere Informationen finden Sie in RFC 5798 VRRP).

## Unterstützung für Version 2/3
VRA unterstützt Protokolle für die Version 2 (Standardwert) und die Version 3 von VRRP. In Version 2 dürfen IPv4 und IPv6 nicht gleichzeitig verwendet werden. In Version 3 können IPv4 und IPv6 jedoch gleichzeitig verwendet werden.

## VPN für Hochverfügbarkeit mit VRRP
VRA bietet die Möglichkeit, die Konnektivität über einen IPsec-Tunnel mithilfe eines VRA-Paares mit VRRP aufrecht zu erhalten. Wenn ein Router ausfällt oder zu Wartungszwecken abgeschaltet wird, stellt der neue VRRP-Master-Router die IPsec-Konnektivität zwischen den lokalen und fernen Netzen wieder her.

Beim Konfigurieren der Hochverfügbarkeits-VPN mit VRRP müssen Sie nach jedem Hinzufügen einer virtuellen VRRP-Adresse zu einer VRA-Schnittstelle  den IPsec-Dämon reinitialisieren. Dies ist erforderlich, weil der IPsec-Service nur Verbindungen zu den Adressen überwacht, die beim Initialisieren des Internet Key Exchange-Servicedämons in der VRA vorhanden sind.

Führen Sie den folgenden Befehl auf dem Master- und dem Backup-Router aus, damit die IPsec-Dämons beim Aktivieren der Funktionsübernahme auf einer neuen Mastereinheit erneut gestartet werden, nachdem die virtuelle IP übernommen wurde, wie im folgenden Beispiel gezeigt:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Firewalls für Hochverfügbarkeit mit VRRP

Wenn zwei Einheiten ein HA-Paar bilden, ist sorgfältig darauf zu achten, dass auf der Mastereinheit nicht der Zugang für die andere Einheit blockiert wird. Der Port 443 muss zwischen beiden Einheiten offen bleiben, damit die Konfigurationssynchronisation funktionsfähig bleibt, und VRRP muss zum Senden und Empfangen freigegeben sein (einschließlich des Multicastbereichs 224.0.0.0/24).

## Zugehörige VLAN-Teilnetze mit VRRP

Für die VIF-Konfiguration mit VRRP wird eine virtuelle IP-Adresse (die erste verwendbare IP im Teilnetz; die Gateway-IP an die jeglicher Datenverkehr in dem Teilnetz weitergeleitet wird) und eine echte Schnittstellen-IP-Adresse für die VIF auf beiden Einheiten benötigt. Um nicht zu viele verwendbare IP-Adressen zu belegen, sollte für die echten Schnittstellen-IPs ein Out-of-band-Adressbereich wie 192.168.0.0/30 (mit der Adresse .1 für die eine Einheit und der Adresse .2 für die andere Einheit) verwendet werden. Sie können mehrere virtuelle VRRP-IPs verwenden, jedoch wird nur eine echte Adresse auf jeder virtuellen Schnittstelle benötigt.

Hier finden Sie ein Beispiel für eine VRRP-Konfiguration mit zwei privaten VLANs und drei Teilnetzen:

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Zwei private Teilnetze/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - Ein privates Teilnetz/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* Eine VRRP- Synchronisationsgruppe (vrrp sync-group) unterscheidet sich von einer VRRP-Gruppe (vrrp group). Wenn eine Schnittstelle, die zu einer Synchronisationsgruppe gehört, den Status ändert, werden alle anderen Mitglieder dieser Synchronisationsgruppe ebenfalls in denselben Status versetzt.
* Die Nummer der VRRP-Gruppe (vrrp-group) der VLAN-Schnittstellen (VIFs) muss nicht mit der Nummer der nativen Schnittstellen oder der anderen VLANs übereinstimmen. Es wird jedoch dringend empfohlen, alle virtuellen Adressen desselben VLANs in einer VRRP-Gruppe - wie in VLAN 10 dargestellt - beizubehalten.
* Die tatsächlichen Schnittstellenadressen auf den nativen VLANs (z.B. dp0bond1: 169.110.20.28/29) befinden sich nicht immer in demselben Teilnetz wie deren VIPs (169.110.21.26/29).

## Manueller VRRP-Ausweichbetrieb
Falls Sie einen VRRP-Ausweichbetrieb erzwingen müssen, kann dies durch die Ausführung des folgenden Befehls auf der Einheit, die gerade VRRP-Master ist, erreicht werden.

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

Die Gruppen-ID ist die VRRP-Gruppen-ID der nativen Schnittstellen und kann - wie oben beschrieben - in Ihrem Paar anders lauten.

## Verbindungssynchronisation
Wenn zwei VRA-Einheiten ein HA-Paar bilden, kann es hilfreich sein, statusabhängige Verbindungen zwischen beiden Einheiten zu überwachen, damit im Falle einer Funktionsübernahme der aktuelle Status aller auf der ausgefallenen Einheit vorhandenen Verbindungen auf die Backupeinheit repliziert werden kann. Dadurch kann auf die vollständige Neuerstellung der aktiven Sitzungen (z. B. SSL-Verbindungen) verzichtet werden, die zu einer Funktionsunterbrechung für den Benutzer führen könnte.

Dieses Verfahren wird als Synchronisierung der Verbindungsüberwachung bezeichnet.

Um diese Funktion zu konfigurieren, müssen Sie die Methode der Funktionsübernahme deklarieren sowie die Schnittstelle zum Senden der Verbindungsüberwachungsdaten und die IP des fernen Peers wie folgt angeben:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

Die andere VRA verfügt über die gleiche Konfiguration, jedoch über einen anderen fernen Peer (`remote-peer`).

Beachten Sie, dass die Leitung überlastet werden kann, wenn viele Verbindungen zu anderen Schnittstellen führen und in Konkurrenz zu anderen Datenverkehrsströmen in der deklarierten Leitung treten.

## VRRP-Startverzögerungsfunktion
Die Vyatta-Betriebssystemversion 1801p und höher enthält den neuen Befehl `vrrp`.

`vrrp` gibt ein Wahlprotokoll an, das die Verantwortung für einen virtuellen Router dynamisch einem der VRRP-Router in einem LAN zuordnet. Der VRRP-Router, der die IPv4- oder IPv6-Adresse(n) steuert, die einem virtuellen Router zugeordnet sind, wird als Master bezeichnet und leitet Pakete weiter, die an diese IPv4- oder IPv6-Adressen gesendet wurden. Der Wahlprozess stellt eine dynamische Funktionsübernahme in der Weiterleitungsfunktion bereit, falls der Master nicht mehr verfügbar sein sollte. Alle Protokollnachrichten werden entweder mit IPv4- oder IPv6-Multicast-Datagrammen ausgeführt. Das Protokoll kann dadurch über eine Vielzahl von LAN-Technologien mit mehreren Zugriffsmöglichkeiten, die IPv4/IPv6-Multicasting unterstützen, betrieben werden.

Um den Datenaustausch im Netz zu minimieren, sendet nur der Master für jeden virtuellen Router regelmäßige VRRP-Mitteilungen als Nachrichten. Ein Backup-Router versucht nicht, den Master präemptiv zu verarbeiten, es sei denn, er hat eine höhere Priorität. Dadurch wird die Serviceunterbrechung beseitigt, die bisher bis zur Verfügbarkeit eines bevorzugteren Pfads bestand. Es ist auch möglich, alle Vorableerungsversuche administrativ zu untersagen. Falls der Master nicht mehr verfügbar ist, wird der Backup mit der höchsten Priorität nach einer kurzen Verzögerung zum Master und bietet einen kontrollierten Übergang der Verantwortung des virtuellen Routers mit minimaler Serviceunterbrechung. 

**HINWEIS:** Für Bereitstellungen von IBM Cloud ist der Startverzögerungswert auf den Standardwert eingestellt. Sie können diesen Wert nach Ihrem Ermessen ändern, wenn Sie Ihre Funktionsübernahme- und Hochverfügbarkeitsmethoden testen.


### Vorableerung vs. keine Vorableerung

Das Protokoll `vrrp` definiert die Logik, die entscheidet, welcher VRRP-Peer in einem Netz die höhere Priorität hat, und daher der beste Peer ist, um die Rolle als Master auszuführen. Mit einer Standardkonfiguration wird VRRP so aktiviert, dass die Vorableerung ausgeführt wird. Das heißt, dass neue Peers mit höherer Priorität im Netz die Übernahme der Masterrolle erzwingen. 

Wenn die Vorableerung inaktiviert ist, übernimmt ein Peer mit höherer Priorität die Masterrolle nur dann, wenn der vorhandene Peer mit geringerer Priorität nicht mehr im Netz verfügbar ist. Das Inaktivieren der Vorableerung ist manchmal in realen Szenarios sinnvoll. Dies gilt für Situationen mit Peers höherer Priorität, wenn diese möglicherweise damit beginnen, regelmäßig aufgrund von Zuverlässigkeitsproblemen mit dem Peer selbst oder mit einer der Netzverbindungen fehlzuschlagen. Außerdem ist es sinnvoll die vorzeitige Übernahme durch einen neuen Peer mit höherer Priorität zu vermeiden, der die Netzkonvergenz noch nicht abgeschlossen hat. 

### Voraussetzungen und Einschränkung der Vorableerung

Falls die VRRP-Peers so konfiguriert wurden, dass die Vorableerung inaktiviert ist, gibt es einige Fälle, in denen die Vorableerung “scheinbar” doch erfolgt. In Wirklichkeit ist das Szenario jedoch nur eine standardmäßige VRRP-Übernahme. Wie oben beschrieben, verwendet VRRP die IP-Multicast-Datagramme als Mittel zur Bestätigung der Verfügbarkeit des aktuell ausgewählten Master-Routers. Da es sich um ein Protokoll der Ebene 3 handelt, das den Ausfall eines VRRP-Peers erkennt, ist es wichtig, dass die Übernahmedetektion in VRRP verzögert wird, bis VRRP und die Ebene 1 bis 2 des Netzwerkstapels bereit und konvergiert sind. In einigen Fällen kann die Netzschnittstelle, auf der VRRP ausgeführt wird, dem Protokoll bestätigen, dass die Schnittstelle aktiv ist, aber andere zugrundeliegende Services wie Spanning Tree oder Bonding wurden möglicherweise nicht abgeschlossen. Im Ergebnis kann dann die IP-Konnektivität zwischen Peers nicht hergestellt werden. Ist dies der Fall, wird VRRP auf einem neuen höheren Peer zum Master, da keine VRRP-Nachrichten vom aktuellen Master-Peer im Netz erkannt werden können. Nach der Konvergenz wird der kurze Zeitraum, in dem zwei Master-VRRP-Peers ausgeführt werden, dazu führen, dass die Dual-Master-Logik des VRRP ausgeführt wird. Das bedeutet, dass der Peer mit der höheren Priorität Master bleibt und der Peer mit der niedrigeren Priorität zum Backup wird. Dieses Szenario kann “scheinbar” einen Fehler in der Funktion “keine Vorableerung” darstellen. 

### Fehler bei der Startverzögerung

Um die Probleme bei der Startverzögerung in der Konvergenz der unteren Ebenen des Netzstapels während der Schnittstellenaktivierung sowie andere Randerscheinungen zu beseitigen, wurde eine neue Funktion mit dem Namen “Startverzögerung” (Startup Delay) mit der Programmkorrektur 1801p eingeführt. Die Funktion bewirkt, dass der VRRP-Status auf einer Maschine, die “erneut geladen wurde” (reloaded), für eine zuvor definierte Verzögerungsdauer, die vom Netzbediener konfiguriert werden kann, im Status INIT bleibt. Die Flexibilität bei diesem Verzögerungswert ermöglicht es dem Netzbediener, die Merkmale des Netzes und der Geräte unter realen Bedingungen anzupassen.

### Befehlsdetails

```
vrrp {
start-delay <0-600>
}
```

`start-delay` kann Werte zwischen 0 (Standardwert) und 600 Sekunden annehmen. 

### Beispielkonfiguration

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
