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

# Backup di una configurazione
{: #backing-up-a-configuration}

Deve essere eseguito il backup dei comandi di configurazione quando si effettua una modifica nel sistema. Questo può avvenire eseguendo il comando in modalità operativa `show configuration commands` e salvando l'output (ad esempio copiandolo e incollandolo dalla sessione SSH). Questo viene considerato un backup minimo della configurazione.

Un backup più completo implica la generazione di un archivio di supporto tecnico per il sistema:   

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

Il file di archiviazione generato può quindi essere copiato dalla VRA (Virtual Router Appliance) al dispositivo di archiviazione di tua scelta. L'archivio contiene i backup delle informazioni di configurazione, le directory home e le informazioni di registrazione. È un backup del sistema molto più completo. 

Ad esempio:

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Considera di eseguire il backup di ogni nota che hai creato mentre configuravi il dispositivo in un'ubicazione centrale accessibile a tutto il tuo staff di gestione.
