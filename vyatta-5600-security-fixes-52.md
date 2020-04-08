---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2020-04-01"

keywords: 5600, security, fixes, patches, brocade, os

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# AT&T Vyatta 5600 vRouter software patches (Archived)
{: #at-t-vyatta-5600-vrouter-software-patches-52}

**Patch received: January 17, 2020**

**Published in documentation: January 27, 2020**

This document lists the patches for previously supported versions of the Vyatta Network OS 5600. With versions 5.2 and older, patches are named using an S number.
{: shortdesc}

When multiple CVE numbers are addressed in a single update, the highest CVSS score is listed.

For current patch information for the Vyatta 5600 OS newer than 17.2, refer to [this topic](/docsvirtual-router-appliance?topic=virtual-router-appliance-at-t-vyatta-5600-vrouter-software-patches).
{: note}

## 5.2R6S12
{: #5-2R6S12}

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

## 5.2R6S11
{: #5-2R6S11}

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

## 5.2R6S10
{: #5-2R6S10}

Released May 17, 2018

**Issues Resolved**
| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41543 | Major | “update config-sync” is reporting errors when “\” backslash is used in configuration descriptions
| VRVDR-27018 | Critical | Running configuration file is globally readable |

## 5.2R6S
{: #5-2R6S}

Released May 8, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40803 | Minor | VIF interfaces are not present in “show vrrp” output after a reboot |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |

## 5.2R6S8
{: #5-2R6S8}

Released April 16, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41283 | Minor |Configd stops processing static routes during boot if the configuration has disabled static routes |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update

## 5.2R6S7
{: #5-2R6S7}

Released March 15, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-38801 | Major | Multi-segment packet received via IPsec VTI causes bond interface to go down

## 5.2R6S6
{: #5-2R6S6}

Released March 12, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40281 | Major | After upgrading from 5.2 to more recent version error “vbash: show: command not found” in operation mode |
| VRVDR-40135 | Major | Spanning tree packets are not received on a VIF interface bridge port |
| VRVDR-39991 | Major | Stateful firewall drops packets between two subnets on the same interface |
| VRVDR-36481 | Major | Upgrade/downgrade from 5.2R4 to 17.1.0/5.2R3 shows /opt/vyatta/sbin/vyatta-install-image.functions: line 372: is_onie_boot: command not found |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8.8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – security update |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5717 / Spectre, aka. Variant #2 |

## 5.2R6S5
{: #5-2R6S5}

Released January 19, 2018.

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5.6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – security update (Meltdown) |
| VRVDR- 38265 | 8.8 | DSA-3970-1 | CVE-2017-1 |

## 5.2R6S4
{; #5-2R6S4}

Released December 15, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39529 | Major | DHCP server failover is not synchronizing databases |
| VRVDR-39399 | Critical | Vyatta dropped off network state FAULT in show vrrp/multiple interfaces flap/seg fault |
| VRVDR-39112 | Major | DNAT traffic that matches ZBF drops only first packet in flow |
| VRVDR-38075 | Minor | When “restart vpn” is issued from responder, initiator does not re- establish connection |
| VRVDR-37934 | Critical | BGPd crashed when aggregate-address summary-only is configured/static routes missing |
| VRVDR-37717 | Minor | Rename hard-enf “Description” and “License” fields in version output |
| VRVDR-37689 | Major | High rate of NIC PF interrupts |
| VRVDR-37633 | Critical | Keepalive hanging |

## 5.2R6S3
{: #5-2R6S3}

Released December 4, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39207 | Critical | ARP failing on bond VIF interface |


## 5.2R6S2
{: #5-2R6S2}

Released November 2, 2017.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39177 | Major | OpenVPN server domain-name option not being applied with –push dhcp-option |
| VRVDR-39129 | Critical | OpenVPN server push-route parameter causes OpenVPN to fail to start |

## 5.2R6S1
{: #5.2R6S1}

Released October 12, 2017.

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9.8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- security update |

The information contained herein is not an offer, commitment, representation or warranty by AT&T and is subject to change. It is not for use or disclosure outside of AT&T companies except under written agreement.

© 2018 AT&T Intellectual Property. All rights reserved. AT&T and Globe logo are registered trademarks of AT&T Intellectual Property. All other marks are the property of their respective owners.
