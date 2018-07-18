---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vorgehensweise zum Anmelden bei einem Vyatta-Gerät

Auf Brocade NFV (Network Functions Virtualization) kann über eine Web-GUI oder direkt mit einer SSH-Anmeldung zugegriffen werden. 

## Authentifizierung beim privaten Netz von SoftLayer

1. Geben Sie die private IP-Adresse aus der Anzeige **Details** (Menü **Netz > Gateway-Appliances**) mit einem 'https://'-Präfix (z. B. https://10.54.207.131/) in Ihren Browser ein. Denken Sie daran, dass möglicherweise ein Zertifikatsfehler angezeigt wird, weil das Gerät kein öffentliches Zertifikat verwendet. Akzeptieren Sie den Fehler, um die Webseite zu öffnen. 

2. Geben Sie die Berechtigungsnachweise von der Registerkarte **Kennwort** im Menü **Geräte** im Webportal für den Benutzer **vyatta** ein.  Sie werden zum Brocade-Dashboard geleitet. 
