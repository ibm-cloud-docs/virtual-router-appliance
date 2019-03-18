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

# Gängige Migrationsprobleme mit Vyatta 5400
{: #vyatta-5400-common-migration-issues}

In der folgenden Tabelle sind gängige Probleme oder Verhaltensänderungen aufgeführt, die nach einer Migration von einem Vyatta 5400-Gerät zu IBM© Virtual Router Appliance auftreten können. In einigen Fällen sind Problemumgehungen angegeben.

## Schnittstellenbasierte Richtlinie mit globalem Status für statusabhängige Firewall

### Probleme
Das Verhalten beim Festlegen des Status der Statusrichtlinie für statusabhängige Firewalls aus Release 5.1 wurde geändert. Wenn Sie in Versionen vor Release 5.1 `state - global -state -policy` für eine statusabhängige Firewall festgelegt haben, fügte der vRouter automatisch eine implizite `Allow`-Regel für die Rückgabekommunikation der Sitzung hinzu.

In Release 5.1 und späteren Versionen müssen Sie eine `Allow`-Regeleinstellung in Virtual Router Appliance hinzufügen. Die statusabhängige Einstellung funktioniert für Schnittstellen auf Vyatta 5400-Geräten und für Protokolle auf VRA-Geräten.

### Problemumgehungen
Falls die Regel `firewall-in` für eine Ingress-Schnittstelle bzw. innere Schnittstelle gilt, muss die Regel `Firewall-out` auf die Egress-Schnittstelle oder äußere Schnittstelle angewendet werden. Andernfalls wird der rücklaufende Datenverkehr an der Egress-Schnittstelle bzw. der äußeren Schnittstelle gelöscht.        

## 'state-enable' in Firewallregeln

### Probleme
Falls `global-state-policy` nicht konfiguriert ist, ist diese Verhaltensänderung nicht betroffen.

Wenn Sie statt `global-state-policy` die Option `state enable` in jeder einzelnen Regel verwenden, ist diese Verhaltensänderung ebenfalls nicht betroffen.

## Zonenbasierte Richtlinie: Handhabung einer lokalen Zone

### Probleme
Es gibt keine Pseudoschnittstelle für eine lokale Zone, die der Zonenrichtlinie hinzugefügt werden kann.

### Problemumgehung
Dieses Verhalten kann simuliert werden, indem Sie eine zonenbasierte Firewall auf physische Schnittstellen anwenden und eine schnittstellenbasierte Firewall auf die Loopback-Schnittstelle. Die Firewall in der Loopback-Schnittstelle filtert den gesamten Datenverkehr an den und vom Router.

Beispiel:

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## Verarbeitungsreihenfolge für Firewalls, NAT, Routing und DNS

### Probleme
In einem Szenario, in dem Masquerade-Source-NAT auf einer IBM Virtual Router Appliance bereitgestellt wird, können Sie die Firewall nicht verwenden, um den Internetzugriff für Hosts festzulegen. Der Grund ist, dass die nachfolgende NAT-Adresse dieselbe ist.

Bei Vyatta 5400-Geräten war diese Operation möglich, da die Firewall vor NAT geschaltet und deshalb die Einschränkung des Internetzugriffs für Hosts realisierbar war.

### Problemumgehungen
Ein neues Routing-Schema ist für die VRA erforderlich:
![routing dns](./images/routing-dns.png)

## Richtlinienbasierte Routing-Tabelle

### Probleme
Das Wort 'Table' in den Konfigurationen ist beim richtlinienbasierten Routing von Vyatta 5400 optional, aber für VRA ist das Feld **Table** obligatorisch, wenn die Aktion `accept` lautet. Wenn die Aktion in der VRA-Konfiguration `drop` lautet, ist das Feld 'Table' optional.

### Problemumgehungen
'Table Main' ist eine verfügbare Option beim richtlinienbasierten Routing von Vyatta 5400. Das funktional entsprechende Element in VRA lautet 'routing-instance default'.

## Richtlinienbasiertes Routing über Tunnelschnittstelle

### Probleme
In der Virtual Router Appliance können Richtlinien für das richtlinienbasierte Routing (PBR) auf Datenebenenschnittstellen für eingehenden Datenverkehr angewendet werden, aber nicht auf Loopback-, Tunnel-, Bridge-, OpenVPN-, VTI- und nicht nummerierte IP-Schnittstellen.

### Problemumgehungen
Für dieses Problem gibt es momentan keine Problemumgehungen.

## TCP-MSS

### Probleme
IBM Virtual Router Appliance verwendet 'nftables' und unterstützt TCP-MSS nicht.

### Problemumgehungen
Für dieses Problem gibt es momentan keine Problemumgehungen.

## OpenVPN

### Probleme
OpenVPN wird nicht gestartet, wenn der Parameter `push-route` in Virtual Router Appliance verwendet wird.

### Problemumgehungen
Verwenden Sie den Parameter `openvpn-option` anstelle von `push-route`.

## GRE/VTI über IPSEC + OSPF

### Probleme
* Wenn für VIF mehrere Teilnetze konfiguriert sind, kann der Datenverkehr diese Teilnetze in der VRA nicht durchqueren.                                             
* InterVLAN-Routing funktioniert in der VRA nicht.

### Problemumgehungen
Verwenden Sie implizite 'Allow'-Regeln, um Datenverkehr über VIF-Schnittstellen hinweg zu akzeptieren.

## IPSec

### Probleme
IPSec (präfixbasiert) funktioniert nicht mit IN-Filter.

### Problemumgehungen
Verwenden Sie IPSec (VTI-basiert).

## IPSEC 'match-none"

### Probleme
Auf Vyatta 5400-Geräten ist die folgende Firewallregel zulässig:

set firewall name allow rule 10 ipsec

Aber auf IBM Virtual Router Appliance gibt es kein IPSec.

### Problemumgehungen
Mögliche alternative Regeln für VRA-Geräte:

```
   match-ipsec  Eingehende IPsec-Pakete
   match-none   Eingehende Nicht-IPsec-Pakete                                                                                                                
```

## IPSEC 'match-ipsec"

### Probleme
Auf Vyatta 5400-Geräten sind die folgenden Firewallregeln zulässig:

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

Aber auf IBM Virtual Router Appliance gibt es kein IPSec.

### Problemumgehungen
Fügen Sie die Protokolle `ESP` und `AH` (IP-Protokolle 50 bzw. 51) hinzu.

Die Regel `action` kann die Werte `accept` oder `drop` haben, siehe unten:

```
set security firewall name <name> rule <regelnummer> action accept
set security firewall name <name> rule <regelnummer> destination port 500
set security firewall name <name> rule <regelnummer> protocol udp
set security firewall name <name> rule <regelnummer> action accept
set security firewall name <name> rule <regelnummer> destination port 4500
set security firewall name <name> rule <regelnummer> protocol udp
set security firewall name <name> rule <regelnummer> action accept
set security firewall name <name> rule <regelnummer> protocol ah
set security firewall name <name> rule <regelnummer> action accept
set security firewall name <name> rule <regelnummer> protocol esp
```

## Site-to-Site IPSEC mit DNAT

### Probleme
IPSec (präfixbasiert) funktioniert nicht mit DNAT.                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1)
===S-S-IPsec=== (12.0.0.1)
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

Das Snippet oben ist ein kurzes Beispiel für die Konfiguration der DNAT-Umsetzung, nachdem ein IPSec-Paket auf einem Vyatta 5400-Gerät verschlüsselt wurde. In dem Beispiel gibt es zwei Vyatta-Geräte, `vyatta1 (11.0.0.1)` und `vyatta2 (12.0.0.1)`. IPsec-Peering ist zwischen `11.0.0.1` und `12.0.0.1` eingerichtet. In diesem Fall verwendet der Client `172.16.1.245` als Ziel, abgeleitet von `10.103.0.1` End-to-End.Das erwartete Verhalten in diesem Szenario ist, dass die Zieladresse `172.16.1.245` im Paketheader in `10.71.68.245` umgesetzt wird.

Ursprünglich führte das Vyatta 5400-Gerät DNAT für den eingehenden IPSec aus, beendete die Schnittstelle und gab den Datenverkehr unter Verwendung der Tabelle zum Nachverfolgen von Verbindungen an den IPsec-Tunnel zurück.

Auf einer Virtual Router Appliance funktioniert die Konfiguration nicht auf diese Weise. Die Sitzung wird erstellt, aber der rücklaufende Datenverkehr umgeht den IPsec-Tunnel, nachdem die Tabelle zum Nachverfolgen von Verbindungen die DNAT-Änderung rückgängig gemacht hat. VRA sendet das Paket dann ohne IPsec-Verschlüsselung. Das vorgeschaltete Gerät erwartet diesen Datenverkehr nicht und wird ihn höchstwahrscheinlich löschen. Die End-to-End-Konnektivität wird unterbrochen, aber dies ist das beabsichtigte Verhalten.   

### Problemumgehungen
Für das oben beschriebene Netzbetriebsszenario hat IBM eine RFE (Request for Enhancement) erstellt. 

Während diese RFE ausgewertet wird, empfehlen wir die folgende Problemumgehung:

**Schnittstellenkonfigurationsbefehle**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN-Konfigurationsbefehle**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT-Konfigurationsbefehle**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**Protokollkonfigurationsbefehle**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR-Konfigurationsbefehle**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### Probleme
PPTP wird in der Virtual Router Appliance nicht länger unterstützt.                                                                                                                                                   

### Problemumgehungen
Verwenden Sie stattdessen das L2TP-Protokoll.

## Skript für den Neustart von IPSec

### Probleme
Immer wenn eine virtuelle VRRP-Adresse zu einer IBM Virtual Router Appliance in einem Hochverfügbarkeits-VPN hinzugefügt wird, müssen Sie den IPsec-Dämon reinitialisieren. Der Grund ist, dass der IPsec-Service nur Verbindungen mit Adressen überwacht, die in VRA vorhanden sind, wenn der IKE-Servicedämon initialisiert wird.

Für ein Paar von VRA-Instanzen mit VRRP verfügt der Standby-Router möglicherweise nicht über die virtuelle VRRP-Adresse, die auf dem Gerät während der Initialisierung vorhanden ist, falls diese Adresse nicht auf dem Master-Router vorhanden ist. Führen Sie deshalb den folgenden Befehl auf den Master- und Backup-Routern aus, um den IPsec-Dämon zu reinitialisieren, wenn eine VRRP-Statusänderung auftritt:

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Aktuelle Anzahl und aktuelle Zeit

### Probleme

Der Zweck der folgenden Regel besteht darin, SSH-Verbindungen auf 3 alle 30 Sekunden für alle SSH-Adressen zu beschränken:

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

In IBM Virtual Router Appliance führt diese Regel zu den folgenden Problemen:

* Die Option für die aktuelle Anzahl ('recent count') und die aktuelle Zeit ('recent time') wird nicht mehr verwendet.

* Die Regel funktioniert folglich nicht wie erwartet und blockiert alle SSH-Verbindungen zur angewandten Schnittstelle.

### Problemumgehungen
Verwenden Sie stattdessen CPP.

## Probleme beim Festlegen der Nachverfolgung von Verbindungen auf dem System

### Probleme

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## Zeitlimitüberschreitung beim Festlegen der Nachverfolgung von Verbindungen auf dem System

### Probleme
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## Zeitbasierte Firewall

### Probleme
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### Probleme
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss' (in 18.x kann 'tcp-mss' nicht mithilfe von PBR festgelegt werden; die einzige Möglichkeit ist das Festlegen in der Schnittstelle direkt, was vermutlich nicht äquivalent ist zu PBR.)
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## Bestimmte Anwendung oder Port in S-S-Ipsec-VPN unterbrochen

### Probleme

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Signifikante Änderung des Protokollierungsverhaltens

### Probleme
Das Protokollierungsverhalten zwischen dem Vyatta 5400-Gerät und der IBM Virtual Router Appliance hat sich signifikant geändert, nämlich von einer sitzungsbasierten Protokollierung in eine paketbasierte Protokollierung.

* Sitzungsprotokollierung: Zeichnet Statusübergänge von statusbehafteten Sitzungen auf.

* Paketprotokollierung: Zeichnet alle Pakete auf, die mit der Regel übereinstimmen. Da die Paketprotokollierung in der Protokolldatei in sogenannten Paketeinheiten aufgezeichnet wird, gibt es einen deutlichen Rückgang des Durchsatzes und der Belastung der Datenträgerkapazität.

* Die Protokollierungsfunktion des vRouters kann zum Erfassen der Firewallaktivität verwendet werden. Wie bei jeder Protokollierungsfunktion sollten Sie dies nur aktivieren, um ein bestimmtes Problem zu beheben, und die Protokollierung anschließend so schnell wie möglich wieder inaktivieren.

Die statusabhängige Firewall, die Firewall-/NAT-Sitzungen verwaltet, schreibt in sogenannten Sitzungseinheiten. Sitzungsprotokollierung wird empfohlen. Unten finden Sie Beispiele für die einzelnen Einstellungen:

**Sitzung / Protokollierung**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Paketprotokollierung - Firewall**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>`
