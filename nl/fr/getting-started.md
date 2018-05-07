---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Initiation
Pour débuter avec IBM Virtual Router Appliance (VRA), accédez à la page des commandes dans le portail client :

1. Dans votre navigateur, ouvrez le [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){: new_window} et connectez-vous à votre compte.
2. Dans la navigation du portail client, sélectionnez **Réseau > Dispositifs de passerelle**.
3. Dans la page de liste **Dispositifs de passerelle**, cliquez sur **Commander passerelle**.
4. Dans la page **Commander**, sélectionnez le centre de données désiré dans le menu déroulant, puis choisissez le type de matériel de serveur.

    **REMARQUE :** La configuration minimale requise de VRA pour les serveurs nécessite 8 Go de mémoire RAM et un coeur d'UC pour chaque capacité réseau de 10 Gbps. Par exemple, un système avec des doubles liaisons montantes publiques et privées de 10 Gbps nécessite au moins quatre coeurs. Réservez plus que la valeur par défaut du disque si vous envisagez d'exécuter des diagnostics réseau produisant des journaux détaillés. Enfin, si votre intention est de configurer des services VPN avec chiffrement, vous souhaiterez peut-être ajouter des coeurs supplémentaires. En ajoutant des coeurs supplémentaires pour des services VPN, vous vous assurez que le dispositif VRA ne s'enlisera pas dans une charge élevée liée à des opérations simultanées de routage et de chiffrement/déchiffrement de données. 

5. Sur la page de **configuration de votre dispositif de passerelle réseau**, sélectionnez l'option **Paire à haute disponibilité** si vous le souhaitez, la version VRA et la vitesse des liaisons montantes sur le réseau.
6. Passez en revue vos sélections, puis cliquez sur **Ajouter à la commande** et la commande sera vérifiée automatiquement.
7. Sur la page **PAIEMENT**, si vous disposez déjà de réseaux locaux virtuels (VLAN) dans le centre de données sélectionné, sélectionnez les VLAN de back-end qui doivent être protégés. Indiquez un nom d'hôte et un nom de domaine pour votre unité VRA. Cochez toutes les cases des dispositions relatives aux contrats pour les services IBM Cloud et les services tiers. Cliquez ensuite sur **Soumettre commande**.

Une fois la commande approuvée, la mise à disposition de l'unité VRA démarre automatiquement. Lorsque le processus de mise à disposition est terminé, la nouvelle unité VRA s'affiche dans la page de liste **Dispositifs de passerelle** du portail client. Cliquez sur le nom de la passerelle pour ouvrir la page **Détails de la passerelle**, puis cliquez sur chaque membre de la passerelle pour ouvrir la page **Détails de l'unité**. Vous y trouverez les adresses IP, le nom d'utilisateur et le mot de passe de connexion correspondant à l'unité.  
 
## Réseaux locaux virtuels (VLAN) et rôle du dispositif de passerelle
Un VLAN (réseau local virtuel) est un mécanisme qui partage un réseau physique en de nombreux segments virtuels. Par commodité, le trafic en provenance de plusieurs VLAN sélectionnés peut être distribué via un seul câble réseau, un processus connu sous le nom de "jonction" (trunking).

VRA est livré en deux parties : un ou plusieurs serveurs VRA et l'installation du dispositif de passerelle. Le dispositif de passerelle vous fournit une interface (interface graphique et API) pour sélectionner les VLAN que vous voulez associer à votre unité VRA. L'association d'un VLAN à un dispositif de passerelle reroute (ou "joint") ce VLAN et tous ses sous-réseaux à votre unité VRA, vous donnant le contrôle en matière de filtrage, de transfert et de protection. Les serveurs d'un VLAN associé sont uniquement accessibles à partir d'autres VLAN en passant par votre unité VRA. Il n'est pas possible de se passer de l'unité VRA sauf si vous contournez ou dissociez le VLAN.

Par défaut, un nouveau dispositif de passerelle est associé à deux VLAN de "transit" non amovibles, un pour le réseau public et un pour le réseau privé. Ils sont en principe utilisés à des fins administratives et peuvent être sécurisés séparément par des commandes VRA.

L'unité VRA ne peut gérer que les VLAN qui lui sont associés via le dispositif de passerelle.

Pour plus d'informations sur la gestion des VLAN à partir de l'écran des détails du dispositif de passerelle, reportez-vous à [Gérer les réseaux locaux virtuels (VLAN)](manage-vlans.html).
