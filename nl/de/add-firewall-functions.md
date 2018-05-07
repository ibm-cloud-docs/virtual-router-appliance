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

# Firewallfunktionen zu Virtual Router Appliance hinzufügen (statusunabhängig und statusabhängig)
Eine Methode für das Einrichten von Firewalls bei der Verwendung von Virtual Router Appliance (VRA) ist das Anwenden von Firewallregelsätzen auf jede Schnittstelle. Jede Schnittstelle weist drei mögliche Firewallinstanzen auf - 'In', 'Out' und 'Local' - und für jede Instanz stehen Regeln zur Verfügung, die auf sie angewendet werden können. 

Die Standardaktion lautet 'Drop'. Dabei ermöglichen Regeln, die in einer Reihenfolge von Regel 1 bis N angewendet werden, einen bestimmten Datenverkehr. Sobald eine Übereinstimmung erzielt wird, wendet die Firewall die festgelegte Aktion für die übereinstimmende Regel an.

Unter den unten aufgelisteten Firewallinstanzen kann nur jeweils eine angewendet werden.

**IN:** Die Firewall filtert Pakete, die über die Schnittstelle ankommen und durch das System durchgeleitet werden. Sie müssen bestimmte IP-Bereiche zulassen, um für Verwaltungszwecke (Ping, Überwachung usw.) Zugriff auf Front-End und Back-End bereitzustellen.

**OUT:** Die Firewall filtert Pakete, die über die Schnittstelle abgehen. Diese Pakete können durch das VRA-System durchgeleitet werden oder vom System stammen.

**LOCAL:** Die Firewall filtert Pakete, die für das VRA-System selbst bestimmt sind und über die Systemschnittstelle ankommen. Sie sollten Einschränkungen für Zugriffsports erstellen, die von externen, nicht eingeschränkten IP-Adressen bei der Einheit ankommen.

Führen Sie die folgenden Schritte aus, um ein Beispiel für eine Firewallregel festzulegen, die ICMP (Internet Control Message Protocol) (ping - IPv4 Echo-Antwortnachricht) für Ihre Virtual Router Appliance-Schnittstellen ausschaltet (Dies ist eine statusunabhängige Aktion. Eine statusabhängige Aktion wird später betrachtet.):

1. Geben Sie in der Eingabeaufforderung `show configuration commands` ein, um anzuzeigen, welche Konfigurationen definiert wurden. Sie sehen eine Liste aller Befehle, die Sie für Ihre Einheit definiert haben. (Das kann z.B. ein Handy sein, wenn Sie eine Migration durchführen möchten und alle Konfigurationen anzeigen wollen. Beachten Sie den Befehl `set firewall all-ping enable`, der angibt, dass ICMP immer noch für Ihre Einheit aktiviert ist. 

2. Geben Sie `configure` ein.

3. Geben Sie `set firwall all-ping disable` ein.

4. Geben Sie `commit` ein.

Wenn Sie jetzt versuchen, Ihre VRA-Einheit mit Ping zu überprüfen, erhalten Sie keine Antwort mehr. 

Um weitergeleitetem Datenverkehr Firewallregeln zuzuordnen, müssen die Regeln auf die VRA-Schnittstellen angewendet werden oder es müssen Zonen erstellt und die Regeln müssen auf diese Zonen angewendet werden. 

Zonen werden für die VLANs erstellt, die bisher verwendet worden sind. 

 VLAN | Zone 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (für die Produktion)
bond0.2254 | private (für die Entwicklung)
bond1.1280 | reserviert für künftige Verwendung
bond1.1894 | reserviert für künftige Verwendung

## Firewallregeln erstellen
Es ist empfehlenswert, die Firewallregeln zu erstellen, die auf die Zonen angewendet werden sollen, bevor die Zonen tatsächlich erstellt werden. Wenn Sie zuerst die Regeln und dann die Zonen erstellen, können Sie diese sofort anwenden. Wenn Sie zuerst die Zone und dann die Regel erstellen, müssen Sie zur Zone zurückkehren, um die Regel anzuwenden. 

Die folgenden Befehle dienen folgenden Zwecken: 

* Erstellen einer Firewallregel mit dem Namen `dmz2private` mit der Standardaktion zum Freigeben eines beliebigen Pakets. 
* Zulassen der Protokollierung von akzeptiertem und verweigertem Datenverkehr für die Regel mit dem Namen `dmz2private`.

1. Geben Sie `configure` in der Eingabeaufforderung ein. 

2. Geben Sie folgende Befehle ein: 

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. Geben Sie die nächste Gruppe von Befehlen ein, so dass IP-Tabellen (iptables) eine Rückgabe des Datenverkehrs für eingerichtete Sitzungen zulassen. Standardmäßig lassen IP-Tabellen (iptables) dies nicht zu. Daher ist eine explizite Regel erforderlich. 

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. Geben Sie folgende Befehle ein, um für TCP/UDP den Zugang zu Port 22 zu ermöglichen, dem Standard für SSH.
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. Geben Sie die nächsten Befehle ein, um für TCP/UDP den Zugang zu Port 80 zu ermöglichen, dem Standard für HTTP.

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. Geben Sie `commit` ein, um sicherzustellen, dass alle Regeln nach der Fertigstellung verwendet werden. 

7. Zeigen Sie die neue Konfiguration an, indem Sie `show firewall name dmz2private` in der Eingabeaufforderung eingeben. 

8. Erstellen Sie eine Firewallregel, damit diese auf die dmz-Zone angewendet wird, indem Sie die folgenden Befehle in der Eingabeaufforderung eingeben. Die Regel wird 'public' genannt.  

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## Erstellen Sie Zonen

Zonen sind logische Darstellungen einer Schnittstelle. In diesem Abschnitt werden Sie folgendes tun: 

* Eine Zone erstellen und eine Richtlinie mit dem Namen 'dmz' mit einer Standardaktion zum Ablehnen von Paketen, die für diese Zone bestimmt sind, erstellen.
* Für die Richtlinie 'dmz' die Verwendung der Schnittstelle `bond1` festlegen.
* Für die Richtlinie 'prod' die Verwendung der Schnittstelle `bond1.2007` festlegen. 
* Eine Zonenrichtlinie mit dem Namen `private` mit einer Standardaktion zum Ablehnen von Paketen, die für diese Zone bestimmt sind, erstellen. 
* Für eine Richtlinie mit dem Namen `private` die Verwendung der Schnittstelle `bond0.2254` festlegen. 

1. Geben Sie die folgenden Befehle in der Eingabeaufforderung ein: 

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. Verwenden Sie die folgenden Befehle zum Festlegen der Firewallrichtlinie für die Zonen: 

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
Sie können eine Firewallregel für eine bestimmte Schnittstelle anwenden, wenn Sie sie nicht auf eine Zonenrichtlinie anwenden möchten. Geben Sie die folgenden Befehle ein, um eine Regel auf eine Schnittstelle anzuwenden. 

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
