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

# Création d'un périmètre sécurisé
L'un des aspects essentiels de l'isolement de réseau est l'établissement d'un périmètre sécurisé.  Un périmètre sécurisé contrôle le trafic entre l'Internet public et les actifs du client hébergés dans IBM© Cloud.  Il permet également d'activer la connectivité directe avec l'entreprise du client via des tunnels de réseau privé virtuel et IBM Cloud Direct.

Un périmètre sécurisé utilise un segment SPS (Secure Perimeter Segment) pour séparer les réseaux entre les actifs. Cette séparation offre plusieurs avantages, notamment le contrôle d'accès et l'isolement du trafic de service entre les segments. Un segment SPS (Secure Perimeter Segment) est constitué de deux réseaux locaux virtuels (VLAN), un VLAN frontal et un VLAN de back end, et d'un dispositif VRA connecté aux VLAN afin de gérer le trafic vers et depuis le segment SPS. Un périmètre sécurisé peut inclure plusieurs segments SPS (par exemple, à des fins de haute disponibilité).

## Configuration du périmètre sécurisé

Les étapes de configuration d'un périmètre sécurisé sont présentées ci-après.  Pour obtenir une description détaillée de ces étapes, voir [Set up a Secure Perimeter in the IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter){:new_window}.

1. Configurez les droits dans IBM Cloud et l'infrastructure permettant d'accéder aux services de cloud et aux composants d'infrastructure impliqués dans le périmètre sécurisé.
2. Créez une limite extérieure dans l'Internet public via un VLAN public et un pare-feu. Cette limite extérieure sera utilisée pour isoler le segment SPS.
3. Placez le segment dans cette limite.
4. Configurez une passerelle pour relier le VLAN public et le segment SPS.
5. Créez un dispositif VRA ({{site.data.keyword.vra_full}}) configuré.
6. Créez et configurez un ou plusieurs segments SPS sécurisés.
7. Créez un VLAN frontal.
8. Créez un VLAN de back end.
9. Créez une paire Vyatta.
10. Configurez Vyatta.
11. Associez des VLAN à une passerelle.
12. Configurez une interface de passerelle pour un VLAN public.
13. Configurez une passerelle sur une machine maître Vyatta.
14. Configurez une passerelle sur une machine de secours Vyatta.
15. Configurez une interface de passerelle pour un VLAN privé.
16. Configurez une passerelle sur une machine maître Vyatta.
17. Configurez une passerelle sur une machine de secours Vyatta.
18. Activez le réglage de réseau.
19. Configurez des règles de pare-feu.
20. Configurez un cluster Kubernetes.
21. Configurez des adresses IP fournies par l'utilisateur (facultatif).
22. Déployez un cluster Kubernetes.

## Solutions dédiées dans IBM Cloud
Un périmètre sécurisé, associé à l'isolement de calcul et au chiffrement de données, constitue une solution dédiée complète dans IBM Cloud public.  Pour une description des modèles de solutions dédiées cloud, voir [Implementing a Dedicated Solution Pattern ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/){:new_window}.
