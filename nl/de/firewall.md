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

# Firewalls verwalten
Virtual Router Appliance (VRA) kann Firewallregeln verarbeiten, um die über die Einheit gesteuerten VLANs zu schützen. Für die Firewalls in der VRA können die beiden folgenden Schritte unterschieden werden:

1. Einen oder mehrere Regelsätze definieren
2. Einen Regelsatz auf eine Schnittstelle oder Zone anwenden (eine Zone besteht aus einer oder mehreren Netzschnittstellen)

Erstellte Firewallregeln sollten unbedingt getestet werden, um sicherzustellen, dass sie die erwartete Wirkung haben und nicht den Verwaltungszugriff auf die Einheit einschränken.

Es wird empfohlen, beim Bearbeiten von Regeln in der Schnittstelle `dp0bond1` die Verbindung zu der Einheit mit `dp0bond0` herzustellen. Die Verbindung zur Konsole kann auch über IPMI (Intelligent Platform Management Interface) hergestellt werden.

## Statusunabhängig oder statusabhängig
Die Firewall ist standardmäßig vom Status unabhängig, aber Sie kann bei Bedarf auch statusabhängig konfiguriert werden. Eine statusunabhängige Firewall erfordert Regeln für den Datenverkehr in beide Richtungen. Statusabhängige Firewalls dagegen verfolgen Verbindungen und ermöglichen automatisch den Rückfluss des akzeptierten Datenverkehrs. Zum Konfigurieren einer statusabhängigen Firewall müssen Sie festlegen, welche Regeln statusabhängig befolgt werden sollen.

Führen Sie die folgenden Befehle aus, um eine statusabhängige Verfolgung von `TCP`-, `UPD`- oder `ICMP`-Datenverkehr zu aktivieren.

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Beachten Sie, dass die Befehle `global-state-policy` nur den Status des Verkehrs verfolgen, der mit einer Firewallregel übereinstimmt, die explizit das entsprechende Protokoll definiert. Beispiel:

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

Da `GLOBAL_STATELESS` nicht `protocol tcp` definiert, gilt der Befehl `global-state-policy tcp` nicht für diese Regel. 

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

In diesem Fall wird `protocol tcp` explizit definiert. Der Befehl `global-state-policy tcp` aktiviert die statusabhängige Verfolgung von Datenverkehr, der mit Regel 1 von `GLOBAL_STATEFUL_TCP` übereinstimmt.


Führen Sie folgende Befehle aus, um einzelne Firewallregeln als statusabhängig zu definieren:

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
Mit diesen Befehlen wird die statusabhängige Verfolgung des gesamten Datenverkehrs aktiviert, der statusabhängig verfolgt werden kann und mit Regel 1 von `TEST` übereinstimmt. Die Verfolgung geschieht unabhängig davon, ob der Befehl `global-state-policy` vorhanden ist. 

## ALG für unterstützte statusabhängige Verfolgung
Einige wenige Protokolle wie zum Beispiel FTP verwenden komplexere Sitzungen, die bei normalem statusabhängigen Firewallbetrieb verfolgt werden können. 
Es gibt vorkonfigurierte Module, die diese Protokolle aktivieren, so dass sie statusabhängig verwaltet werden können.
Es wird empfohlen, diese ALG-Module zu inaktivieren, solange sie nicht für die erfolgreiche Verwendung der jeweiligen Protokolle erforderlich sind.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Firewallregelsätze
Firewallregeln werden zu benannten Regelsätzen gruppiert, um das Anwenden von Regeln auf mehrere Schnittstellen zu erleichtern. Jedem Regelsatz wird eine Standardaktion zugeordnet. Ein Beispiel: 
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

Im Regelsatz `ALLOW_LEGACY` sind zwei Regeln definiert. Die erste Regel löscht jeden Datenverkehr, der von einer Adressgruppe namens `network-group1` stammt. Die zweite Regel löscht und protokolliert jeden Datenverkehr, der für den Telnet-Port (`tcp/23`) bestimmt ist und von der Adressgruppe namens `network-group2` stammt. Die Standardaktion gibt an, dass alle übrigen Datenpakete akzeptiert werden.

## Zugriff auf Rechenzentrum zulassen
IBM stellt mehrere IP-Teilnetze zur Verfügung, die Services und Support für Systeme bereitstellen, die innerhalb des Rechenzentrums ausgeführt werden. Beispiel: Services für DNS-Auflösung werden unter `10.0.80.11` und `10.0.80.12` ausgeführt. Für Bereitstellung und Support werden andere Teilnetze verwendet. Die in den Rechenzentren verwendeten IP-Bereiche finden Sie [hier](https://console.bluemix.net/docs/infrastructure/hardware-firewall-dedicated/ips.html).

Sie können den Zugriff auf das Rechenzentrum zulassen, indem Sie am Anfang des Firewallregelsatzes entsprechende Regeln für `SERVICE-ALLOW` mit einer Aktion `accept` einfügen. Wo der Regelsatz angewendet werden muss, hängt vom implementierten Routing- und Firewalldesign ab.

Es wird empfohlen, die Firewallregeln an der Speicherposition zu platzieren, die am wenigsten doppelten Arbeitsaufwand verursacht. Beispiel: Das Zulassen von Back-End-Teilnetzen für ankommende Daten über `dp0bond0` verursacht weniger Arbeitsaufwand als das Zulassen von Back-End-Teilnetzen für abgehende Daten an jede virtuelle VLAN-Schnittstelle.

### Firewallregeln für jede Schnittstelle
Eine Methode zum Konfigurieren der Firewall in einer VRA besteht darin, Firewallregelsätze auf jede Schnittstelle anzuwenden. In diesem Fall kann eine Schnittstelle entweder eine Datenebenenschnittstelle (`dp0s0`) oder eine virtuelle Schnittstelle (`dp0bond0.303`) sein. Für jede Schnittstelle sind drei Firewallzuweisungen möglich:

`in` - Die Firewall wird auf eingehende Pakete überprüft, die über die Schnittstelle ankommen. Diese Pakete können durchgeleitet werden oder für die VRA bestimmt sein.

`out` - Die Firewall wird auf abgehende Pakete überprüft, die über die Schnittstelle geleitet werden. Diese Pakete können durchgeleitet werden oder von der VRA stammen.

`local` - Die Firewall wird auf Pakete überprüft, die direkt für die VRA bestimmt sind.

Eine Schnittstelle kann über mehrere Regelsätze verfügen, die in jeder Richtung angewendet werden. Sie werden in der Reihenfolge der Konfiguration angewendet. Beachten Sie, dass dies nicht für Firewalldatenverkehr gilt, der von der VRA-Einheit mit Firewalls für einzelne Schnittstellen stammt.

Beispiel: Um den Regelsatz `ALLOW_LEGACY` der Option `in` für die Schnittstelle `bp0s1` zuzuweisen, wird der folgende Konfigurationsbefehl verwendet:  

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Steuerebenenvorgabe (Control Plane Policing, CPP)
Die Steuerebenenvorgabe (Control Plane Policing, CPP) bietet Schutz vor Angriffen auf die Virtual Router Appliance, indem Firewallrichtlinien definiert werden, die den gewünschten Schnittstellen zugewiesen und auf ankommende Pakete in der VRA angewendet werden.

CPP wird implementiert, wenn das Schlüsselwort `local` in Firewallrichtlinien verwendet wird, die einem beliebigen VRA-Schnittstellentyp zugewiesen sind (z. B. Datenebenen- oder Loopback-Schnittstellen). Im Unterschied zu Firewallregeln für durch die VRA durchgeleitete Datenpakete verwenden Firewallregeln für ankommenden oder abgehenden Datenverkehr auf der Steuerebene die Standardaktion `Allow`. Wenn dieses Standardverhalten nicht erwünscht ist, muss der Benutzer explizite Löschregeln hinzufügen.

Die VRA stellt einen CPP-Basisregelsatz als Vorlage bereit. Führen Sie den folgenden Befehl aus, um diese Vorlage mit Ihrer Konfiguration zusammenzuführen: 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

Nachdem der Regelsatz mit der Konfiguration zusammengeführt wurde, wird ein neuer Firewallregelsatz mit dem Namen `CPP` hinzugefügt und auf die Loopback-Schnittstelle angewendet. Es wird empfohlen, diesen Regelsatz an Ihre Umgebung anzupassen.

Bitte beachten Sie, dass CPP-Regeln nicht statusabhängig sein können und nur für eingehenden Datenverkehr gelten.

## Zonenbasierte Firewalls
Ein anderes Firewallkonzept in Virtual Router Appliance sind zonenbasierte Firewalls. Dabei wird eine Schnittstelle einer Zone zugewiesen (nur eine Zone pro Schnittstelle) und Firewallregelsätze werden auf die Grenzen zwischen den Zonen angewendet, um für alle Schnittstellen in einer Zone dieselbe Sicherheitsebene festzulegen und die freie Weiterleitung zu ermöglichen. Der Datenverkehr wird nur beim Übergang von einer Zone in eine andere überprüft. In jeder Zone wird der ankommende Datenverkehr gelöscht, wenn er nicht explizit zugelassen wurde.

Eine Schnittstelle kann entweder einer Zone angehören oder eine schnittstellenbasierte Firewallkonfiguration verwenden, jedoch nicht beides.

Als Beispiel dient das folgende Geschäftsszenario mit drei Abteilungen, die jeweils über ein eigenes VLAN verfügen: 

* Abteilung A - VLAN 10 und 20 (Schnittstelle dp0bond1.10 und dp0bond1.20)
* Abteilung B - VLAN 30 und 40 (Schnittstelle dp0bond1.30 und dp0bond1.40)
* Abteilung C - VLAN 50 (Schnittstelle dp0bond1.50)

Für jede Abteilung kann eine Zone erstellt werden und die zugehörigen Schnittstellen können zu der Zone hinzugefügt werden. Das folgende Beispiel veranschaulicht dieses Szenario:
```
set security zone-policy zone ABTEILUNGA interface dp0bond1.10
set security zone-policy zone ABTEILUNGA interface dp0bond1.20  set security zone-policy zone ABTEILUNGB interface dp0bond1.30  set security zone-policy zone ABTEILUNGB interface dp0bond1.40  set security zone-policy zone ABTEILUNGC interface dp0bond1.50
```

Der Befehl `commit` belegt jede Zone als eine Schnittstelle und die Standardlöschregeln löschen jeden Datenverkehr, der die Zone von außen erreicht. Im vorliegenden Beispiel können VLAN 10 und VLAN 20 Datenpakete übermitteln, da sie zur selben Zone (`ABTEILUNGA`) gehören, während VLAN 10 und VLAN 30 keine Datenpakete übermitteln können, da VLAN 30 zu einer anderen Zone (`ABTEILUNGB`) gehört.

Die Schnittstellen in jeder Zone können Datenverkehr ungehindert weiterleiten und es können Regeln für die Interaktion zwischen den Zonen definiert werden. Ein Regelsatz für den Übergang von Daten aus einer Zone in eine andere Zone wird konfiguriert. 

Der folgende Befehl enthält ein Beispiel für das Konfigurieren einer Regel:

`set security zone-policy zone ABTEILUNGC to ABTEILUNGB firewall ALLOW_PING `

Dieser Befehl weist dem Übergang von ABTEILUNGC zu ABTEILUNGB den Regelsatz namens `ALLOW_PING` zu. In der Zone ABTEILUNGB ankommender Datenverkehr aus der Zone ABTEILUNGC wird mit diesem Regelsatz abgeglichen.

Dabei ist zu beachten, dass diese Zuweisung der Datenweiterleitung aus der Zone ABTEILUNGC in die Zone ABTEILUNGB keine Festlegung für die umgekehrte Richtung beinhaltet. Wenn keine Regeln vorliegen, die den Datenverkehr aus der Zone ABTEILUNGB in die Zone ABTEILUNGC zulassen, fließt kein Datenverkehr (ICMP-Antworten) zurück zu den Hosts in ABTEILUNGC.

`ALLOW_PING` wird als eine ausgehende Firewall (`out`) für Schnittstellen der Zone ABTEILUNG (dp0bond1.30 und dp0bond1.40) verwendet. Da diese über die Zonenrichtlinie installiert wird, wird nur der Verkehr mithilfe des Regelsatzes überprüft, der von den Schnittstellen der Quellenzone (dp0bond1.50) stammt.

## Sitzungs- und Paketprotokollierung
VRA unterstützt zwei Protokollierungstypen:

1. Sitzungsprotokollierung.  Verwenden Sie den Befehl ``security firewall session-log``, um die Protokollierung für Firewallsitzungen zu konfigurieren.
  
	Für UDP, ICMP und alle Nicht-TCP-Datenflüsse durchläuft eine Sitzung während der Lebensdauer des Datenflusses vier Statusübergänge. Sie können konfigurieren, dass die VRA für jeden Übergang eine Nachricht protokolliert. TCP verfügt über zahlreiche Statusübergänge, für die jeweils die Protokollierung konfiguriert werden kann.  

2. Paketprotokollierung. Geben Sie das Schlüsselwort ``log`` in der Firewall- oder NAT-Regel an, um jedes Netzdatenpaket zu protokollieren, das mit der Regel übereinstimmt.

	Die Paketprotokollierung findet auf den Paketweiterleitungspfaden statt und erzeugt umfangreiche Ausgabedaten. Dies kann den Durchsatz der VRA stark beeinträchtigen und die Plattenspeicherbelegung durch Protokolldateien drastisch erhöhen. Es wird empfohlen, die Paketprotokollierung NUR für Fehlerbehebungszwecke zu verwenden. Für alle operativen Zwecke sollte die statusabhängige Sitzungsprotokollierung verwendet werden.
