---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuration à haute disponibilité de Vyatta

La haute disponibilité de Vyatta est prise en charge au moyen de l'utilisation de VRRP (Virtual Routing Redundancy Protocol). Chaque groupe de passerelles possédera deux adresses IP VRRP principales, une pour le côté privé et l'autre pour le côté public des réseaux. 

**REMARQUE :** seuls les systèmes Vyatta privés comporteront le protocole VRRP privé. 

Ces adresses IP sont les adresses IP cible de l'infrastructure de réseau Softlayer afin de router tous les sous-réseaux des réseaux locaux virtuels qui sont associés aux membres de passerelle. Ces adresses IP VRRP ne seront actives que pour un seul système Vyatta à la fois, elles seront arrêtées par l'administrateur pour les autres membres du groupe de passerelles.

La configuration peut être synchronisée entre les deux systèmes Vyatta à l'aide des commandes de configuration "config-sync". Cette configuration autorisera un membre à envoyer la configuration d'options spécifiques à l'autre système Vyatta d'un groupe, de manière sélective. Vous pouvez envoyer uniquement les règles de pare-feu ou uniquement la configuration IPsec, ou n'importe quelle combinaison de jeux de règles. 

Il est recommandé de ne pas essayer d'envoyer des adresses IP ou d'autres configurations de réseau, car config-sync validera instantanément vos modifications sur les autres systèmes Vyatta, en mettant ces interfaces en ligne. Si vous souhaitez activer des interfaces et des services de manière dynamique, vous devez utiliser des scripts de transition pour effectuer cette action lors du basculement. En outre, il est recommandé d'utiliser davantage d'adresses IP VRRP pour vos adresses IP de passerelle sur des réseaux locaux virtuels associés, car cela facilitera la gestion du basculement.

Votre configuration VRRP de base sur les deux machines ressemblera à l'exemple de configuration suivant :

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP nécessite qu'une adresse IP réelle soit d'abord liée à une interface virtuelle avant de pouvoir envoyer des notifications VRRP. Dans la plupart des cas, il vous suffit d'ajouter une adresse IP à partir du sous-réseau principal, mais cela peut être incompatible avec de futures mises à disposition. Ou, vous pouvez envisager de router un réseau local virtuel dont chaque adresse IP de sous-réseau principal est déjà allouée à un serveur. Pour contourner cela, vous pouvez utiliser une paire d'adresses IP hors bande sur les deux systèmes Vyatta afin de leur permettre de communiquer. Par exemple :

Sur le premier système Vyatta :

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

Sur le second système Vyatta :

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

Dans ce cas, les deux systèmes Vyatta auront leur propre adresse IP et il n'y aura aucun risque de conflit avec le sous-réseau alloué. Vous pouvez choisir pratiquement n'importe quelle plage privée. Il vous suffit de sélectionner un petit sous-réseau qui ne sera pas en conflit avec les autres routes que vous possédez éventuellement, par exemple, une plage de sous-réseaux sur un tunnel IPsec, ou une adresse 10.0.0.0/8 qui est incompatible avec Softlayer, et tout ira bien.

Vous souhaiterez également ajouter un nom "sync-group". Toutes les adresses VRRP doivent faire partie du même sync-group. Ainsi, tout incident sur une interface aura pour conséquence de provoquer le basculement de toutes les interfaces appartenant au même sync-group. Dans le cas contraire, vous pourriez vous retrouver avec certaines interfaces de type MASTER et d'autres interfaces de type BACKUP. Utilisez le même nom dans la configuration de réseau local virtuel natif bond0 et bond1.

REMARQUE : la configuration VRRP bond0 et bond1 peut comporter une ligne pour la compatibilité rfc3768. Cela n'est pas nécessaire pour VRRP sur une interface VIF, mais uniquement pour le réseau local virtuel natif bond0 et bond1.

Sur une paire de passerelles nouvellement mise à disposition, config-sync aura une configuration minimale :


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

Vous devrez ajouter des règles pour déterminer les branches de configuration sur lesquelles migrer. Par exemple, si vous souhaitez que la configuration de pare-feu et IPSec soit envoyée, ajoutez les commandes suivantes :


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Une fois ces éléments validés, toutes les modifications apportées à la configuration de pare-feu ou IPSec seront transmises aux autres systèmes Vyatta lors de la validation.

**REMARQUE :** sync-group et sync-map sont deux choses différentes. La configuration sync-map s'applique à des règles afin d'envoyer des changements de configuration à un autre système Vyatta. La seconde, sync-group, permet aux adresses IP VRRP de basculer en tant que groupe et non une par une. La configuration de l'une n'a aucune incidence sur l'autre.
