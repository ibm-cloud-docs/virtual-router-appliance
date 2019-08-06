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

# Configurando seus serviços
{: #configuring-your-services}

Há uma variedade de serviços em execução no {{site.data.keyword.vra_full}} (VRA) que podem ser configurados, incluindo:

`vyatta@vrouter# set service`

Conclusões possíveis incluem:

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

`nat` é configurado como parte do serviço e `https` controla o acesso à GUI da web e às APIs. `connsync` define como dois VRAs podem compartilhar a sincronização de rastreamento de conexão para coisas como failover de firewall stateful.
