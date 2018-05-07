---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuration de base de Vyatta 5400

Pour configurer votre Vyatta 5400, procédez comme suit :

Le réseau local virtuel public 1224, qui est désormais associé et routé, doit être configuré avant de pouvoir transmettre du trafic. Deux éléments d'information sont nécessaires pour terminer la configuration : 

  * La liaison côté public du dispositif Brocade 5400 vRouter
  * La passerelle par défaut de 1224

Notez qu'il s'agit du réseau local virtuel côté public où se trouve l'option compute et non le réseau local virtuel public du dispositif Brocade 5400 vRouter. 

## Dans le portail Softlayer

1\. Dans le portail Web SoftLayer, sous **Devices**, localisez la liaison sur l'onglet **Configuration** pour l'unité Brocade 5400 vRouter. Supposons que <span style="text-decoration: underline">eth1=bond1</span> sur votre unité. 

2\. Sélectionnez **Network > IP Management > VLANs** sur le portail Web afin de trouver la passerelle par défaut pour le réseau local virtuel 1224.

3\. Cliquez sur votre réseau local virtuel dans la liste, puis cliquez sur le **sous-réseau** sur lequel votre passerelle est visible. Notez les détails de routage CIDR situés après la barre oblique.  

## Dans l'interface graphique Vyatta

4\. Depuis le tableau de bord, cliquez sur l'onglet **Configuration**. 

5\. Développez **Interfaces > Bonding > bond1** sur le côté gauche de l'écran, puis mettez en évidence **vif**.

6\. Entrez le nom des réseaux locaux virtuels dans la zone **vif** field (1224 dans notre cas) et cliquez sur le bouton **Create**. Le processus dure quelques secondes. 

7\. Sélectionnez l'élément **vif 1224** nouvellement créé sous l'icône **vif**. 

8\. Cochez la case sous **dhcp** et tapez l'adresse IP de passerelle par défaut et la notation CIDR de masque du réseau local virtuel que vous souhaitez faire passer par Brocade 5400 vRouter (par exemple, VLAN 1224).

9\. Cliquez sur le bouton **Set**, puis sur **Commit**.

10\. Cliquez sur **Save** dans la barre de menu du milieu, faute de quoi, la configuration par défaut sera rétablie lorsque le système sera redémarré. 

**REMARQUE :** l'annulation de la configuration peut s'avérer très utile si vous interrompez votre configuration pendant que vous testez vos modifications. Tant que les modifications ne sont pas sauvegardées, vous pouvez redémarrer le serveur à partir du portail Web et restaurer votre configuration précédente. 

11\. Cliquez sur l'onglet Statistics et ouvrez la nouvelle interface afin de vérifier et surveiller le trafic. 

## Configuration du réseau local virtuel privé à l'aide de l'interface de ligne de commande

L'interface de ligne de commande propose les deux modes commande suivants : opérationnel et configuration. 

Le mode opérationnel permet d'accéder à des commandes opérationnelles pour :

  * Afficher et effacer des informations
  * Activer et désactiver le débogage
  * Configurer des paramètres de terminal
  * Charger et sauvegarder la configuration
  * Redémarrer le système

Le mode configuration permet d'accéder à des commandes pour :

  * Créer
  * Modifier
  * Supprimer
  * Valider
  * Afficher des informations de configuration
  * Naviguer dans la hiérarchie de la configuration

Lorsque vous vous connectez au système, celui-ci est en mode opérationnel ; vous devez passer en mode configuration pour les commandes.

**REMARQUE :** vous pouvez déterminer le mode dans lequel vous travaillez, opérationnel ou configuration, en fonction de l'invite. Vous êtes en mode opérationnel si l'invite est # et vous êtes en mode configuration si l'invite est $.

Pour configurer le réseau local virtuel privé à l'aide de l'interface de ligne de commande, procédez comme indiqué ci-après. N'oubliez pas que les valeurs nécessaires pour configurer le réseau local virtuel sont les suivantes :

  * Nom du réseau local virtuel qui doit être routé via l'unité Brocade 5400 vRouter (2254)
  * Passerelle et masque (format CIDR) du réseau local virtuel qui doit être routé (10.52.69.201/29)
  * Nom de liaison privée de l'unité Brocade 5400 vRouter (bond0)

1. Ouvrez une session SSH dans votre Brocade 5400 vRouter (adresse IP publique ou privée) en utilisant **vyatta** comme **nom d'utilisateur** ; indiquez le mot de passe approprié lorsque vous y êtes invité. 

   **REMARQUE :** vous devez créer un nouvel utilisateur dans Brocade 5400 vRouter et désactiver l'utilisateur initial par défaut `vyatta`.

2. Configurez l'interface VIF :

  * Tapez *configure* à l'invite de commande pour entrer en mode configuration. 
  * Tapez *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* à l'invite de commande pour définir l'interface VIF. 
  * Tapez *commit* à l'invite de commande pour valider les paramètres. 
  * Tapez *save* pour sauvegarder les paramètres. 
  * Tapez *exit* pour quitter le mode opérationnel. 

3\. Tapez *show interfaces* pour vérifier les paramètres que vous venez de valider. 

4\. Routez les réseaux locaux virtuels restants via l'unité Brocade 5400 vRouter. 
