---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vorgehensweise zum Anzeigen des Vyatta-Geräts im Portal

Im San Jose-Rechenzentrum wurde ein einzelnes 1-Gb/s-Gateway bereitgestellt. Es hat redundanten Zugriff auf die öffentliche und die private Infrastruktur. Auf der Registerkarte **Geräte** im Menü **Netz > Gateway-Appliances** können Sie die tatsächliche Konfiguration der bereitgestellten Hardware sehen. 

Beachten Sie die Links **2546 öffentliches VLAN**, **2576 privates VLAN** und **Details zur Netzhardware anzeigen**. Die Netze sind dediziert und enthalten nur Gateway-Einheiten. Sie unterstehen der Kontrolle der bereitstellenden Plattform (auf dieser Registerkarte können Sie keine normalen Server bereitstellen). 

Klicken Sie auf das Menü **Netz > Gateway-Appliances** im Webportal, um die Netzzuordnungen anzuzeigen. 

Die Netzswitch-Ports und Ethernet-Adapter auf dem Brocade 5400 vRouter (Vyatta) sind von SoftLayer alle als Trunks konfiguriert. Unsere Zuordnungen aktualisieren einfach die Portkonfiguration, damit die zusätzlichen virtuellen LANs (VLANs) ausgewählt werden können.<sup>1</sup>

Im Dropdown-Menü **VLAN zuordnen** können Sie ein spezifisches VLAN auswählen, das Ihrem Brocade 5400 vRouter-Gerät aus Ihrem Rechenzentrum heraus zugeordnet werden soll. Sie können **kein** VLAN zuordnen, das durch eine Hardware-Firewall oder eine FortiGate-Appliance geschützt wird, da dies die Steuerung des VLAN unterbrechen würde. Wenn im Dropdown-Menü keine zusätzlichen VLANs aufgelistet sind, können Sie weitere bestellen, indem Sie ein Ticket an SoftLayer einreichen. 

In Abbildung 4 können Sie die der Brocade 5400 vRouter-Appliance zugeordneten VLANs unter 'Zugeordnete VLANs' sehen. Der Standardstatus is 'Umgangen' (SoftLayer hat im Hintergrund die einzuschließenden Trunk-Switch-Ports identifiziert: 1894, 2254 vom privaten VLAN und 2007 sowie 1280 vom öffentlichen VLAN).<sup>2</sup>

In der Anzeige **Details** (**Netz > Gateway-Appliances**) im Dropdown-Menü **Zugeordnetes VLAN, Aktionen** gibt es eine Option **VLAN weiterleiten**. Wenn Sie diese Option auswählen, haben Sie effektiv den SoftLayer-FCR und -BCR beauftragt, das Standardgateway zu entfernen und stattdessen eine Route für die Teilnetze mithilfe der Transit-VLANs zum Brocade 5400 vRouter-Gerät hinzuzufügen. 

Die Brocade vRouter-Appliance ist jetzt die einzig gültige Route in und aus dem VLAN. Denken Sie daran, dass dies nicht nur für Ihren Anwendungsdatenverkehr gilt, sondern auch für alle zusätzlichen SoftLayer-Services, die jetzt Ihr Gerät passieren müssen.<sup>4</sup>

Die Konfiguration auf Brocade 5400 vRouter-Seite muss noch abgeschlossen werden, aber das VLAN mit den Teilnetzen ist nicht mehr erreichbar. Dies betrifft auch neue bereitgestellte Server, da das Webportal das VLAN ebenfalls nicht mehr erreichen kann. 

**Hinweise:**

<sup>1</sup>Die VLAN-Zuordnung bezeichnet den Prozess des Verknüpfens eines infrage kommenden VLANs mit einem Netzgateway, damit es künftig an das Gateway weitergeleitet werden kann. Beim Zuordnungsprozess wird ein VLAN nicht automatisch an ein Gateway weitergeleitet. Das VLAN verwendet weiterhin Front-End- und Back-End-Router des Kunden, bis es manuell zum Gateway weitergeleitet wird. VLANs können jeweils nur einem Gateway zugeordnet sein und dürfen nicht über eine Firewall verfügen, um für eine Zuordnung in Frage zu kommen. 

<sup>2</sup>Durch das Umgehen der Gateway-Appliance kann der Datenverkehr jederzeit wieder über die Front-End- und Back-End-Kundenrouter (Front-End Customer Router, FCR und Back-End Customer Router, BCR) geleitet werden. Beim Umgehen eines VLANs bleibt das betreffende VLAN dem Netzgateway zugeordnet. 

<sup>3</sup>Portierbare Teilnetze können erworben und zu primären Gateways hinzugefügt werden. 

<sup>4</sup>Brocade 5400 vRouter-Geräte werden üblicherweise mit einer vordefinierten SoftLayer-Service-IP-Adresse bereitgestellt. 
