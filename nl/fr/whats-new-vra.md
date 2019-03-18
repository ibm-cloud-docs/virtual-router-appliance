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


# Récentes mises à jour pour IBM Virtual Router Appliance
{: #recent-updates-for-ibm-virtual-router-appliance}

Cette section présente les nouveautés et les fonctions mises à jour dans IBM© Virtual Router Appliance (VRA).

## Août 2018
### Version 18.x du système d'exploitation Brocade
La version 18.x du système d'exploitation Brocade est désormais disponible pour les dispositifs Virtual Router Appliance. Parmi les autres nouveautés, cette version fournit une résolution pour la violation de sécurité Spectre. 

Les nouveautés dans VRA 18.x sont présentées dans les rubriques suivantes :

* [Configuration d'un tunnel IPsec qui fonctionne avec des pare-feux de zone](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [Configuration d'une interface VFP avec IPsec et des pare-feux de zone](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [Utilisation de NAT avec IPsec basé sur le préfixe](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [Traitement des incidents liés à votre interface VFP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

Dans le cadre d'une migration depuis Vyatta 5400, le meilleur moyen d'effectuer une mise à niveau vers 18.x consiste à utiliser la [procédure normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) d'un rechargement de système d'exploitation complet.

Dans la mesure où il n'existe pas de mappage un à un simple de la fonctionnalité entre Vyatta 5400 et Virtual Router Appliance, il est utile de créer une configuration de base de référence pour le dispositif VRA. Un partenaire IBM, WanClouds, peut vous aider dans le cadre de ce processus et fournir des conseils quant à la création d'une fonctionnalité similaire à Vyatta 5400 sur votre VRA.

Pour en savoir plus sur les problèmes communs rencontrés lors de ce processus de mise à niveau, veuillez vous reporter à votre [documentation additionnelle](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).


