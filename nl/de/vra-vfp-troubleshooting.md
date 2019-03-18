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

# Fehlerbehebung bei Ihrer VFP-Schnittstelle
{: #troubleshooting-your-vfp-interface}

Dieser Abschnitt enthält Informationen zur Fehlerbehebung bei VFP-Schnittstellen.

* Eine VFP-Schnittstelle ist keine 'reale' Schnittstelle wie `dp0bond0` (oder auch VIF oder TUN). Sie ist ein Platzhalter für die Firewall- und NAT-Prozesse, um eine Schnittstelle zu blockieren, damit der Datenverkehr ordnungsgemäß verarbeitet werden kann. Sie können den Datenverkehr weiterhin über eine VFP wie über eine reguläre Schnittstelle weiterleiten, wobei aber `tshark` und andere Überwachungsbefehle keinen Datenverkehr sichtbar machen. 
* Bei NAT müssen Sie einen spezifischeren Teilnetzbereich verwenden, um Datenverkehr an VFP weiterzuleiten. Die Kernel-Route, die von IPsec erstellt wurde, ist nicht ausreichend. Falls keine statische Route definiert ist, wird der Kernel-Route gefolgt. Sie können dies mithilfe von `show ip route x.x.x.x` testen.
* DNAT sollte nach der Ausgabe aus VFP ordnungsgemäß verarbeitet werden, aber zurückkehrender Datenverkehr benötigt weiterhin eine festgelegte, statische Route. Suchen Sie nach der Kopfzeile für Nicht-NAT-Datenverkehr in der Ausgabe der IPsec-Schnittstelle, `dp0bond1` oder `dp0bond0` (oder eine beliebige andere Schnittstelle, die IPsec-Datenverkehr verwendet).
* Die Verwendung von Routing-Protokollen über VFP wurde nicht getestet. 
* Die Verwendung eines GRE-Tunnels über VFP wurde ebenfalls nicht getestet.
