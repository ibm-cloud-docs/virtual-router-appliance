---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: configure, service, NAT

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# Configuración de los servicios
{: #configuring-your-services}

Hay variedad de servicios que se ejecutan en {{site.data.keyword.vra_full}} (VRA) que se pueden configurar, incluidos:

`vyatta@vrouter# set service`

Entre las finalizaciones posibles se encuentran:

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

`nat` se configura como parte del servicio y `https` controla el acceso a web GUI y a las API. `connsync` define cómo dos VRA pueden compartir la sincronización de seguimiento en situaciones como la migración del cortafuegos con estado, por ejemplo.
