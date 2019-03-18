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

# Ihre VLANs weiterleiten
{: #routing-your-vlans}

Virtual Router Appliance bietet die Möglichkeit, mehrere VLANs über dieselbe Netzschnittstelle (z. B. `dp0bond0` oder `dp0bond1`) zu leiten. Um dies zu erreichen, müssen der Switch-Port in den Trunking-Modus versetzt und die virtuellen Schnittstellen (Virtual Interfaces, VIFs) in der Einheit konfiguriert werden.

Beispiel:

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Die oben angegebenen Befehle erstellen zwei virtuelle Schnittstellen in der Schnittstelle `dp0bond0`. Die Schnittstelle `dp0bond0.1432` verarbeitet den Datenverkehr für das VLAN 1432 und die Schnittstelle `dp0bond0.1693` verarbeitet den Datenverkehr für das VLAN 1693.
