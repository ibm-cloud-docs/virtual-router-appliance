---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HA et VRRP
L'unité VRA (Virtual Router Appliance) prend en charge le protocole VRRP (Virtual Router Redundancy Protocol) en tant que protocole de haute disponibilité. Le déploiement des unités s'effectue de manière active/passive, avec une machine principale et une autre machine de secours. Toutes les interfaces sur les deux machines seront membres du même groupe de synchronisation ("sync-group"), donc s'il y a une défaillance sur une interface, les autres interfaces du même groupe seront également défaillantes et l'unité cessera d'être l'unité principale. La machine de secours détectera que la machine principale ne diffuse plus les messages de type keepalive/heartbeat (conservation de connexion active/signal de présence), et prendra le contrôle des adresses IP virtuelles VRRP pour devenir la machine principale.

VRRP est la partie la plus importante de la configuration lors de la mise à disposition de passerelles. La fonctionnalité de haute disponibilité (HA) dépend des messages de signal de présence, donc s'assurer qu'ils ne sont pas bloqués est essentiel.

## Adresses IP virtuelles (VIP) VRRP
L'adresse IP virtuelle (ou VIP) VRRP est l'adresse IP flottante qui passe de l'unité principale à l'unité de secours lors du basculement. Lors du déploiement d'une unité VRA, cette unité aura une connexion réseau de liaisons publiques et privées et des adresses IP réelles affectées sur chaque interface. Une adresse VIP est également affectée sur les deux interfaces, que l'unité soit autonome ou fasse partie d'une paire HA. Le trafic ayant une adresse IP de destination dans des sous-réseaux au sein de réseaux locaux virtuels (VLAN) associés à l'unité VRA sera envoyé directement à ces adresses VIP VRRP.

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
La fonction de compatibilité RFC de VRRP active ou désactive les adresses MAC RFC 3768 pour VRRP sur une interface. Cette fonction doit être activée sur les interfaces natives, mais désactivée sur toutes les interfaces VIF configurées. Certains commutateurs virtuels (la plupart VMware) ont des problèmes avec l'activation de cette fonction et entraîneront l'interruption du trafic qui ne sera plus envoyé vers l'adresse IP de la passerelle depuis la machine hôte (hyperviseur). Laissez cette valeur et ne configurez pas de valeur d'interface VIF.

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

```
vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
```

## Pare-feux à haute disponibilité avec VRRP
Lorsque deux unités sont dans une paire à haute disponibilité (HA), veillez, sur votre unité maître, à ne pas bloquer l'accès depuis l'autre unité. Le port 443 doit être autorisé entre les deux unités pour que la synchronisation de la configuration (config-sync) fonctionne, et VRRP doit être autorisé en envoi et réception, notamment pour la plage d'adresses multidiffusion 224.0.0.0/24.

## Sous-réseaux VLAN associés avec VRRP
Pour toute configuration d'interface virtuelle (VIF) avec VRRP, vous aurez besoin d'une adresse IP virtuelle (la première adresse IP utilisable dans le sous-réseau, l'adresse IP de la passerelle vers laquelle transitera tout ce qui se trouve dans ce sous-réseau), ainsi qu'une adresse IP d'interface réelle pour l'interface VIF sur les deux unités. Pour conserver des adresses IP utilisables, il est recommandé que les adresses IP de l'interface réelles utilisent une plage hors bande, telle que 192.168.0.0/30, dans laquelle une unité a l'adresse .1 et l'autre l'adresse .2. Vous pouvez avoir plusieurs adresses IP virtuelles VRRP, mais vous n'avez besoin que d'une seule adresse réelle sur chaque VIF.

## Synchronisation des connexions
Lorsque deux unités VRA figurent dans une paire HA, il peut s'avérer utile de suivre les connexions avec état entre ces deux unités de sorte qu'en cas de basculement, l'état actuel de toutes les connexions sur l'unité défaillante soit répliqué sur l'unité de secours, pour ne pas avoir à régénérer complètement les sessions actives en cours (telles que SSL), ce qui pourrait supprimer des acquis utilisateur.

C'est ce que l'on appelle la synchronisation du suivi des connexions.

Pour configurer cette synchronisation, vous devez déclarer la méthode de basculement, l'interface que vous utiliserez pour envoyer les informations de suivi des connexions et l'adresse IP de l'homologue distant :

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

L'autre unité VRA aura la même configuration mais un homologue distant (`remote-peer`) différent.

Notez que cette opération peut saturer une liaison s'il y a un nombre élevé de connexions entrantes sur d'autres interfaces et entrera en concurrence avec d'autre trafic sur la liaison déclarée.
