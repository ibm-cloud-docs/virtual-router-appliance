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

# A propos de...
Virtual Router Appliance (VRA) vous permet de router de manière sélective le trafic réseau privé et public via un routeur d'entreprise doté de fonctions complètes avec pare-feu, régulation de flux, routage basé sur des règles, VPN et un hôte avec d'autres fonctions. VRA améliore les performances, est facile à configurer et présente les avantages de s'exécuter sur un serveur matériel normal ce qui est pratique pour la maintenance. Le matériel est conçu pour traiter la charge de routage de plusieurs réseaux locaux virtuels et peut être commandé avec des liens réseau redondants et des grappes RAID redondantes. Toutes les fonctions VRA sont gérées par les clients. 

IBM Virtual Router Appliance fournit le dernier système d'exploitation Vyatta 5600 pour des serveurs bare metal x86. Il est proposé en offre Haute disponibilité (HA) ou autonome.

**REMARQUE :** Le pare-feu FortiGate Security Appliance (FSA) 10 Gbps est un pare-feu matériel à service exclusif (dédié), haut débit (10 Gbps) avec les fonctions de nouvelle génération, telles qu'un antivirus (AV), un système de prévention contre les intrusions (IPS) et le filtrage Web. Il peut s'utiliser comme alternative à VRA pour atteindre des objectifs similaires. Pour plus d'informations, reportez-vous à la [documentation de FSA](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Pare-feu
Pour protéger votre environnement contre les menaces externes, Virtual Router Appliance peut être utilisé en tant que pare-feu. Vous pouvez ajouter des règles de pare-feu pour autoriser ou refuser le trafic réseau entrant ou sortant sur les ports sur lesquels s'exécute votre application ou vous pouvez filtrer le trafic au sein de vos propres réseaux. Virtual Router Appliance peut également être configuré afin d'effectuer le filtrage des adresses IPv4 et IPv6 avec état pour protéger vos données critiques.

## Passerelle de réseau privé virtuel (VPN)
Connectez votre centre de données sur site ou votre bureau à IBM Cloud à l'aide de la tunnellisation VPN en mettant à disposition Virtual Router Appliance en tant que dispositif de passerelle réseau. Vous pouvez utiliser un tunnel VPN IPsec de site à site pour sécuriser la communication de votre centre de données d'entreprise ou de votre bureau vers votre réseau IBM Cloud. Les autres options VPN sont VPN IPsec d'accès distant (de client à site), OpenVPN, GRE, L2TP et DMVPN.

Consultez les guides de configuration de Brocade VPN dans la section [Supplemental VRA documentation](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)

## Conversion d'adresses réseau NAT (Network Address Translation)
Avec Virtual Router Appliance, vous pouvez mettre à disposition des serveurs d'applications et de base de données sans interface de réseau public tout en autorisant vos serveurs à accéder à Internet en utilisant la conversion d'adresses réseau (NAT) source. Vous pouvez également masquer vos serveurs derrière le dispositif de passerelle avec la conversion d'adresses réseau (NAT) de destination pour une sécurité renforcée.

## Routage au niveau de l'entreprise

Pour les applications multiniveau sur différents réseaux isolés, Virtual Router Appliance vous permet d'établir la connectivité entre ces réseaux avec une plus grande flexibilité. Vous pourrez configurer le routage dynamique à l'aide de BGP, ce qui vous permettra d'annoncer votre propre espace IP public sur les routeurs IBM Cloud. BGP offrira également davantage de flexibilité pour des configurations de réseau privé personnalisées lors de l'utilisation de différents tunnels et de diverses solutions de lien direct. 

Consultez les guides de configuration de Brocade BGP dans la section [Supplemental VRA documentation](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)
