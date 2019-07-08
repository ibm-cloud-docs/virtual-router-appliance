---

copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:important: .important}
{:tip: .tip}

# Configuración de alta disponibilidad de Vyatta 5400
{: #vyatta-5400-high-availability-configuration}

La alta disponibilidad de Vyatta se admite mediante el uso de VRRP, Virtual Routing Redundancy Protocol. Cada grupo de pasarela tendrá dos direcciones VRRP IP primarias, una para la parte privada y una para la parte pública de las redes.

Los dispositivos Vyatta solo privados tendrán solo el VRRP privado.
{: note}

Estas direcciones IP son las IP de destino de la infraestructura de red de Softlayer para direccionar todas las subredes en VLAN asociadas con los miembros de pasarela. Solo un dispositivo Vyatta tendrá estas IP VRRP en ejecución al mismo tiempo, los demás miembros del grupo de pasarela las tendrán inactivas administrativamente.

La configuración se puede sincronizar entre los dos dispositivos Vyatta con los mandatos de configuración "config-sync". Esta configuración permitirá que un miembro envíe por push la configuración de opciones específicas al otro dispositivo Vyatta en un grupo, y que lo haga hacerlo de forma selectiva. Puede enviar por push sólo las reglas de cortafuegos, sólo la configuración de IPsec, o cualquier combinación de conjuntos de reglas.

Se recomienda no intentar enviar por push las direcciones IP de otras configuraciones de red, porque config-sync confirma instantáneamente los cambios en los demás dispositivos Vyatta, haciendo que dichas interfaces estén en línea. Si desea habilitar dinámicamente interfaces y servicios, debe utilizar scripts de transición para realizar esta acción en caso de migración tras error. Además, se recomienda que utilice más direcciones VRRP IP para las IP de pasarela en VLAN asociadas, de este modo, la migración tras error será más fácil de gestionar.

La configuración VRRP básica en ambas máquinas será similar a la siguiente configuración de ejemplo:

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

VRRP requiere que haya una dirección IP real enlazada a una interfaz virtual antes de poder enviar anuncios de VRRP. En muchos casos, puede simplemente añadir una IP de la subred primaria, pero esto podría crear un conflicto con disposiciones futuras, o quizá desee direccionar una VLAN que ya tenga todas las IP de subred primaria asignadas a un servidor. Para evitar este punto, puede utilizar un par de direcciones IP fuera de banda dirección en ambos Vyatta que pueden utilizar para comunicarse entre sí. A continuación se muestra un ejemplo:

En el primer vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

En el segundo vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

En este caso, ambos Vyattas tendrán su propia IP que no entrará en conflicto con la subred asignada. Puede elegir casi cualquier rango privado que desee, escoja una pequeña subred que no entre en conflicto con ningún otro direccionamiento que pueda tener, como por ejemplo un rango de subred en un túnel IPsec o una dirección 10.0.0.0/8 que entre en conflicto con Softlayer.

También querrá añadir un nombre de "sync-group". Todas las direcciones VRRP deben ser parte del mismo grupo de sincronización. De este modo, si hay un error en una interfaz, todas las interfaces del mismo grupo de sincronización también fallarán. De lo contrario, podría terminar con algunas como MASTER y otras en BACKUP. Utilice el mismo nombre en la configuración de VLAN nativa bond0 y bond1.

La configuración VRRP bond0 y bond1 puede tener una línea para rfc3768-compatibility. No es necesaria para VRRP en una VIF, solo la VLAN nativa de bond0 y bond1.
{: tip}

En un par de pasarela recién suministrado, config-sync sólo tendrá la configuración mínima:


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

Tendrá que añadir reglas para determinar qué ramas de configuración desea migrar. Por ejemplo, si desea enviar por push la configuración ipsec y de cortafuegos, añada estos mandatos:


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Una vez que sean confirmados, los cambios en la configuración ipsec o de cortafuegos se enviarán por push al otro vyatta al realizar la confirmación.

sync-group y sync-map son dos cosas diferentes. La configuración de sync-map es para que las reglas envíen por push los cambios de configuración a otro Vyatta. El otro elemento, sync-group, es para las VRRP IP que fallan como grupo en lugar de una por una. La configuración de uno no afecta al otro.
{: tip}
