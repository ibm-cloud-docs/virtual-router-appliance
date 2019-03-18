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

# Ihre Services konfigurieren
{: #configuring-your-services}

Eine Reihe von Services in Virtual Router Appliance (VRA) können mit dem folgenden Befehl konfiguriert werden:

`vyatta@vrouter# set service`

Dies schließt die folgenden Einstellungen ein:

```
 > connsync        Synchronisationsservice für die Verbindungsüberwachung (conn-sync)
 > dhcp-relay      Relay-Agent für DHCP (Dynamic Host Configuration Protocol)
 > dhcp-server     DHCP (Dynamic Host Configuration Protocol) für den DHCP-Server
 > dhcpv6-relay    Parameter für ADHCPv6-Relay-Agent
 > dhcpv6-server   DHCP für IPv6-Server (DHCPv6)
 > dns             Parameter für Domain Name Server (DNS)
 > flow-monitoring Datenflussüberwachung - Konfiguration für die Überwachung des Datenverkehrs
 > https           Web-Server aktivieren/inaktivieren
 > lldp            LLDP-Einstellungen
 > nat             Netzadressumsetzung (Network Address Translation, NAT)
 > netconf         NETCONF (RFC 6241)
 > portmonitor     Konfiguration für Portüberwachung
 > sflow           sflow - Konfiguration für Datenebene
 > snmp            Simple Network Management Protocol (SNMP)
 > ssh             SSH-Protokoll (SSH = Secure Shell)
 > telnet          TELNET-Protokoll (Protokoll für virtuelles Netzterminal) aktivieren/inaktivieren
 > twamp           Two-Way Active Measurement Protocol (Protokoll für bidirektionale aktive Messungen)
```

`nat` wird als Teil des Service konfiguriert und `https` steuert den Zugriff auf die Web-GUI und die APIs. `connsync` definiert, wie zwei VRAs die Synchronisation der Verbindungsüberwachung gemeinsam nutzen können, um zum Beispiel die Funktionsübernahme für statusabhängige Firewalls zu ermöglichen.
