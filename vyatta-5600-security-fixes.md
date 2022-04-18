---

copyright:
  years: 2017, 2021
lastupdated: "2022-04-17"

keywords:  

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# AT&T Vyatta 5600 vRouter software patches (current)
{: #at-t-vyatta-5600-vrouter-software-patches}

On 30 June 2022, all 1912 versions of IBM Cloud Virtual Router Appliance will be deprecated and no longer supported. To maintain your current functionality, be sure to update to version 2012 prior to 29 June 2022 by opening a [support ticket](/docs/virtual-router-appliance?topic=gateway-appliance-getting-help) and requesting an updated ISO. Once you receive your ISO, you can then follow the instructions for [Upgrading the OS](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) to finish updating your version.
{: deprecated}

**Latest patch received:** January 31, 2022

**Latest documentation published:** January 31, 2022

This document lists the patches for the currently supported versions of Vyatta Network OS 5600. Patches are named with a lowercase letter, excluding “i”, “o”, “l”, and “x”.
{: shortdesc}

When multiple CVE numbers are addressed in a single update, the highest CVSS score is listed.
{: tip}

For the latest full release notes, please open a [support case](/docs/virtual-router-appliance?topic=gateway-appliance-getting-help). For archived patch information for the Vyatta 5600 OS older than 17.2, see [this topic](/docs/virtual-router-appliance?topic=virtual-router-appliance-at-t-vyatta-5600-vrouter-software-patches-52).
{: note}

## 2012g
{: #2012g}

### Security Vulnerabilities Resolved
{: #2012g-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-56909 | 7.8 | N/A | CVE-2021-4034: policykit-1 security update |
{: caption="Security vulnerabilities resolved for 2012g" caption-side="bottom"}

## 1912t
{: #1912t}

### Security Vulnerabilities Resolved
{: #1912t-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-56909 | 7.8 | N/A | CVE-2021-4034: policykit-1 security update |
{: caption="Security vulnerabilities resolved for 1912t" caption-side="bottom"}

## 1912s
{: #1912s}

### Issues Resolved
{: #1912s-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-56188 | Critical | bgpd dumps core in as_list_apply() |
| VRVDR-56672 | Critical | SNAT SIP ALG misinterprets SDP part of packet payload header causing dataplane crash |
| VRVDR-56576 | Critical | Dataplane crash while capturing traffic |
| VRVDR-56131 | Blocker | ping/ssh from remote server to device connected to s9500 SIAD fails, but reachable locally |
| VRVDR-47554 | Major | Validate GRE tunnel transport local-ip |
{: caption="Issues resolved for 1912s" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912s-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-56769 | 8.1 | DLA-2848-1 | CVE-2019-13115, CVE-2019-17498 :Debian DLA-2848-1 : libssh2 - LTS security update |
| VRVDR-56689 | 9.8 | DLA-2836-1 | CVE-2021-43527: Debian DLA-2836-1 : nss - LTS security update |
| VRVDR-56680 | 7.5 | DLA-2837-1 | CVE-2021-43618: DLA-2837-1 : gmp - LTS security update |
| VRVDR-56665 | 9.8 | DLA-2834-1 | CVE-2018-20721: Debian DLA-2834-1 : uriparser - LTS security update |
| VRVDR-56664 | 7.5 | DLA-2833-1 | CVE-2018-5764: Debian DLA-2833-1 : rsync - LTS security update |
| VRVDR-56647 | 8.8 | DLA-2827-1 | CVE-2019-8921, CVE-2019-8922, CVE-2021-41229: Debian DLA-2827-1 : bluez - LTS security update |
| VRVDR-56645 | 8.8 | DLA-2828-1 | CVE-2017-14160, CVE-2018-10392, CVE-2018-10393: Debian DLA-2828-1 : libvorbis - LTS security update |
| VRVDR-56644 | 4.7 | DLA-2830-1 | CVE-2018-20482: Debian DLA-2830-1 : tar - LTS security update |
| VRVDR-56511 | N/A | DLA-2808-1 | CVE-2021-3733, CVE-2021-3737: Debian DLA-2808-1 : python3.5 - LTS security update |
| VRVDR-56503 | 7.5 | DLA-2807-1 | CVE-2018-5740, CVE-2021-25219: Debian DLA-2807-1 : bind9 - LTS security update |
| VRVDR-56497 | 5.5 | DLA-2805-1 | CVE-2019-1010305: Debian DLA-2805-1 : libmspack - LTS security update |
| VRVDR-56496 | 8.8 | DLA-2804-1 | CVE-2019-7572, CVE-2019-7573, CVE-2019-7574, CVE-2019-7575, CVE-2019-7576, CVE-2019-7577, CVE-2019-7578, CVE-2019-7635, CVE-2019-7636, CVE-2019-7637, CVE-2019-7638, CVE-2019-13616:Debian DLA-2804-1 : libsdl1.2 - LTS security update |
| VRVDR-56495 | 6.7 | DLA-2801-1 | CVE-2017-9525, CVE-2019-9704, CVE-2019-9705, CVE-2019-9706:Debian DLA-2801-1 : cron - LTS security update |
| VRVDR-56493 | 9.8 | DLA-2802-1 | CVE-2018-16062, CVE-2018-16402, CVE-2018-18310, CVE-2018-18520, CVE-2018-18521, CVE-2019-7150, CVE-2019-7665:Debian DLA-2802-1 :elfutils - LTS security update |
| VRVDR-56459 | N/A | DLA-2798-1 | Debian DLA-2798-1 : libdatetime-timezone-perl - LTS security update |
| VRVDR-56458 | N/A | DLA-2797-1 | Debian DLA-2797-1 : tzdata - LTS security update |
| VRVDR-56315 | 7.5 | DLA-2788-1 | CVE-2021-41991: Debian DLA-2788-1: A denial-ofservice vulnerability in the in-memory certificate |
{: caption="Security vulnerabilities resolved for 1912s" caption-side="bottom"}

## 1912r
{: #1912r}

### Issues Resolved
{: #1912r-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-56188 | Critical | bgpd dumps core in as_list_apply() |
{: caption="Issues resolved for 1912r" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912r-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-56321 | 7.5 | DLA-2786-1 | CVE-2018-1000168, CVE-2020-11080: Debian DLA-2786-1 : nghttp2 - LTS security update |
| VRVDR-56308 | 7.4 | DLA-2780-1 | CVE-2021-31799, CVE-2021-31810, CVE-2021-32066: Debian DLA-2780-1 : ruby2.3 - LTS security update |
| VRVDR-56295 | 5.5 | DLA-2784-1 | CVE-2020-21913: Debian DLA-2784-1 : icu - LTS security update |
| VRVDR-56230 | 7.5 | DLA-2777-1 | CVE-2020-19131, CVE-2020-19144: Debian DLA-2777-1 : tiff - LTS security update |
| VRVDR-56229 | 7.4 | DLA-2774-1 | CVE-2021-3712: Debian DLA-2774-1 : openssl1.0 - LTS security update |
| VRVDR-56228 | 7.5 | DLA-2773-1 | CVE-2021-22946, CVE-2021-22947: Debian DLA-2773-1 : curl - LTS security update |
| VRVDR-56221 | 6.5 | DLA-2771-1 | CVE-2018-5729, CVE-2018-5730, CVE-2018-20217, CVE-2021-37750: Debian DLA-2771-1 : krb5 - LTS security update |
| VRVDR-56210 | 7.4 | DLA-2766-1 | CVE-2021-3712: Debian DLA-2766-1 : openssl - LTS security update |
{: caption="Security vulnerabilities resolved for 1912r" caption-side="bottom"}

## 1912q
{: #1912q}

### Issues Resolved
{: #1912q-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-55753 | Major | Multicast: eliminate or hide FAL counter logs |
| VRVDR-55749 | Major | Swapped in SFP doesn't pick up configured MTU |
| VRVDR-55569 | Major | MRIBv6 FIB: Peek error Resource temporarily unavailable |
| VRVDR-55011 | Major | Can't log into a SIAD with read-only SSD |
| VRVDR-54591 | Blocker | TACACS authentications fails when TACACS accounting has a large backlog |
| VRVDR-53135 | Major | "protocols multicast ip log-warning" doesn't log any warnings |
| VRVDR-53114 | Major | TACACS+ session accounting may still use hostname instead of IP address |
| VRVDR-53099 | Major | TACACS+ starts only when service is restarted manually |
| VRVDR-53085 | Major | Multicast IPv4 and IPv6 is mutually exclusive on SIAD |
| VRVDR-52997 | Major | tacplusd get_tty_login_addr() may overflow buffer |
| VRVDR-52912 | Critical | service-user creation fails due to moved SSSD databases |
| VRVDR-52855 | Critical | Creating service users fails |
| VRVDR-52842 | Major | sssd pipes should not be shared with user sandboxes |
| VRVDR-52730 | Major | sssd should not run as root |
| VRVDR-52671 | Critical | sssd_nss crashes on startup if filesystem containing in-memory cache backing files is full |
| VRVDR-52241 | Major | TACACS: Sanity Test Command Authorisation fails due to Tacacs+ DBus Daemon restart |
| VRVDR-52120 | Major | Hostname may be sent instead of IP address in TACACS+ accounting requests |
| VRVDR-52091 | Major | tacplusd should not run as root |
| VRVDR-51809 | Major | TACACS+ session accounting: task_id in stop record differs from task_id in start record |
| VRVDR-51580 | Critical | Command Accounting: Start record support |
| VRVDR-50803 | Major | tacplusd logs are very chatty by default |
| VRVDR-50552 | Major | 'TACACS daemon is not running' even with all TACACS config |
| VRVDR-50310 | Major | SIAD multicast traffic counted on output interface |
| VRVDR-50036 | Major | Add TACACS+/SSSD information to tech support output |
| VRVDR-42098 | Major | TACACS+ Server Connection Timeout |
| VRVDR-42094 | Minor | TACACS+ Server Enable / Disable |
{: caption="Issues resolved for 1912q" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912q-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-55979 | N/A | DLA-2738-1 | CVE-2021-3672: Debian DLA-2738-1 : c-ares - LTS security update |
| VRVDR-55951 | 6.5 | DLA-2735-1 | CVE-2018-14662, CVE-2018-16846, CVE-2020-1760, CVE-2020-10753, CVE-2021-3524: Debian DLA-2735-1: ceph – LTS security update |
| VRVDR-55948 | 7.4 | DLA-2734-1 | CVE-2021-22898, CVE-2021-22924: Debian DLA-2734-1: curl – LTS security update |
| VRVDR-55792 | 5.5 | DLA-2715-1 | CVE-2021-33910: Debian DLA-2715-1: systemd - LTS security update |
| VRVDR-55761 | 7.8 | DSA-4941-1 | CVE-2020-36311, CVE-2021-3609, CVE-2021-33909, CVE-2021-34693: Debian DSA-4941-1: linux security update |
| VRVDR-55648 | N/A | DLA-2703-1 | Debian DLA-2703-1 : ieee-data - LTS security update |
| VRVDR-55538 | 7.8 | DLA-2690-1 | CVE-2020-24586, CVE-2020-24587, CVE-2020-24588, CVE-2020-25670, CVE-2020-25671, CVE-2020-25672, CVE-2020-26139, CVE-2020-26147, CVE-2020-26558, CVE-2020-29374, CVE-2021-0129, CVE-2021-3483, CVE-2021-3506, CVE-2021-3564, CVE-2021-3573, CVE-2021-3587, CVE-2021-23133, CVE-2021-23134, CVE-2021-28688, CVE-2021-28964, CVE-2021-28971, CVE-2021-29154,CVE-2021-29155, CVE-2021-29264, CVE-2021-29647, CVE-2021-29650, CVE-2021-31829, CVE-2021-31916, CVE-2021-32399, CVE-2021-33034:Debian DLA-2690-1: linux LTS security update |
{: caption="Security vulnerabilities resolved for 1912q" caption-side="bottom"}

## 1912p
{: #1912p}

### Issues Resolved
{: #1912p-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-55160 | Blocker | `dataplane: srvAdaptiveFrequencyReferenceTracker.c:91: getStraightenedLocalTimestampsAdaptiveFrequency: Assertion '((direction == E_srvUpLinkDirection) \|\| (direction == E_srvDownLinkDirection))' failed` |
| VRVDR-54128 | Critical | PDV syslogs are observed in huge number |
| VRVDR-53790 | Critical | Crash in mngPtpSessionStop |
{: caption="Issues resolved for 1912p" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912p-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-55602 | 8.8 | DSL-2699-1 | CVE-2020-5208: Debian DLA-2699-1 : ipmitool - LTS security update |
| VRVDR-55600 | 9.8 | DLA-2695-1 | CVE-2021-31870, CVE-2021-31871, CVE-2021-31872, CVE-2021-31873: Debian DLA-2695-1 : klibc - LTS security update | 
| VRVDR-55556 | 7.8 | DLA-2694-1 | CVE-2020-35523, CVE-2020-35524: Debian DLA-2694-1 : tiff security update |
| VRVDR-55555 | 5.7 | DLA-2692-1 | CVE-2020-26558, CVE-2021-0129: Debian DLA-2692-1 : bluez security update |
| VRVDR-55537 | 7.5 | DLA-2691-1 | CVE-2021-33560: Debian DLA-2691-1 : libgcrypt |
| VRVDR-55273 | 6.5 | DLA-2669-1 | CVE-2021-3541: Debian DLA-2669-1 : libxml2 security update |
| VRVDR-55219 | 6.3 | DLA-2623-1 | CVE-2020-17380, CVE-2021-20203, CVE-2021-20255, CVE-2021-20257, CVE-2021-3392, CVE-2021-3409, CVE-2021-3416:Debian DLA-2623-1 : qemu security update |
| VRVDR-55218 | 9.8 | DLA-2666-1 | CVE-2021-31535: Debian DLA-2666-1 : libx11 security update |
| VRVDR-55127 | 5.3 | DLA-2664-1 | CVE-2021-22876: Debian DLA-2664-1 : curl security update |
| VRVDR-55071 | 8.8 | DLA-2653-1 | CVE-2021-3516, CVE-2021-3517, CVE-2021-3518, CVE-2021-3537: Debian DLA-2653-1 : libxml2 security update |
| VRVDR-55024 | 9.8 | DLA-2647-1 | CVE-2021-25214, CVE-2021-25215, CVE-2021-25216: Debian DLA-2647-1 : bind9 security update |
| VRVDR-54850 | 7.8 | DLA-2610-1 | Debian DLA-2610-1 : linux-4.19 security update |
{: caption="Security vulnerabilities resolved for 1912p" caption-side="bottom"}

## 1912n
{: #1912n}

### Issues Resolved
{: #1912n-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-54900 | Major | Constant attempts to revive old duplicate CHILD_SA are causing rekey flood and occasional traffic drop. |
| VRVDR-54765 | Major | ALG session may cause dataplane crash when cleared |
{: caption="Issues resolved for 1912n" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912n-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-54926 | 6.1 | DLA-2628-1 | CVE-2019-16935, CVE-2021-23336: Debian DLA-2628-1 : python2.7 security update | 
| VRVDR-54858 | 9.8 | DLA-2619-1 | CVE-2021-23336, CVE-2021-3177, CVE-2021-3426:Debian DLA-2619-1 : python3.5 security update | 
| VRVDR-54849 | 7.5 | DLA-2614-1 | CVE-2021-28831: Debian DLA-2614-1 : busybox security update | 
| VRVDR-54848 | N/A | DLA-2611-1 | CVE-2020-27840, CVE-2021-20277: Debian DLA-2611-1 : ldb security update | 
| VRVDR-54712 | 8.1 | DLA-2588-1 | CVE-2021-20234, CVE-2021-20235: Debian DLA-2588-1 : zeromq3 security update | 
{: caption="Security vulnerabilities resolved for 1912n" caption-side="bottom"}

## 1912m
{: #1912m}

### Issues Resolved
{: #1912m-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-54586 | Major | Dataplane crash in connection sync on closing tcp session |
| VRVDR-53889 | Major | BFD mbuf leak when deployed in a VNF using PCI-Passthrough on ixgbe |
{: caption="Issues resolved for 1912m" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912m-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-54801 | 7.2 | DLA-2605-1 | CVE-2021-27928: Debian DLA-2605-1 : mariadb-10.1 security update | 
| VRVDR-54788 | 8.1 | DLA-2604-1 | CVE-2020-25681, CVE-2020-25682, CVE-2020-25683, CVE-2020-25684, CVE-2020-25687: Debian DLA-2604-1 : dnsmasq security update | 
| VRVDR-54770 | 9.8 | DLA-2596-1 | CVE-2017-12424, CVE-2017-20002: Debian DLA-2596-1 : shadow security update | 
| VRVDR-54563 | 7.5 | DLA-2574-1 | CVE-2021-27212: Debian DLA-2574-1 : openldap security update | 
| VRVDR-54562 | 9.8 | DLA-2570-1 | CVE-2021-26937: Debian DLA-2570-1: screen security update | 
| VRVDR-54531 | 8.1 | DLA-2568-1 | CVE-2020-8625: Debian DLA-2568-1 : bind9 security update | 
{: caption="Security vulnerabilities resolved for 1912m" caption-side="bottom"}

## 1912k
{: #1912k}

### Issues Resolved
{: #1912k-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-54360 | Major| Operator level user cannot execute 'show firewall ...' commands |
| VRVDR-54272 | Critical | tech-support archive generated uncompressed breaking user expectations | 
| VRVDR-54238 | Major | Dataplane crash in map_rcu_freeon system shutdown | 
| VRVDR-54225 | Minor | VFPinterface does not pick up IP Address from donor loopback interface | 
| VRVDR-54160 | Major | LACP with VIF -Slaves not selected in 'lacp' & 'balanced' modes | 
| VRVDR-54144 | Blocker | Marvell FALplugin should drop backplane packets with RX Errors | 
| VRVDR-54119 | Critical | Repeated PTP tunnel failures due to busy state | 
| VRVDR-54027 | Major | Migrating loopback to self GRE tun50 configuration to newer code versions | 
| VRVDR-51846 | Critical | RIB table not updated correctly for OSPFv3 routes after flapping the primary path by making dataplane/switch interface link failure/recovery |
{: caption="Issues resolved for 1912k" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912k-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-54536 | 9.1 | DLA-2566-1 | CVE-2019-20367: Debian DLA-2566-1 : libbsd security update | 
| VRVDR-54535 | N/A | DLA-2565-1 | CVE-2021-23840, CVE-2021-23841:Debian DLA-2565-1 : openssl1.0 security update | 
| VRVDR-54534 | N/A | DLA-2563-1 | CVE-2021-23840, CVE-2021-23841: Debian DLA-2563-1 : openssl security update | 
| VRVDR-54499 | 8.8 | DLA-2557-1 | CVE-2020-27815, CVE-2020-27825, CVE-2020-27830, CVE-2020-28374, CVE-2020-29568, CVE-2020-29569, CVE-2020-29660, CVE-2020-29661, CVE-2020-36158, CVE-2021-20177, CVE-2021-3347: Debian DLA-2557-1 : linux-4.19 security update | 
| VRVDR-54445 | 7.8 | DLA-2549-1 | CVE-2020-0256, CVE-2021-0308: Debian DLA-2549-1 : gdisk security update | 
| VRVDR-54436 | 7.5 | DLA-2547-1 | CVE-2019-13619, CVE-2019-16319, CVE-2019-19553, CVE-2020-7045, CVE-2020-9428, CVE-2020-9430, CVE-2020-9431, CVE-2020-11647, CVE-2020-13164, CVE-2020-15466, CVE-2020-25862, CVE-2020-25863, CVE-2020-26418, CVE-2020-26421, CVE-2020-26575, CVE-2020-28030: Debian DLA-2547-1: wireshark security update | 
| VRVDR-54400 | 7.5 | DLA-2544-1 | CVE-2020-36221, CVE-2020-36222, CVE-2020-36223, CVE-2020-36224, CVE-2020-36225, CVE-2020-36226, CVE-2020-36227, CVE-2020-36228, CVE-2020-36229, CVE-2020-36230 :Debian DLA-2544-1 : openldapsecurity update | 
| VRVDR-54399 | N/A | DLA-2543-1 | Debian DLA-2543-1 : libdatetime-timezone-perl new upstream version | 
| VRVDR-54398 | N/A | DLA-2542-1 | Debian DLA-2542-1 : tzdata new upstream version | 
| VRVDR-54337 | 6.5 | DLA-2538-1 | CVE-2020-14765, CVE-2020-14812: Debian DLA-2538-1 : mariadb-10.1 security update | 
| VRVDR-54287 | 7.8 | DLA-2534-1 | CVE-2021-3156: Debian DLA-2534-1 : sudo security update | 
{: caption="Security vulnerabilities resolved for 1912k" caption-side="bottom"}

## 1912j
{: #1912j}

### Issues Resolved
{: #1912j-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-54142 | Critical | Inconsistent VRRP interface status upon reboot | 
| VRVDR-54047 | Critical | On i40e driver when bond is disabled the link-state of member interfaces is u/D when configured but u/u after a reboot | 
| VRVDR-53964 | Major | User-isolation feature is not present in licensed 'B' images | 
| VRVDR-53962 | Critical | Reboot D2MSN backup connection created systemd-coredump with BGP authentication enabled | 
| VRVDR-53928 | Major | Jumbo Frame MTU setting on Intel IGB interface causes link to go down | 
| VRVDR-53854 | Major | Interfaces went down / panic: runtime error: slice bounds out of range | 
| VRVDR-53368 | Minor | Alpha-numeric common pattern with preceding '0' in `resources group <name>` causes out of order list on config-sync slave | 
{: caption="Issues resolved for 1912j" caption-side="bottom"}
  
### Security Vulnerabilities Resolved
{: #1912j-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-54046 | 7.5 | DLA-2513-1 | CVE-2020-29361, CVE-2020-29362: Debian DLA-2513-1 : p11-kit security update | 
| VRVDR-54039 | 7.5 | DLA-2116-1 | CVE-2015-9542: Debian DLA 2116-1:libpam-radius-auth security update | 
| VRVDR-53970 | N/A | DLA-2510-1 | Debian DLA-2510-1 : libdatetime-timezone-perl new upstream release | 
| VRVDR-53969 | N/A | DLA-2509-1 | Debian DLA-2509-1 : tzdata new upstream version| 
| VRVDR-53968 | 7.5 | DLA-2500-1 | CVE-2020-8284, CVE-2020-8285, CVE-2020-8286: Debian DLA-2500-1 : curl security update | 
| VRVDR-53967 | 8.1 | DLA-2498-1 | CVE-2018-1311: Debian DLA-2498-1 : xerces-c security update | 
| VRVDR-53966 | N/A | DLA-2488-2 | Debian DLA-2488-2 : python-apt regression update | 
| VRVDR-53965 | 6.1 | DLA-2467-2 | CVE-2020-27783: Debian DLA-2467-2 : lxml regression update | 
| VRVDR-53861 | 8.2 | DLA-2483-1 | CVE-2019-19039, CVE-2019-19377, CVE-2019-19770, CVE-2019-19816, CVE-2020-0423, CVE-2020-8694, CVE-2020-14351, CVE-2020-25656, CVE-2020-25668, CVE-2020-25669, CVE-2020-25704, CVE-2020-25705, CVE-2020-27673, CVE-2020-27675, CVE-2020-28941, CVE-2020-28974: Debian DLA-2483-1: linux-4.19 security update | 
{: caption="Security vulnerabilities resolved for 1912j" caption-side="bottom"}

## 1912h
{: #1912h}

### Issues Resolved
{: #1912h-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-53699 | Blocker | PTP implementation noms all the PTP packets. Even though not destined for it. | 
| VRVDR-53596 | Critical | config-sync is not operational when the configuration contains quotes |
| VRVDR-53570 | Critical | Storm control policy may have non-zero packet counts on being applied |
| VRVDR-53515 | Critical | PTP remains in acquiring state long enough to trigger an alarm |
| VRVDR-53373 | Critical | When bond is disabled the link-state of member interfaces is u/D when configured but u/u after a reboot |
| VRVDR-53367 | Minor | config-sync does not work if a modified candidate config exists on peer |
| VRVDR-53324 | Blocker | ADI XS uCPE: InDiscards seen in the switch backplane at 1G |
| VRVDR-53083 | Critical | Coredumpobserved at in.telnetd |
| VRVDR-52877 | Blocker | ADI QoS Performance Issue with specific packet sizes |
| VRVDR-52074 | Major | Mark maps using DSCP resource groups don't pick up resource group changes |
| VRVDR-51940 | Blocker | Changing DSCP Values Causes BFD Instability Which Requires Reboot |
| VRVDR-51529 | Critical | Config Sync fails displaying 'vyatta-interfaces-v1:interfaces' when firewall action configured |
| VRVDR-43453 | Minor | `show l2tpeth/ show l2tpeth <interface>` returns "Use of uninitialized value in printf at /opt/vyatta/bin/vplane-l2tpeth-show.pl line 41" with the output |
{: caption="Issues resolved for 1912h" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912h-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-53860 | 7.5 | DLA-2340-2 | CVE-2019-20218: Debian DLA-2340-2 : sqlite3 regression update |
| VRVDR-53859 | 2.8 | DLA-2488-1 | CVE-2020-27351: Debian DLA-2488-1 : python-apt security update |
| VRVDR-53858 | 5.7 | DLA-2487-1 | CVE-2020-27350: Debian DLA-2487-1 : apt security update |
| VRVDR-53824 | N/A | DLA-2481-1 | CVE-2020-25709, CVE-2020-25710: Debian DLA-2481-1: openldap security update |
| VRVDR-53769 | 6.1 | DLA-2467-1 | CVE-2018-19787, CVE-2020-27783: Debian DLA-2467-1 : lxml security update |
| VRVDR-53688 | 7.5 | DLA-2456-1 | CVE-2019-20907, CVE-2020-26116: Debian DLA-2456-1 : python3.5 security update |
| VRVDR-53626 | 6.5 | DLA-2445-1 | CVE-2020-28241: Debian DLA-2445-1 : libmaxminddb security update |
| VRVDR-53625 | 7.5 | DLA-2444-1 | CVE-2020-8037: Debian DLA-2444-1 : tcpdump security update |
| VRVDR-53624 | 7.5 | DLA-2443-1 | CVE-2020-15166: Debian DLA-2443-1 : zeromq3 security update |
| VRVDR-53526 | 7.5 | DLA-2423-1 | CVE-2019-10894, CVE-2019-10895, CVE-2019-10896, CVE-2019-10899, CVE-2019-10901, CVE-2019-10903, CVE-2019-12295: Debian DLA-2423-1 : wireshark security update |
| VRVDR-53525 | N/A | DLA-2425-1 | Debian DLA-2425-1 : openldap security update |
| VRVDR-53524 | N/A | DLA-2424-1 | Debian DLA-2424-1 : tzdata new upstream version |
| VRVDR-53448 | N/A | DLA-2409-1 | CVE-2020-15180: Debian DLA-2409-1 : mariadb-10.1 security update |
{: caption="Security vulnerabilities resolved for 1912h" caption-side="bottom"}

## 1912g
{: #1912g}

### Issues Resolved
{: #1912g-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-53517 | Critical | PTP de-referencing bad interface pointer | 
| VRVDR-53459 | Critical | ATT-VROUTER-PTP-MIB::attVrouterPtpServoFailure no longer sent | 
| VRVDR-53429 | Blocker | Up-rev Ufi hwdiag to 3.1.11 | 
| VRVDR-53385 | Blocker | Repeat PTP servo failure messages | 
| VRVDR-53372 | Critical | Dataplane crash in ptp_peer_resolver_cb | 
| VRVDR-53317 | Critical | PTP: port packet counters ignore signalling messages | 
| VRVDR-53305 | Blocker | Incoming PTP traffic is not being trapped to the PTP firmware | 
| VRVDR-53302 | Critical | Boundary Clock lost sync and is unable to re-acquire lock | 
| VRVDR-53014 | Critical | commit-confirm not working via vcli scripts | 
| VRVDR-52995 | Critical | Grub update during image upgrade is broken | 
| VRVDR-52879 | Blocker | PTP: Unable to peer with master when route to GM fails over to backup vlan | 
| VRVDR-52877 | Blocker | ADI QoS Performance Issue with specific packet sizes | 
| VRVDR-52825 | Minor | Configuring three sub-levels of time-zone is not possible, causing upgrade from earlier version to fail | 
| VRVDR-52739 | Major | Port value in tunnel policy without specifying protocol causes error "protocol must be formatted as well-known string." for IPsec 'show' commands | 
| VRVDR-52677 | Major | When multiple peers use the same local-address, no authentication ids, and unique pre-shared-keys IKEv2 based IPsec stuck in 'init' for all but one peer | 
| VRVDR-52668 | Major | Configuration fails to load after upgrade from 1801ze to 1912e when firewall rule with port range 0-65535 statement is present | 
| VRVDR-52611 | Major | i40e driver silently drops multicast packets causing VRRP dual master | 
| VRVDR-52425 | Major | TACACS+ command authorization/accounting bypass via NETCONF | 
| VRVDR-52424 | Major | NETCONF edit-config applies changes with "none" default-operation, and no specified operation | 
| VRVDR-52410 | Critical | IPsec: SNMP trap no longer sent when IPsec tunnel goes up or down | 
| VRVDR-52404 | Major | ICMP error returned with corrupted inner header causes seg-fault when passed through a FW/NAT44/PBR rule with logging enabled | 
| VRVDR-52401 | Critical | Degradation of throughput by 10%-40% on v150 with 100M physical interface & QOS | 
| VRVDR-52221 | Major | Disabled PMTUD on GRE tunnel causes outer packet to inherit inner packet TTL value | 
| VRVDR-52179 | Critical | Overlayfs file corruption of user accounting files | 
| VRVDR-52152 | Critical | PTP: Use monotonic time for semaphores and mutexes | 
| VRVDR-51643 | Major | SNMP Trap not receiving when CHILD_SA deleting | 
| VRVDR-51465 | Blocker | Restore (opt-out) collection of shell history in tech-support | 
| VRVDR-51455 | Critical | Bad file descriptor (src/epoll.cpp:100) when applying config | 
| VRVDR-51443 | Major | IPv6 router-advert CLI missing on switch VLAN interfaces | 
| VRVDR-51332 | Major | PTP: Unable to cope with config change where master and slave swap ds-ports (slave does not come up) | 
| VRVDR-50884 | Major | Grub passwd printed in plain-text in installer logs | 
| VRVDR-50619 | Major | LACP with VIF - still seeing Slaves not selected in 'balanced' mode | 
| VRVDR-50544 | Critical | Opd logging YANG files missing in Edinburgh (VNF), Fleetwood onwards (VR and VNF) | 
| VRVDR-50313 | Major | PTP: SIAD does not send "Follow_Up" msgs to slaves when two-step- flag is enabled | 
| VRVDR-50026 | Critical | Dataplane crash: npf_timeout_get() | 
| VRVDR-49447 | Major | show tech-support still logs /var/log/messages | 
| VRVDR-49409 | Major | Dataplane reports that the bonding drivers doesn't support vlan filtering | 
| VRVDR-49209 | Minor | tech-support should not use any user gpg config when encrypting tech support archives | 
| VRVDR-48480 | Blocker | PTP servo reports 0 pps after path switch during ECMP | 
| VRVDR-48460 | Critical | Tshark permission errors and seg fault when executing monitor command | 
| VRVDR-48055 | Critical | IPsec VPN dataplane crash deleting VRF | 
| VRVDR-47858 | Critical | GRE: "RTNETLINK answers: No such file or directory" on trying to delete tunnel | 
| VRVDR-46493 | Major | IPSec RA-VPN Server : IKE proposal not found on server when setting the local-address to "any" | 
| VRVDR-43307 | Critical | vyatta-ike-sa-daemon: TypeError: 'IKEConfig' object does not support indexing | 
| VRVDR-42123 | Major | opd adds node.tag values under the wrong location in tab completion | 
{: caption="Issues resolved for 1912g" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912g-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-53323 | 7.5 | DLA-2391-1 | CVE-2020-25613: Debian DLA-2391-1 : ruby2.3 security update | 
| VRVDR-53273 | 7.8 | DLA-2385-1 | CVE-2019-3874, CVE-2019-19448, CVE-2019-19813, CVE-2019-19816, CVE-2020-10781, CVE-2020-12888, CVE-2020-14314, CVE-2020-14331, CVE-2020-14356, CVE-2020-14385, CVE-2020-14386, CVE-2020-14390, CVE-2020-16166, CVE-2020-25212, CVE-2020-25284, CVE-2020-25285, CVE-2020-25641, CVE-2020-26088: Debian DLA-2385-1: linux-4.19 LTS security update | 
| VRVDR-53272 | 9.8 | DLA-2388-1 | Debian DLA-2388-1 : nss security update | 
| VRVDR-53231 | N/A | DLA-2382-1 | CVE-2020-8231: Debian DLA-2382-1 : curl security update | 
| VRVDR-53230 | 3.7 | DLA-2378-1 | CVE-2020-1968: Debian DLA-2378-1 : openssl1.0 security update | 
| VRVDR-52817 | 6.4 | N/A | CVE-2020-15705: GRUB2 fails to validate kernel signature when booted directly without shim, allowing secure boot to be bypassed | 
| VRVDR-52457 | 7.8 | DLA-2301-1 | CVE-2020-12762: Debian DLA-2301-1 : json-c security update | 
| VRVDR-52456 | 6.7 | DLA-2290-1 | CVE-2019-5188: Debian DLA-2290-1 : e2fsprogs security update | 
| VRVDR-52454 | N/A | DLA-2295-1 | CVE-2020-8177: Debian DLA-2295-1 : curl security update | 
| VRVDR-52357 | 5.6 | DSA-4733-1 | CVE-2020-8608: Debian DSA-4733-1: qemu security update | 
| VRVDR-52273 | 6.7 | DSA-4728-1 | CVE-2020-10756, CVE-2020-13361, CVE-2020-13362, CVE-2020-13754, CVE-2020-13659: Debian DSA 4728-1: qemu security update | 
| VRVDR-52265 | 9.8 | DLA-2280-1 | CVE-2018-20406, CVE-2018-20852, CVE-2019-5010, CVE-2019-9636, CVE-2019-9740, CVE-2019-9947, CVE-2019-9948, CVE-2019-10160, CVE-2019-16056, CVE-2019-16935, CVE-2019-18348, CVE-2020-8492, CVE-2020-14422: Debian DLA-2280-1 : python3.5 security update | 
| VRVDR-51849 | 7.5 | N/A | CVE-2018-19044, CVE-2018-19045, CVE-2018-19046: Insecure temporary file usage in keepalived | 
{: caption="Security vulnerabilities resolved for 1912g" caption-side="bottom"}

## 1912f
{: #1912f}

### Issues Resolved
{: #1912f-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-52669 | Critical | Cannot display EEPROM info for FINISAR FCLF8522P2BTL Copper Port |
| VRVDR-52643 | Blocker | "request hard qsfp/sfp_status present X" - performance degradation |
| VRVDR-52568 | Blocker | Revert SIAD kernel panic defaults |
| VRVDR-52546| Minor | GUI hangs/loading and finally timeout with an error message on browser |
| VRVDR-52469 | Blocker | i2c MUX reset required on S9500 to mitigate bus lock due to malfunctioning SFP |
| VRVDR-52447 | Blocker | PTP: switching between the same master on multiple ports do not work if chosen port is down |
| VRVDR-52284 | Blocker | S9500 - 'request hardware-diag version' command missing product name, reporting eeprom error |
| VRVDR-52278 | Blocker | S9500 - upgrade HW diags to v3.1.10 |
| VRVDR-52248 | Blocker | vyatta-sfpd can start before platform init complete |
| VRVDR-52228 | Minor | The command ‘show hardware sensors sel’ gives a traceback |
| VRVDR-52190 | Critical | smartd attempting to send email |
| VRVDR-52215 | Critical | Memory use after free when deleting storm control profile |
| VRVDR-52104 | Blocker | S9500 integration of BSP 3.0.11, 3.0.12 and 3.0.13 |
| VRVDR-51754 | Critical | Readonly account failed to stay in after log on |
| VRVDR-51344 | Critical | S9500-30XS: 10G Interface LED sometimes lit when interface is disabled |
| VRVDR-51135 | Critical | NTP client remains sync'd with server even though source interface has no address |
| VRVDR-51114 | Minor | Change command not found error for users running in a sandbox |
| VRVDR-50951 | Critical | OSPFv3 logs are not generated when OSPFv3 process is reset |
| VRVDR-50928 | Minor | PTP: ufispace-bsp-utils 3.0.10 causing /dev/ttyACM0 to disappear |
| VRVDR-50775 | Major | Dataplane "PANIC in bond_mode_8023ad_ext_periodic_cb" w/ locally sourced and terminated GRE traffic |
| VRVDR-50549 | Trivial | PTP: Spelling error in log msg "Successfully configure DPLL 2 fast lcok" |
| VRVDR-50359 | Critical | show int dataplane foo phy issues with vendor-rev |
| VRVDR-49935 | Critical | Dataplane core dump generated following vyatta-dataplane restart in vlan_if_l3_disable |
| VRVDR-49836 | Major | IPsec: Fails to be able to to ping from tunnel endpoint to tunnel endpoint with ping size 1419 using default MTU with site-2-site. Tunnel MTU discovery not working |
| VRVDR-48315 | Critical | Malformed interface names in show ipv6 multicast interface with IPv6 GRE tunnels |
| VRVDR-48090 | Major | Error: /transceiver-info/physical-channels/channel/0/laser-bias- current/: is not a decimal64 at /opt/vyatta/share/perl5/Vyatta/Configd.pm line 208 |
{: caption="Issues resolved for 1912f" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912f-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-53016 | 9.1 | DLA-2369-1 | CVE-2017-18258, CVE-2017-8872, CVE-2018- 14404, CVE-2018-14567, CVE-2019-19956, CVE- 2019-20388, CVE-2020-24977, CVE-2020-7595: Debian DLA-2369-1 : libxml2 security update |
| VRVDR-52844 | 7.5 | DLA-2355-1 | CVE-2020-8622, CVE-2020-8623: Debian DLA-2355- 1 : bind9 security update |
| VRVDR-52723 | 8.8 | DLA-2340-1 | CVE-2018-20346, CVE-2018-20506, CVE-2018- 8740, CVE-2019-16168, CVE-2019-20218, CVE- 2019-5827, CVE-2019-9936, CVE-2019-9937, CVE- 2020-11655, CVE-2020-13434, CVE-2020-13630, CVE-2020-13632, CVE-2020-13871:Debian DLA- 2340-1 : sqlite3 security update |
| VRVDR-52722 | 9.8 | DLA-2337-1 | CVE-2018-20852, CVE-2019-10160, CVE-2019- 16056, CVE-2019-20907, CVE-2019-5010, CVE- 2019-9636, CVE-2019-9740, CVE-2019-9947, CVE- 2019-9948: Debian DLA-2337-1 : python2.7 security update |
| VRVDR-52618 | 9.8 | DLA-2323-1 | CVE-2019-18814, CVE-2019-18885, CVE-2019- 20810, CVE-2020-10766, CVE-2020-10767, CVE- 2020-10768, CVE-2020-12655, CVE-2020-12771, CVE-2020-13974, CVE-2020-15393: Debian DLA- 2323-1 : linux-4.19 new package |
| VRVDR-52476 | 5.9 | DLA-2303-1 | CVE-2020-16135: Debian DLA-2303-1 : libssh security update |
| VRVDR-52197 | N/A | N/A | Privilege escalation in "reset ipv6 neighbors" / "reset ip arp" commands |
{: caption="Security vulnerabilities resolved for 1912f" caption-side="bottom"}

## 1912e
{: #1912e}

### Issues Resolved
{: #1912e-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-51957 | Blocker | Modelled copy command incorrectly enforcing ssh-known-host check in 1912e |  
| VRVDR-51952 | Blocker | Group ownership for non ROOT files got changed to ssh @ 1912e |  
| VRVDR-51937 | Blocker | `show interface dataplane dp0xe<x>` displays incorrect speed for copper ports when interface is down |  
| VRVDR-51828 | Major | SIAD ACL: BCM SDK error when deleting ACL configuration |  
| VRVDR-51639 | Critical | Response for "request hardware-diag version" takes much longer with 1912b |  
| VRVDR-51619 | Critical | SIAD ACL: Ensure that rulesets which would exceed the TCAM are rejected |  
| VRVDR-51616 | Critical | Storm Control triggered snmpd warning messages in journal |  
| VRVDR-51543 | Critical | IPsec peers stuck in 'init' state after upgrade from 1801q to 1912d |  
| VRVDR-51539 | Critical | Repeated FAL BCM "L3 Interface" for VSI 0 Syslog |  
| VRVDR-51521 | Critical | NAT64 opd yang file missing required type field in 1908 and 1912 |  
| VRVDR-51518 | Critical | Dataplane performance fails for forward pkts when scatter mode driver is used |  
| VRVDR-51483 | Major | Removing guest configuration fails with scripting error |  
| VRVDR-51385 | Critical | Dataplane Crash in next_hop_list_find_path_using_ifp |  
| VRVDR-51348 | Major | libsnmp-dev built from DANOS/net-snmp is not API compatible with libsnmp-dev from upstream  |
| VRVDR-51345 | Critical | S9500-30XS: 100G Interface LED lit even when disabled |  
| VRVDR-51311 | Blocker | DAS Switch with 1912b seeing low rate of drops vs 1903m |  
| VRVDR-51295 | Critical | Changing speed on interface resets configured MTU to default |  
| VRVDR-51247 | Major | S9500 - missing hw_rev.cfg file |  
| VRVDR-51238 | Major | After broadcast storm, TACACS doesn't recover |  
| VRVDR-51185 | Blocker | Link doesn't come up after swapping 1000BASE-T SFP for 1000BASE-X SFP |  
| VRVDR-51183 | Major | 'FAL neighbor del' log is generated by dataplane for each ARP received for an unknown address |  
| VRVDR-51179 | Critical | live-cd installs should not install all unique state |  
| VRVDR-51148 | Critical | S9500 interface flaps when MTU is modified |  
| VRVDR-51072 | Critical | L3 SIAD router not fragmenting packet size above MTU |  
| VRVDR-51067 | Critical | DPDK VIRTIO driver does not support multiple MAC addresses |  
| VRVDR-51066 | Blocker | 1908g performance hit with vCSR VNF scenario in small, medium and large platforms |  
| VRVDR-51052 | Blocker | Traffic dropped in SIAD when jumbo frames are > 1522 bytes but under defined MTU limit |  
| VRVDR-51008 | Major | When the /var/log partition exists journal files from previous installs are retained but not rotated |  
| VRVDR-50939 | Blocker | BFD session retained in admin down state when interface is disabled |  
| VRVDR-50927 | Critical | `show interface data <port> phy` not working correctly for Operator class users |  
| VRVDR-50920 | Blocker | SIAD - modelled copy with scp target is operationally unusable |  
| VRVDR-50915 | Critical | Error generating /interfaces/backplane-state on SIAD |  
| VRVDR-50874 | Critical | Storm control errors in 1912b |  
| VRVDR-50559 | Critical | Error: /vyatta-cpu-history-client: GetState failure: Traceback |  
| VRVDR-50256 | Blocker | Login fails with recent master images - Error in service module |  
| VRVDR-50075 | Major | Sandbox cleanup fails for deleted TACACS+ user with open sessions |  
| VRVDR-49985 | Major | L3ACL: CLI command and validation for IPv6 ACL rules with fragment option |  
| VRVDR-49959 | Major | Change the yang accepted on SIAD to refuse ACLs specifying 'protocol final' |  
| VRVDR-49808 | Critical | TACACS+ logins of users with "exotic" usernames fail when user isolation is enabled |  
| VRVDR-49502 | Major | Login fails for isolated users whose name contains an underscore |  
| VRVDR-49491 | Critical | User Isolation shared-storage not accessible in Master image after upgrade |  
| VRVDR-49442 | Major | SNMP related syslog messages at wrong log level |  
| VRVDR-49231 | Critical | PPPoE Client - Not re-establishing dropped connection automatically |  
| VRVDR-48438 | Major | LACP causing interface to remain down |  
| VRVDR-47530 | Critical | OSPF scaling: regression script fails bringing up many OSPF neighbors |  
| VRVDR-45369 | Major | show interface dataplane X physical incorrectly reports speed when down |
{: caption="Issues resolved for 1912e" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912e-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-51526 | 7.8 | DSA-4699-1 | CVE-2019-19462, CVE-2019-3016, CVE-2020-0543, CVE-2020-10711, CVE-2020-10732, CVE-2020-10751, CVE-2020-10757, CVE-2020-12114, CVE-2020-12464, CVE-2020-12768, CVE-2020-12770, CVE-2020-13143: Debian DSA-4699-1 : linux - security update |  
| VRVDR-51525 | 7.8 | DSA-4698-1 | CVE-2019-2182, CVE-2019-5108, CVE-2019-19319, CVE-2019-19462, CVE-2019-19768, CVE-2019-20806, CVE-2019-20811, CVE-2020-0543, CVE-2020-2732, CVE-2020-8428, CVE-2020-8647, CVE-2020-8648, CVE-2020-8649, CVE-2020-9383, CVE-2020-10711, CVE-2020-10732, CVE-2020-10751, CVE-2020-10757, CVE-2020-10942, CVE-2020-11494, CVE-2020-11565, CVE-2020-11608, CVE-2020-11609, CVE-2020-11668, CVE-2020-12114, CVE-2020-12464, CVE-2020-12652, CVE-2020-12653, CVE-2020-12654, CVE-2020-12770, CVE-2020-13143: Debian DSA-4698-1: linux – security update |  
| VRVDR-51236 | 8.6 | DSA-4689-1 | CVE-2019-6477, CVE-2020-8616, CVE-2020-8617: Debian DSA-4689-1 : bind9 - security update |  
| VRVDR-51142 | 5.5 | DSA-4685-1 | CVE-2020-3810: Debian DSA-4685-1 : apt - security update |  
| VRVDR-51054 | 6.7 | DSA-4688-1 |  CVE-2020-10722, CVE-2020-10723, CVE-2020-10724: Debian DSA-4688-1 : dpdk - security update |  
| VRVDR-50886 | 8.8 | DSA-4670-1 | CVE-2018-12900, CVE-2018-17000, CVE-2018-17100, CVE-2018-19210, CVE-2019-7663, CVE-2019-14973, CVE-2019-17546 : Debian DSA-4670-1 : tiff - security update |  
| VRVDR-50851 | 7.5 | DSA-4666-1 | CVE-2020-12243: Debian DSA-4666-1 : openldap - security update |  
| VRVDR-50530 | 7.1 | DSA-4647-1 |  CVE-2020-0556: Debian DSA-4647-1 : bluez - security update |  
| VRVDR-50498 | 8.8 | DSA-4646-1 | CVE-2020-10531: Debian DSA-4646-1 : icu - security update |  
| VRVDR-44891 | N/A | N/A | opd doesn't escape input properly when completing commands |
{: caption="Security vulnerabilities resolved for 1912e" caption-side="bottom"}

## 1912a
{: #1912a}

### Issues Resolved
{: #1912a-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-49822 | Critical | Only shows peering with 16 nodes in "show ptp clock 0" |
| VRVDR-49735 | Critical | IPsec RA VPN: default VRF + VFP is blocking traffic which is supposed to be forwarded |
| VRVDR-49734 | Critical | Strongswan VRRP startup check breaks RAVPN server |
| VRVDR-49684 | Blocker | DHCP services within VRF failed to start after enabling secure boot |
| VRVDR-49656| Minor | IDT servo is built without optimization |
| VRVDR-49633 | Critical | tcp_auth_collapse NULL pointer dereference causes kernel panic during SYN flood |
| VRVDR-49631 | Blocker | PTP error message found on UFI06 |
| VRVDR-49630 | Major | IPsec got warning on committing site-2-site tunnel config "Warning: unable to [VPN toggle net.ipv6.conf.intf.disable_xfrm], received error code 65280" |
| VRVDR-49618 | Critical | Servo notifications always using attVrouterPtpServoFailure |
| VRVDR-49584| Minor | GRE over IPsec in transport mode (IKEv1) - responder intermittently replies "no acceptable traffic selectors found" |
| VRVDR-49568 | Critical| Flexware XS and S: kernel panics on start after update to 4.19.93 |
| VRVDR-49513 | Major | "Failed to connect to system bus" error messages |
| VRVDR-49431 | Minor | Use upstream fix for correcting link speed when link is down |
| VRVDR-49427 | Critical | Bridge commit failure when changing both max-age and forwarding-delay |
| VRVDR-49426 | Major | Mellanox-100G: kernel interface shows up even when dataplane is stopped. |
| VRVDR-49417 | Critical | Wrong counts for pkts matching 3-tuple but not 5-tuple |
| VRVDR-49415 | Critical | Python traceback with "show cgnat session detail exclude-inner" |
| VRVDR-49403 | Critical | LACP - vmxnet3 PMD unable to support additional MAC addresses |
| VRVDR-49391  | Major | PTP: disable (by default) logging of the time adjustments by the IDT servo |
| VRVDR-49376 | Critical | PTP: fails to issue clock servo recovery traps |
| VRVDR-49365 | Critical | Remote Syslog broken by source interface status changes |
| VRVDR-49351 | Major | CGNAT: TCP session with only ext -> int traffic doesn't timeout |
| VRVDR-49350 | Critical | CGNAT - PCP session times outer sooner than expected |
| VRVDR-49344 | Critical | Firewall VFP acceptance tests broken by VRVDR-48094 |
| VRVDR-49185 | Blocker | IP Packet Filter not applied at bootup |
| VRVDR-49119 | Major | DUT stops responding following anomolous DHCP-DISCOVER packet |
| VRVDR-49031 | Blocker | RA-VPN Server +VFP+default VRF : IPsec encryption failing on RA-VPN server for traffic destined or originated between end hosts connected behind the RA-VPN server/client |
| VRVDR-49020 | Major | RA VPN: Spoke not forwarding with "ESP: Replay check failed for SPI" logs |
| VRVDR-48944 | Critical | SIAD Dataplane crash when removing Tunnels interface config |
| VRVDR-48761 | Major | J2: packets with too small IP length value forwarded rather than dropped |
| VRVDR-48728 | Blocker | Network link down observed with VM built from vyatta-1908b- amd64-vrouter_20191010T1100-amd64-Build3.14.hybrid.iso |
| VRVDR-48663 | Major | New SSH errors in 1903h make syslog more chatty |
| VRVDR-48593 | Blocker | Mellanox 100G: The dataplane interface is not up after Disable/Enable the interface. |
| VRVDR-48371 | Critical | IPSec RA VPN - Unable to ping spoke after failover |
| VRVDR-48094 | Critical | IPsec RA VPN client/server: v4 traffic not working with when a concrete remote traffic-selector |
| VRVDR-47473 | Blocker | Mellanox-100G:Observing that the interface(one interface out of two)link shows down after conf/deleting the mtu. Hence observing the traffic loss at that time. |
| VRVDR-46719 | Critical | Poor TCP performance in iperf over IPSEC VTI (expect ~600Mbps but measuring ~2Mbps) |
| VRVDR-46641 | Major | IKE control-plane incorrectly assumes that the IPsec dataplane supports ESP Traffic Flow Confidentiality |
| VRVDR-45753 | Minor | Share storage help text for size missing units |
| VRVDR-45071 | Critical | vyatta-security-vpn: vpn-config.pl: l2tp remote-access dhcp-interface "lo.tag;/tmp/bad.sh;echo " / code injection |
| VRVDR-45069 | Critical | vyatta-security-vpn: set security vpn rsa-keys local-key file "/tmp/bad.sh;/tmp/bad.sh" / code injection |
| VRVDR-45068 | Critical | vyatta-security-vpn: s2s tunnel protocol syntax script / code injection |
| VRVDR-45067 | Critical | vyatta-security-vpn: set security vpn ipsec site-to-site peer $CODE / code injection |
| VRVDR-45066 | Critical | vyatta-security-vpn: check_file_in_config passed unsanitized user input / code injection |
| VRVDR-45065 | Critical | vyatta-security-vpn-secrets: code injection |
{: caption="Issues resolved for 1912a" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1912a-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-49728 | N/A | DSA-4609-1 | CVE-2019-15795, CVE-2019-15796: Debian DSA- 4609-1 : python-apt - security update |
| VRVDR-49642 | 9.8 | DSA-4602-1 | CVE-2019-17349, CVE-2019-17350, CVE-2019- 18420, CVE-2019-18421, CVE-2019-18422, CVE- 2019-18423, CVE-2019-18424, CVE-2019-18425, CVE-2019-19577, CVE-2019-19578, CVE-2019- 19579, CVE-2019-19580, CVE-2019-19581, CVE- 2019-19582, CVE-2019-19583, CVE-2018-12207, CVE-2018-12126, CVE-2018-12127, CVE-2018- 12130, CVE-2019-11091, CVE-2019-11135, CVE- 2019-17348, CVE-2019-17347, CVE-2019-17346, CVE-2019-17345, CVE-2019-17344, CVE-2019- 17343, CVE-2019-17342, CVE-2019-17341, CVE- 2019-17340: Debian DSA-4602-1 : xen - security update (MDSUM/RIDL) (MFBDS/RIDL/ZombieLoad) (MLPDS/RIDL) (MSBDS/Fallout) |
| VRVDR-49486 | 5.3 | DSA-4594-1 | CVE-2019-1551: Debian DSA-4594-1 : openssl1.0 - security update |
| VRVDR-49477 | 7.5 | DSA-4591-1 | CVE-2019-19906: Debian DSA-4591-1 : cyrus-sasl2 - security update |
| VRVDR-49450 | 9.8 | DSA-4587-1 | CVE-2019-15845, CVE-2019-16201, CVE-2019- 16254, CVE-2019-16255: Debian DSA-4587-1 : ruby2.3 - security update |
| VRVDR-49132 | 7.8 | DSA-4564-1 | CVE-2018-12207, CVE-2019-0154, CVE-2019-0155, CVE-2019-11135: Debian DSA-4564-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1912a" caption-side="bottom"}

## 1908h
{: #1908h}

### Issues Resolved
{: #1908h-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-51483 | Major | Removing guest configuration fails with scripting error |
| VRVDR-51443 | Major | ipv6 router-advert CLI missing on switch VLAN interfaces |
| VRVDR-51385 | Critical | Dataplane crash in next_hop_list_find_path_using_ifp |
| VRVDR-51295 | Critical | Changing speed on interface resets configured MTU to default |
| VRVDR-51185 | Blocker | Link doesn't come up after swapping 1000BASE-T SFP for 1000BASE-X SFP |
| VRVDR-51183 | Major| 'FAL neighbour del' log is generated by dataplane for each ARP recieved for an unknown address |
| VRVDR-51179 | Critical| live-cd installs should not install all unique state |
| VRVDR-51066 | Blocker| 1908g performance hit with vCSR vnf scenario in Small, Medium and Large Platforms
| VRVDR-51008 | Major | When the /var/log partition exists journal files from previous installs are retained but not rotated |
| VRVDR-50939 | Blocker | BFD session retained in admin down state when interface is disabled |  
| VRVDR-50754 | Critical | Cannot perform H2O Update Capsule update due to missing efivar tool |  
| VRVDR-50705 | Critical | show history & tech support output incorrectly show order of CLI commands executed |  
| VRVDR-50665 | Critical | Permit local user fallback following TACACS+ failure on read-only filesystem |  
| VRVDR-50621 | Critical | Duplicate entries added to dp_event_register() |  
| VRVDR-50614 | Critical | ADI V150 with 100M physical WAN port doesn't show drops with 100M QOS shaper applied |  
| VRVDR-50569 | Blocker | SIAD BFD inter-op issue with Cisco 7609S |  
| VRVDR-50560 | Critical | "show vpn ike secrets" allows operator and members outside the secrets group to display secrets |  
| VRVDR-50306 | Critical | ADI Spirent probe RFC2544 test failure due to small packet loss w/ 100m speed and 50m QoS shaper |  
| VRVDR-50279 | Major | RX error incrementing on the bond1 interface, but no errors on physical interface |  
| VRVDR-50237 | Critical | QoS not working when applied in certain order |  
| VRVDR-49656 | Minor | PTP: IDT servo is built without optimization |  
| VRVDR-49442 | Major | SNMP related syslog messages at wrong log level |  
| VRVDR-49326 | Major | At system login user level operator "show queuing" command does not work |  
| VRVDR-49231 | Critical | PPPoE Client - Not re-establishing dropped connection automatically |  
| VRVDR-48466 | Critical | DNS nslookup query within a routing instance vrf is broken |  
| VRVDR-48337 | Critical | NCS fails to load `vyatta-*system-image YANG` |  
| VRVDR-48203 | Minor | Split IDT servo into separate shared libraries |
{: caption="Issues resolved for 1908h" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908h-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-51236 | 8.6 | DSA-4689-1 | CVE-2019-6477, CVE-2020-8616, CVE-2020-8617: Debian DSA-4689-1 : bind9 - security update |  
| VRVDR-51142 | 5.5 | DSA-4685-1 | CVE-2020-3810: Debian DSA-4685-1 : apt - security update |  
| VRVDR-51054 | 6.7 | DSA-4688-1 | CVE-2020-10722, CVE-2020-10723, CVE-2020- 10724: Debian DSA-4688-1 : dpdk - security update |  
| VRVDR-50530 | 7.1 | DSA-4647-1 | CVE-2020-0556: Debian DSA-4647-1 : bluez - security update |
{: caption="Security vulnerabilities resolved for 1908h" caption-side="bottom"}

## 1908g
{: #1908g}

### Issues Resolved
{: #1908g-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-50563 | Critical | Transport-link / port peering no longer works on xsm, sm and md |
{: caption="Issues resolved for 1908g" caption-side="bottom"}

## 1908f
{: #1908f}

### Issues Resolved
{: #1908f-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-50467 | Critical | Marvell : Sometimes after dataplane crash front panel ports do not come up |  
| VRVDR-50387 | Major | qemu-wrap.py script confusing libvirt/virsh |  
| VRVDR-50376 | Major | Increase max number of clients of dp_events |  
| VRVDR-50293 | Critical | Forwarded cross VRF traffic blackholed when SNAT is applied |  
| VRVDR-50191 | Critical | Packet capture leaking mbufs under heavy load |  
| VRVDR-49991 | Blocker | Enable hardware platform reboot on NMI panic |  
| VRVDR-49951 | Major | SNMP errors during PTP configuration |  
| VRVDR-49750 | Critical | TACACS+ authz sent for user `*` on Bash path completion |  
| VRVDR-49739 | Major | SFlow not sending packets out |  
| VRVDR-49797 | Major | vyatta-openvpn: code injection due to scripts in tmplscripts
| VRVDR-49683 | Critical | 1908d performance issue with QoS seeing significant reduction in performance |  
| VRVDR-49472 | Major | ENTITY-SENSOR-MIB: Incorrect OID values |  
| VRVDR-49470 | Critical | ENTITY-MIB: Missing entPhysicalDescr OID |  
| VRVDR-49316 | Blocker | SNMP entity subagent failed to handle month 12 |  
| VRVDR-48861 | Critical | Vyatta VNF creating extra RX queues |  
| VRVDR-47761 | Minor | Spurious log: LLADDR: NEWNEIGH without link layer address? |  
| VRVDR-45649 | Major | Route Leaking into VRF not working as expected - pings not resolving |
{: caption="Issues resolved for 1908f" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908f-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-50166 | 9.8 | DSA-4633-1 | CVE-2019-5436, CVE-2019-5481, CVE-2019-5482: Debian DSA-4633-1 : curl - security update |  
| VRVDR-50161 | 9.8 | DSA-4632-1 | CVE-2020-8597: Debian DSA-4632-1 : ppp - security update |
{: caption="Security vulnerabilities resolved for 1908f" caption-side="bottom"}

## 1908e
{: #1908e}

### Issues Resolved
{: #1908e-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-49924 | Blocker | Commit failed in IPsec site-to-site configuration |  
| VRVDR-49822 | Critical | Only shows peering with 16 nodes in "show ptp clock 0" |  
| VRVDR-49684 | Blocker | DHCP services within VRF failed to start after enabling secure boot |  
| VRVDR-49633 | Critical | tcp_auth_collapse NULL pointer dereference causes kernel panic during SYN flood |  
| VRVDR-49631 | Blocker | PTP error message found on UFI06 |  
| VRVDR-49584 | Minor | GRE over IPsec in transport mode (IKEv1) - responder intermittently replies "no acceptable traffic selectors found" |  
| VRVDR-49568 | Critical | Flexware XS and S: kernel panics on start after update to 4.19.93 |  
| VRVDR-49459 | Major | Ping monitor may send more packets than specified in "packets" |  
| VRVDR-49439 | Major | Path Monitor does not handle fractional ping loss correctly |  
| VRVDR-48944 | Critical | SIAD dataplane crash when removing tunnels interface config |  
| VRVDR-47869 | Minor | L2TP/IPsec with x.509 authentication fails due to incorrect path to certificates |  
| VRVDR-46719 | Critical | Poor TCP performance in iperf over IPSEC VTI (expect ~600Mbps but measuring ~2Mbps) |  
| VRVDR-45071 | Critical | vyatta-security-vpn: vpn-config.pl: l2tp remote-access dhcp-interface "lo.tag;/tmp/bad.sh;echo " / code injection |  
| VRVDR-45069 | Critical] | vyatta-security-vpn: set security vpn rsa-keys local-key file "/tmp/bad.sh;/tmp/bad.sh" / code injection |  
| VRVDR-45068 | Critical | vyatta-security-vpn: s2s tunnel protocol syntax script / code injection |  
| VRVDR-45067 | Critical | vyatta-security-vpn: set security vpn ipsec site-to-site peer $CODE / code injection |  
| VRVDR-45066 | Critical | vyatta-security-vpn: check_file_in_config passed unsanitized user input / code injection |  
| VRVDR-45065 | Critical | vyatta-security-vpn-secrets: code injection |
{: caption="Issues resolved for 1908e" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908e-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-49834 | 7.8 | DSA-4614-1 | CVE-2019-18634: Debian DSA-4614-1 : sudo - security update |  
| VRVDR-49832 | 9.8 | DSA-4616-1 | CVE-2019-15890, CVE-2020-7039, CVE-2020-1711: Debian DSA-4616-1: qemu – security update |  
| VRVDR-49728 | N/A | DSA-4609-1 | CVE-2019-15795, CVE-2019-15796: Debian DSA- 4609-1 : python-apt - security update |  
| VRVDR-49642 | 9.8 | DSA-4602-1 | CVE-2019-17349, CVE-2019-17350, CVE-2019- 18420, CVE-2019-18421, CVE-2019-18422, CVE- 2019-18423, CVE-2019-18424, CVE-2019-18425, CVE-2019-19577, CVE-2019-19578, CVE-2019- 19579, CVE-2019-19580, CVE-2019-19581, CVE- 2019-19582, CVE-2019-19583, CVE-2018-12207, CVE-2018-12126, CVE-2018-12127, CVE-2018- 12130, CVE-2019-11091, CVE-2019-11135, CVE- 2019-17348, CVE-2019-17347, CVE-2019-17346, CVE-2019-17345, CVE-2019-17344, CVE-2019- 17343, CVE-2019-17342, CVE-2019-17341, CVE- 2019-17340: Debian DSA-4602-1 : xen - security update (MDSUM/RIDL) (MFBDS/RIDL/ZombieLoad) (MLPDS/RIDL) (MSBDS/Fallout) |  
| VRVDR-49132 | 7.8 | DSA-4564-1 | CVE-2018-12207, CVE-2019-0154, CVE-2019-0155, CVE-2019-11135: Debian DSA-4564-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1908e" caption-side="bottom"}

## 1908d
{: #1908d}

### Issues Resolved 
{: #1908d-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-49618 | Critical | Servo notifications always using attVrouterPtpServoFailure |  
| VRVDR-49426 | Major | Mellanox-100G: kernel interface shows up even when dataplane is stopped |  
| VRVDR-49391 | Major | Disable (by default) logging of the time adjustments by the IDT server |  
| VRVDR-49246 | Critical | Flexware stops forwarding pkts over hardware switch after flooding unknown unicasts |  
| VRVDR-49223 | Major | Hardware CPP rate limiter feature accepted packet count not working |  
| VRVDR-49185 | Blocker | IP Packet Filter not applied at bootup |  
| VRVDR-49137 | Major | Syslog rate-limit not respected for above 65000 messages per interval |  
| VRVDR-49020 | Major | RA VPN: Spoke not forwarding with "ESP: Replay check failed for SPI" logs |  
| VRVDR-48992 | Minor | Syslog generates message "Child xxxxx has terminated, reaped by main-loop" at wrong priority |  
| VRVDR-48960 | Critical | SIAD - audit logs with no priority default to syslog level NOTICE and are overly chatty |  
| VRVDR-48892 | Blocker | Ping failure with storm-control & QoS |  
| VRVDR-48891 | Blocker | Dataplane crashed while changing PTP configuration |  
| VRVDR-48850 | Major | PTP: Frequently logging Slave Unavailable/Available msg in the console log |  
| VRVDR-48820 | Critical | PTP: master not tracked correctly across port changes |  
| VRVDR-48728 | Blocker | Network link down observed with VM built from vyatta-1908b- amd64-vrouter_20191010T1100-amd64-Build3.14.hybrid.iso |  
| VRVDR-48720 | Critical | PTP: assert in IDTStackAdaptor_UpdateBestMasterSelection |
| VRVDR-48660 | Critical | No rotation occuring for /var/log/messages |  
| VRVDR-48585 | Major | ICMP Unreachable not returned when decrypted IPsec packet is too large to pass tunnel interface MTU |  
| VRVDR-48461 | Critical | SNMP Not working in 1908a |  
| VRVDR-47203 | Major | 1903d yang package fatal error |  
| VRVDR-47002 | Minor | PTP: network information is not cleared from disabled (skipped) ports during reconfiguration |  
| VRVDR-44104 | Blocker | Creating a switch interface doesn't work with QinQ |
{: caption="Issues resolved for 1908d" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908d-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-49486 | 5.3 | DSA-4594-1 | CVE-2019-1551: Debian DSA-4594-1 : openssl1.0 - security update |  
| VRVDR-49477 | 7.5 | DSA-4591-1 | CVE-2019-19906: Debian DSA-4591-1 : cyrus-sasl2 - security update |  
| VRVDR-49450 | 9.8 | DSA-4587-1 | CVE-2019-15845, CVE-2019-16201, CVE-2019- 16254, CVE-2019-16255: Debian DSA-4587-1 : ruby2.3 - security update |  
| VRVDR-49155 | 7.2 | N/A | CVE-2018-5265: Devices allow remote attackers to execute arbitrary code with admin credentials, because /opt/vyatta/share/vyatta- cfg/templates/system/static-host-mapping/host-name/node.def does not sanitize the 'alias' or 'ips' parameter for shell metacharacters. |  
| VRVDR-48691 | 7.5 | DSA-4544-1 | CVE-2019-16866: Debian DSA-4544-1: unbound - security update |  
| VRVDR-48133 | 8.8 | DSA-4512-1 | CVE-2019-13164, CVE-2019-14378: Debian DSA- 4512-1: qemu – security update |  
| VRVDR-48132 | 7.5 | DSA-4511-1 | CVE-2019-9511, CVE-2019-9513: Debian DSA-4511- 1: nghttp2 – security update |  
| VRVDR-47885 | 8.1 | DSA-4495-1 | CVE-2018-20836, CVE-2019-1125, CVE-2019-1999, CVE-2019-10207, CVE-2019-10638, CVE-2019- 12817, CVE-2019-12984, CVE-2019-13233, CVE- 2019-13631, CVE-2019-13648, CVE-2019-14283, CVE-2019-14284: Debian DSA-4495-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1908d" caption-side="bottom"}

## 1908c
{: #1908c}

### Issues Resolved
{: #1908c-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-49060 | Major | RA VPN: no ESP traffic from Hub to Spoke |  
| VRVDR-49035 | Major | RA VPN: "show vpn ipsec sa" inbound/outbound bytes stats are swapped |  
| VRVDR-48949 | Major | Add output for determining punt-path programming state to tech- support |  
| VRVDR-48893 | Critical | RA VPN: intermittent ICMP loss through HUB due to misprogrammed punt path |  
| VRVDR-48889 | Critical | RA VPN: client IPsec SAs are piling up when make-before-break (client) + reauth-time (server) is configured |  
| VRVDR-48878 | Critical | VPN client log overflow in auth.log |  
| VRVDR-48837 | Critical | Reduce "sending DPD request" loglevel temporarily to reduce logging load |  
| VRVDR-48717 | Major | Resources group address-group address-range entries do not work together with address entries |  
| VRVDR-48672 | Critical | SIAD stops forwarding traffic after 4-5 hours of long duration test |  
| VRVDR-48057 | Minor | Add additional IPSec debug support to tech-support |  
| VRVDR-47596 | Major | NAT used count is showing count larger than total available |
{: caption="Issues resolved for 1908c" caption-side="bottom"}

## 1908b
{: #1908b}

### Issues Resolved
{: #1908b-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-48774 | Minor | PTP: When changing port states the old and new states are backward
| VRVDR-48644 | Minor | add logging for PTP slaves similar to PTP master |  
| VRVDR-48623 | Critical | Assert in IDTStackAdaptor_AddDownlinkTimeStampDifferences |  
| VRVDR-48600 | Critical | Upgrade to 3.0.8 version of UfiSpace's BSP utils |  
| VRVDR-48588 | Critical | PTP fails to create ports when config is removed and reapplied |  
| VRVDR-48567 | Blocker | DPLL3 is not in free-run by default |  
| VRVDR-48560 | Major | Kernel neighbour updates may cause dataplane neighbour to transiently become invalid |  
| VRVDR-48559 | Major | Static ARP entry not always noted in dataplane ARP table |  
| VRVDR-48553 | Blocker | SIAD not updating L3 neighbour entry on MAC change |  
| VRVDR-48542 | Critical | "ipsec sad" was not containing "virtual-feature-point" |  
| VRVDR-48527 | Blocker | SIAD: 1G dataplane interfaces fail to start |  
| VRVDR-48522 | Blocker | MACVLAN interface not receiving packets with programmed MAC address (VRRP with RFC-compatibility) |  
| VRVDR-48519 | Major | Operator in secrets group cannot view redacted secret in "show config" but can in "show config command" |  
| VRVDR-48484 | Blocker | QOS policy dropping all traffic by policer intermittently |  
| VRVDR-48430 | Critical | Issue trap/notification when servo failure is resolved |  
| VRVDR-48415 | Major | OSPF flap to INIT state when changing (add or delete) network statements in OSPF |  
| VRVDR-48408 | Major | Upgrade Insyde phy_alloc module to version 6 |  
| VRVDR-48390 | Minor | Enable some IDT log messages |  
| VRVDR-48384 | Major | Change CGNAT to stop using the NPF interface structure |  
| VRVDR-48372 | Major | Source NAT is using PPPoE Server (default GW) IP and not local PPPoE interface IP |  
| VRVDR-48366 | Major | Some RFC 7951 data test are wrong causing build breakage 1% of the time |  
| VRVDR-48338 | Critical | IDT servo fails to reliably negotiate an higher packets rates with GM |  
| VRVDR-48332 | Major | TACACS+ AAA plugin should restart on DBus failures |  
| VRVDR-48327 | Blocker | HW forwarding failure due to incorrect L2 Rewrite info |  
| VRVDR-48273 | Major | Show sfp info in `show interface dataplane <intf> physical` on Flexware |  
| VRVDR-48243 | Blocker | SIAD Boundary Clock not staying locked to GM when using ECMP paths |  
| VRVDR-48224 | Major | "show cgnat session" with complex filter missing entry |  
| VRVDR-48222 | Major | Isolate configd and opd from plugin panics |  
| VRVDR-48201 | Blocker | Mellanox 100G: Needs improvement for performance of 128, 256 Byte pkts; 64Byte pkt has better performance |  
| VRVDR-48169 | Critical | Mellanox 100G: improve traffic throughput performance |  
| VRVDR-48167 | Critical | 'show tech-support' hangs 'WARNING: terminal is not fully functional' |  
| VRVDR-48157 | Critical | Center LED status for S/M/L is not working as expected |  
| VRVDR-48124 | Critical | Azure: System does not provision ssh key pair |  
| VRVDR-48113 | Major | OSPF not on vtun interface |  
| VRVDR-48108 | Minor | Debug level messages for VRRP seen in journal |  
| VRVDR-48102 | Critical | Fails to operate when the number of interfaces with PTP enabled is scaled up |  
| VRVDR-48098 | Critical | BroadPTP fails to re-mark SIGNALING messages with appropriate DSCP |  
| VRVDR-48093 | Blocker | Missing SFP 'Measured values' on FTLF1518P1BTL optics |  
| VRVDR-48077 | Critical | Update BIOS strings for the Flexware XSmall platform |  
| VRVDR-48033 | Minor | Keepalived: Packet filter picked up an IPv4 advertisement from the local box - dropping it before processing |  
| VRVDR-47990 | Critical | Vyatta vRouter for vNAT usecase(s) in Azure external cloud |  
| VRVDR-47986 | Major | Change CGNAT policy match from a prefix to an address-group |  
| VRVDR-47975 | Critical | TACACS: wall: /dev/pts/2: No such file or directory observed on system reboot |  
| VRVDR-47927 | Major | DPDK - enable selected test apps |  
| VRVDR-47882 | Major | CGNAT logs inconsistent with NAT |  
| VRVDR-47863 | Critical | VRRPv3 VRF IPv6 IPAO: Reconfig of LL vip results in MASTER/MASTER scenario |  
| VRVDR-47842 | Minor | mGRE tunnel is not coming up after making address change at the spoke |  
| VRVDR-47828 | Critical | Crash of keepalived when reloading the daemon (accessing invalid memory) |  
| VRVDR-47816 | Major | NAT statistics not displaying in 'show tech-support save' output |  
| VRVDR-47792 | Major | "clear cgnat session" sometimes errors out after scale test |  
| VRVDR-47747 | Blocker | Dataplane killed by OOM during CGNAT scale test |  
| VRVDR-47710 | Major | NHRP overloads IPsec daemon communication |  
| VRVDR-47701 | Major | CGNAT: Calculate and store RTT times in microseconds |  
| VRVDR-47675 | Major | Sessions are not deleted after deleting CGNAT configurations - stays until original timeout expires in particular scenario |  
| VRVDR-47611 | Major | CGNAT: RPC keyerror if non-existing interface name is used in get- session-information |  
| VRVDR-47601 | Major | VRRP retains MASTER when device is disabled due to license invalid/expired |  
| VRVDR-47472 | Critical | Mellanox-100G: Observing the traffic forwards even after disabling the dataplane interface |  
| VRVDR-47397 | Blocker | PTP logging "STATE: Overall for path '[service ptp instance]'" every 75 seconds |  
| VRVDR-47130 | Major | Send gratuitous ARP on MAC address change |  
| VRVDR-47006 | Major | PTP `show ptp <command>` intermittent fails to return any output |  
| VRVDR-46868 | Blocker | Log the port block allocation logs, subscriber logs and resource constraint logs to a different log other than syslog |  
| VRVDR-46829 | Minor | The reported timestamps in packet traces are not consistent with the actual time and system clock |  
| VRVDR-45781 | Major | 'reset dns forwarding cache routing-instance red' not finding VRF instance |  
| VRVDR-42161 | Minor | tech-support should contain "CLI: coredumpctl info" prefix for COREDUMPS header |
{: caption="Issues resolved for 1908b" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908b-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-48841 | 9.8 | DSA-4550-1 | CVE-2019-18218: Debian DSA-4550-1 : file - security update |  
| VRVDR-48746 | 9.8 | DSA-4547-1 | CVE-2018-10103, CVE-2018-10105, CVE-2018- 14461, CVE-2018-14462, CVE-2018-14463, CVE- 2018-14464, CVE-2018-14465, CVE-2018-14466, CVE-2018-14467,CVE-2018-14468, CVE-2018- 14469, CVE-2018-14470, CVE-2018-14879, CVE- 2018-14880, CVE-2018-14881, CVE-2018-14882, CVE-2018-16227, CVE-2018-16228, CVE-2018- 16229, CVE-2018-16230, CVE-2018-16300, CVE- 2018-16451, CVE-2018-16452, CVE-2019-15166: Debian DSA-4547-1: tcpdump – security update |  
| VRVDR-48652 | N/A | DSA-4543-1 | CVE-2019-14287: Debian DSA-4543-1 : sudo - security update |  
| VRVDR-48502 | 5.3 | DSA-4539-1 | CVE-2019-1547, CVE-2019-1549, CVE-2019-1563: Debian DSA-4539-1 : openssl - security update |  
| VRVDR-48446 | 6.7 | DSA-4535-1 | CVE-2019-5094: Debian DSA-4535-1 : e2fsprogs - security update |  
| VRVDR-48412 | 9.8 | DSA-4531-1 | CVE-2019-14821, CVE-2019-14835, CVE-2019- 15117, CVE-2019-15118, CVE-2019-15902: Debian DSA-4531-1 : linux - security update  |
| VRVDR-47897 | 8.1 | DSA-4497-1 | CVE-2015-8553, CVE-2018-5995, CVE-2018-20836 , CVE-2018-20856, CVE-2019-1125, CVE-2019-3882, CVE-2019-3900, CVE-2019-10207, CVE-2019- 10638, CVE-2019-10639, CVE-2019-13631, CVE- 2019-13648, CVE-2019-14283, CVE-2019-14284: DSA-4497-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1908b" caption-side="bottom"}

## 1908a
{: #1908a}

### Issues Resolved
{: #1908a-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-48082 | Blocker | IPSec RA VPN Client PATH MONITOR not functional |  
| VRVDR-48048 | Critical | SHOW POE command not working for XS/SM Blinkboot |  
| VRVDR-48041 | Major | ptp: support the maximum number of clock ports in BroadPTP |  
| VRVDR-48040 | Major | Upgrade journalbeat to latest 6.x |  
| VRVDR-47974 | Blocker | BFD packets incorrectly scheduled on egress |  
| VRVDR-47947 | Major | Dataplane wrongly logging failure to delete hash table |  
| VRVDR-47934 | Major | QoS: `show policy qos <if-name> class` can display no output |  
| VRVDR-46077 | Major | Build and sign Insyde phy_alloc module |  
| VRVDR-47924 | Major | BGP 'show' output for default-vrf not captured in 'show tech-support' |  
| VRVDR-47908 | Blocker | SIAD displays incorrect serial number in 'show version' |  
| VRVDR-47907 | Blocker | Mellanox-100G: UDP or TCP traffic with 10K flows only reaches 10% line rate |  
| VRVDR-47893 | Critical | SIAD : up-rev Ufi diags to v3.1.7 |  
| VRVDR-47888 | Blocker | IPsec v4 tunnel traffic not working after upgrade to 1908
| VRVDR-47534 | Blocker | ptp: lower servo requirements for lock |  
| VRVDR-47871 | Critical | Permission denied error when attempting to clear bridge interface counters |  
| VRVDR-47870 | Major | Don't disable PTP when port is referenced twice in the configuration and removed |  
| VRVDR-47851 | Minor | Increase the number of clock ports supported |  
| VRVDR-47840 | Blocker | dp0xe1 u/D on Medium after upgrade to 1908 |  
| VRVDR-47824 | Critical | Got bridge sw0 does not exist message with XS running in Blinkboot BIOS mode |  
| VRVDR-47814 | Critical | system ip gratuitous-arp not setting policy |  
| VRVDR-47809 | Major | Configd does not expand grouping defined under a nested augment |  
| VRVDR-47807 | Major | SIAD loses OSPFv3 neighbours periodically for 180s |  
| VRVDR-47624 | Blocker | PTP fails to start with PTP config present at bootup |  
| VRVDR-47481 | Critical | PTP with 2 slaves dataplane crash in bcm_ptp_unicast_slave_subscribe |  
| VRVDR-47391 | Blocker | PTP fails to return to time-locked state after master clock stopped and re-started |  
| VRVDR-47244 | Critical | dataplane crash on restart - no code changes |  
| VRVDR-41129 | Blocker | Journalbeat can't export logs to destination in routing instance |
{: caption="Issues resolved for 1908a" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1908a-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-48074 | 9.8 | DSA-4506-1 | CVE-2018-20815, CVE-2019-13164, CVE-2019- 14378: Debian DSA-4506-1 : qemu - security update |  
| VRVDR-47707 | 7.8 | DSA-4484-1 | CVE-2019-13272: Debian DSA-4484-1: linux security update |
{: caption="Security vulnerabilities resolved for 1908a" caption-side="bottom"}

## 1801zf
{: #1801zf}

### Issues Resolved
{: #1801zf-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-50956 | Critical | VRRP goes into fault state after reboot |
| VRVDR-49924 | Blocker | Commit failed in IPsec site-to-site configuration |
| VRVDR-49760 | Major | VRRP Failover happens when one of the interfaces in bonding group is physically UP |
| VRVDR-49737 | Major | GUI displays wrong/different information than CLI |
| VRVDR-49707 | Major | vyatta-openvpn: code injection due to scripts in tmplscripts |
| VRVDR-49584 | Minor | GRE over IPsec in transport mode (IKEv1) - responder intermittently replies "no acceptable traffic selectors found" |
| VRVDR-49439 | Major | Path Monitor does not handle fractional ping loss correctly |
| VRVDR-48145 | Critical | VRRP - Cores Generated by keepalived |
| VRVDR-48067 | Minor | VPN commit returns "Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], received error code 65280" |
| VRVDR-45071 | Critical | vyatta-security-vpn: vpn-config.pl: l2tp remote-access dhcp-interface "lo.tag;/tmp/bad.sh;echo " / code injection |
| VRVDR-45069 | Critical | vyatta-security-vpn: set security vpn rsa-keys local-key file "/tmp/bad.sh;/tmp/bad.sh" / code injection |
| VRVDR-45068 | Critical | vyatta-security-vpn: s2s tunnel protocol syntax script / code injection |
| VRVDR-45067 | Critical | vyatta-security-vpn: set security vpn ipsec site-to-site peer $CODE / code injection |
| VRVDR-45066 | Critical | vyatta-security-vpn: check_file_in_config passed unsanitized user input / code injection |
| VRVDR-45065 | Critical | vyatta-security-vpn-secrets: code injection |
| VRVDR-40303 | Critical | fsck doesn't seem to be running on boot |
{: caption="Issues resolved for 1801zf" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801zf-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-51236 | 8.6 | DSA-4689-1 | CVE-2019-6477, CVE-2020-8616, CVE-2020-8617: Debian DSA-4689-1 : bind9 - security update |
| VRVDR-51142 | 5.5 | DSA-4685-1 | CVE-2020-3810: Debian DSA-4685-1 : apt - security update |
| VRVDR-50886 | 8.8 | DSA-4670-1 | CVE-2018-12900, CVE-2018-17000, CVE-2018-17100, CVE-2018-19210, CVE-2019-7663, CVE-2019-14973, CVE-2019-17546 : Debian DSA-4670-1 : tiff - security update |
| VRVDR-50851 | 7.5 | DSA-4666-1 | CVE-2020-12243: Debian DSA-4666-1 : openldap - security update |
| VRVDR-50498 | 8.8 | DSA-4646-1 | CVE-2020-10531: Debian DSA-4646-1 : icu - security update |
| VRVDR-50166 | 9.8 | DSA-4633-1 | CVE-2019-5436, CVE-2019-5481, CVE-2019-5482: Debian DSA-4633-1 : curl - security update |
| VRVDR-50161 | 9.8 | DSA-4632-1 | CVE-2020-8597: Debian DSA-4632-1 : ppp - security update |
| VRVDR-49834 | 7.8 | DSA-4614-1 | CVE-2019-18634: Debian DSA-4614-1 : sudo - security update |
| VRVDR-49832 | 9.8 | DSA-4616-1 | CVE-2019-15890, CVE-2020-7039, CVE-2020-1711: Debian DSA-4616-1: qemu – security update |
| VRVDR-49728 | N/A | DSA-4609-1 | CVE-2019-15795, CVE-2019-15796: Debian DSA-4609-1 : python-apt - security update |
| VRVDR-49704 | 8.8 | DSA-4608-1 | CVE-2019-14973, CVE-2019-17546 : Debian DSA 4608-1 : tiff security update |
| VRVDR-49642 | 9.8 | DSA-4602-1 | CVE-2019-17349, CVE-2019-17350, CVE-2019-18420, CVE-2019-18421, CVE-2019-18422, CVE-2019-18423, CVE-2019-18424, CVE-2019-18425, CVE-2019-19577, CVE-2019-19578, CVE-2019-19579, CVE-2019-19580, CVE-2019-19581, CVE-2019-19582, CVE-2019-19583, CVE-2018-12207, CVE-2018-12126, CVE-2018-12127, CVE-2018-12130, CVE-2019-11091, CVE-2019-11135, CVE-2019-17348, CVE-2019-17347, CVE-2019-17346, CVE-2019-17345, CVE-2019-17344, CVE-2019-17343, CVE-2019-17342, CVE-2019-17341, CVE-2019-17340: Debian DSA-4602-1 : xen -security update (MDSUM/RIDL) (MFBDS/RIDL/ZombieLoad)(MLPDS/RIDL) (MSBDS/Fallout) |
| VRVDR-49155 | 7.2 | N/A | CVE-2018-5265 : remote attackers able to execute arbitrary code with admin credentials |
| VRVDR-49132 | 7.8 | DSA-4564-1 | CVE-2018-12207, CVE-2019-0154, CVE-2019-0155, CVE-2019-11135: Debian DSA-4564-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1801zf" caption-side="bottom"}

## 1801ze
{: #1801ze}

### Issues Resolved
{: #1801ze-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-49402 | Blocker | Non-GRE Tunnel intfs fail to come back to up state after toggling state |
| VRVDR-49137 | Major | Syslog rate-limit not respected for above 65000 messages per interval |
| VRVDR-48992 | Minor | Syslog generates message "Child xxxxx has terminated, reaped by main-loop" at wrong priority |
| VRVDR-48719 | Minor | Perl traceback when deleting resources group address-group addressrange |
| VRVDR-48705 | Major | High volume of csync logs causing firewall logs to be suppressed |
| VRVDR-48585 | Major | ICMP Unreachable not returned when decrypted IPsec packet is too large to pass tunnel interface MTU |
| VRVDR-48057 | Minor | Add additional IPsec debug support to tech-support |
| VRVDR-47681 | Critical | Resetting a single VRRP group causes all VRRP groups to reset |
{: caption="Issues resolved for 1801ze" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801ze-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-49486 | 5.3 | DSA-4594-1 | CVE-2019-1551: Debian DSA-4594-1 : openssl1.0 - security update |
| VRVDR-49477 | 7.5 | DSA-4591-1 | CVE-2019-19906: Debian DSA-4591-1 : cyrus-sasl2 - security update |
| VRVDR-49450 | 9.8 | DSA-4587-1 | CVE-2019-15845, CVE-2019-16201, CVE-201916254, CVE-2019-16255: Debian DSA-4587-1 : ruby2.3 - security update |
| VRVDR-48841 | 9.8 | DSA-4550-1 | CVE-2019-18218: Debian DSA-4550-1 : file - security update |
| VRVDR-48691 | 7.5 | DSA-4544-1 | CVE-2019-16866: Debian DSA-4544-1: unbound security update |
| VRVDR-48133 | 8.8 | DSA-4512-1 | CVE-2019-13164, CVE-2019-14378: Debian DSA4512-1: qemu – security update |
| VRVDR-48132 | 7.5 | DSA-4511-1 | CVE-2019-9511, CVE-2019-9513: Debian DSA-45111: nghttp2 – security update |
| VRVDR-47885 | 8.1 | DSA-4495-1 | CVE-2018-20836, CVE-2019-1125, CVE-2019-1999, CVE-2019-10207, CVE-2019-10638, CVE-201912817, CVE-2019-12984, CVE-2019-13233, CVE2019-13631, CVE-2019-13648, CVE-2019-14283, CVE-2019-14284: Debian DSA-4495-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1801ze" caption-side="bottom"}

## 1801zd
{: #1801zd}

### Issues Resolved
{: #1801zd-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-48717 | Major | Resources group address-group address-range entries do not work together with address entries |
| VRVDR-48473 | Minor | Error getting Login User Id |
| VRVDR-47596 | Minor | NAT used count is showing count larger than total available |
| VRVDR-41091 | Minor | Off-by-one error in lcore id in copying rule stats |
{: caption="Issues resolved for 1801zd" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801zd-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-48746 | 9.8 | DSA-4547-1 | CVE-2018-10103, CVE-2018-10105, CVE-201814461, CVE-2018-14462, CVE-2018-14463, CVE2018-14464, CVE-2018-14465, CVE-2018-14466, CVE-2018-14467,CVE-2018-14468, CVE-201814469, CVE-2018-14470, CVE-2018-14879, CVE2018-14880, CVE-2018-14881, CVE-2018-14882, CVE-2018-16227, CVE-2018-16228, CVE-201816229, CVE-2018-16230, CVE-2018-16300, CVE2018-16451, CVE-2018-16452, CVE-2019-15166: Debian DSA-4547-1: tcpdump – security update |
| VRVDR-48652 | N/A | DSA-4543-1 | CVE-2019-14287: Debian DSA-4543-1 : sudo - security update |
| VRVDR-48502 | 5.3 | DSA-4539-1 | CVE-2019-1547, CVE-2019-1549, CVE-2019-1563: Debian DSA-4539-1 : openssl - security update |
| VRVDR-48446 | 6.7 | DSA-4535-1 | CVE-2019-5094: Debian DSA-4535-1 : e2fsprogs - security update |
{: caption="Security vulnerabilities resolved for 1801zd" caption-side="bottom"}

## 1801zc
{: #1801zc}

### Issues Resolved
{: #1801zc-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-48148 | Major | Can't stat /var/run/gre" error seen on deleting erspan tunnel |
| VRVDR-47842 | Minor | mGRE tunnel is not coming up after making address change at the spoke |
| VRVDR-47816 | Major | NAT statistics not displaying in 'show tech-support save' output |
| VRVDR-47601 | Major | VRRP retains MASTER when device is disabled due to license invalid/expired |
| VRVDR-46829 | Minor | The reported timestamps in packet traces are not consistent with the actual time and system clock |
| VRVDR-36174  | Major | A-Time in the output of, 'show vpn ike sa' is always 0 |
{: caption="Issues resolved for 1801zc" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801zc-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-48412 | 9.8 | DSA-4531-1 | CVE-2019-14821, CVE-2019-14835, CVE-201915117, CVE-2019-15118, CVE-2019-15902: Debian DSA-4531-1 : linux - security update |
| VRVDR-47897 | 8.1 | DSA-4497-1 | CVE-2015-8553, CVE-2018-5995, CVE-2018-20836 , CVE-2018-20856, CVE-2019-1125, CVE-2019-3882, CVE-2019-3900, CVE-2019-10207, CVE-201910638, CVE-2019-10639, CVE-2019-13631, CVE-2019-13648, CVE-2019-14283, CVE-2019-14284: DSA-4497-1: Linux – security update |
{: caption="Security vulnerabilities resolved for 1801zc" caption-side="bottom"}

## 1801zb
{: #1801zb}

### Issues Resolved
{: #1801zb-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-47924 | Major | BGP 'show' output for default-vrf not captured in 'show tech-support' |
| VRVDR-47869 | Minor | L2TP/IPsec with x.509 authentication fails due to incorrect path to certificates |
| VRVDR-47711 | Minor | changing 'syslog global facility all level' overwrites individual 'facility <> level' settings |
| VRVDR-47710 | Major | nhrp overloads IPsec daemon communication |
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
{: caption="Issues resolved for 1801zb" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801zb-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-47707 | 7.8 | DSA-4484-1 | CVE-2019-13272: Debian DSA-4484-1: linux security update |
| VRVDR-37993 | 5.0 | N/A | CVE-2013-5211: Network Time Protocol (NTP) Mode 6 Scanner |
{: caption="Security vulnerabilities resolved for 1801zb" caption-side="bottom"}

## 1801za
{: #1801za}

### Issues Resolved
{: #1801za-i}

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
{: caption="Issues resolved for 1801za" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801za-sv}

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
{: caption="Security vulnerabilities resolved for 1801za" caption-side="bottom"}

## 1801z
{: #1801z}

### Issues Resolved
{: #1801z-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-46941 | Minor| Traffic that has SNAT session is filtered using stateless ZBF on return |
| VRVDR-46659 | Major | I350 intfs with mtu 9000 remains stuck at u/D state on upgrade from 1808* to 1903a |
| VRVDR-46623 | Minor | Firewall 'description' logs a perl error on commit when the description has more than one word |
| VRVDR-46549 | Critical | Shell injection privilege escalation/sandbox escape in `show ip route routing-instance <name> variance` command |
| VRVDR-46389 | Major | BGP configuration changes may not take effect if applied after (re)boot |
| VRVDR-45949 | Minor | Netflow generates a NOTICE log for every sample sent when certain non-key fields are configured |
| VRVDR-43169 | Minor | Logging everytime one calls a configd C based API but doesn't supply an error struct is no longer useful |
| VRVDR-41225 | Minor | When configuring interface description, every white space is treated as a new line |
{: caption="Issues resolved for 1801z" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801z-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-46824 | N/A | DSA-4440-1 | CVE-2018-5743, CVE-2018-5745, CVE-2019-6465: Debian DSA-4440-1 : bind9 - security update |
| VRVDR-46603 | 5.3 | DSA-4435-1 | CVE-2019-7317: Debian DSA-4435-1 : libpng1.6 - security update |
| VRVDR-46425 | N/A | DSA-4433-1 | CVE-2019-8320, CVE-2019-8321, CVE-2019-8322, CVE-2019-8323, CVE-2019-8324, CVE-2019-8325: Debian DSA-4433-1 : ruby2.3 - security update |
| VRVDR-46350 | 9.1 | DSA-4431-1 | CVE-2019-3855, CVE-2019-3856, CVE-2019-3857, CVE-2019-3858, CVE-2019-3859, CVE-2019-3860, CVE-2019-3861, CVE-2019-3862, CVE-2019-3863: Debian DSA-4431-1 : libssh2 - security update |
{: caption="Security vulnerabilities resolved for 1801z" caption-side="bottom"}

## 1801y
{: #1801y}

### Issues Resolved
{: #1801y-i}

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
{: caption="Issues resolved for 1801y" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801y-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-46139 | 7.0 | DSA-4428-1 | CVE-2019-3842: Debian DSA-4428-1 : systemd - security update |
| VRVDR-46087 | N/A | DSA-4425-1 | CVE-2019-5953: Debian DSA-4425-1 : wget - security update |
| VRVDR-45897 | 7.5 | DSA-4416-1 | CVE-2019-5716, CVE-2019-5717, CVE-2019-5718, CVE-2019-5719, CVE-2019-9208, CVE-2019-9209, CVE-2019-9214: Debian DSA-4416-1 : wireshark - security update |
| VRVDR-45553 | 5.9 | DSA-4400-1 | CVE-2019-1559: Debian DSA-4400-1 : openssl1.0 - security update |
| VRVDR-45549 | 6.5 | DSA-4397-1 | CVE-2019-3824: Debian DSA-4397-1 : ldb - security update |
| VRVDR-45347 | 6.8  | DSA-4387-1 | CVE-2018-20685, CVE-2019-6109, CVE-2019-6111: Debian DSA-4387-1 : openssh - security update |
{: caption="Security vulnerabilities resolved for 1801y" caption-side="bottom"}

The following commands have been deprecated from this patch and are no longer available:
   • `policy route pbr <name> rule <rule-number> application name <name>`
   • `policy route pbr <name> rule <rule-number> application type <type>`
   • `policy qos name <policy-name> shaper class <class-id> match <match-name> application name <name>`
   • `policy qos name <policy-name> shaper class <class-id> match <match-name> application type <type>`
   • `security application firewall name <name> rule <rule-number> name <app-name>`

Running any of the above commands will result with the error message “This feature is disabled.”

## 1801w
{: #1801w}

### Issues Resolved 
{: #1801w-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-45672 | Critical | The RSA private key at /opt/vyatta/etc/config/ipsec.d/rsakeys/localhost.key has wrong permissions |
| VRVDR-45591 | Critical | Interface IP MTU change not taking effect for Intel x710 NICs |
| VRVDR-45466 | Minor | IPv6 address not abbreviated when config is loaded via PXE boot causing config-sync issues |
| VRVDR-45414 | Minor | Vyatta-cpu-shield fails to start and throws `OSError:[Errno 22] Invalid argument` for various cores on a two socket system |
{: caption="Issues resolved for 1801w" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801w-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-45253 | 7.5 | DSA-4375-1 | CVE-2019-3813: Debian DSA 4375-1: spice - security update |
| VRVDR-44922 | 7.5 | DSA-4355-1 | CVE-2018-0732, CVE-2018-0734, CVE-2018-0737, CVE-2018-5407: Debian DSA-4355-1 : openssl1.0 - security update |
| VRVDR-43936 | 7.5 | DSA-4309-1 | CVE-2018-17540: Debian DSA-4309-1 : strongswan - security update |
{: caption="Security vulnerabilities resolved for 1801w" caption-side="bottom"}

## 1801v
{: #1801v}

### Issues Resolved
{: #1801v-i}

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
{: caption="Issues resolved for 1801v" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801v-sv}

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
{: caption="Security vulnerabilities resolved for 1801v" caption-side="bottom"}

## 1801u
{: #1801u}

### Issues Resolved
{: #1801u-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44406 | Critical | With multiple subnet on same VIF low rate of transit traffic observed when compared to 5400 performance |
| VRVDR-44253 | Minor | MSS clamping on bonding interface stops functioning after reboot |
{: caption="Issues resolved for 1801u" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801u-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-44277 | N/A | DSA-4332-1 | CVE-2018-16395, CVE-2018-16396: Debian DSA-4332-1 : ruby2.3 - security update |
| VRVDR-44276 | N/A | DSA-4331-1 | CVE-2018-16839, CVE-2018-16842: Debian DSA-4331-1 : curl - security update |
{: caption="Security vulnerabilities resolved for 1801u" caption-side="bottom"}

## 1801t
{: #1801t}

### Issues Resolved
{: #1801t-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44172 | Blocker | Error “interfaces [openvpn] is not valid” reported in mss-clamp tests |
| VRVDR-43969 | Minor | Vyatta 18.x GUI reports the wrong status check memory usage |
| VRVDR-43847  | Major | Slow throughput for TCP conversations on bonding interface |
{: caption="Issues resolved for 1801t" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801t-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-43842 | N/A | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – security update |
{: caption="Security vulnerabilities resolved for 1801t" caption-side="bottom"}

## 1801s
{: #1801s}

### Issues Resolved
{: #1801s-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-44041 | Major | SNMP ifDescr oid slow response time |
{: caption="Issues resolved for 1801s" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801s-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-44074| 9.1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – security update|
| VRVDR-44054 | 8.8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – security update |
| VRVDR-44038 | N/A | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – security update |
| VRVDR-44033 | N/A | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – security update |
|VRVDR-43922 | 7.8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – security update |
| VRVDR-43908 | 9.8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - security update |
| VRVDR-43884 | 7.5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - security update |
{: caption="Security vulnerabilities resolved for 1801s" caption-side="bottom"}

## 1801r
{: #1801r}

### Issues Resolved
{: #1801r-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-43738 | Major | ICMP Unreachable packets returned through SNAT session are not delivered |
| VRVDR-43538 | Major | Receive oversize errors on bondinginterface |
| VRVDR-43519 | Major | Vyatta-keepalived is running with no config present |
| VRVDR-43517 | Major | Traffic fails when endpoint of VFP/Policy-based IPsec resides on the vRouter itself |
| VRVDR-43477 | Major | Committing the IPsec VPN configuration returns the warning “Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], received error code 65280 |
| VRVDR-43379 | Minor | NAT statistics incorrectly shown |
{: caption="Issues resolved for 1801r" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801r-sv}

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
{: caption="Security vulnerabilities resolved for 1801r" caption-side="bottom"}

## 1801q
{: #1801q}

### Issues Resolved
{: #1801q-i}

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
{: caption="Issues resolved for 1801q" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801q-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-43288 | 5.6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – Linux security update |
| VRVDR-43111 | N/A | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – Linux security update |
{: caption="Security vulnerabilities resolved for 1801q" caption-side="bottom"}

## 1801n
{: #1801n}

### Issues Resolved
{: #1801n-i}

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
{: caption="Issues resolved for 1801n" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801n-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-42505 | N/A | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - security update |
| VRVDR-42427 | N/A | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - security update |
| VRVDR-42383 | N/A | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - security update |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – security update |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – security update |
{: caption="Security vulnerabilities resolved for 1801n" caption-side="bottom"}

## 1801m
{: #1801m}

Released June 15, 2018.

### Issues Resolved
{: #1801m-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42256 | Critical | No outbound traffic if latest established CHILD_SA gets deleted |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-42018 | Minor | When “restart vpn” is run, an “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” error is thrown |
| VRVDR-42017 | Minor | When “show vpn ipsec sa” is running on VRRP backup, “ConnectionRefusedError” error is thrown related to vyatta-op-vpn- ipsec-vici line 563 |
{: caption="Issues resolved for 1801m" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801m-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – security update |
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – security update |
{: caption="Security vulnerabilities resolved for 1801m" caption-side="bottom"}

## 1801k
{: #1801k}

Released June 8, 2018.

### Issues Resolved
{: #1801k-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-42084 | Blocker | NAT sessions linked to VFP interfaces for PB IPsec tunnels are not being created for packets that arrive on the router even though the router is configured to do so |
| VRVDR-41944 | Major | After VRRP fail-over some VTI tunnels fail to re-establish until a “vpn restart” or peer reset is issued |
| VRVDR-41906 | Major | PMTU discovery fails as ICMP type 3 scode 4 messages are sent out from wrong source IP |
| VRVDR-41558 | Major | The reported timestamps in packet traces are not consistent with the actual time and system clock |
| VRVDR-41469 | Major | One interface link down – bond is not carrying traffic |
| VRVDR-41420 | Major | LACP bonding state/link “u/D” with mode change active-backup to LACP |
| VRVDR-41313 | Critical | IPsec – VTI interface instability |
{: caption="Issues resolved for 1801k" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801k-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – security update |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639: Speculative execution, variant 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – security update |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – security update |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – security update |
{: caption="Security vulnerabilities resolved for 1801k" caption-side="bottom"}

## 1801j
{: #1801j}

Released May 18, 2018

### Issues Resolved
{: #1801j-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41481 | Minor | VRRP on bond interface does not send VRRP advertisement |
| VRVDR-39863 | Major | VRRP fails over when customer removes routing-instance with GRE associated and tunnel local-address is part of VRRP |
| VRVDR-27018 | Critical | Running configuration file is globally readable |
{: caption="Issues resolved for 1801j" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801j-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – security update |
{: caption="Security vulnerabilities resolved for 1801j" caption-side="bottom"}

## 1801h
{: #1801h}

Released May 11, 2018.

### Issues Resolved
{: #1801h-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41664 | Critical | Dataplane drops MTU sized ESP packets |
| VRVDR-41536 | Minor | Dnsmasq service start-init limit hit when adding more than 4 static host entries if dns forwarding is enabled |
{: caption="Issues resolved for 1801h" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801h-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7.8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux security update |
{: caption="Security vulnerabilities resolved for 1801h" caption-side="bottom"}

## 1801g
{: #1801g}

Released May 4, 2018.

### Issues Resolved
{: #1801g-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-41620 | Major | vTI interface traffic stops sending traffic after new vIF is added |
| VRVDR-40965 | Major | Bonding does not recover after a data plane crash |
{: caption="Issues resolved for 1801g" caption-side="bottom"}

## 1801f
{: #1801f}

Released April 23, 2018

### Issues Resolved
{: #1801f-i}

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
{: caption="Issues resolved for 1801f" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801f-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – security update |
| VRVDR- 41331 | 6.5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – security update
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – security update |
| VRVDR- 41215 | 6.1 |CVE-2018-1059 | CVE-2018-1059 – DPDK vhost out of bound host memory access from VM guests |
{: caption="Security vulnerabilities resolved for 1801f" caption-side="bottom"}

## 1801e
{: #1801e}

Released March 28, 2018.

### Issues Resolved
{: #1801e-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-39985 | Minor | TCP DF packets larger than GRE tunnel MTU are dropped with no ICMP fragmentation needed returned |
| VRVDR-41088 | Critical | Extended (4 byte) ASN not represented internally as unsigned type |
| VRVDR-40988 | Critical | Vhost not starting when used with certain number of interfaces |
| VRVDR-40927 | Critical | DNAT: SDP in SIP 200 OK not translated when it follows a 183 response |
| VRVDR-40920 | Major | With 127.0.0.1 as listen-address snmpd does not start |
| VRVDR-40920 | Critical | ARP doesn’t work over bonded SR-IOV interface |
| VRVDR-40294 | Major | Dataplane doesn’t restore previous queues after slave is removed from bonding group |
{: caption="Issues resolved for 1801e" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801e-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/A | DSA-4140-1 | DSA 4140-1: libvorbis security update |
{: caption="Security vulnerabilities resolved for 1801e" caption-side="bottom"}

## 1801d
{: #1801d}

Released March 8, 2018.

### Issues Resolved
{: #1801d-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40940 | Major | Data plane crash related to NAT/firewall |
| VRVDR-40886 | Major | Combining `icmp name <value>` with a number of other configuration for the rule will cause firewall to not load |
| VRVDR-39879 | Major | Configuring bonding for jumbo frames fails |
{: caption="Issues resolved for 1801d" caption-side="bottom"}

### Security Vulnerabilities Resolved
{: #1801d-sv}

| Issue Number | CVSS score | Advisory | Summary |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1 | CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – security update |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch target injection / CVE-2017-5715 / Spectre, aka variant #2 |
{: caption="Security vulnerabilities resolved for 1801d" caption-side="bottom"}

## 1801c
{: #1801c}

Released March 7, 2018.

### Issues Resolved
{: #1801c-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40281 | Major | After upgrading from 5.2 to more recent version error “-vbash: show: command not found” in operation mode |
{: caption="Issues resolved for 1801c" caption-side="bottom"}

## 1801b
{: #1801b}

Released February 21, 2018.

### Issues Resolved
{: #1801b-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40622 | Major | Cloud-init images fail to detect correctly if IP address has been obtained from DHCP server |
| VRVDR-40613 | Critical | Bond interface does not come up if one of the physical links are down |
| VRVDR-40328 | Major | Cloud-init images take a long time to boot |
{: caption="Issues resolved for 1801b" caption-side="bottom"}

## 1801a
{: #1801a}

Released February 7, 2018.

### Issues Resolved
{: #1801a-i}

| Issue Number | Priority | Summary |
| --- | --- | --- |
| VRVDR-40324 | Major | Load averages exceed 1.0 with no load on router with bonding interface |
{: caption="Issues resolved for 1801a" caption-side="bottom"}
