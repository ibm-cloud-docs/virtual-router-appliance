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

# Hinweise zum Upgrade für die Betriebssystemversion 18.01

Diese Datei enthält eine Liste der Dinge, die Sie wissen müssen, wenn Sie ein Upgrade für die VRA (Vyatta 5600) vom Betriebssystem Brocade 5.X auf das Betriebssystem 18.01 durchführen. Dies schließt die Korrektur für das Sicherheitsproblem von Spectre ein. Es gibt verschiedene Aspekte, die Sie beim Upgrade beachten müssen.

## Wer sollte diesen Abschnitt lesen?

* Alle mit einer aktuellen VRA, die nicht unter dem Betriebssystem 1801 ausgeführt wird. (Die aktuelle Version ist 18.01g.)

* Alle, die die neue Version 18.01f für eine {{site.data.keyword.vra_full}} installieren möchten (zum Beispiel alle, die ein Upgrade von 17.2X durchführen).

* Wenn Sie über eine Vyatta 5400 verfügen, gelten diese Informationen für Sie nicht.

## Warum benötigen Sie diese Informationen?

Version 1801f der VRA enthält eine Programmkorrektur für die Sicherheitslücke bei Spectre. Es muss jedoch eine Änderung am Installationsprogramm selbst vorgenommen werden, bevor die Programmkorrektur installiert werden kann. Es ist ein Zwischenschritt zur Installation von Version 1801C erforderlich.

## Normale Upgradeprozedur
Wenn Sie ein Upgrade auf Version 18.01f durchführen möchten, müssen Sie zuerst die Programmkorrektur 18.01C herunterladen und installieren, um das VRA-Installationsprogramm zu aktualisieren:

1. Laden Sie die Programmkorrektur 1801c von dieser Position herunter, und folgen Sie dann der [normalen Prozedur](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os), um sie zu installieren.

2. Sie führen dann einen Warmstart durch und laden anschließend die Programmkorrektur 18.01f von dieser Position herunter und installieren sie mithilfe der [normalen Prozedur](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

Jetzt sollte die Version 18.01f ausgeführt werden, in der die Sicherheitslücke bei Spectre behoben ist. 

## Vollständige Neuladeprozedur
Als Alternative können Sie auch ein vollständiges Neuladen der VRA auf Version 18.01f durchführen:

*WARNUNG:* Bei dieser Vorgehensweise können Sie den Zwischenschritt, das Herunterladen und Installieren der beiden Programmkorrekturen, überspringen. Allerdings verlieren Sie bei einem vollständigen Neuladen unter Verwendung von ISO alle Daten.

1. Rufen Sie 18.01f ISO von dieser Position ab. 
2. Führen Sie die [normale Prozedur](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) zum Installieren und erneuten Laden aus.

Jetzt sollte die Version 1801f ausgeführt werden, in der die Sicherheitslücke bei Spectre behoben ist.

## Zusätzliche Anleitung

Da es keine einfache Eins-zu-eins-Zuordnung zwischen den Funktionen von Vyatta 5400 und der {{site.data.keyword.vra_full}} gibt, ist es hilfreich, eine Baseline-Konfiguration für die VRA zu erstellen. WanClouds, ein IBM© Partner, kann Sie bei diesem Prozess unterstützen und Anleitungen zur Verfügung stellen, um ähnliche Funktionen wie in Vyatta 5400 in Ihrer VRA zu erstellen.

Weitere Informationen zu allgemeinen Problemen, die beim Upgradeprozess auftreten können, finden Sie in der [weiteren Dokumentation](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
