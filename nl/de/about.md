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

# Produktinfo
IBM Virtual Router Appliance (VRA) ermöglicht die selektive Weiterleitung von privatem und öffentlichem Netzdatenverkehr über einen voll ausgestatteten Unternehmensrouter mit Firewall, Regulierung des Datenverkehrs, richtlinienbasierter Weiterleitung, VPN und vielen weiteren Funktionen. Virtual Router Appliance bietet Leistung, einfache Konfiguration und die Wartungsvorteile der Ausführung auf einem traditionellen Server. Die Dimensionierung der Hardware ist auf die Routing-Arbeitslast für mehrere VLANs abgestimmt und kann auf Bestellung mit redundanten Netzverbindungen und redundanten RAID-Arrays ausgestattet werden. Alle VRA-Funktionen werden vom Kunden verwaltet. 

IBM Virtual Router Appliance verfügt über das neueste Vyatta 5600-Betriebssystem für x86-Bare-Metal-Server und wird mit Hochverfügbarkeit (High Availability, HA) oder als Standalone-Version angeboten.

**HINWEIS:** FortiGate Security Appliance (FSA) 10 Gb/s ist eine dedizierte (Single-Tenant) Hardware-Firewall mit hohem Durchsatz (10 Gb/s) und Funktionen der nächsten Generation wie Virenschutz (AntiVirus, AV), Abwehr unbefugter Zugriffe (Intrusion Prevention, IPS) und Webfilterung. FSA kann als Alternative zu VRA verwendet werden, um ähnliche Ziel zu erreichen. Weitere Informationen finden Sie in der [FSA-Dokumentation](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Firewall
Um Ihre Umgebung gegen Bedrohungen von außen zu schützen, kann Virtual Router Appliance als Firewall genutzt werden. Sie können Firewallregeln hinzufügen, um ankommenden bzw. abgehenden Netzverkehr über die Ports, die von Ihrer Anwendung genutzt werden, zuzulassen oder abzuschirmen. Außerdem können Sie den Netzverkehr innerhalb der eigenen Netze filtern. Darüber hinaus kann Virtual Router Appliance für die statusabhängige IPv4- und IPv6-Filterung konfiguriert werden, um Ihre unternehmenskritischen Daten zu schützen.

## Gateway für Virtual Private Network (VPN)
Verbinden Sie Ihr Rechenzentrum oder Ihre Niederlassung vor Ort mit der IBM Cloud durch VPN-Tunnelung, indem Sie Ihre Instanz von Virtual Router Appliance als Netzgatewayeinheit bereitstellen. Sie können einen IPsec-Site-zu-Site-VPN-Tunnel für die sichere Kommunikation von Ihrem Unternehmensrechenzentrum oder Ihrer Niederlassung zu Ihrem IBM Cloud-Netz verwenden. Weitere VPN-Optionen sind IPsec-VPN (Client-zu-Site) über Fernzugriff, OpenVPN, GRE, L2TP und DMVPN.

Weitere Informationen finden Sie in den Konfigurationshandbüchern (Configuration Guides) zu Brocade VPN im Abschnitt [Ergänzende Dokumentation zu VRA](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)

## Netzadressumsetzung (Network Address Translation, NAT)
Mit Virtual Router Appliance können Sie Anwendungs- und Datenbankserver ohne öffentliche Netzschnittstellen bereitstellen und zugleich durch die Quellen-NAT den Internetzugang für Ihre Server ermöglichen. Außerdem können Sie Ihre Server mithilfe der Ziel-NAT hinter dem Gateway-Gerät verbergen und damit die Sicherheit erhöhen.

## Auf Unternehmen abgestimmtes Routing

Für mehrschichtige Anwendungen in verschiedenen isolierten Netzen bietet Virtual Router Appliance größere Flexibilität bei der Gestaltung der Konnektivität zwischen diesen Netzen. Sie können mithilfe von BGP dynamisches Routing einrichten und haben auf diese Weise die Möglichkeit, den IBM Cloud-Routern mitzuteilen, dass Sie über einen eigenen öffentlichen IP-Bereich verfügen. Ferner bietet BGP mehr Flexibilität bei angepassten privaten Netzkonfigurationen, wenn verschiedene Tunnel und direkte Verbindungslösungen verwendet werden.

Weitere Informationen finden Sie in den Konfigurationshandbüchern (Configuration Guides) zu Brocade BGP im Abschnitt [Ergänzende Dokumentation zu VRA](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)
