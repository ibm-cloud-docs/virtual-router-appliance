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

# VLANs verwalten
In der [Anzeige 'Gateway-Appliance-Details'](access-gateway-details.html) können Sie zahlreiche verschiedene Aktionen ausführen.

## Ein VLAN einer Gateway-Appliance zuordnen

Ein VLAN muss einer Gateway-Appliance zugeordnet werden, bevor es weitergeleitet werden kann. Die VLAN-Zuordnung bezeichnet das Verknüpfen eines infrage kommenden VLANs mit einem Netzgateway, damit es künftig an eine Gateway-Appliance weitergeleitet werden kann. Beim Zuordnungsprozess wird ein VLAN nicht automatisch an eine Gateway-Appliance weitergeleitet. Das VLAN verwendet weiterhin Front-End- und Back-End-Router des Kunden, bis es zum Gateway weitergeleitet wird. 

VLANs können jeweils nur einem Gateway zugeordnet sein und dürfen nicht über eine Firewall verfügen. Führen Sie die folgende Prozedur aus, um ein VLAN einem Netzgateway zuzuordnen.

1. [Rufen Sie die Anzeige 'Gateway-Appliance-Details' im Kundenportal auf](access-gateway-details.html). 
2. Wählen Sie das gewünschte VLAN in der Dropdown-Liste **VLAN zuordnen** aus.
3. Klicken Sie auf die Schaltfläche **Zuordnen**, um das VLAN zuzuordnen.

Nachdem ein VLAN der Gateway-Appliance zugeordnet wurde, wird es im Abschnitt 'Zugehörige VLANs' der Anzeige 'Gateway-Appliance-Details' angezeigt. In diesem Abschnitt kann das VLAN an das Gateway weitergeleitet oder die Zuordnung zu dem Gateway aufgehoben werden. Sie können jederzeit weitere infrage kommende VLANs einer Gateway-Appliance zuordnen, indem Sie die oben angegebenen Schritte wiederholen.

## Zugehöriges VLAN weiterleiten

Die zugehörigen VLANs sind mit einer Gateway-Appliance verknüpft, aber der ankommende und abgehende Datenverkehr für das VLAN erreicht das Gateway erst, nachdem das VLAN weitergeleitet wurde. Nach dem Weiterleiten eines zugehörigen VLANs wird der gesamte Front-End- und Back-End-Datenverkehr über die Gateway-Appliance weitergeleitet anstatt über die Router des Kunden. 

Gehen Sie wie folgt vor, um ein zugehöriges VLAN weiterzuleiten:

1. [Rufen Sie die Anzeige 'Gateway-Appliance-Details' im Kundenportal auf](access-gateway-details.html). 
2. Lokalisieren Sie das gewünschte VLAN im Bereich 'Zugehörige VLANs'.
3. Wählen Sie im Dropdown-Menü 'Aktionen' die Option **VLAN weiterleiten** aus.
4. Klicken Sie auf **Ja**, um das VLAN weiterzuleiten. 

Nach dem Routing eines VLANs wird der gesamte zugehörige Front-End- und Back-End-Datenverkehr von den Kundenroutern auf das Netzgateway verlagert. Weitere Steuerungsmaßnahmen für den Datenverkehr und die Gateway-Appliance können über das Management-Tool des Gateways durchgeführt werden. Das Routing über das Netzgateway kann durch [Umgehen der Gateway-Appliance](#bypass-gateway-appliance-routing-for-a-vlan) jederzeit beendet werden.

## Gateway-Appliance-Routing für ein VLAN umgehen

Nach dem Weiterleiten eines VLANs wird der gesamte zugehörige Front-End- und Back-End-Datenverkehr über das Netzgateway geleitet. Durch das Umgehen der Gateway-Appliance kann der Datenverkehr jederzeit wieder über die Front-End- und Back-End-Kundenrouter (Front-End Customer Router, FCR und Back-End Customer Router, BCR) geleitet werden. 

Beim Umgehen eines VLANs bleibt das betreffende VLAN dem Netzgateway zugeordnet. Wenn das VLAN nicht länger der Gateway-Appliance zugeordnet bleiben soll, finden Sie weitere Informationen in [Zuordnung eines VLANs zu einer Gateway-Appliance aufheben](#disassociate-a-vlan-from-a-gateway-appliance). 

Gehen Sie wie folgt vor, um das Gateway-Routing für ein VLAN zu umgehen:

1. [Rufen Sie die Anzeige 'Gateway-Appliance-Details' im Kundenportal auf](access-gateway-details.html). 
2. Lokalisieren Sie das gewünschte VLAN im Bereich 'Zugehörige VLANs'.
3. Wählen Sie im Dropdown-Menü 'Aktionen' die Option **VLAN umgehen** aus.
4. Klicken Sie auf **Ja**, um das Gateway zu umgehen. 

Nach dem Umgehen des Netzgateways wird der gesamte zugehörige Front-End- und Back-End-Datenverkehr über den zugehörigen FCR und BCR des VLANs geleitet. Das VLAN bleibt der Gateway-Appliance zugeordnet und kann jederzeit wieder zur Gateway-Appliance umgeleitet werden.

## Zuordnung eines VLANs zu einer Gateway-Appliance aufheben

VLANs können durch [Zuordnen](#associate-a-vlan-to-a-gateway-appliance) jeweils mit einer Gateway-Appliance verknüpft werden. Durch das Zuordnen kann das jeweilige VLAN jederzeit zur Gateway-Appliance umgeleitet werden. Wenn ein VLAN einer anderen Gateway-Appliance zugeordnet werden oder nicht länger dem aktuellen Gateway zugeordnet bleiben soll, muss die bestehende Zuordnung aufgehoben werden. Beim Aufheben der Zuordnung wird die "Verknüpfung" zwischen dem VLAN und der Gateway-Appliance entfernt. Anschließend kann die Zuordnung zu einem anderen Gateway eingerichtet werden, falls erforderlich. 

Gehen Sie wie folgt vor, um die Zuordnung eines VLANs zu einer Gateway-Appliance aufzuheben:

1. [Rufen Sie die Anzeige 'Gateway-Appliance-Details' im Kundenportal auf](access-gateway-details.html). 
2. Lokalisieren Sie das gewünschte VLAN im Bereich 'Zugehörige VLANs'.
3. Wählen Sie im Dropdown-Menü **Aktionen** die Option **Zuordnung aufheben** aus. 
4. Klicken Sie auf **Ja**, um die Zuordnung des VLANs aufzuheben. 

Nachdem die Zuordnung eines VLANs zu einer Gateway-Appliance aufgehoben wurde, kann das VLAN einem anderen Gateway zugeordnet werden. Das VLAN kann jederzeit erneut der Gateway-Appliance zugeordnet werden. Nachdem die Zuordnung eines VLANs zu einer Gateway-Appliance aufgehoben wurde, kann der Datenverkehr des VLANs nicht mehr über das Gateway geleitet werden. VLANs können erst weitergeleitet werden, wenn eine Zuordnung zu einer Gateway-Appliance besteht.

## Mehrere VLANs über dieselbe Netzschnittstelle leiten
Virtual Router Appliance bietet die Möglichkeit, mehrere VLANs über dieselbe Netzschnittstelle (z. B. `dp0bond0` oder `dp0bond1`) zu leiten. Um dies zu erreichen, müssen der Switch-Port in den Trunking-Modus versetzt und die virtuellen Schnittstellen (Virtual Interfaces, VIFs) in der Einheit konfiguriert werden.

Beispiel: 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Die oben angegebenen Befehle erstellen zwei virtuelle Schnittstellen in der Schnittstelle `dp0bond0`. Die Schnittstelle `dp0bond0.1432` verarbeitet den Datenverkehr für das VLAN 1432 und die Schnittstelle `dp0bond0.1693` verarbeitet den Datenverkehr für das VLAN 1693.
