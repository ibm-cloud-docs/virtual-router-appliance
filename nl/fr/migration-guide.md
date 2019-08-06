---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# Présentation de la migration 
{: #migration-overview}

Les utilisateurs d'un ancien système Vyatta 5400 doivent migrer vers {{site.data.keyword.vra_full}} (VRA) dès que possible, car Vyatta 5400 a été retiré depuis le 31 mars 2019. Pour lire l'annonce complète de fin de support et pour plus d'informations, cliquez [ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## Avantages de la mise à niveau 
{: #how-upgrading-can-benefit-you}

Outre une variété de nouvelles fonctionnalités offertes par le dispositif {{site.data.keyword.vra_full}} d'IBM, un certain nombre d'améliorations sont également apportées. Reportez-vous à notre [FAQ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) pour plus de détails.

La mise à niveau vers un nouveau périphérique implique une configuration différente de celle de Vyatta 5400. Par conséquent, la configuration (fichier) de Vyatta 5400 n'est pas transférée vers votre nouveau dispositif.
{: note}

## Documentation sur la migration 
{: #migration-documentation}

Pour vous aider à migrer du Vyatta 5400, nous avons préparé la documentation et le support suivants : 

| Documentation sur la migration | Description |
| ------------- | ------------- |
| [Mise à niveau de Vyatta 5400 et réutilisation de ses adresses IP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Instructions sur la mise à niveau de Vyatta 5400 vers un dispositif {{site.data.keyword.vra_full}} équivalent, lors de la réutilisation des adresses IP de Vyatta 5400. |
| [Migration de Vyatta 5400 vers Juniper vSRX ou Fortigate Security Appliance (FSA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Instructions sur la migration vers un dispositif Juniper vSRX ou Fortigate Security Appliance. Cette option ne vous permet pas de réutiliser votre périphérique Vyatta 5400 existant ni de conserver les adresses IP associées. |
| [Problèmes de migration courants](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Informations sur les problèmes les plus fréquemment rencontrés ou les changements de comportement que vous pouvez rencontrer après la migration d'un périphérique Vyatta 5400 vers un dispositif {{site.data.keyword.vra_full}}. Dans de nombreux cas, des solutions de contournement sont proposées.|

Si vous devez simplement commander un nouveau dispositif {{site.data.keyword.vra_full}}, [cliquez ici](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started) pour plus de détails.
{: note}
