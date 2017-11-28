---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Configurer des services
Il existe toute une variété de services qui s'exécutent sur Virtual Router Appliance (VRA) et peuvent être configurés, notamment :

`vyatta@vrouter# set service`

Les possibilités d'achèvement sont les suivantes :

```
 > connsync        Connection tracking synchronization (conn-sync) service
 > dhcp-relay      Dynamic Host Configuration Protocol (DHCP) relay agent
 > dhcp-server     Dynamic Host Configuration Protocol (DHCP) for DHCP server
 > dhcpv6-relay    DHCPv6 Relay Agent parameters
 > dhcpv6-server   DHCP for IPv6 (DHCPv6) server
 > dns             Domain Name Server (DNS) parameters
 > flow-monitoring Flow-Monitoring traffic monitoring configuration
 > https           Enable/disable the Web server
 > lldp            LLDP settings
 > nat             Network Address Translation (NAT)
 > netconf         NETCONF (RFC 6241)
 > portmonitor     Portmonitor configuration
 > sflow           sflow configuration for dataplane
 > snmp            Simple Network Management Protocol (SNMP)
 > ssh             Secure Shell (SSH) protocol
 > telnet          Enable/disable Network Virtual Terminal Protocol (TELNET) protocol
 > twamp           Two-Way Active Measurement Protocol
```

Manifestement, `nat` est configuré comme une composante du service et `https` contrôle l'accès à l'interface graphique Web et aux API. `connsync` définit comment deux unités VRA peuvent partager la synchronisation du suivi des connexions pour des actions telles que le basculement d'un pare-feu avec état.
