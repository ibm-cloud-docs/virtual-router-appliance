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

# AT&T Vyatta 5600 vRouter Software-Patches

**Datum: 2. November 2018**

In diesem Dokument sind die Patches (Programmkorrekturen) für die derzeit unterstützten Versionen des Netzbetriebssystems Vyatta 5600 aufgelistet. Programmkorrekturen für Version 5.2 und ältere Versionen werden unter Verwendung einer S-Nummer benannt. Ab Version 17.1 werden Programmkorrekturen mit einem Kleinbuchstaben mit Ausnahme von “i”, “o”, “l” und “x” benannt.

Wenn mehrere CVE-Nummern von einer einzigen Aktualisierung betroffen sind, wird die höchste CVSS-Bewertung aufgelistet. 

## 1801t

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-44172 | Blocker | Fehler “Schnittstelle [openvpn] ist ungültig” berichtet in mss-clamp-Tests. |
| VRVDR-43969 | Niedrig | Vyatta 18.x GUI berichtet falsche Speicherbelegung bei Statusprüfung|
| VRVDR-43847  | Hoch | Geringer Durchsatz für TCP-Datenaustausch bei Bonding-Schnittstelle|

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-43842 | Nicht zutreffend | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – Sicherheitsupdate |

## 1801s

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-44041 | Hoch | SNMP ifDescr oid lange Antwortzeit |
 
**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-44074| 9.1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – Sicherheitsupdate|
| VRVDR-44054 | 8.8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – Sicherheitsupdate|
| VRVDR-44038 | nicht zutreffend | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – Sicherheitsupdate|
| VRVDR-44033 | nicht zutreffend | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – Sicherheitsupdate| 
|VRVDR-43922 | 7.8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – Sicherheitsupdate|
| VRVDR-43908 | 9.8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - Sicherheitsupdate|
| VRVDR-43884 | 7.5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - Sicherheitsupdate|

## 1801r

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-43738 | Hoch | ICMP Nicht erreichbare über SNAT-Sitzung zurückgegebene Pakete wurden nicht übergeben. |
| VRVDR-43538 | Hoch | Übergrößenfehler bei Bonding-Schnittstelle empfangen | 
| VRVDR-43519 | Hoch | Vyatta-keepalived wird ohne vorhandene Konfiguration ausgeführt. | 
| VRVDR-43517 | Hoch | Datenverkehr schlägt fehl, wenn Endpunkt von VFP/Policy-basiertem IPsec sich auf dem vRouter selbst befindet. | 
| VRVDR-43477 | Hoch | Beim Festschreiben der IPsec-VPN-Konfiguration wird die folgende Warnung zurückgegeben: “Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], received error code 65280 |
| VRVDR-43379 | Niedrig | NAT-Statistikdaten falsch angezeigt |
 
**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-43837 | 7.5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1: libarchive-zip-perl –Sicherheitsupdate |
| VRVDR-43693 | Nicht zutreffend | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1: mgetty –Sicherheitsupdate | 
| VRVDR-43578 | Nicht zutreffend | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1: curl -Sicherheitsupdate |
| VRVDR-43326 | Nicht zutreffend | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1: openssh -Sicherheitsupdate | 
| VRVDR-43198 | Nicht zutreffend | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1: linux Sicherheitsupdate (FragmentSmack) |
| VRVDR-43110 | Nicht zutreffend | DSA-4265-1 | Debian DSA-4265-1 : xml-security-c -Sicherheitsupdate | 
| VRVDR-43057 | Nicht zutreffend | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : libmspack -Sicherheitsupdate | 
| VRVDR-43026 | 9.8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 -Sicherheitsupdate VRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 :fuse -Sicherheitsupdate |

## 1801q

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-43531 | Hoch |Boot mit 1801p führt zu Kernelpanik innerhalb von ca. 40 Sekunden. |
| VRVDR-43104 | Kritisch | Fake Gratuitous ARP über DHCP-Netz, wenn IPsec aktiviert ist |
| VRVDR-41531 | Hoch | IPsec versucht nach dem Aufheben der Bindung weiter, die VFP-Schnittstelle zu verwenden. |
| VRVDR-43157 | Niedrig | Wenn der Tunnel zurückweist, ist die SNMP-Alarmnachricht nicht ordnungsgemäß generiert worden. |
| VRVDR-43114 | Kritisch | Ein Router in einem HA-Paar mit einer höheren Priorität als sein Peer, beachtet seine eigene Konfiguration “preemt false” beim Warmstart nicht und wird sofort nach dem Warmstart Master. |
| VRVDR-42826 | Niedrig | Mit der fernen ID “0.0.0.0” schlägt die Peerfestlegung fehl, weil ein pre-shared-key nicht übereinstimmt. |
| VRVDR-42774 | Kritisch | X710-Treiber (i40e) sendet Ablaufsteuerungsframes mit hoher Geschwindigkeit. |
| VRVDR-42635 | Niedrig | Richtlinienänderung der route-map bei BGP-Umverteilung wird nicht wirksam. |
| VRVDR-42620 | Niedrig | Während der Tunnel aktiv zu sein scheint, löst Vyatta-ike-sa-daemon folgenden Fehler aus: “Command failed: establishing CHILD_SA passthrough-peer” |
| VRVDR-42483 | Niedrig | TACACS-Authentifizierung fehlgeschlagen |
| VRVDR-42283 | Hoch | VRRP-Statusänderung für alle Schnittstellen zu FAULT, wenn eine vif-Schnittstellen-IP gelöscht wird. |
| VRVDR-42244 | Niedrig | Flow-monitoring exportiert nur 1000 Beispiele zum Collector. |
| VRVDR-42114 | Kritisch | HTTPS-Service darf TLSv1 NICHT offenlegen. |
| VRVDR-41829 | Hoch | Kernspeicherauszug für Datenebene wird erstellt, bis System bei SIP-ALG-Soaktest nicht mehr antwortet. |
| VRVR-41683 | Blocker | DNS-Namensserveradresse, die über VRF erlernt wurde, wurde nicht konsistent erkannt. |
| VRVDR-41628 | Niedrig | Route/Präfix von router-advertisement in Kernel und Datenebene aktiv aber von RIB ignoriert |
 
**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-43288 | 5.6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – Linux-Sicherheitsupdate |
| VRVDR-43111 | Nicht zutreffend | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – Linux-Sicherheitsupdate |

## 1801n

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-42588 | Niedrig | Sensitive Konfiguration des Routing-Protokolls versehentlich in Systemprotokoll veröffentlicht |
| VRVDR-42566 | Kritisch | Nach einem Upgrade von 17.2.0h auf 1801m traten einen Tag später zahlreiche Warmstarts auf beiden HA-Mitgliedern auf. |
| VRVDR-42490 | Hoch | VTI-IPSEC IKE SAs schlagen ca. eine Minute nach dem VRRP-Übergang fehl. |
| VRVDR-42335 | Hoch | IPSEC: Verhalten der remote-id “hostname” ändert sich von 5400 zu 5600. |
| VRVDR-42264 | Kritisch | Keine Konnektivität über SIT-Tunnel – “kernel: sit: non-ECT from 0.0.0.0 with TOS=0xd” |
| VRVDR-41957 | Niedrig | Bidirektional NAT-Pakete sind zu groß für GRE und die Rückgabe von ICMP Type 3 Code 4 schlägt fehl. |
| VRVDR-40283 | Hoch | Konfigurationsänderungen generieren zahlreiche Protokollnachrichten. |
| VRVDR-39773 | Hoch | Verwendung einer route-map mit dem BGP-Befehl vrrp-failover kann dazu führen, dass alle Präfixe entfernt werden. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-42505 | Nicht zutreffend | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - Sicherheitsupdate |
| VRVDR-42427 | Nicht zutreffend | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - Sicherheitsupdate |
| VRVDR-42383 | Nicht zutreffend | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - Sicherheitsupdate |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – Sicherheitsupdate |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – Sicherheitsupdate |

## 5.2R6S12

Freigabe am 21. Juni 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-42084 | Blocker | Vfp-Schnittstelle in “show dataplane route” als “non-dataplane interface” markiert, wenn nat/ipsec-Konfiguration erneut angewendet wird (re-applied). |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – Sicherheitsupdate |
| VRVDR-42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – Sicherheitsupdate |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – Sicherheitsupdate |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – Sicherheitsupdate (Spectre) |

## 1801m

Freigabe am 15. Juni 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-42256 | Kritisch | Kein abgehender Datenverkehr, wenn zuletzt eingerichtete CHILD_SA gelöscht wird |
| VRVDR-42084 | Blocker | NAT-Sitzungen mit VFP-Schnittstellen verbunden, da PB-IPsec-Tunnel nicht für Pakete erstellt werden, die auf dem Router eintreffen, auch wenn der Router dafür konfiguriert wurde. |
| VRVDR-42018 | Niedrig | Wenn “restart vpn” ausgeführt wird, wird der Fehler “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” ausgegeben. |
| VRVDR-42017 | Niedrig | Wenn “show vpn ipsec sa” auf einem VRRP-Backup ausgeführt wird, wird der Fehler “ConnectionRefusedError” bezüglich vyatta-op-vpn- ipsec-vici Zeile 563 ausgelöst. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – Sicherheitsupdate |
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – Sicherheitsupdate |

## 5.2R6S11

Freigabe am 11. Juni 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-42109 | Kritisch | Nur 1 ICMP Antwortpaket wurde mit SNAT+FW auf 5.2R6S7 empfangen. |
| VRVDR-42084 | Blocker | NAT-Sitzungen mit VFP-Schnittstellen verbunden, da PB-IPsec-Tunnel nicht für Pakete erstellt werden, die auf dem Router eintreffen, auch wenn der Router dafür konfiguriert wurde. |
| VRVDR-42027 | Hoch | SFLOW verwendet falsche Eingabe ifIndex. |
| VRVDR-41558 | Hoch | Die berichteten Zeitmarken in Pakettraces sind nicht mit der aktuellen Zeit und der Systemuhr konsistent. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – Sicherheitsupdate |
| VRVDR- 42013 | Nicht zutreffend | DSA-4210-1 | CVE-2018-3639: spekulative Ausführung, Variante 4: spekulativer Speicherbypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – Sicherheitsupdate |
| VRVDR- 41946 | Nicht zutreffend | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – Sicherheitsupdate |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – Sicherheitsupdate |

## 1801k

Freigabe am 8. Juni 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-42084 | Blocker | NAT-Sitzungen mit VFP-Schnittstellen verbunden, da PB-IPsec-Tunnel nicht für Pakete erstellt werden, die auf dem Router eintreffen, auch wenn der Router dafür konfiguriert wurde. |
| VRVDR-41944 | Hoch | Nach einer VRRP-Funktionsübernahme (fail-over) können einige VTI-Tunnel nicht erneut erstellt (re-establish) werden, bis ein “vpn restart” oder eine Peer-Zurücksetzung erfolgt. |
| VRVDR-41906 | Hoch | PMTU-Erkennung schlägt fehl, wenn ICMP-Nachrichten (Typ 3 scode 4) von der falschen Quellen-IP gesendet werden. |
| VRVDR-41558 | Hoch | Die berichteten Zeitmarken in Pakettraces sind nicht mit der aktuellen Zeit und der Systemuhr konsistent. |
| VRVDR-41469 | Hoch | Ein Schnittstellenlink ist inaktiv – Bond führt keinen Datenverkehr aus. |
| VRVDR-41420 | Hoch | LACP-Bonding Status/Link “u/D” mit Modusänderung active-backup auf LACP |
| VRVDR-41313 | Kritisch | IPsec – Instabilität der VTI-Schnittstelle |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – Sicherheitsupdate |
| VRVDR- 42013 | Nicht zutreffend | DSA-4210-1 | CVE-2018-3639: Spekulative Ausführung, Variante 4: spekulativer Geschäftsbypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – Sicherheitsupdate |
| VRVDR- 41946 | Nicht zutreffend | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – Sicherheitsupdate |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – Sicherheitsupdate |

## 1801j

Freigabe am 18. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41481 | Niedrig | VRRP auf Bondschnittstelle sendet keine VRRP-Mitteilungen. |
| VRVDR-39863 | Hoch | VRRP übernimmt die Funktion, wenn der Kunde die Routinginstanz (routing-instance) mit zugehörigem GRE und der lokalen Tunneladresse (local-address) als Teil der VRRP entfernt. |
| VRVDR-27018 | Kritisch | Aktive Konfigurationsdatei ist global lesbar. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1: linux – Sicherheitsupdate |

## 5.2R6S10

Freigabe am 17. Mai 2018

**Gelöste Probleme**
| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41543 | Hoch | “update config-sync” berichtet Fehler, wenn der umgekehrte Schrägstrich “\” in Konfigurationsbeschreibungen verwendet wird.
| VRVDR-27018 | Kritisch | Aktive Konfigurationsdatei ist global lesbar. |

## 1801h

Freigabe am 11. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41664 | Kritisch | Datenebene löscht ESP-Pakte mit MTU-Größe. |
| VRVDR-41536 | Niedrig | Dnsmasq-Service start-init-Begrenzung beim Hinzufügen von mehr als vier statischen Hosteinträgen missachtet, wenn dns-Weiterleitung aktiv ist. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7.8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: Linux-Sicherheitsupdate |

## 5.2R6S

Freigabe am 8. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40803 | Niedrig | VIF-Schnittstellen nach Warmstart nicht in Ausgabe “show vrrp” vorhanden. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – Sicherheitsupdate |

## 1801g

Freigabe am 4. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41620 | Hoch | vTI-Schnittstellendatenverkehr stoppt das Senden von Daten nach Hinzufügen eines neuen vIF. |
| VRVDR-40965 | Hoch | Bonding wird nach einem Absturz der Datenebene nicht wieder hergestellt. |

## 1801f

Freigabe am 23. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41537 | Niedrig | Ping funktioniert nicht über den IPsec-Tunnel auf 1801d. |
| VRVDR-41283 | Niedrig | Configd stoppt die Verarbeitung statischer Routen während des Bootens, wenn die Konfiguration inaktivierte statische Routen aufweist. |
| VRVDR-41266 | Hoch | Statische Route die an VRF weiterleitet, überträgt nach dem Warmstart keinen Datenverkehr über mGRE-Tunnel. |
| VRVDR-41255 | Hoch | Wenn der Slave inaktiv wird, dauert es bis zu 60 Sek. bis der Master-Link-State das widerspiegelt. |
| VRVDR-41252 | Hoch | Die Löschregel wird bei ungebundenem VTI in der Zonenrichtlinie abhängig von der Festschreibungsreihenfolge der Zonenregeln übergangen. |
| VRVDR-41221 | Kritisch | Upgrade der vRouter von 1801b auf 1801c und auf 1801d mit 10% Fehlerrate |
| VRVDR-40967 | Hoch | Inaktivierung der IPv6-Weiterleitung verhindert die Weiterleitung von IPv4-Paketen mit VTI als Quelle. |
| VRVDR-40858 | Hoch | VTI-Schnittstelle zeigt MTU 1428 und verursacht TCP-PMTU-Probleme. |
| VRVDR-40857 | Kritisch | Vhost-Bridge startet für getagtes VLAN mit Schnittstellennamen einer bestimmten Länge nicht. |
| VRVDR-40803 | Niedrig | VIF-Schnittstellen nach Warmstart in Ausgabe “show vrrp” nicht vorhanden. |
| VRVDR-40644 | Hoch | IKEv1: QUICK_MODE erneute Übertragungen (re-transmits) werden nicht ordnungsgemäß ausgeführt. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – Sicherheitsupdate |
| VRVDR- 41331 | 6.5 | DSA-4158-1 | CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – Sicherheitsupdate
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – Sicherheitsupdate |
| VRVDR- 41215 | 6.1 |CVE-2018-1059 | CVE-2018-1059 – DPDK vhost außerhalb des gültigen Bereichs des Hostspeicherzugriffs durch VM-Gäste |

##5.2R6S8
Freigabe am 16. Mai 2018

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-41283 | Niedrig | Configd stoppt die Verarbeitung statischer Routen während des Bootens, wenn die Konfiguration inaktivierte, statische Routen aufweist. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – Sicherheitsupdate 

##1801e
Freigabe am 28. März 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-39985 | Niedrig | TCP-DF-Pakete, die größer als GRE-Tunnel MTU sind, werden gelöscht, ohne dass eine ICMP-Fragmentierung zurückgegeben werden muss. | 
| VRVDR-41088 | Kritisch | Erweitertes ASN (4 Byte) intern nicht als nicht signierter Typ dargestellt. |
| VRVDR-40988 | Kritisch | Vhost startet nicht, wenn er mit einer bestimmten Anzahl an Schnittstellen verwendet wird. |
| VRVDR-40927 | Kritisch | DNAT: SDP in SIP 200 OK nicht übersetzt, wenn nach Antwort 183 |
| VRVDR-40920 | Hoch | Mit 127.0.0.1 als Empfangsadresse startet snmpd nicht. |
| VRVDR-40920 | Kritisch | ARP funktioniert nicht über SR-IOV-Schnittstelle mit Bonding. |
| VRVDR-40294 | Hoch | Datenebene stellt vorherige Warteschlangen nach Entfernen des Slave aus der Bondinggruppe nicht wieder her. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 41172 | Nicht zutreffend | DSA-4140-1 | DSA 4140-1: libvorbis Sicherheitsupdate |

##5.2R6S7
Freigabe am 15. März 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-38801 | Hoch | Empfangen eines Multisegmentpakets über IPsec VTI führt zum Absturz der Bonding-Schnittstelle. 

##5.2R6S6
Freigabe am 12. März 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40281 | Hoch | Nach Upgrade von 5.2 auf neuere Version: Fehler “vbash: show: command not found” im Betriebsmodus |
| VRVDR-40135 | Hoch | Spanning Tree-Pakete werden am Bridge-Port einer VIF-Schnittstelle nicht empfangen. |
| VRVDR-39991 | Hoch | Statusabhängige Firewall löscht Pakte zwischen zwei Teilnetzen auf derselben Schnittstelle. | 
| VRVDR-36481 | Hoch | Upgrade/Downgrade von 5.2R4 auf 17.1.0/5.2R3 zeigt folgenden Fehler /opt/vyatta/sbin/vyatta-install-image.functions: Zeile 372: is_onie_boot: command not found |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8.8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – Sicherheitsupdate |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Zweigzieleinblendung / CVE-2017-5717 / Spectre, aka. Variante #2 |

##1801d
Freigabe am 8. März 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40940 | Hoch | Absturz der Datenebene im Zusammenhang mit NAT/Firewall |
| VRVDR-40886 | Hoch | Kombination von “icmp name <wert>” mit eine Anzahl anderer Konfigurationen für die Regel führt dazu, dass die Firewall nicht geladen wird. |
| VRVDR-39879 | Hoch | Konfiguration von Bonding für Jumbo-Frame schlägt fehl. |

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – Sicherheitsupdate |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Zweigzieleinblendung / CVE-2017-5715 / Spectre, aka Variante #2 |

##1801c
Freigabe am 7. März 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40281 | Hoch | Nach einem Upgrade von 5.2 auf eine neuere Version: Fehler “-vbash: show: command not found” im Betriebsmodus |

##1801b
Freigabe am 21. Februar 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40622 | Hoch | Cloud-init-Images können nicht korrekt ermitteln, ob die IP-Adresse vom DHCP-Server abgerufen wurde. |
| VRVDR-40613 | Kritisch | Die Bondschnittstelle startet nicht, wenn einer der physischen Verbindungen inaktiv ist. |
| VRVDR-40328 | Hoch | Cloud-init-Images brauchen sehr viel Zeit zum Booten. |

##1801a
Freigabe am 7. Februar 2018.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-40324 | Hoch | Durchschnittliche Lasten überschreiten 1.0 mit keiner Last auf dem Router mit der Bonding-Schnittstelle. |

##5.2R6S5
Freigabe am 19. Januar 2018.

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5.6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – Sicherheitsupdate (Meltdown) |
| VRVDR- 38265 | 8.8 | DSA-3970-1 | CVE-2017-1 |

##5.2R6S4
Freigabe am 15. Dezember 2017.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-39529 | Hoch | DHCP-Serverfunktionsübernahme synchronisiert keine Datenbanken. |
| VRVDR-39399 | Kritisch | Vyatta legt den Netzstatus FAULT ab, wenn VRRP- bzw. mehrere Schnittstellen (vrrp/multiple) mit flap/seg-Fehler angezeigt werden. |
| VRVDR-39112 | Hoch | DNAT-Datenverkehr, der mit ZBF übereinstimmt, gibt nur das erste Paket im Datenfluss frei. |
| VRVDR-38075 | Niedrig | Wenn “restart vpn” vom Responder ausgegeben wird, stellt der Initiator die Verbindung nicht erneut her. |
| VRVDR-37934 | Kritisch | Absturz von BGPd wenn nur die Zusammenfassung der Aggregatadressen (aggregate-address) konfiguriert wird / statische Routen fehlen|
| VRVDR-37717 | Niedrig | Die Felder (hard-enf) “Beschreibung” und “Lizenz” in Versionsausgabe umbenennen. |
| VRVDR-37689 | Hoch |Hohe Rate von NIC-PF-Unterbrechungen |
| VRVDR-37633 | Kritisch |Blockierung bei Keepalive |

## 5.2R6S3
Freigabe am 4. Dezember 2017.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-39207 | Kritisch | ARP schlägt bei VIF-Bond-Schnittstelle fehl. |


##5.2R6S2
Freigabe am 2. November 2017.

**Gelöste Probleme**

| Problemnummer | Priorität | Zusammenfassung |
| --- | --- | --- |
| VRVDR-39177 | Hoch | OpenVPN-Serverdomänennamensoption (domain-name) wurde mit –push dhcp-option nicht angewendet. |
| VRVDR-39129 | Kritisch | OpenVPN-Serverpushroutenparameter (push-route) verhindert Start von OpenVPN. |

##5.2R6S1
Freigabe am 12. Oktober 2017.

**Behobene Sicherheitslücken**

| Problemnummer | CVSS-Bewertung | Empfehlung | Zusammenfassung |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9.8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- Sicherheitsupdate |

Die hier enthaltenen Informationen stellen weder ein Angebot, noch eine verbindliche Zusage, noch eine Darstellung oder Gewährleistung seitens AT&T dar. Änderungen sind vorbehalten. Diese Informationen dürfen nur nach einer schriftlichen Genehmigung außerhalb von AT&T-Unternehmen verwendet oder weitergegeben werden. 

© 2018 AT&T Intellectual Property. Alle Rechte vorbehalten. AT&T und das Globe-Logo sind eingetragene Marken von AT&T Intellectual Property. Alle anderen Marken sind Eigentum der jeweiligen Rechtsinhaber.
