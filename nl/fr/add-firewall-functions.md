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

# Ajout de fonctions de pare-feu à VRA (Virtual Router Appliance) (sans état et avec état)
Appliquer des jeux de règles de pare-feu à chaque interface est une méthode de tunnellisation possible avec l'unité VRA (Virtual Router Appliance). Chaque interface comporte trois instances de pare-feu possibles - In, Out et Local - et des règles peuvent être appliquées à chacune d'elles. 

L'action par défaut est Drop, avec des règles qui permettent à un trafic spécifique d'être appliqué depuis la règle 1 à N. Dès lors qu'une correspondance est établie, le pare-feu applique l'action spécifique de la règle correspondante.

Pour l'une quelconque des trois instances de pare-feu ci-dessous, une seule action peut être appliquée.

**IN :** le pare-feu filtre les paquets entrant dans l'interface et traversant le système. Vous devrez autoriser certaines plages d'adresses IP à accéder aux systèmes front-end et back-end à des fins de gestion (ping, surveillance, etc.).

**OUT :** le pare-feu filtre les paquets sortant de l'interface. Ces paquets peuvent traverser le système VRA ou débuter sur le système.

**LOCAL :** le pare-feu filtre les paquets destinés au système VRA proprement dit à l'aide de l'interface système. Vous devez établir des restrictions sur les ports d'accès entrant dans l'unité à partir d'adresses IP externes qui ne sont pas restreintes.

Suivez les étapes décrites ci-dessous pour définir un exemple de règle de pare-feu afin de désactiver le protocole ICMP (Internet Control Message Protocol) (ping - IPv4 echo reply message) sur les interfaces de votre unité VRA (Virtual Router Appliance) (il s'agit d'une action sans état ; une action avec état sera examinée ultérieurement) :

1. Tapez `show configuration commands` à l'invite de commande pour voir les configurations qui sont définies. Une liste de toutes les commandes que vous avez définies sur votre unité apparaîtra (cela peut être pratique si vous décidez d'effectuer une migration et souhaitez voir toutes vos configurations). Notez la commande `set firewall all-ping enable`, qui indique que le protocole ICMP est toujours activé pour votre unité.

2. Tapez `configure`.

3. Tapez `set firwall all-ping disable`.

4. Tapez `commit`.

A présent, si vous essayez d'envoyer une commande PING à votre unité VRA, vous ne recevrez plus aucune réponse.

Pour que des règles de pare-feu puissent être affectées à du trafic routé, des règles doivent être appliquées aux interfaces ou aux create zones de l'unité VRA, puis être appliquées aux zones.

Pour cet exemple, des zones seront créées pour les réseaux locaux virtuels qui ont été utilisés jusqu'à maintenant.

 Réseau local virtuel | Zone 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (pour la production)
bond0.2254 | private (pour le développement)
bond1.1280 | réservé à un usage ultérieur
bond1.1894 | réservé à un usage ultérieur

## Création de règles de pare-feu
Avant de créer effectivement les zones, il convient de créer les règles de pare-feu qui doivent être appliquées aux zones. Créer les règles avant les zones vous permet de les appliquer immédiatement, au lieu de créer la zone, puis les règles, et revenir à la zone pour l'application de règle.

Les commandes suivantes permettent d'effectuer ce qui suit :

* Créer une règle de pare-feu nommée `dmz2private` avec l'action par défaut qui consiste à supprimer tous les paquets.
* Activer la journalisation du trafic accepté ou refusé pour la règle nommée `dmz2private`.

1. Tapez `configure` à l'invite de commande.

2. Entrez les commandes suivantes :

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. Entrez le jeu de commandes suivant de sorte que iptables autorise le retour du trafic pour une session établie. Par défaut, iptables n'autorise pas cela, et c'est la raison pour laquelle une règle explicite est requise.

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. Entrez les commandes suivantes pour autoriser TCP/UDP à passer par le port 22, défini par défaut pour SSH.
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. Entrez les commandes suivantes pour autoriser TCP/UDP à passer par le port 80, défini par défaut pour HTTP.

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. Tapez `commit` afin de vous assurer que toutes les règles sont prises lorsque vous avez terminé.

7. Affichez la nouvelle configuration en tapant `show firewall name dmz2private` à l'invite de commande.

8. Créez une règle de pare-feu à appliquer à la zone dmz en entrant les commandes suivantes à l'invite. La règle prendra le nom public. 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## Création de zones

Les zones sont des représentations logiques d'une interface. Cette section vous permet de :

* créer une zone et une stratégie nommée dmz avec une action par défaut qui consiste à supprimer les paquets destinés à cette zone ;
* définir la stratégie dmz pour utiliser l'interface `bond1` ;
* définir la stratégie prod pour utiliser l'interface `bond1.2007` ;
* créer une stratégie de zone nommée `private` avec une action par défaut qui consiste à supprimer les paquets destinés à cette zone ;
* définir la stratégie nommée `private` pour utiliser l'interface `bond0.2254`.

1. Entrez les commandes suivantes à l'invite :

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. Utilisez les commandes suivantes pour définir la stratégie de pare-feu pour les zones :

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
Vous pouvez appliquer une règle de pare-feu à une interface spécifique si vous ne voulez pas l'appliquer à une stratégie de zone. Utilisez les commandes ci-dessous pour appliquer une règle à une interface.

~~~
set interfaces bonding bond1 firewall local name public
commit
~~~
