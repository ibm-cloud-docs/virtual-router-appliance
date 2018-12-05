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
{:faq: data-hd-content-type='faq'}

# Häufig gestellte technische Fragen
Die folgenden häufig gestellten Fragen betreffen die Konfiguration der IBM Virtual Router Appliance (VRA) und die Migration von Vyatta 5400 auf die VRA.

## Wie ermögliche ich Internetdatenverkehr von den Hosts in einem privaten VLAN?
{:faq}

Dieser Verkehr muss eine öffentliche Quellen-IP abrufen, da eine Source NAT die private IP mit der öffentlichen IP der VRA maskieren muss.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

Die Konfiguration oben führt nur SNAT von dem Datenverkehr aus, der von den Servern im privaten `10.0.0.0/8`-Netz stammt.

Dadurch wird sichergestellt, dass Pakete, die bereits über eine Quellenadresse verfügen, die über das Internet weitergeleitet werden kann, nicht beeinträchtigt werden.

## Wie kann ich Internetdatenverkehr filtern und nur bestimmte Protokolle oder Ziele zulassen?
{:faq}

Dies ist eine allgemeine Frage, wenn Source NAT und eine Firewall kombiniert werden müssen.

Bitte bedenken Sie die Reihenfolge der Operationen in der VRA, wenn Sie Ihren Regelsatz entwerfen.

Kurz gesagt, werden Firewallregeln *nach* SNAT angewendet.

Um den gesamten ausgehenden Datenverkehr in einer Firewall zu blockieren und nur bestimmte SNAT-Datenflüsse zuzulassen, müssen Sie die Filterlogik auf Ihrem SNAT verschieben.

Um zum Beispiel nur HTTPS-Internetdatenverkehr für einen Host zuzulassen, würde die SNAT-Regel wie folgt lauten:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` wäre die öffentliche Adresse für die VRA. 

Es wird dringend empfohlen, die öffentliche VRRP-Adresse der VRA zu verwenden, sodass Sie zwischen Host und öffentlichem VRA-Datenverkehr unterscheiden können.

Gehen Sie davon aus, dass `150.1.2.3` die VRRP VRA-Adresse ist und `150.1.2.5` die tatsächliche dp0bond1-Adresse.

Die statusabhängige Firewall, die auf `dp0bond1 out` angewendet wird, wäre dann:

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

Beachten Sie, dass die Kombination von Source NAT und Firewall das erforderliche Designziel erreicht. 

Stellen Sie sicher, dass die Regeln Ihrem Design entsprechen und dass keine anderen Regeln Datenverkehr zulassen, der blockiert werden sollte. 

## Wie schütze ich die VRA selbst mit einer zonenbasierten Firewall?
{:faq}

Die VRA verfügt über keine `lokale Zone`.

Sie können stattdessen die CPP-Funktion (Control Plane Policing) verwenden, da diese als eine `lokale` Firewall bei Loopback angewendet wird.

Beachten Sie, dass dies eine statusunabhängige Firewall ist und dass Sie explizit die Rückgabe von Datenverkehr ausgehender Sitzungen, der von der VRA selbst stammt, zulassen müssen.

## Wie beschränke ich SSH und blockiere Verbindungen, die aus dem Internet kommen?
{:faq}

Ein bewährtes Verfahren ist es, keine SSH-Verbindungen vom Internet zuzulassen und andere Möglichkeiten für den Zugriff auf die private Adresse zu verwenden, zum Beispiel SSL VPN.

Die VRA akzeptiert standardmäßig auf allen Schnittstelle SSH.
Um nur SSH-Verbindungen auf privaten Schnittstellen zu überwachen, muss die folgende Konfiguration definiert werden.

```
set service ssh listen-address '10.1.2.3'
```

Denken Sie daran, dass die IP-Adresse durch die Adresse ersetzt werden muss, die zur VRA gehört.
