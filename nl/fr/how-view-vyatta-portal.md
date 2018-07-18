---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Comment afficher le système Vyatta dans le portail

Une seule passerelle 1 Gbps a été déployée dans le centre de données San Jose ; elle dispose d'un accès redondant à l'infrastructure publique et privée. Utilisez l'onglet**Unités** sous le menu **Réseau > Dispositifs de passerelle** pour voir la configuration réelle de matériel déployée. 

Notez les liens **2546 Public VLAN**, **2576 Private VLAN** et **Network Hardware Show Details**. Les réseaux sont dédiés et contiennent uniquement des périphériques de passerelle. Ils sont contrôlés par la plateforme de mise à disposition (vous ne pouvez pas mettre à disposition des serveurs normaux via cet onglet). 

Pour afficher les associations de réseau, cliquez sur le menu **Réseau, dispositifs de passerelle** dans le portail Web.

Les ports de commutateur réseau et les cartes Ethernet sur Brocade 5400 vRouter (Vyatta) sont tous configurés par SoftLayer comme des liaisons. Nos associations mettent simplement à jour la configuration de port pour autoriser les VLAN virtuels supplémentaires sélectionnés.<sup>1</sup>

Le menu déroulant **Associer un VLAN** vous permet de sélectionner un VLAN spécifique pour qu'il soit connu de ou "associé" à votre unité Brocade 5400 vRouter depuis votre centre de données. Vous **ne pouvez pas** associer un VLAN qui est protégé par pare-feu de matériel ou un dispositif FortiGate car cela mettrait fin au contrôle du VLAN. Si aucun VLAN supplémentaire n'est répertorié dans le menu déroulant, vous pouvez en commander d'autres en soumettant un ticket à Softlayer.

Vous pouvez voir les VLAN associés au dispositif Brocade 5400 vRouter sous VLAN associés dans la figure 4. L'état par défaut est Ignoré (dans l'ombre, SoftLayer a identifié les ports de commutation de liaison pour inclure 1894, 2254 à partir du VLAN privé et 2007 et 1280 à partir du VLAN public).<sup>2</sup>

Sur l'écran **Détails** (**réseau, Dispositifs de passerelle**) sous le menu déroulant **VLAN associé, Actions**, se trouve l'option **Router le VLAN**. Si vous sélectionnez cette option, vous demandez effectivement au routeur client frontal (FRC) et au routeur client de back end (FCR) SoftLayer de retirer la passerelle par défaut et d'ajouter à la place une route pour les sous-réseaux qui utilisent les VLAN de transit au périphérique Brocade 5400 vRouter. 

Le dispositif Brocade vRouter est désormais la seule route valide vers et depuis le VLAN. Par dessus tout, vous devez vous rappeler que cela ne concerne pas seulement votre trafic d'application, mais également les éventuels services SoftLayer supplémentaires qui doivent à présent passer par votre périphérique.<sup>4</sup>

Il reste à réaliser la configuration côté Brocade 5400 vRouter, car ce qui figure sur le VLAN avec les sous-réseaux n'est plus accessible. Cela signifie également que les nouveaux serveurs mis à disposition dans le cadre du portail Web, ne peuvent pas atteindre le VLAN. 

**Remarques :**

<sup>1</sup> : l'association d'un VLAN consiste à lier un VLAN éligible à un dispositif de passerelle, de sorte qu'il puisse être routé vers la passerelle par la suite. Le processus d'association n'effectue pas le routage automatique d'un VLAN vers une passerelle. Le VLAN continue à utiliser les routeurs FCR et BCR jusqu'à ce qu'il soit manuellement routé vers la passerelle. Les VLAN peuvent être associés à une seule passerelle à la fois et ne doivent pas avoir de pare-feu pour pouvoir être considérés comme éligibles pour association. 

<sup>2</sup>A tout moment, la passerelle peut être contournée de sorte que le trafic soit réorienté sur les routeurs client FCR et BCR. Le contournement du VLAN permet au VLAN de rester associé à la passerelle réseau. 

<sup>3</sup> : les sous-réseaux portables peuvent être achetés et ajoutés à des passerelles principales. 

<sup>4</sup> : les périphériques Brocade 5400 vRouter sont normalement mis à disposition avec l'adresse IP de service SoftLayer prédéfinie. 
