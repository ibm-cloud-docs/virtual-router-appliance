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

# VRA-Grundlagen
Die VRA kann mithilfe einer fernen Konsolensitzung über SSH oder durch Anmelden bei der Web-GUI konfiguriert werden. Die Web-GUI ist standardmäßig nicht im öffentlichen Internet verfügbar. Melden Sie sich über SSH an, um die Web-GUI zu aktivieren.

**HINWEIS:** Das Konfigurieren von VRA außerhalb der zugehörigen Shell oder Schnittstelle kann zu unerwarteten Ergebnissen führen und wird daher nicht empfohlen.

## Über SSH auf die Einheit zugreifen
Die Mehrzahl der UNIX-basierten Betriebssysteme wie Linux, BSD und Mac OSX verfügen über OpenSSH-Clients, die in ihre jeweilige Standardinstallation integriert sind. Windows-Benutzer können einen SSH-Client (z. B. PuTTy) herunterladen.

Es wird empfohlen, SSH-Verbindungen zur öffentlichen IP zu inaktivieren und nur SSH-Verbindungen zur privaten IP zuzulassen. Verbindungen zu privaten IPs erfordern eine Verbindung Ihres Client zum privaten Netz. Sie können sich mit einer der VPN-Standardoptionen (PPTP, SSL-VPN und IPsec) anmelden, die im Kundenportal zur Verfügung stehen. Sie können auch eine angepasste VPN-Lösung verwenden, die in der VRA konfiguriert ist.

Melden Sie sich mit dem auf der Seite **Einheitendetails** angegebenen Vyatta-Konto über SSH an. Das Rootkennwort wird ebenfalls bereitgestellt, aber die Rootanmeldung ist aus Sicherheitsgründen standardmäßig inaktiviert.

`ssh vyatta@[IP-adresse] `

**HINWEIS:** Es wird empfohlen, die Rootanmeldung nicht zu aktivieren. Melden Sie sich mit dem Vyatta-Konto an und verwenden Sie die Rootebene nur bei Bedarf.

SSH-Schlüssel können auch bei der Bereitstellung angegeben werden, um die Anmeldung mit dem Vyatta-Konto zu vermeiden. Nachdem überprüft wurde, dass der Zugriff auf Ihre VRA mit Ihrem SSH-Schlüssel möglich ist, können Sie die Standardanmeldung mit Benutzername und Kennwort inaktivierten, indem Sie die folgenden Befehle ausführen:

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Über die Web-GUI auf die Einheit zugreifen

Melden Sie sich unter Verwendung der oben angegebenen SSH-Anweisungen bei VRA an und führen Sie die folgenden Befehle aus, um den HTTPS-Service zu aktivieren:

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

Geben Sie nach Beendigung dieser Befehle in der Adressleiste Ihres Browsers Folgendes ein: `https://<ip.address>`. Ersetzen Sie dabei die IP-Adresse durch die IP-Adresse Ihrer VRA. Sie werden gegebenenfalls aufgefordert, das selbst ausgegebene Zertifikat der VRA zu akzeptieren. Akzeptieren Sie das Zertifikat und melden Sie sich nach Aufforderung mit Ihren Vyatta-Berechtigungsnachweisen bei der Webschnittstelle an.

## Modi
**Konfigurationsmodus:** Dieser Modus wird mit dem Befehl `configure` aufgerufen und ermöglicht das Konfigurieren des VRA-Systems.

**Betriebsmodus:** Der Anfangsmodus nach der Anmeldung an einem VRA-System. In diesem Modus können mit dem Befehl `show` Informationen zum Status des Systems abgefragt werden. Außerdem kann das System in diesem Modus erneut gestartet werden.

Die VRA-Shell ist eine modale Schnittstelle mit mehreren Betriebsarten. Der primäre Modus (Standardmodus) ist **Operativ**). Dieser Modus wird beim Anmelden aktiviert. Im Modus **Operativ** können Sie Informationen anzeigen und Befehle absetzen, die sich auf die aktuelle Ausführung des Systems auswirken (z. B. Datum und Uhrzeit festlegen oder die Einheit neu starten).

Der Befehl `configure` aktiviert den Modus **Konfiguration** für Benutzer, der das Bearbeiten der Konfiguration ermöglicht. Dabei ist zu beachten, dass Konfigurationsänderungen nicht sofort wirksam werden. Sie müssen festgeschrieben und gespeichert werden. Eingegebene Befehle werden zunächst in einen Konfigurationspuffer geschrieben. Führen Sie nach dem Eingeben der gewünschten Befehle den Befehl `commit` aus, damit die Änderungen wirksam werden.

Um Befehle permanent zu speichern, führen Sie den Befehl `save` nach dem Befehl `commit` aus.

Befehle für den Betriebsmodus können im Konfigurationsmodus ausgeführt werden, indem der Befehl `run` vorangestellt wird. Beispiel:


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

Das Hashzeichen (`#`) bezeichnet den Konfigurationsmodus. Wenn der Befehl mit `run` eingeleitet wird, erkennt die VRA-Shell, dass es sich um einen operativen Befehl handelt. Das vorangegangene Beispiel veranschaulicht auch die Anwendung von "grep" auf die Ausgabe von Befehlen.

## Befehlsübersicht aufrufen

Die VRA-Befehlsshell enthält Funktionen zum Vervollständigen mithilfe der Tabulatortaste. Wenn Sie wissen möchten, welche Befehle verfügbar sind, drücken Sie die Tabulatortaste, um eine Liste und eine kurze Erläuterung aufzurufen. Diese Funktion steht sowohl in der Shelleingabeaufforderung als auch bei der Befehlseingabe zur Verfügung. Beispiel:

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Beispielkonfiguration
Konfigurationen werden als hierarchische Knotenstruktur dargestellt. Das folgende Beispiel zeigt einen Codeblock für eine statische Route:

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

Dieser Codeblock wird durch die folgenden Befehle erstellt:

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

Das vorliegende Beispiel macht deutlich, dass ein (statischer) Knoten über mehrere untergeordnete Knoten verfügen kann. Die Route für `192.168.1.0/24` kann mit dem Befehl `delete protocols static route 192.168.1.0/24` entfernt werden. Wenn die Angabe `192.168.1.0/24` in dem Befehl fehlt, werden beide Routenknoten zum Löschen markiert.

Beachten Sie, dass die Konfiguration erst nach dem Absetzen des Befehls `commit` tatsächlich geändert wird. Um die gegenwärtig aktive Konfiguration mit den im Konfigurationspuffer angegebenen Änderungen zu vergleichen, verwenden Sie den Befehl `compare`. Um den Konfigurationspuffer zu löschen, verwenden Sie den Befehl `discard`.

## Benutzer und die rollenbasierte Zugriffssteuerung (Role-based Access Control, RBAC)
Für Benutzerkonten können drei verschiedene Zugriffsebenen konfiguriert werden:

* admin
* operator
* Superuser

Benutzer mit der Zugriffsebene 'operator' können `show`-Befehle ausführen, um den Ausführungsstatus des Systems anzuzeigen, und `reset`-Befehle, um Services auf der Einheit erneut zu starten. Die Zugriffsebene 'operator' impliziert nicht den Lesezugriff.

Benutzer mit der Zugriffsebene 'admin' haben uneingeschränkten Zugriff auf alle Konfigurationen und Operationen für die Einheit. Benutzer mit der Zugriffsebene 'admin' können Konfigurationseinstellungen für die Einheit ändern sowie die Einheit erneut starten und beenden.

Benutzer mit der Zugriffsebene 'superuser' können Befehle mit Rootberechtigung (über den Befehl `sudo`) ausführen und verfügen zudem über die Berechtigungen der Zugriffsebene 'admin'.

Für Benutzer kann die Kennwortauthentifizierung und/oder die Public-Key-Authentifizierung konfiguriert werden. Die Public-Key-Authentifizierung wird mit SSH verwendet und ermöglicht Benutzern die Anmeldung über eine Schlüsseldatei in ihrem System. So erstellen Sie einen Benutzer mit der Zugriffsebene 'operator' und mit einem Kennwort:

```
set system login user [konto] authentication plaintext-password [kennwort]
set system login user [konto] level operator
commit
```

**HINWEIS:** Wenn keine Zugriffsebene angegeben ist, wird die Ebene 'admin' verwendet. In diesem Fall werden die Benutzerkennwörter in der Konfigurationsdatei verschlüsselt dargestellt.

Die rollenbasierte Zugriffssteuerung (Role-based Access Control, RBAC) ist ein Verfahren, das den Zugriff auf einen Teil der Konfiguration nur berechtigten Benutzern gewährt. Mit RBAC können Benutzer mit der Zugriffsebene 'admin' Regeln für eine Benutzergruppe definieren, die festlegen, dass nur bestimmte Befehle ausgeführt werden dürfen.

Bei Verwendung von RBAC wird eine Gruppe erstellt, die dem ACM-Regelsatz (ACM = Access Control Management) zugeordnet ist. In dieser Gruppe wird ein Benutzer hinzugefügt und es wird ein Regelsatz erstellt, der die Gruppe mit den Pfaden im System abgleicht. Schließlich wird das System so konfiguriert, dass die für die Gruppe angelegten Pfade entweder gesperrt oder freigegeben werden.

In der Standardeinstellung ist in Virtual Router Appliance kein ACM-Regelsatz definiert und ACM ist inaktiviert. Wenn Sie mit RBAC eine differenzierte Zugriffssteuerung einrichten möchten, müssen Sie ACM aktivieren und zusätzlich zu den von Ihnen definierten Regeln die folgenden ACM-Standardregeln hinzufügen:

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

Lesen Sie die ergänzende [Dokumentation](vra-docs.html), bevor Sie versuchen, ACM-Regeln zu aktivieren. Fehlerhafte ACM-Regeleinstellungen können Zugriffssperren für die Einheit oder Funktionsstörungen im System verursachen.
