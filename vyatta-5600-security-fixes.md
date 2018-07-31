# AT&T Vyatta 5600 vRouter Software Patches

**As of: July 9, 2018**

This document lists the patches for the currently supported versions of the Vyatta Network OS 5600. With versions 5.2 and older, patches are named using an S number. With versions 17.1 and newer, patches are named with a lower case letter, excluding “i”, “o”, “l”, and “x”.

When multiple CVE numbers are addressed in a single update, the highest CVSS score is listed.

## 18001n

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42588 | Minor | Sensitive routing protocol configuration inadvertently leaked in system log |
| VRVDR-42566 | Critical | After upgrading from 17.2.0h to 1801m, a day later multiple reboots occurred on both HA members |
| VRVDR-42490 | Major | VTI-IPSEC IKE SAs fail around a minute after VRRP transition |
| VRVDR-42335 | Major | IPSEC: remote-id “hostname” behavior changes from 5400 to 5600 |
| VRVDR-42264 | Critical | No connectivity over SIT tunnel – “kernel: sit: non-ECT from 0.0.0.0 with TOS=0xd” |
| VRVDR-41957 | Minor | Bi-directional NAT’ed packets too large for GRE fail to return ICMP Type 3 Code 4 |
| VRVDR-40283 | Major | Configuration changes generate lots of log messages |
| VRVDR-39773 | Major | Using a route-map with BGP vrrp-failover command can cause all prefixes to be withdrawn |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-42505 | N/A | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - security update |
| VRVDR-42427 | N/A | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - security update |
| VRVDR-42383 | N/A | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - security update |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – security update |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – security update |

## 5.2R6S12

Released June 21, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42084 | Blocker | Vfp interface marked as “non-dataplane interface” in “show dataplane route” when nat/ipsec config is re-applied |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR-42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – security update |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update (Spectre) |

## 1801m

Released June 15, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42256 | Critical | No outbound traffic if latest established CHILD_SA gets deleted |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-42018 | Minor | When “restart vpn” is run, an “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” error is thrown |
| VRVDR-42017 | Minor | When “show vpn ipsec sa” is running on VRRP backup, “ConnectionRefusedError” error is thrown related to vyatta-op-vpn- ipsec-vici line 563 |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |

## 5.2R6S11

Released June 11, 2018

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42109 | Critical | Only 1 ICMP reply packet received with SNAT+FW on 5.2R6S7 |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-42027 | Major | SFLOW using incorrect input ifIndex |
| VRVDR-41558 | Major | The reported timestamps in packet traces are not consistent with the actual time and system clock |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – security update |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639: speculative execution, variant 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – security update |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – security update |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – security update |

## 1801k

Released June 8, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-41944 | Major | After VRRP fail-over some VTI tunnels fail to re-establish until a “vpn restart” or peer reset is issued |
| VRVDR-41906 | Major | PMTU discovery fails as ICMP type 3 scode 4 messages are sent out from wrong source IP |
| VRVDR-41558 | Major | The reported timestamps in packet traces are not consistent with the actual time and system clock |
| VRVDR-41469 | Major | One interface link down – bond is not carrying traffic |
| VRVDR-41420 | Major | LACP bonding state/link “u/D” with mode change active-backup to LACP |
| VRVDR-41313 | Critical | IPsec – VTI interface instability |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – security update |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639: Speculative execution, variant 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – security update |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – security update |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – security update |

## 18001j

Released May 18, 2018

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41481 | Minor | VRRP on bond interface does not send VRRP advertisement |
| VRVDR-39863 | Major | VRRP fails over when customer removes routing-instance with GRE associated and tunnel local-address is part of VRRP |
| VRVDR-27018 | Critical | Running configuration file is globally readable |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update |

## 5.2R6S10

Released May 17, 2018

**Issues Resolved**
| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41543 | Major | “update config-sync” is reporting errors when “\” backslash is used in configuration descriptions
| VRVDR-27018 | Critical | Running configuration file is globally readable |

## 1801h

Released May 11, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41664 | Critical | Dataplane drops MTU sized ESP packets |
| VRVDR-41536 | Minor | Dnsmasq service start-init limit hit when adding more than 4 static host entries if dns forwarding is enabled |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7.8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux security update |

## 5.2R6S

Released May 8, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40803 | Minor | VIF interfaces are not present in “show vrrp” output after a reboot |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |

## 1801g

Released May 4, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41620 | Major | vTI interface traffic stops sending traffic after new vIF is added |
| VRVDR-40965 | Major | Bonding does not recover after a dataplane crash |

## 1810f

Released April 23, 2018

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41537 | Minor | Ping is not working over IPsec tunnel on 1801d |
| VRVDR-41283 | Minor | Configd stops processing static routes during boot if the configuration has disabled static routes |
| VRVDR-41266 | Major | Static route leaking to VRF does not transit traffic across mGRE tunnel after reboot |
| VRVDR-41255 | Major | When slave goes down it takes over 60s for master link state to reflect that |
| VRVDR-41252 | Major | With unbound VTI in zone-policy, drop rule is bypassed depending on commit order of zone rules |
| VRVDR-41221 | Critical | Upgrading vRouters from 1801b to 1801c to 1801d with 10% failure rate |
| VRVDR-40967 | Major | Disabling IPv6 forwarding prevents routing of VTI sourced IPv4 packets |
| VRVDR-40858 | Major | VTI interface showing MTU 1428 causing TCP PMTU issues |
| VRVDR-40857 | Critical | Vhost-bridge does not come up for tagged VLAN with interface names of a certain length |
| VRVDR-40803 | Minor | VIF interfaces are not present in “show vrrp” output after a reboot |
| VRVDR-40644 | Major | IKEv1: QUICK_MODE re-transmits are not handled correctly |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update || VRVDR- 41331 | 6.5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – security update| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update || VRVDR- 41215 | 6.1 |CVE-2018-1059 | CVE-2018-1059 – DPDK vhost out of bound host memory access from VM guests |

##5.2R6S8
Released April 16, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41283 | Minor |Configd stops processing static routes during boot if the configuration has disabled static routes |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update

##1801e
Released March 28, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39985 | Minor | TCP DF packets larger than GRE tunnel MTU are dropped with no ICMP fragmentation needed returned | | VRVDR-41088 | Critical | Extended (4 byte) ASN not represented internally as unsigned type || VRVDR-40988 | Critical | Vhost not starting when used with certain number of interfaces || VRVDR-40927 | Critical | DNAT: SDP in SIP 200 OK not translated when it follows a 183 response || VRVDR-40920 | Major | With 127.0.0.1 as listen-address snmpd does not start |
| VRVDR-40920 | Critical | ARP doesn’t work over bonded SR-IOV interface || VRVDR-40294 | Major | Dataplane doesn’t restore previous queues after slave is removed from bonding group |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/A | DSA-4140-1 | DSA 4140-1: libvorbis security update |

##5.2R6S7
Released March 15, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-38801 | Major | Multi-segment packet received via IPsec VTI causes bond interface to go down

##5.2R6S6
Released March 12, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40281 | Major | After upgrading from 5.2 to more recent version error “vbash: show: command not found” in operation mode |
| VRVDR-40135 | Major | Spanning tree packets are not received on a VIF interface bridge port || VRVDR-39991 | Major | Stateful firewall drops packets between two subnets on the same interface | | VRVDR-36481 | Major | Upgrade/downgrade from 5.2R4 to 17.1.0/5.2R3 shows /opt/vyatta/sbin/vyatta-install-image.functions: line 372: is_onie_boot: command not found |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8.8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – security update || VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5717 / Spectre, aka. Variant #2 |

##1801d
Released March 8, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40940 | Major | Dataplane crash related to NAT/firewall || VRVDR-40886 | Major | Combining “icmp name <value>” with a number of other configuration for the rule will cause firewall to not load || VRVDR-39879 | Major | Configuring bonding for jumbo frames fails |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – security upate | | VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5715 / Spectre, aka variant #2 |

##1801c
Released March 7, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40281 | Major | After upgrading from 5.2 to more recent version error “-vbash: show: command not found” in operation mode |

##1801b
Released February 21, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40622 | Major | Cloud-init images fail to detect correctly if IP address has been obtained from DHCP server || VRVDR-40613 | Critical | Bond interface does not come up if one of the physical links are down || VRVDR-40328 | Major | Cloud-init images take a long time to boot |

##1801a
Released February 7, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40324 | Major | Load averages exceed 1.0 with no load on router with bonding interface |

##5.2R6S5
Released January 19, 2018.

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5.6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – security update (Meltdown) || VRVDR- 38265 | 8.8 | DSA-3970-1 | CVE-2017-1 |

##5.2R6S4
Released December 15, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39529 | Major | DHCP server failover is not synchronizing databases |
| VRVDR-39399 | Critical | Vyatta dropped off network state FAULT in show vrrp/multiple interfaces flap/seg fault || VRVDR-39112 | Major | DNAT traffic that matches ZBF drops only first packet in flow || VRVDR-38075 | Minor | When “restart vpn” is issued from responder, initiator does not re- establish connection || VRVDR-37934 | Critical | BGPd crashed when aggregate-address summary-only is configured/static routes missing |
| VRVDR-37717 | Minor | Rename hard-enf “Description” and “License” fields in version output || VRVDR-37689 | Major | High rate of NIC PF interrupts || VRVDR-37633 | Critical | Keepalived hanging |

## 5.2R6S3
Released December 4, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39207 | Critical | ARP failing on bond VIF interface |


##5.2R6S2
Released November 2, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39177 | Major | OpenVPN server domain-name option not being applied with –push dhcp-option || VRVDR-39129 | Critical | OpenVPN server push-route parameter causes OpenVPN to fail to start |

##5.2R6S1
Released October 12, 2017.

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9.8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- security update |

The information contained herein is not an offer, commitment, representation or warranty by AT&T and is subject to change. It is not for use or disclosure outside of AT&T companies except under written agreement.

© 2018 AT&T Intellectual Property. All rights reserved. AT&T and Globe logo are registered trademarks of AT&T Intellectual Property. All other marks are the property of their respective owners.
