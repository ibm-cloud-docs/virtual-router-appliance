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

# Considérations de mise à niveau pour la version 18.01 du système d'exploitation

Ce fichier contient une liste d'éléments que vous devez connaître lorsque vous effectuez une mise à niveau (qui inclut la résolution de la violation de sécurité Spectre) du système d'exploitation Brocade 5.X vers le système d'exploitation 18.01 pour le dispositif VRA (Vyatta 5600). Vous devez tenir compte de plusieurs problèmes lorsque vous effectuez cette mise à niveau.

## A qui s'adresse cette rubrique ?

* Toute personne possédant un dispositif VRA qui ne s'exécute pas sous le système d'exploitation 1801. (La version en cours est 18.01g.)

* Toute personne cherchant à installer une nouvelle version 18.01f pour un dispositif VRA ({{site.data.keyword.vra_full}}) (par exemple, toute personne effectuant une mise à niveau à partir de 17.2X).

* Si vous possédez un Vyatta 5400, ces informations ne vous concernent pas.

## Pour quelles raisons avez-vous besoin de ces informations ?

La version 1801f du dispositif VRA contient un correctif de sécurité pour la vulnérabilité Spectre, cependant, une modification doit être apportée au programme d'installation proprement dit avant que le correctif puisse être installé. Une étape intermédiaire consistant à installer la version 1801C est obligatoire.

## Procédure de mise à niveau normale
Avant d'effectuer une mise à niveau vers la version 18.01f, vous devez d'abord télécharger et installer le correctif 18.01c pour mettre à jour le programme d'installation de VRA :

1. Téléchargez le correctif 1801c à partir de cet emplacement, puis suivez la [procédure normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) pour l'installer.

2. Une fois le système redémarré, téléchargez le correctif 18.01f à partir de cet emplacement et installez-le à l'aide de la [procédure normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

A présent, vous disposez de la version 18.01f et le problème de sécurité Spectre est résolu.

## Procédure de rechargement complet
Il existe une autre solution qui consiste à effectuer un rechargement complet du dispositif VRA au niveau 18.01f :

*AVERTISSEMENT :* avec cette procédure, vous pouvez ignorer l'étape intermédiaire qui consiste à télécharger et à installer deux correctifs, en revanche, vous perdrez toutes vos données durant le rechargement complet à l'aide d'ISO.

1. Procurez-vous l'ISO 18.01f à partir de cet emplacement.
2. Suivez la [procédure normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) pour l'installer et réamorcez le système.

A présent, vous disposez de la version 1801f et le problème de sécurité Spectre est résolu.

## Recommandations complémentaires

Dans la mesure où il n'existe pas de mappage un à un simple de la fonctionnalité entre Vyatta 5400 et {{site.data.keyword.vra_full}}, il est utile de créer une configuration de base de référence pour le dispositif VRA. Un partenaire IBM©, WanClouds, peut vous aider dans le cadre de ce processus et fournir des conseils quant à la création d'une fonctionnalité similaire à Vyatta 5400 sur votre VRA.

Pour plus d'informations sur les problèmes couramment rencontrés durant ce processus de mise à niveau, voir notre [documentation supplémentaire](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
