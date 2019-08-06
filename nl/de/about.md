---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informationen zur VRA
{: #about-the-vra}

Die {{site.data.keyword.vra_full}} (VRA) stellt das neueste Vyatta 5600-Betriebssystem für **x86**-Bare-Metal-Server zur Verfügung. Es wird als Hochverfügbarkeits- (High Availability, HA) oder als Standalone-Konfiguration angeboten.

Mit der {{site.data.keyword.vra_full}} können Sie Datenverkehr im privaten und öffentlichen Netz selektiv über einen voll ausgestatteten Unternehmensrouter weiterleiten, der über Firewall, Regulierung des Datenverkehrs (Traffic-Shaping), richtlinienbasiertes Routing, VPN und weitere Funktionsmerkmale verfügt. Die VRA bietet hohe Leistung mit einfach auszuführender Konfiguration. Sie weist die Wartungsvorteile der Ausführung auf einem normalen Hardware-Server auf. Die VRA-Hardware-Appliance ist auf die Routing-Arbeitslast für mehrere VLANs ausgelegt und kann auf Bestellung mit redundanten Netzverbindungen und redundanten RAID-Arrays ausgestattet werden. Alle VRA-Funktionen werden von Ihnen, dem Kunden, verwaltet. 

**Alternativen:** Die FortiGate Security Appliance (FSA) 10Gbps ist eine dedizierte (Single-Tenant) Hardware-Firewall mit hohem Durchsatz (10 Gb/s) und Funktionen der nächsten Generation wie Virenschutz (AntiVirus, AV), Abwehr unbefugter Zugriffe (Intrusion Prevention, IPS) und Webfilterung. FSA kann als Alternative zu VRA verwendet werden, um ähnliche Ziel zu erreichen. Weitere Informationen finden Sie in der [FSA-Dokumentation](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps).

## Firewall
Um Ihre Umgebung gegen Bedrohungen von außen zu schützen, kann {{site.data.keyword.vra_full}} als Firewall genutzt werden. Sie können Firewallregeln hinzufügen, um ankommenden bzw. abgehenden Netzverkehr über die Ports, die von Ihrer Anwendung genutzt werden, zuzulassen oder abzuschirmen. Außerdem können Sie den Netzverkehr innerhalb der eigenen Netze filtern. Darüber hinaus kann {{site.data.keyword.vra_full}} für die statusabhängige IPv4- und IPv6-Filterung konfiguriert werden, um Ihre unternehmenskritischen Daten zu schützen.

## Gateway für Virtual Private Network (VPN)
Verbinden Sie Ihr Rechenzentrum oder Ihre Niederlassung vor Ort mit der IBM Cloud durch VPN-Tunnelung, indem Sie Ihre Instanz von {{site.data.keyword.vra_full}} als Netzgatewayeinheit bereitstellen. Sie können einen IPsec-Site-zu-Site-VPN-Tunnel für die sichere Kommunikation von Ihrem Unternehmensrechenzentrum oder Ihrer Niederlassung zu Ihrem IBM Cloud-Netz verwenden. Weitere VPN-Optionen sind IPsec-VPN (Client-zu-Site) über Fernzugriff, OpenVPN, GRE, L2TP und DMVPN.

Weitere Informationen finden Sie in den Konfigurationshandbüchern (Configuration Guides) zu Brocade VPN im Abschnitt [Ergänzende Dokumentation zu VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).

## Netzadressumsetzung (Network Address Translation, NAT)
Mit {{site.data.keyword.vra_full}} können Sie Anwendungs- und Datenbankserver ohne öffentliche Netzschnittstellen bereitstellen und zugleich durch die Quellen-NAT den Internetzugang für Ihre Server ermöglichen. Außerdem können Sie Ihre Server mithilfe der Ziel-NAT hinter dem Gateway-Gerät verbergen und damit die Sicherheit erhöhen.

## Auf Unternehmen abgestimmtes Routing

Für mehrschichtige Anwendungen in verschiedenen isolierten Netzen bietet {{site.data.keyword.vra_full}} flexible Möglichkeiten bei der Gestaltung der Konnektivität zwischen diesen Netzen. Sie können mithilfe von BGP dynamisches Routing einrichten und haben auf diese Weise die Möglichkeit, den IBM Cloud-Routern mitzuteilen, dass Sie über einen eigenen öffentlichen IP-Bereich verfügen. Ferner bietet BGP mehr Flexibilität bei angepassten privaten Netzkonfigurationen, wenn verschiedene Tunnel und direkte Verbindungslösungen verwendet werden.

Weitere Informationen finden Sie in den Konfigurationshandbüchern (Configuration Guides) zu Brocade BGP im Abschnitt [Ergänzende Dokumentation zu VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).
