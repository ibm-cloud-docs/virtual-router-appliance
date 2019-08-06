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

# 서비스 구성
{: #configuring-your-services}

다음을 포함하여 VRA({{site.data.keyword.vra_full}})에서 실행되는 구성 가능한 여러 서비스가 있습니다.

`vyatta@vrouter# set service`

가능한 완료는 다음과 같습니다.

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

`nat`는 서비스의 일부로 구성되고 `https`는 웹 GUI 및 API에 대한 액세스를 제어합니다. `connsync`는 두 VRA가 Stateful 방화벽 장애 복구와 같은 연결 추적 동기화를 공유하는 방법을 정의합니다.
