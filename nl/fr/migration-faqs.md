---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Foire aux questions concernant la fin du support de Vyatta 5400 
{: #vyatta-5400-end-of-support-faqs}

Les questions fréquemment posées lors de la migration de Vyatta 5400 sont les suivantes. 

## Pourquoi la date de fin du support (EoS) de Vyatta 5400 est-elle le 31 mars 2019 ? 
{: faq}

En septembre 2017, Vyatta 5400 a annoncé que sa date de fin du support (EoS) serait le 20 février 2018, conformément aux règles de cycle de vie d’IBM en matière de support, soit six mois après la date de disponibilité générale (GA) de la prochaine version, en juillet, à savoir le dispositif IBM Virtual Router Appliance (VRA). 

Pour respecter les délais de migration des clients, la date EoS de Vyatta 5400 a été prolongée jusqu'au 31 mars 2019. Dans la mesure où le logiciel Debian 7 n'est plus pris en charge par la communauté Debian Open Source, AT&T n'envisage pas de prolonger le support fournisseur. 

[Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) pour consulter l'annonce officielle de la fin du support.
{: important}

## Que signifie "Fin de support" pour moi en tant que client ? 
{: faq}

Après la date de fin de support, AT&T ne fournit plus aucun correctif de code et n'accepte plus d'escalade de support émanant d'IBM. 

De même, IBM Cloud Support ne résout plus les problèmes de configuration ou de réseau sur les déploiements Vyatta 5400. Le support est limité aux demandes de niveau matériel (disque dur, RAM, etc.), à la connectivité d'alimentation et à la connectivité hors bande (IPMI). 

Nous recommandons vivement à nos clients de prendre des mesures immédiates pour migrer vers une autre solution, tel que le dispositif Virtual Router Appliance (VRA basé sur Vyatta 5600) ou Juniper vSRX. [Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) pour commencer.

## Que se passe-t-il si j'exécute toujours mes charges de travail IBM Cloud à l'aide de Vyatta 5400 après le 31 mars ? 
{: faq}

Votre périphérique Vyatta 5400 continue de fonctionner après le 31 mars. Toutefois, vos environnements d'entreprise et d'application peuvent être exposés à des menaces potentielles pour la sécurité et à d'autres violations de contrefaçon en raison de vulnérabilités latentes du logiciel Vyatta 5400. 

Si vous rencontrez un problème réseau qui nuit à votre environnement d'entreprise et d'application et que la cause remonte au Vyatta 5400, transmettez ce problème à notre responsable des offres 5400 car le support n'est plus disponible ni chez IBM ni chez AT&T. Vous pouvez contacter l'équipe de gestion des offres par courrier électronique à l'adresse `nwom@us.ibm.com`.

## Qu'en est-il de mon matériel Bare Metal Server sous-jacent - est-il toujours pris en charge ? 
{: faq}

Les remplacements de matériel sont pris en charge, mais si le traitement des incidents indique que votre problème est lié au système d'exploitation Vyatta, vous serez invité à migrer immédiatement vers une offre matérielle prise en charge. [Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) pour commencer.

## En tant que client propriétaire d'un Vyatta 5400, que dois-je faire avant le 31 mars 2019 ? 
{: faq}

Les clients possédant un Vyatta 5400 doivent migrer vers VRA (Vyatta 5600), Juniper vSRX ou Fortigate Security Appliance (FSA) 10G. Le dispositif VRA (Vyatta 5600) est toujours entièrement pris en charge. Il n’y a pas de date de fin de prise en charge actuelle ou prévue pour le VRA provenant d’IBM Cloud ou d’AT&T. [Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) pour commencer.

  Les VRA et vSRX sont des périphériques gérés par le client.
  {: note}

## IBM fournit-il un support pour la migration du Vyatta 5400 vers le VRA, le vSRX ou le FSA 10G ? 
{: faq}

Le service de conversion de configuration Vyatta 5400 à VRA (5600) est toujours disponible : 

* Pour les clients existants, IBM Cloud propose une offre gratuite pour les aider à reformater votre configuration Vyatta 5400 existante dans les formats Virtual Router Appliance (VRA), Juniper vSRX ou Fortigate Security Appliance (FSA) 10G. Pour envoyer une demande pour le service de conversion de configuration, envoyez un courrier électronique à nwom@us.ibm.com avec l'objet suivant : `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx. `.

  Veillez à insérer votre choix d'application à la place de `aaaaaaaa` (Virtual Router Appliance, Juniper vSRX ou Fortigate Security Appliance (FSA) 10G), et votre numéro de compte spécifique à la place de `xxxxxx` dans la ligne d'objet.
  {: note}

* Wanclouds, notre partenaire dans ce processus de configuration de conversion, a réalisé plusieurs centaines de missions de migration réussies. Il transforme votre périphérique Vyatta 5400 existant pour créer une fonctionnalité similaire sur la plateforme Vyatta 5600. Il fournit ses services à deux niveaux, décrits ci-dessous : 

  <img src="images/tiers.png" alt="dessin" style="width: 700px;"/>

[Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) pour commencer.

##  Existe-t-il des services de migration payants supplémentaires disponibles auprès des Partenaires commerciaux IBM pour la migration à partir de Vyatta 5400 ?
{: faq}

Plusieurs Partenaires commerciaux IBM fournissent une assistance technique payante pour les migrations de Vyatta 5400. [Cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) pour plus d'informations.

## Existe-t-il un contact de support pour l'équipe responsable des offres de Vyatta 5400 chez IBM où je peux poser des questions relatives à ma migration de Vyatta 5400 ? 
{: faq}

Contactez l'équipe responsable des offres IBM Vyatta 5400 et VRA Network pour toute question sur le site `nwom@us.ibm.com`. Vous pouvez également la contacter en utilisant l'espace de travail IBM Watson Cloud Platform : `#vyatta-migration`

## Quelles ressources supplémentaires sont disponibles pour m'aider dans cette migration ? 
{: faq}

Pour plus d'informations, consultez les ressources documentaires suivantes du dispositif Virtual Router Appliance : 

  * [Initiation à IBM Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [A propos du dispositif VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Présentation de la migration de Vyatta 5400](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
