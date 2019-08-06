---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

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

# Migration de Vyatta 5400 vers Juniper vSRX ou Fortigate Security Appliance (FSA) 10 Gbps 
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Outre la mise à niveau de votre périphérique Vyatta 5400 vers un dispositif {{site.data.keyword.vra_full}}, vous pouvez migrer vers certaines des options les plus économiques, fiables et sécurisées du marché : Juniper vSRX et Fortigate Security Appliance. Cependant, vous ne pourrez pas réutiliser le périphérique Vyatta 5400 existant ni conserver vos adresses IP associées. 

Les procédures suivantes fournissent des instructions pour la mise à niveau d'un périphérique Vyatta 5400 autonome ou de deux périphériques Vyatta 5400 fonctionnant en tant que Paire à haute disponibilité (HA), vers Juniper vSRX ou FSA. 

## Mise à niveau d'un périphérique Vyatta 5400 autonome 
{: #upgrading-a-stand-alone-vyatta-5400}

Pour mettre à niveau un seul périphérique Vyatta 5400 vers Juniper vSRX ou FSA-10G avec le moins de temps d'indisponibilité, nous vous recommandons de suivre la procédure suivante. 

1. Assurez-vous d'avoir sauvegardé votre périphérique Vyatta 5400 et stocké les données à deux emplacements différents. Cela inclut les clés SSH, les certificats SSL, les scripts et tout autre fichier nécessaire à la restauration de la configuration actuelle de Vyatta 5400, si nécessaire. Pour ce faire, appliquez [les instructions suivantes](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contactez nwom@us.ibm.com pour demander la conversion de votre configuration.

3. Commandez le nouveau périphérique en suivant les instructions pour [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) ou pour [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

  Il s'agit d'une bonne occasion d’envisager de commander une solution à haute disponibilité. {: note}

4. Chargez la configuration que vous avez reçue de nwom@us.ibm.com sur le nouveau périphérique commandé.

5. Prévoyez du temps pour migrer vos réseaux VLAN vers votre nouveau périphérique.

  Cette étape nécessite un bref temps d'indisponibilité.
  {: note}

6. Testez et vérifiez que votre nouveau périphérique fonctionne correctement. 

7. Ouvrez un ticket d'assistance pour annuler votre périphérique Vyatta 5400. 

## Mise à niveau d'une Paire à haute disponibilité Vyatta 5400 
{: #upgrading-a-vyatta-5400-high-availability-pair}

Pour mettre à niveau deux périphériques Vyatta 5400 en une Paire à haute disponibilité, procédez comme suit : 

1. Assurez-vous d'avoir sauvegardé votre périphérique 5400 et stocké les données à deux emplacements différents. Cela inclut les clés SSH, les certificats SSL, les scripts et tout autre fichier nécessaire à la restauration de la configuration actuelle du périphérique Vyatta 5400, si nécessaire. Pour ce faire, appliquez [les instructions suivantes](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contactez nwom@us.ibm.com et demandez la conversion de votre configuration.

3. Commandez le nouveau périphérique en suivant les instructions pour [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) ou pour [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

4. Chargez la configuration que vous avez reçue de nwom@us.ibm.com sur le nouveau périphérique commandé.

5. Prévoyez du temps pour migrer vos réseaux VLAN vers votre nouveau périphérique.

  Cette étape nécessite un bref temps d'indisponibilité.
  {: note}

6. Testez et vérifiez que votre nouveau périphérique fonctionne correctement. 

7. Ouvrez un ticket d'assistance pour annuler vos périphériques Vyatta 5400. 
