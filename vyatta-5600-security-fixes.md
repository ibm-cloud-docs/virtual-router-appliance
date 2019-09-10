---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-4-17"

keywords: 5600, security, fixes, patches, brocade, os

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# AT&T Vyatta 5600 vRouter Software Patches
{: #at-t-vyatta-5600-vrouter-software-patches}

**As of: September 5, 2019**

This document lists the patches for the currently supported versions of the Vyatta Network OS 5600. With versions 5.2 and older, patches are named using an S number. With versions 17.1 and newer, patches are named with a lower case letter, excluding “i”, “o”, “l”, and “x”.

When multiple CVE numbers are addressed in a single update, the highest CVSS score is listed.

## 1801zb
{: #1801zb}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-47924 | Major | BGP 'show' output for default-vrf not captured in 'show tech-support' |
| VRVDR-47869 | Minor | L2TP/IPsec with x.509 authentication fails due to incorrect path to certificates |
| VRVDR-47711 | Minor | changing 'syslog global facility all level' overwrites individual 'facility <> level' settings |
| VRVDR-47710 | Major | nhrp overloads IPSec daemon communication |
| VRVDR-47661 | Minor | L2TP in high availability pair will not allow connections after VRRP failover |
| VRVDR-47606 | Major | Configuring "service https listen-address" bypasses the TLSv1.2 enforcement |
| VRVDR-47543 | Blocker | Long Login Delay due to pam_systemd failed to create session |
| VRVDR-47506 | Minor | ntpq segfault in ld-2.24.so |
| VRVDR-47485 | Major | VRRP snmp MIB stops working when any configuration changes made to SNMP |
| VRVDR-47381 | Major | When a vrrp vif is disabled the next change may prevent the interface from being displayed in 'show interfaces' |
| VRVDR-47229 | Blocker | netplugd crash on configuration change |
| VRVDR-46417 | Major | Dataplane is sending GRE packets sourced from non-exist VRRP VIP when router is BACKUP |
| VRVDR-45396 | Critical | Shunt policy installation race |
| VRVDR-42108 | Minor | After 25s ssh login delay 'systemctl --user status' fails with "Failed to connect to bus: No such file or directory" |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-47707 | 7.8 | DSA-4484-1 | CVE-2019-13272: Debian DSA-4484-1: linux security update |
| VRVDR-37993 | 5.0 | N/A | CVE-2013-5211: Network Time Protocol (NTP) Mode 6 Scanner |

## 1801za
{: #1801za}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-47387 | Major | NAT statistics displaying unrealistic usage values |
| VRVDR-47345 | Minor | Syslog rate-limiting does not take effect when configured |
| VRVDR-47290 | Minor | SNMP agent memory cleanup issue on interface scans for ipAddrTable GET/GETNEXT fetch requests |
| VRVDR-47224 | Minor | OSPF debug logs are incorrectly showing when logging level is set to info |
| VRVDR-47222 | Minor | GUI not responding after RO users login |
| VRVDR-47179 | Major | “Update config-sync” overwrites IPsec pre-shared secret key with masked value of asterisks if run by different user than the one used for config-sync itself  |
| VRVDR-47066 | Major | Configuration change to a site-to-site or DMVPN may cause IKE negotiation to fail with INVAL_ID for IKEv1 or TS_UNACCEPT for IKEv2  |
| VRVDR-47001 | Minor | MTU value changes on VIF/VRRP interface after restart or reboot - cosmetic |
| VRVDR-46991 | Minor | “Show tech-support save” should include additional debug detail for site-to-site configs  |
| VRVDR-46775 | Major | Modifying the tunnel configuration of an IPsec peer that uses multiple VFP interfaces may cause an active tunnel to become stale |
| VRVER-45230 | Blocker | Massive memory leak with SNMP polling |
| VRVDR-39747 | Major | Incorrectly reported total available SNAT entries when configuring translation address/mask directly |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-47586 | N/A | DSA-4477-1 | CVE-2019-13132: Debian DSA-4477-1: zeromq3 security update |
| VRVDR-47573 | 7.4 | DSA-4475-1 | CVE-2019-1543: Debian DSA-4475-1 : openssl - security update |
| VRVDR-47532 | 9.8 | DSA-4465-1 | CVE-2019-3846, CVE-2019-5489, CVE-2019-9500, CVE-2019-9503, CVE-2019-10126, CVE-201911477, CVE-2019-11478, CVE-2019-11479, CVE2019-11486, CVE-2019-11599, CVE-2019-11815, CVE-2019-11833, CVE-2019-11884: Debian DSA4465-1: Linux – security update |
| VRVDR-47497 | 7.5 | DSA-4472-1 | CVE-2018-20843: Debian DSA-4472-1 : expat - security update |
| VRVDR-47389 | N/A | DSA-4467-2 | CVE-2019-12735: Debian DSA-4467-2: vim regression update |
| VRVDR-47388 | N/A | DSA-4469-1 | CVE-2019-10161, CVE-2019-10167: Debian DSA4469-1: libvirt security update |
| VRVDR-47363 | 8.6 | DSA-4467-1 | CVE-2019-12735: Debian DSA-4467-1 : vim - security update |
| VRVDR-47358 | 9.8 | N/A | CVE-2016-10228, CVE-2017-12132, CVE-20181000001, CVE-2018-6485, CVE-2017-15670, CVE2017-15671, CVE-2017-15804, CVE-2017-12133, CVE-2017-16887, CVE-2017-1000366, CVE-20155180, CVE-2016-6323, CVE-2016-10228: glibc package update |
| VRVDR-47293 | 7.1 | DSA-4462-1 | CVE-2019-12749: Debian DSA-4462-1 : dbus - security update |
| VRVDR-47202 | N/A | DSA-4454-2 | Debian DSA-4454-2: qemu regression update |


## 1801z
{: #1801z}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-46941 | Minor| Traffic that has SNAT session is filtered using stateless ZBF on return |
| VRVDR-46659 | Major | I350 intfs with mtu 9000 remains stuck at u/D state on upgrade from 1808* to 1903a |
| VRVDR-46623 | Minor | Firewall 'description' logs a perl error on commit when the description has more than one word |
| VRVDR-46549 | Critical | Shell injection privilege escalation/sandbox escape in "show ip route routing-instance <name> variance" command |
| VRVDR-46389 | Major | BGP configuration changes may not take effect if applied after (re)boot |
| VRVDR-45949 | Minor | Netflow generates a NOTICE log for every sample sent when certain non-key fields are configured |
| VRVDR-43169 | Minor | Logging everytime one calls a configd C based API but doesn't supply an error struct is no longer useful |
| VRVDR-41225 | Minor | When configuring interface description, every white space is treated as a new line |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-46824 | N/A | DSA-4440-1 | CVE-2018-5743, CVE-2018-5745, CVE-2019-6465: Debian DSA-4440-1 : bind9 - security update |
| VRVDR-46603 | 5.3 | DSA-4435-1 | CVE-2019-7317: Debian DSA-4435-1 : libpng1.6 - security update |
| VRVDR-46425 | N/A | DSA-4433-1 | CVE-2019-8320, CVE-2019-8321, CVE-2019-8322, CVE-2019-8323, CVE-2019-8324, CVE-2019-8325: Debian DSA-4433-1 : ruby2.3 - security update |
| VRVDR-46350 | 9.1 | DSA-4431-1 | CVE-2019-3855, CVE-2019-3856, CVE-2019-3857, CVE-2019-3858, CVE-2019-3859, CVE-2019-3860, CVE-2019-3861, CVE-2019-3862, CVE-2019-3863: Debian DSA-4431-1 : libssh2 - security update |

## 1801y
{: #1801y}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-46029 | Major | VRRP authentication either with simple text password or AH type does not work properly |
| VRVDR-45864 | Critical | Shell injection privilege escalation/sandbox escape in vyatta-techsupport remote copy |
| VRVDR-45748 | Major | Missing checks for zmsg_popstr returning a NULL pointer causing connsync to crash dataplane |
| VRVDR-45740 | Minor | 'generate tech-support archive' should not aggregate all existing archives |
| VRVDR-45720 | Major | vrrp gets stuck waiting for a packet when start_delay used with only a single router |
| VRVDR-45655 | Critical | "PANIC in rte_mbuf_raw_alloc" when performing VRRP failover |
| VRVDR-45059 | Major | null deref in sip_expire_session_request |
| VRVDR-41419 | Major | Static Analysis dataplane fixes |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-46139 | 7.0 | DSA-4428-1 | CVE-2019-3842: Debian DSA-4428-1 : systemd - security update |
| VRVDR-46087 | N/A | DSA-4425-1 | CVE-2019-5953: Debian DSA-4425-1 : wget - security update |
| VRVDR-45897 | 7.5 | DSA-4416-1 | CVE-2019-5716, CVE-2019-5717, CVE-2019-5718, CVE-2019-5719, CVE-2019-9208, CVE-2019-9209, CVE-2019-9214: Debian DSA-4416-1 : wireshark - security update |
| VRVDR-45553 | 5.9 | DSA-4400-1 | CVE-2019-1559: Debian DSA-4400-1 : openssl1.0 - security update |
| VRVDR-45549 | 6.5 | DSA-4397-1 | CVE-2019-3824: Debian DSA-4397-1 : ldb - security update |
| VRVDR-45347 | 6.8  | DSA-4387-1 | CVE-2018-20685, CVE-2019-6109, CVE-2019-6111: Debian DSA-4387-1 : openssh - security update |

**Security Vulnerabilities Resolved**

The following commands have been deprecated from this patch and are no longer available: 
  • policy route pbr <name> rule <rule-number> application name <name> 
  • policy route pbr <name> rule <rule-number> application type <type> 
  • policy qos name <policy-name> shaper class <class-id> match <match-name> application name <name> 
  • policy qos name <policy-name> shaper class <class-id> match <match-name> application type <type> 
  • security application firewall name <name> rule <rule-number> name <app-name> 

Running any of the above commands will result with the error message “This feature is disabled.” 

## 1801w
{: #1801w}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-45672 | Critical | The RSA private key at /opt/vyatta/etc/config/ipsec.d/rsakeys/localhost.key has wrong permissions |
| VRVDR-45591 | Critical | Interface IP MTU change not taking effect for Intel x710 NICs |
| VRVDR-45466 | Minor | IPv6 address not abbreviated when config is loaded via PXE boot causing config-sync issues |
| VRVDR-45414 | Minor | Vyatta-cpu-shield fails to start and throws `OSError:[Errno 22] Invalid argument` for various cores on a two socket system |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-45253 | 7.5 | DSA-4375-1 | CVE-2019-3813: Debian DSA 4375-1: spice - security update |
| VRVDR-44922 | 7.5 | DSA-4355-1 | CVE-2018-0732, CVE-2018-0734, CVE-2018-0737, CVE-2018-5407: Debian DSA-4355-1 : openssl1.0 - security update |
| VRVDR-43936 | 7.5 | DSA-4309-1 | CVE-2018-17540: Debian DSA-4309-1 : strongswan - security update |

## 1801v
{: #1801v}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-45175 | Critical | Rsyslogd core dump when VRFs configured |
| VRVDR-45057 | Critical | IPsec VTI tunnel interface in A/D state after initially coming up, IPsec SA remain UP |
| VRVDR-44985 | Major | DNAT and Input Firewall logging / order of operation |
| VRVDR-44944 | Critical | vyatta-config-vti.pl: Unsafe temporary file usage |
| VRVDR-44941 | Minor| Static route missing in kernel due to brief VTI interface flap |
| VRVDR-44914 | Critical | RPC ALG crash on both members of HA pair |
| VRVDR-44668 | Major | With production traffic flow-monitoring stalls and stops reporting netflow statistics |
| VRVDR-44667 | Minor | The interface order is not consistent between executions of 'show flow-monitoring' |
| VRVDR-44657 | Major | IKEv1 re-key collision causes VTI interface to stay down when tunnels are up |
| VRVDR-44560 | Major| Multiple rcu_sched CPU stalls pointing to ip_gre driver |
| VRVDR-44517 | Minor | Dataplane crashes with panic in rte_ipv6_fragment_packet |
| VRVDR-44282 | Major | Issue deleting /32 mask when both address with /32 mask and without are present together in address group |
| VRVDR-44278 | Minor | "show address-group all ipv4 optimal" not producing any output |
| VRVDR-44239 | Major | Request to enhance Web GUI verbiage for protocol drop-down when 'all' protocols are required |
| VRVDR-44076 | Major | memory-leak in flow-monitoring leading to dataplane seg-fault and outage |
| VRVDR-44007 | Critical | Dataplane segmentation fault at npf_dataplane_session_establish |
| VRVDR-43909 | Minor| Connsync causes interfaces to go down after "restart vrrp" |
| VRVDR-42679 | Major | syslog - crash in zactor_is |
| VRVDR-42020 | Major | RIB stuck adding same route over and over again |
| VRVDR-18095 | Minor | Flow monitoring stats is not captured as part of 'show tech-support' |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-45148 | N/A | DSA-4371-1 | CVE-2019-3462: Debian DSA-4371-1 – apt security update |
| VRVDR-45043 | 8.8 | DSA-4369-1 | CVE-2018-19961, CVE-2018-19962, CVE-2018- 19965, CVE-2018-19966, CVE-2018-19967: DSA 4369-1 - Xen security update |
| VRVDR-45042 | N/A | DSA-4368-1 | CVE-2019-6250: Debian DSA-4368-1 : zeromq3 - security update |
| VRVDR-45035 | N/A | DSA-4367-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - security update |
| VRVDR-44956 | 7.5 | DSA-4359-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - security updateCVE-2018-12086, CVE-2018-18225, CVE-2018- 18226, CVE-2018-18227, CVE-2018-19622, CVE- 2018-19623, CVE-2018-19624, CVE-2018-19625, CVE-2018-19626, CVE-2018-19627, CVE-2018- 19628: Debian DSA-4359-1 : wireshark - security update |
| VRVDR-44747 | N/A | DSA-4350-1 | CVE-2018-19788: Debian DSA-4350-1 : policykit-1 - security update |
| VRVDR-44634 | 8.8 | DSA-4349-1 | CVE-2017-11613, CVE-2017-17095, CVE-2018- 10963, CVE-2018-15209, CVE-2018-16335, CVE- 2018-17101, CVE-2018-18557, CVE-2018-5784, CVE-2018-7456, CVE-2018-8905:Debian DSA-4349- 1 : tiff - security update |
| VRVDR-44633 | 7.5 | DSA-4348-1| CVE-2018-0732, CVE-2018-0734, CVE-2018-0735, CVE-2018-0737, CVE-2018-5407: Debian DSA-4348- 1 : openssl - security update |
| VRVDR-44611 | 9.8 | DSA-4347-1| CVE-2018-18311, CVE-2018-18312, CVE-2018- 18313, CVE-2018-18314: Debian DSA-4347-1 : perl - security update |
| VRVDR-44348| 9.8 | DSA-4338-1 | CVE-2018-10839, CVE-2018-17962, CVE-2018- 17963: Debian DSA-4338-1: qemu security update |
| VRVDR-43264| 5.6 | DSA-4274-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4274- 1: xen security update |

## 1801u
{: #1801u}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44406 | Critical | With multiple subnet on same VIF low rate of transit traffic observed when compared to 5400 performance |
| VRVDR-44253 | Minor | MSS clamping on bonding interface stops functioning after reboot |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-44277 | N/A | DSA-4332-1 | CVE-2018-16395, CVE-2018-16396: Debian DSA-4332-1 : ruby2.3 - security update |
| VRVDR-44276 | N/A | DSA-4331-1 | CVE-2018-16839, CVE-2018-16842: Debian DSA-4331-1 : curl - security update |

## 1801t
{: #1801t}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44172 | Blocker | Error “interfaces [openvpn] is not valid” reported in mss-clamp tests |
| VRVDR-43969 | Minor | Vyatta 18.x GUI reports the wrong status check memory usage |
| VRVDR-43847  | Major | Slow throughput for TCP conversations on bonding interface |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-43842 | N/A | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – security update |

## 1801s
{: #1801s}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44041 | Major | SNMP ifDescr oid slow response time |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-44074| 9.1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – security update|
| VRVDR-44054 | 8.8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – security update |
| VRVDR-44038 | N/A | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – security update |
| VRVDR-44033 | N/A | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – security update |
|VRVDR-43922 | 7.8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – security update |
| VRVDR-43908 | 9.8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - security update |
| VRVDR-43884 | 7.5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - security update |

## 1801r
{: #1801r}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-43738 | Major | ICMP Unreachable packets returned through SNAT session are not delivered |
| VRVDR-43538 | Major | Receive oversize errors on bondinginterface |
| VRVDR-43519 | Major | Vyatta-keepalived is running with no config present |
| VRVDR-43517 | Major | Traffic fails when endpoint of VFP/Policy-based IPsec resides on the vRouter itself |
| VRVDR-43477 | Major | Committing the IPsec VPN configuration returns the warning “Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], received error code 65280 |
| VRVDR-43379 | Minor | NAT statistics incorrectly shown |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-43837 | 7.5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1: libarchive-zip-perl –security update |
| VRVDR-43693 | N/A | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1: mgetty –security update |
| VRVDR-43578 | N/A | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1: curl -security update |
| VRVDR-43326 | N/A | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1: openssh -security update |
| VRVDR-43198 | N/A | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1: linux security update (FragmentSmack) |
| VRVDR-43110 | N/A | DSA-4265-1 | Debian DSA-4265-1 : xml-security-c -security update |
| VRVDR-43057 | N/A | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : libmspack -security update |
| VRVDR-43026 | 9.8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 -security updateVRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 :fuse -security update |

## 1801q
{: #1801q}

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-43531 | Major |Boot on 1801p results in kernel panic within roughly 40 seconds |
| VRVDR-43104 | Critical | Fake Gratuitous ARP over DHCP network when IPsec is enabled |
| VRVDR-41531 | Major | IPsec continues to attempt to use VFP interface after unbinding it |
| VRVDR-43157 | Minor | When tunnel bounces SNMP trap is not properly generated. |
| VRVDR-43114 | Critical | Upon reboot, a router in an HA pair with a higher priority than its peer does not honor its own “preempt false” configuration and becomes the master immediately following the boot |
| VRVDR-42826 | Minor | With remote-id “0.0.0.0” peer negotiation fails due to pre-shared-key mismatch |
| VRVDR-42774 | Critical| X710 (i40e) driver sending flow control frames at a very high rate |
| VRVDR-42635 | Minor | BGP redistribute route-map policy change does not take effect |
| VRVDR-42620 | Minor | Vyatta-ike-sa-daemon throws error “Command failed: establishing CHILD_SA passthrough-peer” while tunnel appears to be up |
| VRVDR-42483 | Minor | TACACS authentication failing |
| VRVDR-42283 | Major | VRRP state changes to FAULT for all interfaces when a vif interface ip is deleted |
| VRVDR-42244 | Minor | Flow-monitoring only exports 1000 samples to collector |
| VRVDR-42114 | Critical | HTTPS service MUST NOT expose TLSv1 |
| VRVDR-41829 | Major | Dataplane core dumps until system becomes unresponsive with SIP ALG soak test |
| VRVR-41683 | Blocker | DNS name server address learned over VRF is not consistently recognized |
| VRVDR-41628 | Minor | Route/prefix from router-advertisement active in kernel and data plane but ignored by RIB |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-43288 | 5.6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – Linux security update |
| VRVDR-43111 | N/A | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – Linux security update |

## 1801n
{: #1801n}

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

## 1801m
{: #1801m}

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

## 1801k
{: #1801k}

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

## 1801j
{: #1801j}

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
{: #5-2R6S10}

Released May 17, 2018

**Issues Resolved**
| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41543 | Major | “update config-sync” is reporting errors when “\” backslash is used in configuration descriptions
| VRVDR-27018 | Critical | Running configuration file is globally readable |

## 1801h
{: #1801h}

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

## 1801g
{: #1801g}

Released May 4, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41620 | Major | vTI interface traffic stops sending traffic after new vIF is added |
| VRVDR-40965 | Major | Bonding does not recover after a data plane crash |

## 1801f
{: #1801f}

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
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |
| VRVDR- 41331 | 6.5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – security update
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update |
| VRVDR- 41215 | 6.1 |CVE-2018-1059 | CVE-2018-1059 – DPDK vhost out of bound host memory access from VM guests |

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

## 1801e
{: #1801e}

Released March 28, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39985 | Minor | TCP DF packets larger than GRE tunnel MTU are dropped with no ICMP fragmentation needed returned |
| VRVDR-41088 | Critical | Extended (4 byte) ASN not represented internally as unsigned type |
| VRVDR-40988 | Critical | Vhost not starting when used with certain number of interfaces |
| VRVDR-40927 | Critical | DNAT: SDP in SIP 200 OK not translated when it follows a 183 response |
| VRVDR-40920 | Major | With 127.0.0.1 as listen-address snmpd does not start |
| VRVDR-40920 | Critical | ARP doesn’t work over bonded SR-IOV interface |
| VRVDR-40294 | Major | Dataplane doesn’t restore previous queues after slave is removed from bonding group |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/A | DSA-4140-1 | DSA 4140-1: libvorbis security update |

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

## 1801d
{: #1801d}

Released March 8, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40940 | Major | Data plane crash related to NAT/firewall |
| VRVDR-40886 | Major | Combining “icmp name <value>” with a number of other configuration for the rule will cause firewall to not load |
| VRVDR-39879 | Major | Configuring bonding for jumbo frames fails |

**Security Vulnerabilities Resolved**

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – security update |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5715 / Spectre, aka variant #2 |

## 1801c
{: #1801c}

Released March 7, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40281 | Major | After upgrading from 5.2 to more recent version error “-vbash: show: command not found” in operation mode |

## 1801b
{: #1801b}

Released February 21, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40622 | Major | Cloud-init images fail to detect correctly if IP address has been obtained from DHCP server |
| VRVDR-40613 | Critical | Bond interface does not come up if one of the physical links are down |
| VRVDR-40328 | Major | Cloud-init images take a long time to boot |

## 1801a
{: #1801a}

Released February 7, 2018.

**Issues Resolved**

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40324 | Major | Load averages exceed 1.0 with no load on router with bonding interface |

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
