---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# NAT-Regeln auf Vyatta konfigurieren
Dieses Thema enthält Beispiele von NAT-Regeln (Network Address Translation), die auf Vyatta verwendet werden.

## Eins-zu-viele-NAT-Regel (Maskieren)

Geben Sie in der Eingabeaufforderung folgende Befehle ein:

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

Verbindungsanforderung von Maschinen im `10.xxx.xxx.xxx`-Netz werden der IP an bond1 zugeordnet und erhalten einen zugehörigen ephemeren Port, wenn ausgehender Datenverkehr auftritt. Die Absicht ist, Eins-zu-viele-Maskierungsregeln eine höhere Zahl zuzuordnen, damit diese nicht mit niedrigeren NAT-Regeln, über die Sie möglicherweise verfügen, in Konflikt geraten,

**HINWEIS:** Sie müssen den Server konfigurieren, um dessen Internetdatenverkehr über die VRA zu übergeben, damit das zugehörige Standardgateway die private IP-Adresse  des verwalteten VLANs (virtuelles LAN) ist. So lautet zum Beispiel für `bond0.2254` das Gateway `10.52.69.201`. Dies sollte die Gateway-Adresse für den Server sein, der den Internetdatenverkehr übergibt.

**HINWEIS:** Verwenden Sie den folgenden Befehl, um die NAT-Fehlersuche zu unterstützen: 

'''
run show nat source translations detail 
'''

## 1:1-NAT-Regel

Die Befehle unten zeigen, wie eine 1:1-NAT-Regel definiert wird. Beachten Sie, dass die Regelnummern niedriger sind als die der Maskierungsregel. Dadurch erhalten die 1-Regeln Vorrang vor den Eins-zu-viele-Regeln.

**HINWEIS:** IP-Adressen, die 1:1-zugeordnet werden, können nicht maskiert werden. Wenn Sie eine eingehende IP übersetzen, müssen Sie die entsprechende ausgehende IP übersetzen, damit der Datenverkehr in beide Richtungen erfolgt.

Die folgenden Befehle sind für die Quellen- und Zielregel gedacht. Geben Sie im Konfigurationsmodus `show nat` ein, um die NAT-Regeltypen anzuzeigen.

**HINWEIS:** Verwenden Sie folgenden Befehl, um Fehler bei der NAT zu beheben: `run show nat source translations detail`. 

Geben Sie folgende Befehle in der Eingabeaufforderung ein, nachdem Sie sichergestellt haben, dass Sie sich im Konfigurationsmodus befinden:

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

Wenn der Datenverkehr an der IP `50.97.203.227` bei bond1 eintrifft, wird diese IP der IP `10.52.69.202` (in einer beliebigen definierten Schnittstelle) zugeordnet. Ausgehender Verkehr mit der IP `10.52.69.202` (in einer beliebigen definierten Schnittstelle) wird in die IP `50.97.203.227` übersetzt und an der Schnittstelle bond1 als ausgehender Verkehr fortgesetzt.

**HINWEIS:** Verwenden Sie folgenden Befehl, um Fehler bei der NAT zu beheben: `run show nat source translations detail`.

## IP-Bereiche über Ihre VRA hinzufügen

Abhängig von Ihrer VRA-Konfiguration möchten Sie möglicherweise bestimmte IBM Cloud IP-Adressen akzeptieren. 

Neue vRouter-Bereitstellungen werden mit den Servicenetz-IP-Adressen von IBM Cloud geliefert, die in einer Firewallregel mit dem Namen `SERVICE-ALLOW` definiert sind.

Es folgt ein Beispiel für `SERVICE-ALLOW`. Dies ist kein vollständiger privater IP-Regelsatz.

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

Sobald Sie die Firewallregeln definiert haben, können Sie diese an passender Stelle zuordnen. Unten sind zwei Beispiele aufgelistet. 

Anwendung auf eine Zone:

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Anwendung auf eine bond-Schnittstelle:

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`

**HINWEISE:**

* IP-Adressen, die 1:1 zugeordnet werden, können nicht maskiert werden. Wenn Sie eine eingehende IP übersetzen, müssen Sie dieselbe ausgehende IP übersetzen, damit der Datenverkehr in beide Richtungen erfolgen kann.

* Verwenden Sie folgenden Befehl, um Fehler bei der NAT zu beheben: `run show nat source translations detail`. 

