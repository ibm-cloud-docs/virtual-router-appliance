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

# Sauvegarde d'une configuration
Les commandes de configuration doivent être sauvegardées en cas de changement apporté au système. Cette opération s'effectue en exécutant la commande du mode opérationnel `show configuration commands` puis en sauvegardant la sortie (par exemple en effectuant un copier-coller depuis la session SSH). C'est ce que l'on considère comme une sauvegarde minimale de la configuration.

Une sauvegarde plus complète implique la génération d'une archive de support technique pour le système :   

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

Le fichier archive généré peut alors être copié de l'unité Virtual Router Appliance sur l'unité de stockage de votre choix. L'archive contient les sauvegardes des informations de configuration, des répertoires de base et des informations de journalisation. Il s'agit d'une sauvegarde de système beaucoup plus complète. 

Par exemple :

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Pensez à sauvegarder toutes les notes que vous avez créées lors de la configuration de l'unité à un emplacement centralisé accessible à tout votre personnel d'administration.
