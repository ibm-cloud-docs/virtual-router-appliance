---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Mise à niveau de Vyatta 5400 et réutilisation de ses adresses IP 
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

Cette option vous permet de réutiliser votre périphérique Vyatta 5400 existant en tant que dispositif VRA ({{site.data.keyword.vra_full}}) équivalent et de conserver vos adresses IP associées. Les procédures suivantes fournissent des instructions pour la mise à niveau d'un périphérique Vyatta 5400 autonome ou de deux périphériques Vyatta 5400 fonctionnant en tant que Paire à haute disponibilité (HA). 

## Mise à niveau d'un périphérique Vyatta 5400 autonome 
{: #upgrading-a-stand-alone-vyatta-5400}

Pour mettre à niveau un périphérique Vyatta 5400 vers un dispositif IBM Virtual Router, procédez comme suit : 

1. Assurez-vous d'avoir sauvegardé votre périphérique Vyatta 5400 et stocké les données à deux emplacements différents. Cela inclut les clés SSH, les certificats SSL, les scripts et tout autre fichier nécessaire à la restauration de la configuration actuelle de Vyatta 5400, si nécessaire. 
2. Rechargez votre périphérique Vyatta 5400 sur un dispositif {{site.data.keyword.vra_full}} par défaut en suivant les instructions de [cette rubrique](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os). 

  Un rechargement du système d'exploitation génère un nouveau mot de passe pour les ID utilisateur racine et Vyatta.
  {: note}

4. Notez le nouveau mot de passe du dispositif {{site.data.keyword.vra_full}}.
5. Configurez le VRA rechargé avec les paramètres souhaités en suivant [ces instructions](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance). 

## Mise à niveau d'une Paire à haute disponibilité Vyatta 5400 
{: #upgrading-a-vyatta-5400-high-availability-pair}

Pour mettre à niveau deux périphériques Vyatta 5400 en une Paire à haute disponibilité, procédez comme suit : 

1. Identifiez le périphérique Backup Vyatta 5400 et rechargez-le d'abord en tant que dispositif {{site.data.keyword.vra_full}}, en suivant la procédure ci-dessus. 

  Idéalement, une configuration HA fonctionnelle affiche toutes les interfaces en tant que BACKUP ou MASTER sur un noeud particulier. En cas de doute, utilisez la commande `show vrrp` pour confirmer.
  {: note}
2. Configurez le VRA rechargé avec les paramètres souhaités en suivant [ces instructions](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance). 
3. Master Vyatta 5400 et Backup {{site.data.keyword.vra_full}} forment une paire VRRP. Modifiez le dispositif Backup {{site.data.keyword.vra_full}} en cours pour qu'il devienne le maître à l'aide de la commande VRRP `reset VRRP master`. 

  Cette action provoque une panne dans les sessions existantes et l'état des sessions est répertorié comme étant perdu. Toute session NAT existante est également réinitialisée.
  {: note}

5. Vérifiez que le nouveau dispositif Master {{site.data.keyword.vra_full}} fonctionne comme vous le souhaitez. 
6. Exécutez la procédure de rechargement ci-dessus sur le périphérique Backup Vyatta 5400 en cours pour le mettre à niveau vers un dispositif VRA. 
7. Après le deuxième rechargement, configurez le dispositif Backup {{site.data.keyword.vra_full}} comme vous le souhaitez, en suivant [ces instructions](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance). 
