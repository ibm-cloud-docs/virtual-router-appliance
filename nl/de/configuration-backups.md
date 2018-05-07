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

# Backup einer Konfiguration durchführen
Die Konfigurationsbefehle müssen gesichert werden, wenn eine Änderung am System vorgenommen wird. Dies kann durch Ausführen des Betriebsmodusbefehls `show configuration commands` und anschließendes Speichern der Ausgabe (z. B. durch Kopieren und Einfügen aus der SSH-Sitzung) erreicht werden. Das Ergebnis ist ein minimales Backup der Konfiguration.

Für ein ausführlicheres Backup muss ein Technical Support-Archiv des Systems wie folgt erstellt werden: 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

Die erstellte Archivdatei kann anschließend aus der Virtual Router Appliance in eine Speichereinheit Ihrer Wahl kopiert werden. Das Archiv enthält Backups der Konfigurationsdaten, Ausgangsverzeichnisse und Protokolldaten. Es beinhaltet ein viel umfassenderes Backup des Systems. 

Beispiel:

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Ziehen Sie außerdem in Betracht, alle Anmerkungen, die Sie beim Konfigurieren der Einheit erstellt haben, an einer zentralen Speicherposition zu sicher, auf die alle Ihre Administratoren zugreifen können.
