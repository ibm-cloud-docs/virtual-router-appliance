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

Issues resolved in 1801n

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-42505 | N/A | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - security update |
| VRVDR-42427 | N/A | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - security update |
| VRVDR-42383 | N/A | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - security update |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – security update |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – security update |

Security vulnerabilities resolved in 1801n


## 5.2R6S12

Released June 21, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42084 | Blocker | Vfp interface marked as “non-dataplane interface” in “show dataplane route” when nat/ipsec config is re-applied |

Issues resolved in 5.2R6S12

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR-42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – security update |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update (Spectre) |

Security vulnerabilities resolved in 5.2R6S12


## 1801m

Released June 15, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42256 | Critical | No outbound traffic if latest established CHILD_SA gets deleted |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-42018 | Minor | When “restart vpn” is run, an “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” error is thrown |
| VRVDR-42017 | Minor | When “show vpn ipsec sa” is running on VRRP backup, “ConnectionRefusedError” error is thrown related to vyatta-op-vpn- ipsec-vici line 563 |

Issues resolved in 181m

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |

Security vulnerabilities resolved in 1801m


The information contained herein is not an offer, commitment, representation or warranty by AT&T and is subject to change. It is not for use or disclosure outside of AT&T companies except under written agreement.

© 2018 AT&T Intellectual Property. All rights reserved. AT&T and Globe logo are registered trademarks of AT&T Intellectual Property. All other marks are the property of their respective owners.
