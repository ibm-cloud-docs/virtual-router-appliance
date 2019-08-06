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

# Häufig gestellte Fragen für {{site.data.keyword.vra_full}}
{: #faqs-for-ibm-virtual-router-appliance}

Der vorliegende Abschnitt enthält häufig gestellte Fragen beim Arbeiten mit {{site.data.keyword.vra_full}} (VRA).

## Was ist eine VRA? 
{:faq}

{{site.data.keyword.vra_full}} (VRA) ermöglicht einem IBM Cloud-Kunden das selektive Weiterleiten von privatem und öffentlichem Netzdatenverkehr über einen voll ausgestatteten Unternehmensrouter mit Firewall, Regulierung des Datenverkehrs, richtlinienbasiertem Routing, VPN und vielen weiteren Funktionen. Alle VRA-Funktionen werden vom Kunden verwaltet. VRA bietet den IBM Cloud-Kunden Steuerungsmöglichkeiten, die in diesem Umfang sonst nur lokalen Netzen vorbehalten sind.

## Was ist eine Gateway-Appliance? 
{:faq}

Eine Gateway-Appliance-Komponente ermöglicht Ihnen die Verwendung des Webportals oder der API zum Auswählen von Netzsegmenten (VLANs) für das Routing über eine VRA. Dabei kann die VLAN-Auswahl jederzeit geändert werden. Die Gateway-Appliance unterstützt außerdem die Hochverfügbarkeit (High Availability, HA) der VRA und das Konfigurieren einer zweiten VRA für die Funktionsübernahme, wenn die erste VRA ausfällt.

## Die Begriffe "Vyatta" und "vRouter" tauchen immer wieder auf. Was haben diese Begriffe mit VRA zu tun?
{:faq}

Vyatta war eine quelloffene, PC-basierte Router-Software, die vollständig angekauft und in lizenzierten Quellcode umgewandelt wurde. Heute bezeichnen die Begriffe "Vyatta" und "Vyatta OS" kommerzielle Softwareanpassungen, die aus diesem geschlossenen Quellcodeprojekt abgeleitet wurden. IBM VRA umfasst Elemente von Vyatta OS sowie wesentliche Funktions- und Serviceerweiterungen, die nur in IBM Cloud zur Verfügung stehen.

"vRouter" bezeichnet eine kurzlebige Markenumstrukturierung durch den damaligen Eigentümer der Marke "Vyatta". In der Dokumentation ist diese Bezeichnung als Synonym für "Vyatta" zu verstehen.

## Wird Vyatta 5400 weiterhin unterstützt?
{:faq}

Vyatta 5400 wird ab dem 31. März 2019 nicht mehr von IBM unterstützt.

## Welche Verbesserungen bietet {{site.data.keyword.vra_full}} (Vyatta 5600) gegenüber Vyatta 5400?
{:faq}

Vyatta 5600 bietet die folgenden funktionalen Erweiterungen gegenüber Vyatta 5400:

- Höherer Durchsatz mit bis zu 10 Gb/s pro CPU-Kern
- Um den Faktor 3 höherer Durchsatz pro IPsec-VPN-Sitzung (Advanced Encryption Standard, AES)
- Höhere Kapazität für Router, Datenfluss und gleichzeitig bestehende Verbindungen
- Aktualisierte Unterstützung für Standards, einschließlich Layer 2 Tunneling Protocol Version 3 (L2TPv3), Internet Key Exchange Version 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2) und 802.1Q-Tunnelung (Q-in-Q) mit Kapselung

## Wie geht es mit dem Produktangebot AT&T vRouter 5600 weiter?
{:faq}

AT&T (früher Brocade) hat das Ende des Lebenszyklus und das Ende der Unterstützung für das Produktangebot Brocade vRouter 5600 angekündigt. Obwohl Brocade vRouter 5600 die Basistechnologie für {{site.data.keyword.vra_full}} bereitstellt, gilt diese Ankündigung nicht für IBM Kunden. Für IBM Kunden wird die Verwendung dieses neuen Produktangebots weiterhin unterstützt.

## Auf welchem Vertriebsweg wird VRA bereitgestellt? 
{:faq}

Sie erhalten VRA, wenn Sie ein Netzgateway bestellen. Im Rahmen dieses optimierten Prozesses können Sie ein Rechenzentrum sowie einen geeigneten VRA-Server auswählen und angeben, ob Sie ein HA-Paar aus zwei VRAs implementieren möchten. Dabei werden die Server, die Betriebssysteme und die Gateway-Appliance-Komponente automatisch bereitgestellt. Nach Abschluss der Bereitstellung können Sie die Gateway-Appliance-Schnittstelle verwenden, um VLANs über die VRA zu steuern. Sie können Ihren VRA-Server mit den Kennwörtern, die im Abschnitt für Hardwaredetails des Kundenportals bereitgestellt werden, direkt über SSH (Secure Shell) konfigurieren.

## Ist mein Kennwort sicher? 
{:faq}

Ja. Allen VRAs werden automatisch generierte Kennwörter zugewiesen, die nur für den Kontoinhaber sichtbar sind. Kennwörter können schnell und einfach geändert werden, ebenso wie SSH-Public-Keys und IP-Zugriffsbeschränkungen für Administratoren.

## Ist VRA auch ohne Gateway-Appliance erhältlich? 
{:faq}

Ja. In diesem Fall kann mit VRA jedoch nur der Datenverkehr zwischen den öffentlichen und privaten VRA-Schnittstellen gesteuert werden. Für VLANs und HA ist die Gateway-Appliance-Komponente erforderlich.

## Wird der gesamte Netzverkehr über die VRA geleitet? 
{:faq}

Nein. Mit der Gateway-Appliance können Sie die privaten und öffentlichen Netzsegmente (VLANs) auswählen, deren Datenverkehr über die VRA geleitet werden soll. Dabei können Sie die VLAN-Auswahl jederzeit ändern und umgehen. Außerdem können Sie mit VRA IP-basierte Regeln definieren, die auf Teilnetze oder IP-Bereiche angewendet werden. Solche Regeln funktionieren nur, wenn der Datenverkehr der VLANs, die diese Teilnetze enthalten, über die VRA geleitet wird.

## Kann eine VRA oder eine dedizierte Firewall die Bereitstellung neuer Server verhindern? 
{:faq}

Ja. Falls irgend möglich, sollten Sie Ihr Netz erst sperren, nachdem es mit den Servern bestückt wurde, die Sie verwenden möchten.

Der IBM Support darf laut den geltenden Richtlinien die Konfiguration der VRA oder der dedizierten Firewall nur mit ausdrücklicher Zustimmung des Kunden ändern. Demzufolge kann der Support in den meisten Fällen nicht erkennen, ob eine VRA für blockierte oder fehlgeschlagene Serverbereitstellungen verantwortlich ist.

Es ist Aufgabe des Kunden, vor dem Bestellen eines Servers sicherzustellen, dass die Konfiguration der VRA oder der Firewall automatisierte Serverbereitstellungen zulässt. Wenn Bereitstellungen durch eine vom Kunden verwaltete VRA oder Firewall blockiert werden, muss der Kunde dafür sorgen, dass diese Blockierung aufgehoben wird. Verzögerte Bereitstellungen durch solche Blockierungen werden nicht vom Service-Level-Agreement (SLA) abgedeckt und nicht rückvergütet. Bestellte Systeme können gegebenenfalls (nach Löschen der Kundendaten) in den Lagerbestand zurückgeführt werden, wenn der Kunde nicht zeitnah antwortet.

Ebenso ist damit zu rechnen, dass eine Bestellung fehlschlägt, wenn nach dem Einsenden einer Bestellung eine VRA oder Firewall umgangen wird. Möglicherweise werden in einem begrenzten Zeitfenster Wiederholungsversuche für die automatische Bereitstellung durchgeführt. Der gesamte Bereitstellungsprozess sollte möglichst ohne netzbedingte Unterbrechungen ablaufen.

## Welche Firewallprodukte bietet IBM an?
{:faq}

Einen ausführlichen Vergleich aller in der IBM Cloud angebotenen Firewallprodukte finden Sie in [diesem Thema](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-exploring-firewalls). 

## Kann eine VRA die unterstützenden Support-Maßnahmen wirkungslos machen? 
{:faq}

Ja, aus den oben genannten Gründen. VRA ist eine "Blackbox", d. h. eine Funktionseinheit mit eingehendem und ausgehendem VLAN-Datenverkehr. IBM hat keine Kenntnis darüber, was im System des Kunden mit den Datenpaketen geschieht.

Der Support ist stets bemüht, die bestmögliche Unterstützung zu geben, doch (1.) hat der Schutz der Kundendaten Vorrang vor der Konnektivität und (2.) verfügt das Standard-Support-Team nicht über die technischen Möglichkeiten zum Analysieren fehlerhafter oder hoch komplexer VRA- oder Firewallkonfigurationen.

In einem ersten Diagnoseschritt werden Sie möglicherweise angewiesen, Ihre VRA- oder Firewall-VLANs zu umgehen. Wenn nach dieser Maßnahme ein Startvorgang, der zuvor fehlgeschlagen war, erfolgreich fortgesetzt werden kann, muss davon ausgegangen werden, dass das Problem in Ihrer VRA- oder Firewallkonfiguration begründet liegt.

## Wie wirkt sich VRA auf die Leistung meines Netzes aus? 
{:faq}

Beachten Sie, dass eine öffentliche Cloud, für die Ihr Netz nicht sichtbar ist, Netze mit anderen Kunden gemeinsam nutzt. Der optimale erreichbare VRA-Durchsatz hängt von der verfügbaren Netzkapazität zum jeweiligen Zeitpunkt ab und davon, welche Distanz die Daten zurücklegen müssen.

Ungeachtet dieser variablen Faktoren kann VRA ein Volumen von 80 Gb/s nicht modifizierte Daten über mehrere Schnittstellen weiterleiten (nach Maßgabe der vereinfachten Formel, dass für jeweils 10 Gb/s ein vollständiger Prozessorkern (ohne Hyperthreading) erforderlich ist). Bei einer Maximalleistung von 40 Gb/s für aktuelle Servereinheiten (2 x 10 Gb/s öffentliche Daten + 2 x 10 Gb/s private Daten) bietet ein Server mit mindestens 8 Kernen genügend Rechenleistung für die Verarbeitung von mehreren allgemeinen VRA-Funktionen bei nahezu optimaler Netzleistung.

## Was kann ich tun, wenn ich mein VRA-Kennwort vergessen habe?
{:faq}

Wenn Systemzugriff besteht, führen Sie den folgenden Befehl aus, um ein neues Kennwort festzulegen:

```
set system login user [konto] authentication plaintext-password [kennwort]  
```

Wenn kein Systemzugriff besteht, können Sie die Einheit neu starten und mit der Option für Kennwortwiederherstellung im GRUB-Menü das Kennwort des Rootbenutzers zurücksetzen.

## Was kann ich tun, wenn ich von der Firewall ausgesperrt bin?
{:faq}

Das Konstrukt `reboot at [time]` kann beim Testen potenziell gefährlicher Firewallregeln hilfreich sein.

Wenn die Regel greift, verwenden Sie den Befehl `reboot cancel`, um den Warmstart abzubrechen. Wenn die Regel Ihnen den Zugriff verweigert, warten Sie einfach, bis der geplante Neustart ausgeführt wird.

Wenn kein Systemzugriff besteht, kann der Zugriff möglicherweise durch einen Warmstart wiederhergestellt werden. Beim Warmstart liest das System die Konfigurationsdatei, die in der Regel unverändert ist, da vorherige Einträge gelöscht wurden.

Wenn Zugriff über IPMI besteht, können Sie die folgenden Aktionen ausführen, um den Zugriff wiederherzustellen:

1. Inaktivieren Sie die Regel, die den Verstoß verursacht, indem Sie den folgenden Befehl ausführen:

	```
	set security firewall name [firewallname] rule [regelnummer] disable
	commit
	```

2. Entnehmen Sie den ganzen Regelsatz aus der erforderlichen Schnittstelle, indem Sie den folgenden Befehl ausführen:

	```
	delete interfaces dataplane [schnittstelle] firewall [typ][firewall name]
	commit
	```

**HINWEIS:** Durch falsche Verwendung dieser Befehle kann Ihre Schnittstellenkonfiguration vollständig gelöscht werden.

## Wie kann ich Rootanmeldungen bei der VRA aktivieren?
{:faq}

Führen Sie den folgenden Befehl aus, um den Rootzugriff über SSH zu aktivieren:

`set service ssh allow-root`

Beachten Sie, dass es als unsicher angesehen wird, Rootzugriff über SSH zu ermöglichen. Eine Alternative für den Zugriff auf eine Root-Shell besteht entweder darin, sich als ein anderer Benutzer anzumelden und mithilfe von `su -` lokal zum Root zu erweitern, oder darin, sudo-Befehle für 'superuser' zuzulassen. 

Gehen Sie wie folgt vor, um Vyatta als Superuser zu konfigurieren:

`set system login vyatta level superuser`
