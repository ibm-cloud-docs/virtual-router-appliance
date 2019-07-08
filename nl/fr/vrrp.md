---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, vrrp, vip

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

# Utilisation de la haute disponibilité et de VRRP
{: #working-with-high-availability-and-vrrp}

Le dispositif VRA (Virtual Router Appliance) prend en charge le protocole VRRP (Virtual Router Redundancy Protocol) en tant que protocole de haute disponibilité. Le déploiement des unités s'effectue de manière active/passive, avec une machine principale et une autre machine de secours. Toutes les interfaces sur les deux machines seront membres du même groupe de synchronisation ("sync-group"), donc s'il y a une défaillance sur une interface, les autres interfaces du même groupe seront également défaillantes et l'unité cessera d'être l'unité principale. La machine de secours détectera que la machine principale ne diffuse plus les messages de type keepalive/heartbeat (conservation de connexion active/signal de présence), et prendra le contrôle des adresses IP virtuelles VRRP pour devenir la machine principale.

VRRP est la partie la plus importante de la configuration lors de la mise à disposition de passerelles. La fonctionnalité de haute disponibilité (HA) dépend des messages de signal de présence, donc s'assurer qu'ils ne sont pas bloqués est essentiel.

## Adresses IP virtuelles (VIP) VRRP
{: #vrrp-virtual-ip-vip-addresses}

L'adresse IP virtuelle VRRP pour `dp0bond1` ou `dp0bond0` ou VIP est l'adresse IP flottante qui passe de l'unité principale à l'unité de secours lors du basculement. Lors du déploiement d'un dispositif VRA, celui-ci a une connexion réseau de liaisons publiques et privées et des adresses IP réelles affectées sur chaque interface. Une adresse VIP est également affectée sur les deux interfaces, que l'unité soit autonome ou fasse partie d'une paire HA. Le trafic ayant une adresse IP de destination dans des sous-réseaux au sein de réseaux locaux virtuels (VLAN) associés au dispositif VRA est envoyé directement à ces adresses VIP VRRP par une route statique sur le FCR/BCR.

Les adresses IP virtuelles VRRP d'un groupe de passerelles ne doivent jamais être modifiées et l'interface VRRP ne doit pas être désactivée. Ces adresses IP constituent une méthode de routage du trafic aux passerelles lorsqu'un VLAN est associé. Par conséquent, si elles sont défaillantes, le trafic VLAN l'est également. Si l'adresse IP n'est pas présente, le trafic ne peut pas être acheminé d'un routeur SoftLayer BCR/FCR au VRA même. Cette adresse virtuelle ou VIP n'est pas modifiable pour le moment. Cette limitation peut changer à l'avenir, mais actuellement, ni la migration d'un VRA entre POD/FCR/BCR, ni la modification du VIP ne sont prises en charge. 

Vous trouverez ci-dessous un exemple des configurations par défaut des adresses VIP `dp0bond0` et `dp0bond1` pour un VRA spécifique. Veuillez noter que vos adresses IP et vos groupes vrrp peuvent être différents de l'exemple ci-dessous : 

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 virtual-address '10.127.170.2/26'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 virtual-address '159.8.98.214/29'
```

Veuillez consulter les sections [Ajout de plusieurs sous-réseaux à un unique VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans#add-multiple-subnets-to-a-single-vlan) ou [Sous-réseaux VLAN associés avec VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp) pour plus d'informations sur la configuration d'adresses virtuelles pour les VIF. 

**Par défaut, VRRP est défini sur désactivé.** Cela garantit que les nouvelles mises à disposition et les rechargements ne causent pas de pannes sur le périphérique maître. Pour que le trafic VLAN fonctionne, le VRRP doit être réactivé une fois la mise à disposition ou un rechargement terminé. L'exemple suivant détaille la configuration par défaut : 

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

Pour activer VRRP sur ces deux interfaces après une mise à disposition ou un rechargement, **ce qui est nécessaire pour les dispositifs autonomes et les paires à haute disponibilité**, exécutez les commandes suivantes, puis validez la modification :

```
delete interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
delete interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

## Groupe VRRP
{: #vrrp-group}

Un groupe VRRP est constitué d'un cluster d'interfaces ou d'interfaces virtuelles qui fournissent la redondance pour une interface principale ou "maître" dans le groupe. Chaque interface du groupe est en principe sur un routeur distinct. La redondance est gérée par le processus VRRP sur chaque système. Le groupe VRRP a un identificateur numérique unique et jusqu'à 20 adresses IP virtuelles peuvent lui être affectées.  

L'ID du groupe VRRP est attribué par SoftLayer et ne doit pas être modifié. Lorsqu'un nouveau groupe de passerelles est mis à disposition derrière un routeur FCR (Front Customer Route)/BCR (Backend Customer Router) pour la première fois, il reçoit le numéro de groupe VRRP 1. Les mises à disposition des groupes de passerelles suivants incrémenteront cette valeur afin d'éviter tout conflit : le groupe suivant sera le groupe 2, puis il y aura le groupe 3, et ainsi de suite. Ce numéro est calculé et attribué par le processus de mise à disposition de SoftLayer. La modification de cette valeur risque d'entrer en conflit avec d'autres groupes actifs et de générer des conflits maître/maître, ce qui peut entraîner une indisponibilité dans les groupes de passerelles.

Si vous effectuez une migration à partir d'une configuration antérieure, il est recommandé de vérifier votre code de configuration pour vous assurer que l'ID du groupe VRRP n'est pas attribué de manière statique.

L'ID du groupe VRRP est conservé dans la base de données SoftLayer, par conséquent, la même valeur d'ID de groupe sera utilisée lors du rechargement du système d'exploitation ou d'une mise à niveau. Tout ID de groupe VRRP modifié par un utilisateur sera remplacé par la valeur affectée par le système lors d'un rechargement du système d'exploitation.  

## Priorité VRRP
{: #vrrp-priority}

La première machine dans un groupe de passerelles se verra affecter la priorité 254, et cette valeur est décrémentée pour la prochaine unité mise à disposition. Vous ne devez jamais définir une priorité avec la valeur 255, car elle définit le "propriétaire d'adresse" VIP et peut induire un comportement non désiré lorsque la machine est mise en ligne avec une configuration différente de celle de la machine principale (maître) active en cours d'exécution.

## Préemption VRRP
{: #vrrp-preemption}

La fonction de préemption doit toujours être désactivée sur toutes les interfaces VRRP, de sorte qu'une nouvelle unité ou une unité soumise au rechargement de son système d'exploitation n'essaie pas de prendre le contrôle du cluster.

## Intervalle de publication VRRP
{: #vrrp-advertise-interval}

Pour signaler qu'elle est toujours en service, l'interface maître ou VIF envoie des paquets "heartbeat" nommés "advertisements" au segment LAN, en utilisant les adresses de multidiffusion affectées par l'IANA pour le VRRP (`224.0.0.18` pour IPv4 et `FF02:0:0:0:0:0:0:12` pour IPv6).

Par défaut, cet intervalle est défini avec la valeur 1. Cette valeur peut être augmentée mais il est déconseillé d'indiquer une valeur supérieure à 5. Dans un réseau occupé, il faudra beaucoup plus qu'une seconde pour que toutes les annonces VRRP parviennent sur l'unité de secours à partir de l'unité principale (master), donc une valeur de 2 doit être utilisée pour les réseaux dont le trafic est important.

## Synchronisation VRRP (sync-group)
{: #vrrp-synchronization-sync-group-}

Les interfaces figurant dans un groupe de synchronisation VRRP ("sync group") sont synchronisées de sorte que si l'une d'entre elles bascule sur l'unité de secours, toutes les interfaces du groupe suivent et basculent également sur l'unité de secours. Par exemple, dans de nombreux cas, en cas de défaillance d'une interface sur un routeur maître, le routeur complet bascule sur un routeur de secours.

Cette valeur est différente de celle du groupe VRRP, car elle définit les interfaces d'une unité qui basculeront lorsqu'une interface de ce groupe enregistrera une défaillance. Il est recommandé que toutes les interfaces appartiennent au même groupe de synchronisation (sync-group), autrement, certaines interfaces seront maîtres et auront des adresses IP de passerelle et d'autres seront des interfaces de secours, et le trafic ne sera plus acheminé correctement. Les configurations de type actif/actif ne sont pas prises en charge.

## Compatibilité RFC de VRRP
{: #vrrp-rfc-compatibility}

La fonction de compatibilité RFC active ou désactive les adresses MAC RFC 3768 pour VRRP sur une interface. Cette fonction doit être activée sur les interfaces natives, mais désactivée sur toutes les interfaces VIF configurées. Certains commutateurs virtuels (la plupart VMware) ont des problèmes avec l'activation de cette fonction et entraîneront l'interruption du trafic qui ne sera plus envoyé vers l'adresse IP de la passerelle depuis la machine hôte (hyperviseur). Laissez cette valeur et ne configurez pas de valeur d'interface VIF.

## Authentification VRRP
{: #vrrp-authenticaton}

Si un mot de passe est défini pour l'authentification VRRP, le type d'authentification doit également être défini. Si le mot de passe est défini et que le type d'authentification ne l'est pas, le système génère une erreur lorsque vous essayez de valider la configuration.

De même, vous ne pouvez pas supprimer le mot de passe VRRP sans supprimer aussi le type d'authentification VRRP. Autrement, le système génère une erreur lorsque vous essayez de valider la configuration. Si vous supprimez à la fois le mot de passe et le type d'authentification VRRP, l'authentification VRRP est désactivée.

Le groupe de travail IETF a décidé de ne pas utiliser l'authentification pour VRRP version 3. Pour plus d'informations, voir la norme RFC 5798 VRRP.

## Prise en charge des versions 2/3
{: #version-2-3-support}

VRA prend en charge les protocoles VRRP version 2 (valeur par défaut) et version 3. Dans la version 2, vous ne pouvez pas avoir des adresses IPv4 avec des adresses IPv6. Mais dans la version 3, vous pouvez avoir ces deux types d'adresse (IPv4 et IPv6) en même temps.

## Réseau privé virtuel (VPN) à haute disponibilité avec VRRP
{: #high-availability-vpn-with-vrrp}

VRA offre la possibilité de conserver la connectivité via un tunnel IPsec en utilisant une paire d'unités VRA avec VRRP. Lorsqu'un routeur tombe en panne ou est arrêté à des fins de maintenance, le nouveau routeur maître VRRP restaure la connectivité IPsec entre les réseaux locaux et distants.

Lorsque vous configurez un réseau privé virtuel à haute disponibilité avec VRRP, dès qu'une adresse virtuelle VRRP est ajoutée à une interface VRA, vous devez réinitialiser le démon IPsec car le service IPsec est uniquement à l'écoute des connexions sur les adresses présentes dans l'interface VRA lorsque le démon du service IKE (Internet Key Exchange) est initialisé.

Exécutez la commande suivante sur les routeurs maître et les routeurs de secours, de sorte qu'en cas de reprise, les démons IPsec soient redémarrés sur une nouvelle unité maître après transit de l'adresse VIP, comme indiqué dans l'exemple suivant :

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Pare-feux à haute disponibilité avec VRRP
{: #high-availability-firewalls-with-vrrp}

Lorsque deux unités sont dans une paire à haute disponibilité (HA), veillez, sur votre unité maître, à ne pas bloquer l'accès depuis l'autre unité. Le port 443 doit être autorisé entre les deux unités pour que la synchronisation de la configuration (config-sync) fonctionne, et VRRP doit être autorisé en envoi et réception, notamment pour la plage d'adresses multidiffusion 224.0.0.0/24.

## Sous-réseaux VLAN associés avec VRRP
{: #associated-vlan-subnets-with-vrrp}

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
*  Le nombre d'éléments vrrp-group des interfaces VLAN (VIF) doit presque toujours être identique à celui de l'interface native correspondante. Par exemple, `dp0bond1.789` doit toujours avoir le même nombre d'éléments vrrp-group que `dp0bond1`, à moins que les deux interfaces ne partagent le même élément sync-group. Avoir un nombre de groupes différent sur le VIF et l'interface native peut provoquer une condition de division (split brain) en cas d'association à des éléments sync-group différents. Lorsque deux interfaces partagent le même élément sync-group, même si elles appartiennent à des éléments vrrp-group différents, elles effectuent simultanément la transition entre le maître et la sauvegarde, évitant ainsi la condition de division (split brain). D'autre part, si l'interface native est configurée avec un nombre d'éléments vrrp-group différent et un élément sync-group différent en tant qu'interface VLAN, dans la mesure où les sous-réseaux configurés sur les interfaces VLAN sont routés de manière statique vers l'adresse virtuelle sur l'interface native, si l'interface VLAN apparaît en tant que sauvegarde et que l'interface native est maître, le routeur envoie toujours le trafic de l'interface VLAN vers le VRA, où l'interface native est maître, même si l'interface VLAN est secondaire et inactive sur le VRA. Cette situation spécifique provoque une condition de division (split brain) et une panne. C'est pourquoi il est très important de prendre conscience de la manière dont les éléments sync-groups sont configurés en association avec les éléments vrrp-groups. Il est également très important de ne pas utiliser les mêmes nombres d'éléments vrrp-group entre plusieurs paires VRA à haute disponibilité dans le même réseau VLAN de transit, dans la mesure où quatre périphériques Vyatta utilisant le même groupe entraînent trois VRA à assumer potentiellement l'état de sauvegarde, alors qu'un seul est maître, ce qui entraîne une panne. 
* Les adresses d'interface réelles sur les réseaux locaux virtuels natifs (par exemple, dp0bond1: 169.110.20.28/29) ne figurent pas toujours dans le même sous-réseau que leur VIP (169.110.21.26/29).


## Basculement VRRP manuel
{: #manual-vrrp-failover}

Si vous devez forcer un basculement VRRP, vous pouvez le faire en exécutant la commande suivante sur l'unité qui est actuellement le maître VRRP :

`vyatta@vrouter$ reset VRRP master interface dp0bond0 group 1`

L'ID de groupe est l'ID de groupe VRRP des interfaces natives, et, comme indiqué ci-dessus, peut être différent dans votre paire.

## Synchronisation des connexions
{: #connection-synchronization}

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
{: #vrrp-start-delay-feature}

La version 1801p et les versions ultérieures du système d'exploitation Vyatta comportent une nouvelle commande `vrrp`.

`vrrp` spécifie un protocole d'élection qui affecte de façon dynamique la responsabilité pour un routeur virtuel à l'un des routeurs VRRP sur un réseau local. Le routeur VRRP qui contrôle la ou les adresses IPv4 ou IPv6 associée(s) à un routeur virtuel est appelé le maître, et il achemine les paquets envoyés à ces adresses IPv4 ou IPv6. Le processus d'élection fournit un basculement dynamique dans la responsabilité d'acheminement si le maître devient indisponible. Toute la messagerie de protocole est effectuée via des datagrammes multidiffusion IPv4 ou IPv6, par conséquent, le protocole peut fonctionner sur une grande variété de technologies LAN multi-accès prenant en charge la multidiffusion IPv4/IPv6.

Pour réduire le trafic réseau, seul le maître de chaque routeur virtuel envoie régulièrement des messages d'annonce VRRP. Un routeur de sauvegarde ne tentera pas de préempter le maître sauf s'il est doté d'une priorité plus élevée. Cela élimine les interruptions de service sauf si un chemin préféré devient disponible. Il est également possible d'interdire de manière administrative toutes les tentatives de préemption. Si le maître devient indisponible, le routeur de sauvegarde doté d'une priorité plus élevée devient le maître après un court délai, fournissant une transition contrôlée de la responsabilité de routeur virtuel avec un minimum d'interruptions de service.

Dans les déploiements mis à disposition par IBM© Cloud, la valeur de délai de démarrage prend la valeur par défaut. Vous souhaiterez peut-être modifier cela à votre gré lorsque vous testerez vos méthodes de basculement et de haute disponibilité.
{: note}

### Préemption ou pas de préemption
{: #preemption-vs-no-preemption}

Le protocole `vrrp` définit une logique qui décide quelle paire VRRP sur un réseau est dotée de la priorité la plus élevée, et par conséquent, est la plus à même de porter le rôle de maître. Avec une configuration par défaut, VRRP est activé pour effectuer la préemption, ce qui signifie qu'une nouvelle paire ayant la priorité la plus élevée sur le réseau forcera la reprise du rôle de maître.

Lorsque la préemption est désactivée, une paire avec la priorité plus élevée reprendra le rôle de maître uniquement si la paire avec la priorité la moins élevée n'est plus disponible sur le réseau. Il est parfois utile de désactiver la préemption dans des scénarios du monde réel, car cela permet de mieux faire face aux situations où la paire avec la priorité plus élevée a commencé à bagoter régulièrement en raison de problèmes de fiabilité avec la paire proprement dite ou d'une de ses connexions réseau. Il est également utile d'empêcher le basculement prématuré vers une nouvelle paire avec une priorité plus élevée qui n'a pas terminé la convergence du réseau.

### Hypothèses et limitations de préemption
{: #assumptions-and-limitations-of-preemption}

Si les paires VRRP ont été configurées pour désactiver la préemption, il peut arriver que la préemption semble se produire, mais en réalité, le scénario est juste un basculement VRRP standard. Comme décrit précédemment, un protocole VRRP utilise des datagrammes de multidiffusion IP afin de confirmer la disponibilité du routeur maître élu. Dans la mesure où l'échec d'une paire VRRP est détecté par un protocole de couche 3, il est important que la détection de basculement dans VRRP soit retardée jusqu'à ce que VRRP et les couches 1 à 2 de la pile réseau soient prêts et convergés. Dans certains cas, l'interface réseau qui exécute VRRP peut confirmer au protocole que l'interface est active, mais il se peut que d'autres services sous-jacents, tels que l'arbre maximal ou la liaison n'aient pas abouti. Par conséquent, la connectivité IP entre les paires ne peut pas être établie. Si cela se produit, VRRP sur une nouvelle paire avec une priorité plus élevée devient le maître, car il ne peut pas détecter les messages VRRP émis par la paire maître actuelle sur le réseau. Après la convergence, pendant la courte période où il existe deux paires VRRP maître, la logique maître double de VRRP est exécutée, la paire avec la priorité plus élevée reste la paire maître et la paire avec la priorité moins élevée devient la paire de sauvegarde. Le scénario semble faire apparaître une faille dans la fonctionnalité "sans préemption".

### Fonction Délai de démarrage
{: #start-delay-feature}

Pour traiter les problèmes liés au retard de convergence des niveaux inférieurs de la pile réseau durant un événement d'interface active, ainsi qu'à d'autres facteurs, une nouvelle fonction nommée "Délai de démarrage" est introduite dans le correctif 1801p. Avec cette fonction, l'état VRRP sur une machine qui a été rechargée demeure INIT pendant une période qui va au-delà d'un délai prédéfini, qui peut être configuré par l'opérateur réseau. La flexibilité de cette valeur de délai permet à l'opérateur réseau de personnaliser les caractéristiques de son réseau et de ses périphériques dans des conditions réelles.

### Détails de la commande
{: #command-details}

```
vrrp {
start-delay <0-600>
}
```

`start-delay` peut avoir une valeur comprise entre 0 (par défaut) et 600 secondes.

### Exemple de configuration
{: #example-configuration}

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
