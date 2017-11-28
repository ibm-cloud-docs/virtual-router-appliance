---

copyright:
  years: 2017
lastupdated: "2017-10-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HA-Konfiguration synchronisieren
Die Konfiguration von zwei Virtual Router Appliances (VRAs), die ein HA-Paar bilden, müssen so weit synchronisiert werden, dass beide Einheiten das gleiche Verhalten zeigen. Hierfür werden Zuordnungen für die Konfigurationssynchronisation (`configuration sync-maps`) verwendet, und Sie können auswählen, welche Teile der Konfiguration synchronisiert werden. Wenn Sie auf einer Maschine eine Änderung vornehmen, wird die markierte Konfiguration mit einer Push-Operation auf die andere Einheit übertragen.

**HINWEIS:** Dies bedeutet nicht, dass die Konfiguration der anderen Einheit ebenfalls gesichert wurde. Es bedeutet lediglich, dass die aktive Konfiguration geändert wird.

Eine spezifische Konfiguration für ein bestimmtes System sollte nicht auf eine andere Einheit übertragen werden. Beispielsweise sollten konkrete IP-Adressen und MACs nicht synchronisiert werden. Der Konfigurationsknoten `system config-sync` und der Knoten `service https` können nicht synchronisiert werden.

Das folgende Beispiel veranschaulicht den Befehl 'config-sync':

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

Die beiden ersten Zeilen erstellen die Synchronisationszuordnung (sync-map). Dabei wird die Konfigurationszeilengruppe für `security firewall` in `sync-map` festgelegt. Dadurch wird veranlasst, dass alle am Konfigurationsknoten vorgenommenen Änderungen per Push-Operation auf die ferne Einheit übertragen werden. Es werden jedoch keine Änderungen für `security user` übertragen, da dieses Element nicht mit der Regel übereinstimmt. Die Synchronisationszuordnung (`sync-map`) kann nach Belieben detailliert oder allgemein gestaltet werden.

Die drei nächsten Zeilen geben den Benutzer, das Kennwort und die IP-Adresse für `config-sync` auf dem fernen Router an sowie die zu übertragende Synchronisationszuordnung. Alle Änderungen, die mit den Regeln für `TEST` übereinstimmen, werden unter Verwendung der Anmeldeinformationen an `remote-router 192.168.1.22` übermittelt. Beachten Sie, dass ein `REST`-Aufruf über die VRA-API ausgeführt wird, um dies zu veranlassen. Dies bedeutet, dass der HTTPS-Server auf dem fernen Router aktiv und betriebsbereit sein muss.

Der Befehl 'config-sync' wird jedesmal ausgeführt, wenn Sie eine Änderung festschreiben. Achten Sie daher auf Fehlernachrichten, die von der fernen Einheit stammen. Wenn die Konfiguration nicht synchron ist, muss eine entsprechende Korrektur auf der fernen Einheit erfolgen, um sie wieder betriebsbereit zu machen.

Unterschiede in der Konfiguration können auch mit dem Befehl `show config-sync difference` angezeigt werden.
