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

# Utilisation de la haute disponibilité et de VRRP
{: #working-with-high-availability-and-vrrp}

Le dispositif VRA (Virtual Router Appliance) prend en charge le protocole VRRP (Virtual Router Redundancy Protocol) en tant que protocole de haute disponibilité. Le déploiement des unités s'effectue de manière active/passive, avec une machine principale et une autre machine de secours. Toutes les interfaces sur les deux machines seront membres du même groupe de synchronisation ("sync-group"), donc s'il y a une défaillance sur une interface, les autres interfaces du même groupe seront également défaillantes et l'unité cessera d'être l'unité principale. La machine de secours détectera que la machine principale ne diffuse plus les messages de type keepalive/heartbeat (conservation de connexion active/signal de présence), et prendra le contrôle des adresses IP virtuelles VRRP pour devenir la machine principale.

VRRP est la partie la plus importante de la configuration lors de la mise à disposition de passerelles. La fonctionnalité de haute disponibilité (HA) dépend des messages de signal de présence, donc s'assurer qu'ils ne sont pas bloqués est essentiel.

## Adresses IP virtuelles (VIP) VRRP

L'adresse IP virtuelle (ou VIP) VRRP est l'adresse IP flottante qui passe de l'unité principale à l'unité de secours lors du basculement. Lors du déploiement d'un dispositif VRA, celui-ci aura une connexion réseau de liaisons publiques et privées et des adresses IP réelles affectées sur chaque interface. Une adresse VIP est également affectée sur les deux interfaces, que l'unité soit autonome ou fasse partie d'une paire HA. Le trafic ayant une adresse IP de destination dans des sous-réseaux au sein de réseaux locaux virtuels (VLAN) associés au dispositif VRA sera envoyé directement à ces adresses VIP VRRP.

Les adresses IP virtuelles VRRP d'un groupe de passerelles ne doivent pas être modifiées et l'interface VRRP ne doit pas être désactivée. Ces adresses IP constituent une méthode de routage du trafic aux passerelles lorsqu'un VLAN est associé. Si l'adresse IP n'est pas présente, le trafic ne peut pas être acheminé d'un routeur SoftLayer BCR/FCR à la passerelle même.

## Groupe VRRP

Un groupe VRRP est constitué d'un cluster d'interfaces ou d'interfaces virtuelles qui fournissent la redondance pour une interface principale ou "maître" dans le groupe. Chaque interface du groupe est en principe sur un routeur distinct. La redondance est gérée par le processus VRRP sur chaque système. Le groupe VRRP a un identificateur numérique unique et jusqu'à 20 adresses IP virtuelles peuvent lui être affectées.  

L'ID du groupe VRRP est attribué par SoftLayer et ne doit pas être modifié. Lorsqu'un nouveau groupe de passerelles est mis à disposition derrière un routeur FCR (Front Customer Route)/BCR (Backend Customer Router) pour la première fois, il reçoit le numéro de groupe VRRP 1. Les mises à disposition des groupes de passerelles suivants incrémenteront cette valeur afin d'éviter tout conflit : le groupe suivant sera le groupe 2, puis il y aura le groupe 3, et ainsi de suite. Ce numéro est calculé et attribué par le processus de mise à disposition de SoftLayer. La modification de cette valeur risque d'entrer en conflit avec d'autres groupes actifs et de générer des conflits maître/maître, ce qui peut entraîner une indisponibilité dans les groupes de passerelles.

Si vous effectuez une migration à partir d'une configuration antérieure, il est recommandé de vérifier votre code de configuration pour vous assurer que l'ID du groupe VRRP n'est pas attribué de manière statique.

L'ID du groupe VRRP est conservé dans la base de données SoftLayer, par conséquent, la même valeur d'ID de groupe sera utilisée lors du rechargement du système d'exploitation ou d'une mise à niveau. Tout ID de groupe VRRP modifié par un utilisateur sera remplacé par la valeur affectée par le système lors d'un rechargement du système d'exploitation.  

## Priorité VRRP

La première machine dans un groupe de passerelles se verra affecter la priorité 254, et cette valeur est décrémentée pour la prochaine unité mise à disposition. Vous ne devez jamais définir une priorité avec la valeur 255, car elle définit le "propriétaire d'adresse" VIP et peut induire un comportement non désiré lorsque la machine est mise en ligne avec une configuration différente de celle de la machine principale (maître) active en cours d'exécution.

## Préemption VRRP

La fonction de préemption doit toujours être désactivée sur toutes les interfaces VRRP, de sorte qu'une nouvelle unité ou une unité soumise au rechargement de son système d'exploitation n'essaie pas de prendre le contrôle du cluster.

## Intervalle de publication VRRP

Pour signaler qu'elle est toujours en service, l'interface maître ou VIF envoie des paquets "heartbeat" nommés "advertisements" au segment LAN, en utilisant les adresses de multidiffusion affectées par l'IANA pour le VRRP (`224.0.0.18` pour IPv4 et `FF02:0:0:0:0:0:0:12` pour IPv6).

Par défaut, cet intervalle est défini avec la valeur 1. Cette valeur peut être augmentée mais il est déconseillé d'indiquer une valeur supérieure à 5. Dans un réseau occupé, il faudra beaucoup plus qu'une seconde pour que toutes les annonces VRRP parviennent sur l'unité de secours à partir de l'unité principale (master), donc une valeur de 2 doit être utilisée pour les réseaux dont le trafic est important.

## Synchronisation VRRP (sync-group)

Les interfaces figurant dans un groupe de synchronisation VRRP ("sync group") sont synchronisées de sorte que si l'une d'entre elles bascule sur l'unité de secours, toutes les interfaces du groupe suivent et basculent également sur l'unité de secours. Par exemple, dans de nombreux cas, en cas de défaillance d'une interface sur un routeur maître, le routeur complet bascule sur un routeur de secours.

Cette valeur est différente de celle du groupe VRRP, car elle définit les interfaces d'une unité qui basculeront lorsqu'une interface de ce groupe enregistrera une défaillance. Il est recommandé que toutes les interfaces appartiennent au même groupe de synchronisation (sync-group), autrement, certaines interfaces seront maîtres et auront des adresses IP de passerelle et d'autres seront des interfaces de secours, et le trafic ne sera plus acheminé correctement. Les configurations de type actif/actif ne sont pas prises en charge.

## Compatibilité RFC de VRRP

La fonction de compatibilité RFC active ou désactive les adresses MAC RFC 3768 pour VRRP sur une interface. Cette fonction doit être activée sur les interfaces natives, mais désactivée sur toutes les interfaces VIF configurées. Certains commutateurs virtuels (la plupart VMware) ont des problèmes avec l'activation de cette fonction et entraîneront l'interruption du trafic qui ne sera plus envoyé vers l'adresse IP de la passerelle depuis la machine hôte (hyperviseur). Laissez cette valeur et ne configurez pas de valeur d'interface VIF.

## Authentification VRRP
Si un mot de passe est défini pour l'authentification VRRP, le type d'authentification doit également être défini. Si le mot de passe est défini et que le type d'authentification ne l'est pas, le système génère une erreur lorsque vous essayez de valider la configuration.

De même, vous ne pouvez pas supprimer le mot de passe VRRP sans supprimer aussi le type d'authentification VRRP. Autrement, le système génère une erreur lorsque vous essayez de valider la configuration. Si vous supprimez à la fois le mot de passe et le type d'authentification VRRP, l'authentification VRRP est désactivée.

Le groupe de travail IETF a décidé de ne pas utiliser l'authentification pour VRRP version 3. Pour plus d'informations, voir la norme RFC 5798 VRRP.

## Prise en charge des versions 2/3
VRA prend en charge les protocoles VRRP version 2 (valeur par défaut) et version 3. Dans la version 2, vous ne pouvez pas avoir des adresses IPv4 avec des adresses IPv6. Mais dans la version 3, vous pouvez avoir ces deux types d'adresse (IPv4 et IPv6) en même temps.

## Réseau privé virtuel (VPN) à haute disponibilité avec VRRP
VRA offre la possibilité de conserver la connectivité via un tunnel IPsec en utilisant une paire d'unités VRA avec VRRP. Lorsqu'un routeur tombe en panne ou est arrêté à des fins de maintenance, le nouveau routeur maître VRRP restaure la connectivité IPsec entre les réseaux locaux et distants.

Lorsque vous configurez un réseau privé virtuel à haute disponibilité avec VRRP, dès qu'une adresse virtuelle VRRP est ajoutée à une interface VRA, vous devez réinitialiser le démon IPsec car le service IPsec est uniquement à l'écoute des connexions sur les adresses présentes dans l'interface VRA lorsque le démon du service IKE (Internet Key Exchange) est initialisé.

Exécutez la commande suivante sur les routeurs maître et les routeurs de secours, de sorte qu'en cas de reprise, les démons IPsec soient redémarrés sur une nouvelle unité maître après transit de l'adresse VIP, comme indiqué dans l'exemple suivant :

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Pare-feux à haute disponibilité avec VRRP

Lorsque deux unités sont dans une paire à haute disponibilité (HA), veillez, sur votre unité maître, à ne pas bloquer l'accès depuis l'autre unité. Le port 443 doit être autorisé entre les deux unités pour que la synchronisation de la configuration (config-sync) fonctionne, et VRRP doit être autorisé en envoi et réception, notamment pour la plage d'adresses multidiffusion 224.0.0.0/24.

## Sous-réseaux VLAN associés avec VRRP

Pour toute configuration d'interface virtuelle (VIF) avec VRRP, vous aurez besoin d'une adresse IP virtuelle (la première adresse IP utilisable dans le sous-réseau, l'adresse IP de la passerelle vers laquelle transitera tout ce qui se trouve dans ce sous-réseau), ainsi qu'une adresse IP d'interface réelle pour l'interface VIF sur les deux unités. Pour conserver des adresses IP utilisables, il est recommandé que les adresses IP de l'interface réelles utilisent une plage hors bande, telle que 192.168.0.0/30, dans laquelle une unité a l'adresse .1 et l'autre l'adresse .2. Vous pouvez avoir plusieurs adresses IP virtuelles VRRP, mais vous n'avez besoin que d'une seule adresse réelle sur chaque VIF.

Voici un exemple d'une configuration VRRP avec deux VLAN privés et trois sous-réseaux :

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* Un élément sync-group VRRP est différent d'un groupe VRRP. Lorsqu'une interface qui appartient à un élément sync-group change d'état, tous les autres membres du même élément sync-group prennent le même état.
* Il n'est pas nécessaire que le nombre d'éléments vrrp-group des interfaces VLAN (VIF) soit identique à celui de l'une des interfaces natives ou des autres réseaux locaux virtuels. Toutefois, il est fortement recommandé de conserver toutes les adresses virtuelles du même réseau local virtuel dans un seul élément vrrp-group, comme dans VLAN 10.
* Les adresses d'interface réelles sur les réseaux locaux virtuels natifs (par exemple, dp0bond1: 169.110.20.28/29) ne figurent pas toujours dans le même sous-réseau que leur VIP (169.110.21.26/29).

## Basculement VRRP manuel
Si vous devez forcer un basculement VRRP, vous pouvez le faire en exécutant la commande suivante sur l'unité qui est actuellement le maître VRRP :

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

L'ID de groupe est l'ID de groupe VRRP des interfaces natives, et, comme indiqué ci-dessus, peut être différent dans votre paire.

## Synchronisation des connexions
Lorsque deux unités VRA figurent dans une paire HA, il peut s'avérer utile de suivre les connexions avec état entre ces deux unités de sorte qu'en cas de basculement, l'état actuel de toutes les connexions sur l'unité défaillante soit répliqué sur l'unité de secours, pour ne pas avoir à régénérer complètement les sessions actives en cours (telles que SSL), ce qui pourrait supprimer des acquis utilisateur.

C'est ce que l'on appelle la synchronisation du suivi des connexions.

Pour configurer cette synchronisation, vous devez déclarer la méthode de basculement, l'interface que vous utiliserez pour envoyer les informations de suivi des connexions et l'adresse IP de l'homologue distant :

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

L'autre dispositif VRA aura la même configuration mais un homologue distant (`remote-peer`) différent.

Notez que cette opération peut saturer une liaison s'il y a un nombre élevé de connexions entrantes sur d'autres interfaces et entrera en concurrence avec d'autre trafic sur la liaison déclarée.

## Fonction Délai de démarrage VRRP
La version 1801p et les versions ultérieures du système d'exploitation Vyatta comportent une nouvelle commande `vrrp`.

`vrrp` spécifie un protocole d'élection qui affecte de façon dynamique la responsabilité pour un routeur virtuel à l'un des routeurs VRRP sur un réseau local. Le routeur VRRP qui contrôle la ou les adresses IPv4 ou IPv6 associée(s) à un routeur virtuel est appelé le maître, et il achemine les paquets envoyés à ces adresses IPv4 ou IPv6. Le processus d'élection fournit un basculement dynamique dans la responsabilité d'acheminement si le maître devient indisponible. Toute la messagerie de protocole est effectuée via des datagrammes multidiffusion IPv4 ou IPv6, par conséquent, le protocole peut fonctionner sur une grande variété de technologies LAN multi-accès prenant en charge la multidiffusion IPv4/IPv6.

Pour réduire le trafic réseau, seul le maître de chaque routeur virtuel envoie régulièrement des messages d'annonce VRRP. Un routeur de sauvegarde ne tentera pas de préempter le maître sauf s'il est doté d'une priorité plus élevée. Cela élimine les interruptions de service sauf si un chemin préféré devient disponible. Il est également possible d'interdire de manière administrative toutes les tentatives de préemption. Si le maître devient indisponible, le routeur de sauvegarde doté d'une priorité plus élevée devient le maître après un court délai, fournissant une transition contrôlée de la responsabilité de routeur virtuel avec un minimum d'interruptions de service.

**Remarque :** dans les déploiements mis à disposition par IBM© Cloud, la valeur de délai de démarrage prend la valeur par défaut. Vous souhaiterez peut-être modifier cela à votre gré lorsque vous testerez vos méthodes de basculement et de haute disponibilité.


### Préemption ou pas de préemption

Le protocole `vrrp` définit une logique qui décide quelle paire VRRP sur un réseau est dotée de la priorité la plus élevée, et par conséquent, est la plus à même de porter le rôle de maître. Avec une configuration par défaut, VRRP est activé pour effectuer la préemption, ce qui signifie qu'une nouvelle paire ayant la priorité la plus élevée sur le réseau forcera la reprise du rôle de maître.

Lorsque la préemption est désactivée, une paire avec la priorité plus élevée reprendra le rôle de maître uniquement si la paire avec la priorité la moins élevée n'est plus disponible sur le réseau. Il est parfois utile de désactiver la préemption dans des scénarios du monde réel, car cela permet de mieux faire face aux situations où la paire avec la priorité plus élevée a commencé à bagoter régulièrement en raison de problèmes de fiabilité avec la paire proprement dite ou d'une de ses connexions réseau. Il est également utile d'empêcher le basculement prématuré vers une nouvelle paire avec une priorité plus élevée qui n'a pas terminé la convergence du réseau.

### Hypothèses et limitations de préemption

Si les paires VRRP ont été configurées pour désactiver la préemption, il peut arriver que la préemption semble se produire, mais en réalité, le scénario est juste un basculement VRRP standard. Comme décrit précédemment, un protocole VRRP utilise des datagrammes de multidiffusion IP afin de confirmer la disponibilité du routeur maître élu. Dans la mesure où l'échec d'une paire VRRP est détecté par un protocole de couche 3, il est important que la détection de basculement dans VRRP soit retardée jusqu'à ce que VRRP et les couches 1 à 2 de la pile réseau soient prêts et convergés. Dans certains cas, l'interface réseau qui exécute VRRP peut confirmer au protocole que l'interface est active, mais il se peut que d'autres services sous-jacents, tels que l'arbre maximal ou la liaison n'aient pas abouti. Par conséquent, la connectivité IP entre les paires ne peut pas être établie. Si cela se produit, VRRP sur une nouvelle paire avec une priorité plus élevée devient le maître, car il ne peut pas détecter les messages VRRP émis par la paire maître actuelle sur le réseau. Après la convergence, pendant la courte période où il existe deux paires VRRP maître, la logique maître double de VRRP est exécutée, la paire avec la priorité plus élevée reste la paire maître et la paire avec la priorité moins élevée devient la paire de sauvegarde. Le scénario semble faire apparaître une faille dans la fonctionnalité "sans préemption". 

### Fonction Délai de démarrage

Pour traiter les problèmes liés au retard de convergence des niveaux inférieurs de la pile réseau durant un événement d'interface active, ainsi qu'à d'autres facteurs, une nouvelle fonction nommée "Délai de démarrage" est introduite dans le correctif 1801p. Avec cette fonction, l'état VRRP sur une machine qui a été rechargée demeure INIT pendant une période qui va au-delà d'un délai prédéfini, qui peut être configuré par l'opérateur réseau. La flexibilité de cette valeur de délai permet à l'opérateur réseau de personnaliser les caractéristiques de son réseau et de ses périphériques dans des conditions réelles.

### Détails de la commande

```
vrrp {
start-delay <0-600>
}
```

`start-delay` peut avoir une valeur comprise entre 0 (par défaut) et 600 secondes.

### Exemple de configuration

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
