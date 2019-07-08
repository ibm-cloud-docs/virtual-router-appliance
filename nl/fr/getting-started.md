---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Initiation à IBM Virtual Router Appliance
{: #getting-started}

IBM© Virtual Router Appliance fournit le dernier système d'exploitation Vyatta 5600 pour des serveurs bare metal x86. Il est proposé en configuration à haute disponibilité (HA) ou autonome et vous permet de router de manière sélective le trafic réseau privé et public via un routeur d'entreprise doté de fonctions complètes avec pare-feu, régulation de flux, routage basé sur des règles, VPN et d'autres fonctions. 

La configuration minimale requise de VRA pour les serveurs nécessite 8 Go de mémoire RAM et un coeur d'UC pour chaque capacité réseau de 10 Gbit/s. Par exemple, un système avec des doubles liaisons montantes publiques et privées de 10 Gbps nécessite au moins quatre coeurs. En outre, si votre intention est de configurer des services VPN avec chiffrement, vous souhaiterez peut-être ajouter des coeurs supplémentaires. L'ajout de coeurs supplémentaires pour les services VPN garantit que le VRA ne sera pas encombré par de lourdes charges lors du routage et du chiffrement/déchiffrement simultané des données. 

## Commande d'un dispositif Virtual Router Appliance
{: #order-vra}

Pour commander un dispositif VRA, procédez comme suit :

1. Dans votre navigateur, ouvrez la page Dispositifs de passerelle dans la [console d'interface graphique IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} et connectez-vous à votre compte. 

  Vous pouvez également accéder à cette page en sélectionnant le menu de navigation dans l'angle supérieur gauche du [Catalogue IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com) et en sélectionnant **Infrastructure classique  > Réseau > Dispositif de passerelle**.
  {: tip}

2. Dans la section **Fournisseur de passerelle**, sélectionnez l'option **AT&T** (quand elle est sélectionnée, une coche bleue apparaît sur le bouton). Dans le menu déroulant de ce même bouton, choisissez la bande passante (20 Gbps ou 2 Gbps). 

  	<img src="images/ordering_vra.png" alt="dessin" style="width: 500px;"/>

3. Dans la section **Dispositif de passerelle**, entrez les informations **Nom d'hôte** et **Domaine**. Ces zones sont déjà renseignées avec les informations par défaut. Assurez-vous donc que les valeurs sont correctes. Cochez l'option **Haute disponibilité** si vous le souhaitez, puis sélectionnez l'**Emplacement** du centre de données souhaité et le **Pod** spécifique dans le menu déroulant. 

  Seuls les pods ayant déjà un VLAN associé s'affichent ici. Si vous souhaitez mettre à disposition votre dispositif de passerelle dans un pod ne figurant pas dans la liste, créez d'abord un réseau VLAN.
  {: note}

4. Dans la section **Configuration**, choisissez votre processeur en sélectionnant vos clés RAM et SSH (si nécessaire).

  <img src="images/ordering_vra_2.png" alt="dessin" style="width: 600px;"/>
  
  Le processeur approprié est automatiquement sélectionné en fonction de la version de la licence que vous choisissez à l'étape deux. Vous pouvez cependant choisir différentes configurations de RAM.
  {: note}

5. Dans la section **Disques de stockage**, choisissez les options correspondant à vos besoins en stockage. 

  Les options RAID0 et RAID1 offrent une protection supplémentaire contre la perte de données, tout comme les unités de secours à chaud (composants de sauvegarde pouvant être mis en service immédiatement en cas de défaillance d'un composant principal).
  {: note}

  Vous pouvez installer jusqu'à quatre disques par VRA. La "taille de disque" associée à une configuration RAID correspond à la taille de disque utilisable, car les configurations RAID sont mises en miroir.
  {: note}

  Réservez plus que la valeur par défaut du disque si vous envisagez d'exécuter des diagnostics réseau produisant des journaux détaillés.
  {: tip}

6. Dans la section **Interface réseau**, sélectionnez les **Vitesses de port pour la liaison montante**. La sélection par défaut est une interface unique, mais il existe également des options redondantes et privées uniquement. Choisissez celle qui correspond le mieux à vos besoins. 

  La section **Modules complémentaires** de l'Interface réseau vous permet de sélectionner une adresse IPv6 si nécessaire et vous indique les options par défaut supplémentaires incluses.  
  
8. Passez en revue vos sélections, vérifiez que vous avez lu les contrats de service tiers, puis cliquez sur **Créer**. La commande est vérifiée automatiquement. 

Une fois la commande approuvée, la mise à disposition du dispositif Virtual Router Appliance démarre automatiquement. Lorsque le processus de mise à disposition est terminé, le nouveau dispositif VRA s'affiche dans la page de la liste des Dispositifs de passerelle. Cliquez sur le nom de la passerelle pour ouvrir la page Détails de la passerelle. Vous y trouverez les adresses IP, le nom d'utilisateur et le mot de passe de connexion correspondant à l'unité.  

  <img src="images/gateway_details.png" alt="dessin" style="width: 500px;"/>

N'oubliez pas que dès que vous commandez et configurez votre dispositif VRA à partir du Catalogue IBM Cloud, vous devez également configurer le dispositif proprement dit avec les mêmes paramètres.
{: tip}

## Réseaux locaux virtuels (VLAN) et rôle du dispositif de passerelle
{: #vlans-and-the-gateway-appliance-s-role}

Un VLAN (réseau local virtuel) est un mécanisme qui partage un réseau physique en de nombreux segments virtuels. Par commodité, le trafic en provenance de plusieurs VLAN sélectionnés peut être distribué via un seul câble réseau, un processus connu sous le nom de "jonction" (trunking).

Le dispositif Virtual Router Appliance est livré en deux parties : un ou plusieurs serveurs VRA et l'installation du dispositif de passerelle. Le dispositif de passerelle vous fournit une interface (interface graphique et API) pour sélectionner les VLAN que vous voulez associer à votre dispositif VRA. L'association d'un VLAN à un dispositif de passerelle reroute (ou "joint") ce VLAN et tous ses sous-réseaux à votre dispositif VRA, vous donnant le contrôle en matière de filtrage, de transfert et de protection. Pour chaque VLAN associé au dispositif de passerelle, ce VLAN est autorisé sur les ports de commutateur auxquels le VRA est connecté. Tout sous-réseau de ce VLAN est routé de manière statique vers l'IP VRRP publique du VRA (si le sous-réseau est un sous-réseau public) ou routé de manière statique vers l'adresse IP VRRP privée du VRA (si le sous-réseau est un sous-réseau privé). Ce routage est effectué au niveau du routeur sur lequel se trouve le VRA, qui est le routeur FCR (Frontend Customer Router) ou le routeur BCR (Backend Customer Router) pour le trafic public et privé, respectivement.  

Sachez que VRRP est désactivé par défaut et qu'il doit être activé pour que le trafic VLAN fonctionne, y compris sur des périphériques Vyatta autonomes. Il s'agit d'une conséquence du routage des sous-réseaux sur les VLAN associés vers l'adresse IP VRRP ou l'adresse virtuelle attribuée au VRA. Pour plus d'informations, reportez-vous à [Adresses IP virtuelles (VIP) VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses).
{: important}

Les serveurs d'un VLAN associé sont uniquement accessibles à partir d'autres VLAN en passant par votre dispositif Virtual Router Appliance. Il n'est pas possible de se passer du dispositif VRA sauf si vous contournez ou dissociez le VLAN.

Par défaut, un nouveau dispositif de passerelle est associé à deux VLAN de "transit" non amovibles, un pour le réseau public et un pour le réseau privé. Ils sont en principe utilisés à des fins administratives et peuvent être sécurisés séparément par des commandes VRA.

Les VLAN de transit sont destinés à des périphériques réseau, tels que des pare-feux ou des équilibreurs de charge afin de leur permettre d'acheminer du trafic tout en maintenant d'autres périphériques, tels que des serveurs ou des conteneurs, isolés d'Internet.

En comparaison, les VLAN "de passerelle" sont des emplacements où les périphériques, tels que des serveurs et des conteneurs, sont hébergés.

Le dispositif VRA ne peut gérer que les VLAN qui lui sont associés via le dispositif de passerelle.

Pour plus d'informations sur la gestion des VLAN à partir de l'écran des détails du dispositif de passerelle, reportez-vous à [Gérer les réseaux locaux virtuels (VLAN)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
