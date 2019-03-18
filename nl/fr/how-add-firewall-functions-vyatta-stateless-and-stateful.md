---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Ajout de fonctions de pare-feu à Vyatta 5400 (sans état et avec état)
{: #adding-firewall-functions-to-vyatta-5400-stateless-and-stateful-}

Appliquer des jeux de règles de pare-feu à chaque interface est une méthode de tunnellisation possible avec des unités Brocade 5400 vRouter (Vyatta). Chaque interface comporte trois instances de pare-feu possibles - In, Out et Local - et des règles peuvent être appliquées à chacune d'elles. L'action par défaut est Drop, avec des règles qui permettent à un trafic spécifique d'être appliqué depuis la règle 1 à N. Dès lors qu'une correspondance est établie, le pare-feu applique l'action spécifique de la règle correspondante.

Pour l'une quelconque des trois instances de pare-feu ci-dessous, **une seule** action peut être appliquée.

* **IN :** le pare-feu filtre les paquets entrant dans l'interface et traversant le système Brocade. Vous devrez autoriser certaines plages d'adresses IP SoftLayer à accéder aux systèmes front-end et back-end à des fins de gestion (ping, surveillance, etc.).
* **OUT :** le pare-feu filtre les paquets sortant de l'interface. Ces paquets peuvent transiter par ou provenir du système Brocade.
* **LOCAL:** le pare-feu filtre les paquets destinés au système Brocade vRouter proprement dit via l'interface système. Vous devez établir des restrictions sur les ports d'accès entrant dans l'unité Brocade vRouter à partir d'adresses IP externes qui ne sont pas restreintes.

Suivez les étapes décrites ci-dessous pour définir un exemple de règle de pare-feu afin de désactiver le protocole ICMP (Internet Control Message Protocol) *(ping - IPv4 echo reply message)* sur les interfaces de votre unité Brocade 5400 vRouter (il s'agit d'une action sans état ; une action avec état sera examinée ultérieurement) :

1\. Tapez *show configuration commands* à l'invite de commande pour voir les configurations qui sont définies. Une liste de toutes les commandes que vous avez définies sur votre unité apparaîtra (cela peut être pratique si vous décidez d'effectuer une migration et souhaitez voir toutes vos configurations). Notez la commande *set firewall all-ping 'enable'*, qui indique que le protocole ICMP est toujours activé pour votre unité.

2\. Tapez *configure*.

3\. Tapez *set firwall all-ping 'disable'*.

4\. Tapez *commit*.

A présent, si vous essayez d'envoyer une commande PING à votre unité Brocade 5400 vRouter, vous ne recevrez plus aucune réponse.

Pour que des règles de pare-feu puissent être affectées à du trafic routé, des règles doivent être appliquées aux **interfaces** ou aux **zones créées** de l'unité Brocade 5400 vRouter, puis être appliquées aux zones.

Pour cet exemple, des zones seront créées pour les réseaux locaux virtuels qui ont été utilisés jusqu'à maintenant.

**VLAN = Zone**

bond1 = dmz

bond1.2007 = prod (pour la production)

bond0.2254 = private (pour le développement)

bond1.1280 = réservé à un usage ultérieur

bond1.1894 = réservé à un usage ultérieur

**Création de règles de pare-feu**

Avant de créer effectivement les zones, il convient de créer les règles de pare-feu qui doivent être appliquées aux zones. Créer les règles avant les zones vous permet de les appliquer immédiatement, au lieu de créer la zone, puis les règles, et de devoir revenir à la zone pour l'application de règle.

**Remarque :** une instruction d'action par défaut est définie pour les zones et pour les jeux de règles. A l'aide de Zone-Policies, l'action par défaut est définie par l'instruction zone-policy et est représentée par la règle 10,000. De plus, il est important de garder à l'esprit que l'action par défaut d'un jeu de règles de pare-feu est **drop** pour tout le trafic.

Les commandes suivantes permettent d'effectuer ce qui suit :

* Créer une règle de pare-feu nommée **dmz2private** avec l'action par défaut qui consiste à supprimer tous les paquets.
* Activer la journalisation du trafic accepté ou refusé pour la règle nommée **dmz2private**.


1\. Tapez *configure* à l'invite de commande.

2\. Entrez les commandes suivantes à l'invite :

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

Le jeu de commandes suivant consiste à activer **iptables** afin d'autoriser le retour du trafic pour une session établie. Par défaut, **iptables** n'autorise pas cela, et c'est la raison pour laquelle une règle explicite est requise.

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

Le troisième jeu de commandes autorise TCP/UDP à passer par le port 22, défini par défaut pour SSH.

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

Le dernier jeu de commandes autorise TCP/UDP à passer par le port 80, défini par défaut pour HTTP.

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. Tapez *commit* afin de vous assurer que toutes les règles sont prises lorsque vous avez terminé.

4\. Affichez votre configuration en tapant *show firewall name dmz2private* à l'invite de commande.

La règle de pare-feu suivante que nous allons créer sera appliquée à notre zone **dmz**. La règle prendra le nom **public**. Entrez les commandes suivantes à l'invite :

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**Remarque :** les règles de pare-feu doivent suivre un flux sortant via **prod** vers **dmz**. Utilisez la commande suivante pour faciliter le traitement des incidents liés au flot réseau : *sudo tcpdump -i any host 10.52.69.202*.

**Création de zones**

Les zones sont la représentation logique d'une interface. Les commandes suivantes permettent d'effectuer ce qui suit :

* Créer une zone et une stratégie nommée **dmz** avec une action par défaut qui consiste à supprimer les paquets destinés à cette zone
* Définir la stratégie **dmz** pour utiliser l'interface **bond1**
* Définir la stratégie **prod** pour utiliser l'interface **bond1.2007**
* Créer une stratégie de zone nommée **private** avec une action par défaut qui consiste à supprimer les paquets destinés à cette zone
* Définir la stratégie nommée **private** pour utiliser l'interface **bond0.2254**

1\. Entrez les commandes suivantes à l'invite :

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. Utilisez les commandes suivantes pour définir la stratégie de pare-feu pour les zones :

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

Notez que vous pouvez appliquer une règle de pare-feu à une interface spécifique si vous ne voulez pas l'appliquer à une stratégie de zone. Utilisez les commandes ci-dessous pour appliquer une règle à une interface.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## Pare-feu sans avec état

Un pare-feu *avec état* conserve une table des flux vus précédemment, et des paquets peuvent être acceptés ou supprimés en fonction de leur relation avec des paquets précédents. En règle générale, il est préférable d'utiliser des pare-feux avec état lorsque le trafic d'application est répandu. 

<span style="text-decoration: underline">*L'unité Brocade 5400 vRouter ne suit pas l'état des connexions avec la configuration par défaut. Le pare-feu est sans état tant que l'une des conditions suivantes n'est pas remplie : *</span>

* Configuration d'un pare-feu et un paramètre state est défini pour toutes les règles
* Configuration d'une firewall state-policy
* Configuration de NAT
* Activation du service proxy Web transparent
* Activation d'une configuration d'équilibrage de charge WAN

## Pare-feu sans état

Un pare-feu *sans état* considère chaque paquet isolément. Les parquets peuvent être acceptés ou supprimés en fonction de critères ACL de base uniquement, tels que les zones source et de destination dans l'adresse IP ou les en-têtes TCP/UDP (Transmission Control Protocols/User Datagram Protocol). Une unité Brocade 5400 vRouter sans état ne stocke pas d'informations de connexion et n'exige pas de rechercher la relation de chaque paquet avec les flux précédents, tous deux consommeront de faibles quantités de mémoire et de temps UC. Les performances du transfert de données brutes sont par conséquent meilleures sur un système sans état. Si vous n'avez pas besoin des fonctions spécifiques du statut avec état, Brocade recommande d'utiliser le routeur sans état afin d'optimiser vos performances.
