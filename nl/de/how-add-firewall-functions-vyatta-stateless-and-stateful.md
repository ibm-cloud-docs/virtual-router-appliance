---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vorgehensweise beim Hinzufügen von Firewallfunktionen zu Vyatta (statusunabhängig und statusabhängig)

Firewallregelsätze auf jede Schnittstelle anzuwenden, ist eine Methode zum Aufbau einer Firewall bei Brocade 5400 vRouter-Geräten (Vyatta). Jede Schnittstelle weist drei mögliche Firewallinstanzen auf - 'In', 'Out' und 'Local' - und für jede Instanz stehen Regeln zur Verfügung, die auf sie angewendet werden können. Die Standardaktion lautet 'Drop'. Dabei ermöglichen Regeln das Anwenden von bestimmtem Datenverkehr in einer Reihenfolge von Regel 1 bis N. Sobald eine Übereinstimmung erzielt wird, wendet die Firewall die festgelegte Aktion für die übereinstimmende Regel an.

Unter den unten aufgelisteten Firewallinstanzen kann jeweils **nur eine** angewendet werden.

* **IN:** Die Firewall filtert Pakete, die über die Schnittstelle ankommen und durch das Brocade-System durchgeleitet werden. Sie müssen bestimmte SoftLayer-IP-Bereiche zulassen, um für Verwaltungszwecke (Ping, Überwachung usw.) Zugriff auf Front-End und Back-End bereitzustellen.
* **OUT:** Die Firewall filtert Pakete, die über die Schnittstelle abgehen. Diese Pakete können durch das Brocade-System durchgeleitet werden oder vom System stammen.
* **LOCAL:** Die Firewall filtert Pakete, die für das Brokade vRouter-System selbst bestimmt sind und über die Systemschnittstelle ankommen. Sie sollten Einschränkungen für Zugriffsports erstellen, die von externen, nicht eingeschränkten IP-Adressen beim Brocade vRouter ankommen.<sup>1</sup>

Führen Sie die folgenden Schritte aus, um ein Beispiel für eine Firewallregel festzulegen, die ICMP (Internet Control Message Protocol) *(ping - IPv4 Echo-Antwortnachricht)* für Ihre Brocade 5400 vRouter-Schnittstellen ausschaltet (Dies ist eine statusunabhängige Aktion. Eine statusabhängige Aktion wird später betrachtet.):

1\. Geben Sie in der Eingabeaufforderung *show configuration* ein, um anzuzeigen, welche Konfigurationen definiert wurden. Sie sehen eine Liste aller Befehle, die Sie für Ihre Einheit definiert haben. (Das kann z.B. ein Handy sein, wenn Sie eine Migration durchführen möchten und alle Konfigurationen anzeigen wollen. Beachten Sie den Befehl *set firewall all-ping 'enable'*, der angibt, dass ICMP immer noch für Ihre Einheit aktiviert ist.

2\. Geben Sie *configure* ein.

3\. Geben Sie *set firwall all-ping 'disable'* ein.

4\. Geben Sie *commit* ein.

Wenn Sie jetzt versuchen, Ihren Brocade 5400 vRouter mit Ping zu überprüfen, erhalten Sie keine Antwort mehr.

Um weitergeleitetem Datenverkehr Firewallregeln zuzuordnen, müssen die Regeln auf die Brocade 5400 vRouter-**Schnittstellen** angewendet werden oder Sie müssen **Zonen erstellen**, auf die die Regeln angewendet werden.

Zonen werden für die VLANs erstellt, die bisher verwendet worden sind.

**VLAN = Zone**

bond1 = dmz

bond1.2007 = prod (für die Produktion)

bond0.2254 = private (für die Entwicklung)

bond1.1280 = reserviert für künftige Verwendung

bond1.1894 = reserviert für künftige Verwendung

**Firewallregeln erstellen**

Es ist empfehlenswert, die Firewallregel zu erstellen, die auf die Zonen angewendet werden sollen, bevor die Zonen tatsächlich erstellt werden. Wenn Sie zuerst die Regeln und dann die Zonen erstellen, können Sie diese sofort anwenden. Wenn Sie zuerst die Zone und dann die Regeln erstellen, müssen Sie zur Zone zurückkehren, um die Regeln anzuwenden.<sup>2</sup> 

Die folgenden Befehle dienen diesen Zwecken:

* Erstellen einer Firewallregel mit dem Namen **dmz2private** mit der Standardaktion zum Ablehnen aller Pakete. 
* Zulassen der Protokollierung von akzeptiertem und verweigertem Datenverkehr für die Regel mit dem Namen **dmz2private**. 


1\. Geben Sie *configure* in der Eingabeaufforderung ein.

2\. Geben Sie in der Eingabeaufforderung folgende Befehle ein:

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

Die nächste Gruppe von Befehlen dient dazu, **iptables** zu aktivieren, um die den Rückfluss von Datenverkehr für eine eingerichtete Sitzung zuzulassen. Standardmäßig lassen **IP-Tabellen** dies nicht zu. Daher ist eine explizite Regel erforderlich.

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

Die dritte Gruppe von Befehlen ermöglicht TCP/UDP den Zugang zu Port 22, dem Standard für SSH.

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

Die letzte Gruppe von Befehlen ermöglicht TCP/UDP den Zugang zu Port 80, dem Standard für HTTP.

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. Geben Sie *commit* ein, um sicherzustellen, dass alle Regeln nach der Fertigstellung verwendet werden.

4\. Zeigen Sie Ihre Konfiguration an, indem Sie *show firewall name dmz2private* in der Eingabeaufforderung eingeben.

Die nächste Firewallregel, die wir erstellen, wird auf Ihre **dmz**-Zone angewendet. Die Regel wird den Namen **public** tragen. Geben Sie in der Eingabeaufforderung folgende Befehle ein:

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**Erstellen Sie Zonen**

Zonen sind logische Darstellungen einer Schnittstelle. Die folgenden Befehle dienen folgenden Zwecken:

* Erstellen Sie eine Zone und eine Richtlinie mit dem Namen **dmz** mit einer Standardaktion zum Ablehnen von Paketen, die für diese Zone bestimmt sind.
* Legen Sie für die Richtlinie **dmz** die Verwendung der Schnittstelle **bond1** fest. 
* Legen Sie für die Richtlinie **prod** die Verwendung der Schnittstelle **bond1.2007** fest. 
* Erstellen Sie eine Zonenrichtlinie mit dem Namen **private** mit einer Standardaktion zum Ablehnen von Paketen, die für diese Zone bestimmt sind. 
* Legen Sie für eine Richtlinie mit dem Namen **private** die Verwendung der Schnittstelle **bond0.2254** fest. 

1\. Geben Sie die folgenden Befehle in der Eingabeaufforderung ein:

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. Verwenden Sie die folgenden Befehle zum Festlegen der Firewallrichtlinie für die Zonen:

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

Denken Sie daran, dass Sie eine Firewallregel für eine bestimmte Schnittstelle anwenden können, wenn Sie sie nicht auf eine Zonenrichtlinie anwenden möchten. Geben Sie die folgenden Befehle ein, um eine Regel auf eine Schnittstelle anzuwenden.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

**Hinweise:**

<sup>1</sup>Eine *statusabhängige* Firewall behält die Tabelle zuvor angezeigter Datenflüsse bei und Pakete können entsprechend ihrer Beziehung zu vorherigen Paketen akzeptiert oder abgelehnt werden. Als allgemeine Regel kann man sagen, statusabhängige Firewalls werden generell bevorzugt, wobei der Anwendungsdatenverkehr vorherrschend ist. 

<span style="text-decoration: underline">*Der Brocade 5400 vRouter verfolgt den Status der Verbindungen mit Standardkonfiguration nicht. Die Firewall ist so lange statusunabhängig, bis eine der folgenden Bedingungen eintritt:*</span>

* Konfiguration einer Firewall und eine beliebige Regel weist einen Statusparameter s auf
* Konfiguration einer Statusrichtlinie für die Firewall
* Konfiguration von NAT
* Aktivierung des transparenten Web-Proxy-Service
* Aktivierung einer WAN-Lastausgleichskonfiguration

Eine *statusunabhängige* Firewall betrachtet jedes Paket isoliert. Pakete können allein aufgrund der ACL-Basiskriterien (Access Control List) wie Quellen- oder Zielfelder im IP- oder TCP/UDP-Header (Transmission Control Protocols/User Datagram Protocol) akzeptiert oder abgelehnt werden. Ein statusunabhängiger Brocade 5400 vRouter speichert keine Verbindungsinformationen und muss nicht die Beziehung jedes Pakets zum vorherigen Datenfluss untersuchen. Beides verbraucht eine geringe Menge an Speicher und CPU-Zeit. Eine unaufbereitete Weiterleitungsleistung ist daher bei einem statusunabhängigen System am besten. Brocade empfiehlt den Router statusunabhängig zu belassen, um die beste Leistung zu erzielen. Der Router kann statusunabhängig bleiben, solange Sie nicht spezifische statusabhängige Funktionen benötigen.

<sup>2</sup>Zonen und Regelsätze verfügen über eine Standardaktionsanweisung. Bei der Verwendung von Zonenrichtlinien wird die Standardaktion durch die Zonenrichtlinienanweisung definiert und durch die Regel 10,000 dargestellt. Es ist außerdem wichtig, dass Sie daran denken, dass die Standardaktion einer Firewallregel darin besteht, den gesamten Datenverkehr zu abzulehnen. ****

<sup>3</sup>Firewallregeln müssen nach außen über **prod** an **dmz** abgehen. Verwenden Sie die folgenden Befehle, um die Fehlersuche im Netzfluss zu unterstützen: *sudo tcpdump -i any host 10.52.69.202*. 
