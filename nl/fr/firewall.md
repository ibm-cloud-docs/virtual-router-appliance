---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: firewall, manage, stateless, stateful, alg, firewall, rules, CPP, Logging

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
{:important: .important}

# Gestion de vos pare-feux IBM
{: #manage-your-ibm-firewalls}

{{site.data.keyword.vra_full}} (VRA) a la capacité de traiter les règles de pare-feu pour protéger les réseaux locaux virtuels (VLAN) routés via l'unité. Les pare-feux dans le dispositif VRA peuvent être divisés en deux étapes :

1. Définition d'un ou de plusieurs jeux de règles.
2. Application d'un jeu de règles à une interface ou à une zone. Une zone consiste en une ou plusieurs interfaces réseau.

Il est important de tester les règles de pare-feu après leur création pour s'assurer qu'elles fonctionnent comme prévu et que les nouvelles règles n'empêchent pas l'accès des administrateurs à l'unité.

Lors de la manipulation des règles sur l'interface `dp0bond1`, il est conseillé de se connecter à l'unité à l'aide de `dp0bond0`. La connexion à la console à l'aide de l'interface IMPI (Intelligent Platform Management Interface) est également possible.

## Sans et avec état
{: #stateless-vs-stateful}

Par défaut, le pare-feu est configuré sans état, mais il peut être configuré avec état si nécessaire. Un pare-feu sans état nécessitera des règles pour le trafic bidirectionnel, alors que les pare-feu avec état contrôlent les connexions et autorisent automatiquement le trafic en retour des flux acceptés. Pour configurer un pare-feu avec état, vous devez indiquer les règles qui doivent fonctionner avec état.

Pour activer un suivi 'avec état' du trafic `tcp`, `udp` ou `icmp`, exécutez les commandes suivantes :

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Notez que les commandes `global-state-policy` suivront uniquement l'état du trafic ayant été mis en correspondance avec une règle de pare-feu qui définit explicitement le protocole correspondant. Par exemple :

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

Dans la mesure où `GLOBAL_STATELESS` ne spécifie pas `protocol tcp`, la commande `global-state-policy tcp` ne s'applique pas à cette règle.

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

Dans ce cas, `protocol tcp` est défini explicitement. La commande `global-state-policy tcp` active le suivi avec état du trafic correspondant à la règle 1 de `GLOBAL_STATEFUL_TCP`


Pour que les règles de pare-feu individuelles soient "avec état" :

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
Cela permet d'activer le suivi avec état de tout le trafic qui peut être suivi avec état et qui correspond à la règle 1 de `TEST`, que les commandes `global-state-policy` existent ou non.

## ALG pour le suivi avec état assisté
{: #alg-for-assisted-stateful-tracking}

Une poignée de protocoles, tels que FTP, utilisent des sessions plus complexes que celles qui peuvent être suivies par l'opération de pare-feu avec état normale.
Il existe des modules préconfigurés qui permettent d'effectuer des opérations de gestion avec état de ces protocoles.


Il est recommandé de désactiver ces modules ALG, sauf s'ils sont requis pour l'utilisation réussie des protocoles respectifs.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Jeux de règles de pare-feu
{: #firewall-rule-sets}

Les règles de pare-feu sont regroupées dans des ensembles nommés pour faciliter leur application à plusieurs interfaces. Chaque jeu de règles comporte une action par défaut qui lui est associée. Prenez l'exemple suivant : 
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

Dans le jeu de règles `ALLOW_LEGACY`, deux règles sont définies. La première règle supprime le trafic en provenance d'un groupe d'adresses nommé `network-group1`. La deuxième règle supprime et consigne tout le trafic destiné au port telnet (`tcp/23`) du groupe d'adresses nommé `network-group2`. L'action par défaut (default-action) indique que tout le reste est accepté.

## Autorisation d'accès au centre de données
{: #allowing-data-center-access}

IBM© offre plusieurs sous-réseaux d'adresses IP pour fournir des services et prendre en charge des systèmes s'exécutant dans le centre de données. Par exemple, les services du programme de résolution DNS s'exécutent sur `10.0.80.11` et `10.0.80.12`. D'autres sous-réseaux sont utilisés lors de la mise à disposition et du support. Les plages d'adresses IP utilisées dans les centres de données sont présentées dans [cette rubrique](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges).

Vous pouvez autoriser l'accès au centre de données en mettant les règles `SERVICE-ALLOW` adéquates au début des jeux de règles de pare-feu avec l'action `accept`. L'emplacement d'application du jeu de règles dépend de la conception de routage et de pare-feu implémentée.

Il est recommandé de placer les règles de pare-feu à l'emplacement qui entraînera le moins de double-emploi. Par exemple, autoriser les réseaux de back-end entrants sur `dp0bond0` est moins contraignant qu'autoriser des sous-réseaux de back-end sortants vers chaque interface virtuelle de VLAN.

### Règles de pare-feu par interface
{: #per-interface-firewall-rules}

Une méthode de configuration de pare-feu sur un dispositif VRA consiste à appliquer des jeux de règles de pare-feu à chaque interface. Dans ce cas, une interface dataplane (`dp0s0`) ou une interface virtuelle (`dp0bond0.303`). Chaque interface comporte trois possibilités d'affectation de pare-feu :

`in` - La vérification du pare-feu s'effectue par rapport aux paquets entrant via l'interface. Ces paquets peuvent transiter par/ou être destinés au dispositif VRA.

`out` - La vérification du pare-feu s'effectue par rapport aux paquets sortant via l'interface. Ces paquets peuvent transiter par ou provenir du dispositif VRA.

`local` - La vérification du pare-feu s'effectue par rapport aux paquets destinés directement au dispositif VRA.

Une interface peut avoir plusieurs jeux de règles appliqués dans chaque direction. Ils sont appliqués dans l'ordre où ils ont été configurés. Notez qu'il n'est pas possible de créer un pare-feu pour le trafic en provenance du dispositif VRA en utilisant des pare-feux par interface.

Par exemple, pour affecter le jeu de règles `ALLOW_LEGACY` à l'option `in` pour l'interface `bp0s1`, vous utiliserez la commande de configuration suivante :  

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
{: #control-plane-policing-cpp-}

Control Plane Policing (CPP) fournit une protection contre les attaques sur le dispositif VRA ({{site.data.keyword.vra_full}}) en vous autorisant à configurer des règles d'administration de pare-feu affectées aux interfaces désirées et en appliquant ces règles aux paquets entrant dans le dispositif VRA.

CPP est implémenté lorsque le mot-clé `local` est utilisé dans les règles d'administration de pare-feu affectées à n'importe quel type d'interface VRA, que ce soit des interfaces de plan de données ou de bouclage. Contrairement aux règles de pare-feu appliquées aux paquets qui transitent via le dispositif VRA, l'action par défaut des règles de pare-feu pour le trafic entrant ou sortant du plan de contrôle est `Allow`. Les utilisateurs doivent ajouter des règles de suppression explicites si le comportement par défaut n'est pas souhaité.

Le dispositif VRA fournit une règle CPP de base comme modèle. Vous pouvez la fusionner dans votre configuration en exécutant la commande suivante :  

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

Une fois ce jeu de règles fusionné, un nouveau jeu de règles de pare-feu nommé `CPP` est ajouté et appliqué à l'interface de bouclage. Il est recommandé de modifier ce jeu de règles pour l'adapter à votre environnement.

Notez que les règles CPP ne peuvent pas être avec état et s’appliqueront uniquement au trafic entrant.

## Mise en place d'un pare-feu de zone
{: #zone-firewalling}

Un autre concept de pare-feu dans {{site.data.keyword.vra_full}} consiste en pare-feux basés sur des zones. Dans une opération de pare-feu basé sur des zones, une interface est affectée à une zone (il n'y a qu'une seule zone par interface) et des jeux de règles de pare-feu sont affectés aux limites entre les zones dans l'idée que toutes les interfaces dans une zone aient le même niveau de sécurité et soient autorisées à circuler librement. Le trafic n'est examiné que lorsqu'il passe d'une zone à l'autre. Les zones suppriment tout trafic entrant dans leur périmètre qui n'est pas explicitement autorisé.

Une interface peut appartenir à une zone ou avoir une configuration de pare-feu par interface, mais pas les deux.

Imaginez le scénario d'entreprise suivant avec trois services, chaque service (department) ayant son propre réseau local virtuel (VLAN) :  

* Department A - VLANs 10 and 20 (interface dp0bond1.10 and dp0bond1.20)
* Department B - VLANs 30 and 40 (interface dp0bond1.30 and dp0bond1.40)
* Department C - VLAN 50 (interface dp0bond1.50)

Une zone peut être créée pour chaque service et les interfaces de ces services peuvent être ajoutées à la zone. L'exemple suivant illustre ce cas de figure : 
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

La commande `commit` remplit chaque zone en tant qu'interface et les règles de suppression par défaut suppriment tout trafic provenant de l'extérieur d'entrer dans la zone. Dans cet exemple, les VLAN 10 et 20 peuvent autoriser leur trafic car ils sont dans la même zone (`DEPARTMENTA`) mais le VLAN 10 et le VLAN 30 ne le peuvent pas car le VLAN 30 se trouve dans une autre zone (`DEPARTMENTB`).

Les interfaces au sein de chaque zone peuvent autoriser le trafic librement et des règles peuvent être définies pour les interactions entre les zones. Un jeu de règles est configuré pour sortir d'une zone et entrer dans une autre.

La commande suivante montre un exemple de configuration de règle :

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

Cette commande associe la transition du service DEPARTMENTC vers le service DEPARTMENTB avec un jeu de règles nommé `ALLOW_PING`. Le trafic entrant dans la zone DEPARTMENTB depuis la zone DEPARTMENTC sera vérifié par rapport à ce jeu de règles.

Il est important de comprendre que cette affectation depuis la zone DEPARTMENTC ver la zone DEPARTMENTB n'implique pas l'inverse. S'il n'y a aucune règle autorisant le trafic de la zone DEPARTMENTB vers la zone DEPARTMENTC, le trafic (réponses ICMP) ne reviendra pas vers les hôtes situés dans la zone DEPARTMENTC.

`ALLOW_PING` est appliqué en tant que pare-feu `out` sur les interfaces de la zone DEPARTMENTB (dp0bond1.30 and dp0bond1.40). Comme cette installation est effectuée par la règle de zone, seul le trafic provenant des interfaces de la zone source (dp0bond1.50) sera vérifié par rapport au jeu de règles.

## Journalisation de session et par paquet
{: #session-and-packet-logging}

VRA prend en charge deux types de journalisation :

1. Journalisation de session.  Utilisez la commande ``security firewall session-log`` pour configurer la journalisation d'une session de pare-feu.

	Pour UDP, ICMP et tous les flux autres que TCP, une session passe par quatre états sur la durée de vie du flux. Vous pouvez configurer le dispositif VRA pour consigner un message à chaque transition. TCP a un nombre plus important de transitions d'état, chacune pouvant être configurée pour figurer dans le journal.  Voici un exemple de configuration du journal de session pour votre pare-feu :

```
set security firewall session-log icmp established
set security firewall session-log tcp established
set security firewall session-log udp established
```

2. Journalisation par paquet. Insérez le mot-clé ``log`` dans la règle NAT ou la règle de pare-feu pour consigner tous les paquets réseau correspondant à la règle.

	La journalisation par paquet intervient dans les chemins de transfert de paquet et génère de grandes quantités de données en sortie. Il est très important de noter que la journalisation par paquet peut réduire considérablement le débit du dispositif VRA, causer des problèmes de performance et augmenter substantiellement l'espace disque utilisé pour les fichiers journaux. Nous vous recommandons d'utiliser la journalisation par paquet uniquement à des fins de débogage. Pour tout autre objectif opérationnel, la journalisation de session avec état doit être utilisée au lieu de la journalisation par paquet.
	{: important}
