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

# HA y VRRP
Virtual Router Appliance (VRA) proporciona soporte a Virtual Router Redundancy Protocol (VRRP) como protocolo de alta disponibilidad. El despliegue de dispositivos se realiza de manera activa/pasiva, donde una máquina es maestro y la otra es copia de seguridad. Todas las interfaces de ambas máquinas serán un miembro del mismo grupo de sincronización "sync-group", de manera que si se produce un error en una interfaz, el resto de las interfaces del mismo grupo también obtendrá el error y el dispositivo dejará de ser maestro. La copia de seguridad actual detectará que el maestro ya no transmite mensajes de estado activo/latido, supondrá el control de las IP virtuales de VRRP y se convertirá en maestro.

VRRP es la parte más importante de la configuración cuando se suministran pasarelas. La funcionalidad de alta disponibilidad depende de los mensajes latido, por lo que asegurarse de que no están bloqueados es fundamental.

## Direcciones de IP virtuales VRRP (VIP)

La IP virtual de VRRP, o VIP, es la dirección IP flotante cambiada de maestro a dispositivo de copia de seguridad cuando se produce una migración tras error. Cuando se despliega VRA, este dispondrá de una conexión red adherida pública y privada y de IP reales asignadas a cada interfaz. También se asigna una VIP a ambas interfaces, independientemente de si el dispositivo es autónomo o de alta disponibilidad. El tráfico que tenga una IP de destino en subredes en VLAN asociadas con VRA se enviará directamente a estas VIP VRRP.

Las direcciones IP virtuales VRRP para cualquier grupo de pasarela no deben modificarse y la interfaz de VRRP no debería inhabilitarse. Estas direcciones IP son el método mediante el cual el tráfico se direcciona a la pasarela cuando hay una VLAN asociada. Si la dirección IP no está presente, el tráfico no puede enviarse desde softLayer BCR/FCR a la misma pasarela.

## Grupo de VRRP

Un grupo de VRRP consiste en un clúster de interfaces o interfaces virtuales que proporcionan redundancia para la interfaz primaria o "maestro" en el grupo. Normalmente, cada interfaz del grupo está en un direccionador independiente. El proceso de VRRP gestiona la redundancia en cada sistema. El grupo de VRRP tiene un identificador numérico exclusivo y puede asignarse a hasta 20 direcciones IP virtuales.  

El ID de grupo de VRRP lo asigna SoftLayer y no debe cambiarse. Cuando se suministra un nuevo grupo de pasarela tras FCR (Front Customer Router)/BCR (Backend Customer Router) por primera vez, este recibirá un grupo de VRRP de 1. Los suministros del grupo de pasarela subsiguiente aumentarán el valor para evitar conflictos, el siguiente grupo será el grupo 2, grupo 3, etc. A continuación, el proceso de suministro de SoftLayer lo calcula y lo asigna. La modificación de este valor puede resultar en riesgos de colisión con otros grupos activo y con el maestro/contienda de maestro, que provocará una interrupción en ambos grupos de pasarela.

Si migra desde una configuración anterior, se recomienda que compruebe el código de configuración para asegurarse de que el ID de grupo de VRRP no se asigna de forma estática.

El ID de grupo de VRRP se conserva en la base de datos de SoftLayer, de modo que se utilizará el mismo valor de ID de grupo durante la recarga o actualización del SO. Cualquier ID de grupo de VRRP modificado por un usuario se sobrescribirá con el valor asignado por el sistema durante la recarga de SO.  

## Prioridad de VRRP

La primera máquina en un grupo de pasarelas tendrá una prioridad de 254, y este valor se reducirá en el próximo dispositivo que se suministre. No debe establecer una prioridad de 255, ya que de esta forma se define el "propietario de la dirección" de la VIP, que puede provocar un comportamiento no deseado cuando la máquina está en línea con una configuración que difiere del maestro activo en ejecución.

## Preferencia de VRRP

Debe desactivarse siempre la preferencia en todas las interfaces de VRRP para que el nuevo dispositivo u otro que pasa por el proceso de recarga del SO no intenten hacerse cargo del clúster.

## Intervalo de anuncio de VRRP

Para indicar que todavía está en servicio, la interfaz maestro o VIF envía paquetes "latido" denominados "anuncios" al segmento LAN, mediante direcciones multidifusión asignadas de IANA para el VRRP (`224.0.0.18` para IPv4 y `FF02:0:0:0:0:0:0:12` para IPv6).

De forma predeterminada, el intervalo se establece en 1. Este valor puede aumentar, pero no es recomendable establecer un valor de 5. En una red ocupada, es posible que los anuncios de VRRP tarden más de un segundo en llegar al dispositivo de seguridad desde el maestro, de modo que debe utilizarse el valor 2 para las redes con mucho tráfico.

## Sincronización de VRRP (sync-group)

Las interfaces de un grupo de sincronización ("sync group") VRRP se sincronizan de manera que si una de las interfaces del grupo migra tras error a una copia de seguridad, el resto también migrará. Por ejemplo, en muchos casos, si una interfaz de un direccionador maestro falla, el direccionador migrará tras error a un direccionador de copia de seguridad.

Este valor es diferente al grupo de VRRP, ya que define qué interfaces de un dispositivo migrarán tras error cuando una interfaz de dicho grupo registre un error. Se recomienda que todas las interfaces pertenezcan al mismo grupo de sincronización o, de lo contrario, algunas interfaces serán maestro y tendrán IP de pasarela, y otras serán una copia de seguridad y el tráfico no avanzará adecuadamente. No se admiten las configuraciones de tipo activo/activo.

## Compatibilidad rfc de VRRP

La compatibilidad rfc habilita o inhabilita las direcciones MAC RFC 3768 para VRRP en una interfaz. Esto debe habilitarse en interfaces nativas, pero no está habilitado en ninguno de los VIF configurados. Algunos conmutadores virtuales (en general vmware) tienen problemas con que esté habilitado, y harán que el tráfico se reduzca y no se envíe a la IP de pasarela de la máquina de host de hipervisor. No haga nada con estos valores y no los configure para ningún VIF.

## Autenticación de VRRP
Si se establece una contraseña para la autenticación de VRRP, también debe definirse el tipo de autenticación. Si se establece la contraseña y el tipo de autenticación no está definido, el sistema generará un error cuando intente confirmar la configuración.

Asimismo, no puede suprimir la contraseña de VRRP sin suprimir también el tipo de autenticación de VRRP. Si lo hace, el sistema generará un error cuando intente confirmar la configuración. Si suprime la contraseña de autenticación de VRRP y el tipo de autenticación, se inhabilita la autenticación de VRRP.

IETF ha decidido que la autenticación no se puede utilizar en la versión 3 de VRRP. Para obtener más información, consulte VRRP de RFC 5798.

## Soporte de las versiones 2 y 3
VRA da soporte a la versión de VRRP (predeterminado) 2 y 3 de los protocolos. En la versión 2, IPv4 e IPv6 no pueden estar juntos. Sin embargo, en la versión 3, puede tener IPv4 e IPv6 al mismo tiempo.

## VPN de alta disponibilidad con VRRP
Virtual Router Appliance proporciona la capacidad de mantener la conectividad mediante un túnel IPsec utilizando un par de VRA con VRRP. Cuando un direccionador falla o se desactiva por mantenimiento, el nuevo direccionador de maestro de VRRP restaura la conectividad IPsec entre las redes locales y remotas.

Al configurar una VPN de alta disponibilidad con VRRP, siempre que una dirección virtual VRRP se añada a una interfaz VRA, debe reinicializar el daemon IPsec, ya que el servicio de IPsec solo escucha las conexiones en las direcciones que están presentes en VRA cuando se inicializa el daemon del servicio Internet Key Exchange (IKE).

Ejecute el mandato siguiente en los direccionadores de maestro y copia de seguridad para que cuando se produzca una migración tras error, los daemons IPsec se reinicien en un nuevo dispositivo de maestro tras el tránsito de VIP, como se muestra en el ejemplo siguiente:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Cortafuegos de alta disponibilidad con VRRP

Cuando dos dispositivos están en un par de alta disponibilidad, debe tener cuidado en su dispositivo maestro de no bloquear el acceso del otro dispositivo. El puerto 443 debe estar permitido entre ambos dispositivos para la sincronización de la configuración para poder trabajar, y debe permitirse el envío y la obtención de VRRP, incluido el rango de multidifusión de 224.0.0.0/24.

## Subredes VLAN asociadas con VRRP

Para cualquier configuración de VIF con VRRP necesitará una dirección de IP virtual (la primera IP que se pueda usar en la subred, la IP de pasarela a la que se direccionará todo en la subred) y también una dirección IP de interfaz real para VIF en ambos dispositivos. Para conservar las IP que se pueden utilizar, se recomienda que las IP de interfaz reales utilicen un rango fuera de banda como, por ejemplo, 192.168.0.0/30, en el que un dispositivo tiene la dirección .1 y el otro tiene .2. Puede tener varias IP virtuales de VRRP, pero solo necesita una dirección real en cada VIF.

A continuación, se muestra un ejemplo de una configuración VRRP con dos vlan privadas y tres subredes:

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

* Un grupo de sincronización (sync-group) de vrrp es diferente a un grupo de vrrp. Cuando cambia el estado de una interfaz que pertenece a un grupo de sincronización, los demás miembros del mismo grupo de sincronización cambian al mismo estado.
* El número de vrrp-group de las interfaces de VLAN (VIF) no tiene que ser el mismo que el de las interfaces nativas o que el de las demás vlan. Sin embargo, se recomienda mantener todas las direcciones virtuales de la misma VLAN en un vrrp-group, como se ve en VLAN 10.
* Las direcciones de la interfaz real de las vlan nativas (por ejemplo dp0bond1: 169.110.20.28/29) no siempre están en la misma subred que sus VIP (169.110.21.26/29).

## Migración tras error de VRRP manual
Si necesita forzar una migración tras error de vrrp, puede lograrlo mediante la ejecución del siguiente mandato en el dispositivo maestro de VRRP:

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

El ID de grupo es el ID de grupo de vrrp de las interfaces nativas y, como se ha mencionado anteriormente, podría ser diferente en su par.

## Sincronización de conexión
Cuando dos dispositivos VRA están en un par de alta disponibilidad, puede ser útil rastrear conexiones de estado entre los dos dispositivos de forma que si se produce una migración tras error, el estado actual de todas las conexiones que están en el dispositivo con el error se replican en el dispositivo de copia de seguridad, de modo que no es necesario reconstruir desde cero cualquier sesión activa actual (como las conexiones SSL), hecho que puede provocar una interrupción de la experiencia de usuario.

Esto se denomina sincronización de seguimiento de conexión.

Para configurarla, debe declarar cual es el método de migración, qué interfaz utilizará para enviar la información de seguimiento de conexión y la dirección IP del igual remoto:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

El otro VRA tendrá la misma configuración, pero un igual remoto `remote-peer` distinto.

Tenga en cuenta que esto puede saturar un enlace si hay un gran número de conexiones que entran en otras interfaces, y competirá con otro tráfico en el enlace declarado.

## Característica VRRP Start Delay
Ahora el sistema operativo Vyatta versión 1801p y posteriores incluye un nuevo mandato `vrrp`.

`vrrp` especifica un protocolo de elección que asigna dinámicamente la responsabilidad de un direccionador virtual a uno de los direccionadores VRRP de una LAN. El direccionador de VRRP que controla las direcciones IPv4 o IPv6 asociadas a un direccionador virtual se denomina el maestro y reenvía paquetes enviados a estas direcciones IPv4 o IPv6. El proceso de elección proporciona una migración tras error dinámica en cuanto a la responsabilidad del reenvío si el maestro deja de estar disponible. Toda la mensajería de protocolo se realiza utilizando datagramas de multidifusión de IPv4 o IPv6; como resultado, el protocolo puede operar a través de una variedad de tecnologías de LAN multiacceso que dan soporte a la multidifusión IPv4/IPv6.

Para minimizar el tráfico en la red, solo el maestro de cada direccionador virtual envía mensajes periódicos de anuncio de VRRP. Un direccionador de copia de seguridad no intentará sustituir al maestro a menos que tenga una prioridad superior. Esto elimina la interrupción del servicio a menos que esté disponible una vía de acceso preferida. También es posible prohibir administrativamente todos los intentos de sustitución. Si el maestro deja de estar disponible, la copia de seguridad de prioridad más alta se convertirá en maestro después de un breve retardo, proporcionando una transición controlada de la responsabilidad de direccionador virtual con una interrupción de servicio mínima.

**NOTA:** en los despliegues suministrados por IBM Cloud, el valor de retardo inicial está establecido en el valor predeterminado. Si lo desea puede modificarlo a medida que pruebe los métodos de migración tras error y de alta disponibilidad.


### Preferencia frente a no preferencia

El protocolo `vrrp` define la lógica que decide qué igual VRRP de una red tiene la prioridad más alta, y, como tal, es el mejor igual para adoptar el rol de maestro. Con una configuración predeterminada, VRRP se habilitará para realizar la sustitución, lo que significa que un nuevo igual con prioridad superior de la red forzará una migración tras error del rol de maestro.

Cuando la preferencia está inhabilitada, un igual de prioridad superior solo tendrá que migrar tras error el rol de maestro si el igual de prioridad inferior existente ya no está disponible en la red. La inhabilitación de la preferencia resulta a veces útil en escenarios del mundo real, ya que gestiona mejor situaciones en las que el igual de prioridad superior puede haber empezado a comportarse de forma inestable periódicamente debido a problemas de fiabilidad con el propio igual o con una de sus conexiones de red. También resulta útil para evitar una migración tras error prematura a un nuevo igual de prioridad superior que no ha completado la convergencia de red.

### Supuestos y limitaciones de la preferencia

Si los iguales VRRP se han configurado de modo que inhabilitan la sustitución, hay algunos casos en los que puede "aparecer" que se produce la sustitución, pero en realidad el escenario es sólo una migración tras error de VRRP estándar. Tal como se ha descrito anteriormente, VRRP utiliza datagramas de multidifusión IP como medio para confirmar la disponibilidad del direccionador maestro actualmente elegido. Puesto que es un protocolo de capa 3 que detecta el error de un igual VRRP, es importante que la detección de la migración tras error en VRRP se retrase hasta que VRRP y la capa 1 a 2 de la pila de red estén listos y converjan. En algunos casos, la interfaz de red que ejecuta VRRP puede confirmar ante el protocolo que la interfaz está activa, pero puede que otros servicios subyacentes, como el árbol de expansión o Bonding, no se hayan completado. Como resultado, no se puede establecer la conectividad IP entre iguales. Si esto ocurre, VRRP en un nuevo igual superior se convertirá en maestro, ya que no puede detectar mensajes de VRRP del igual maestro actual en la red. Después de la convergencia, el breve periodo de tiempo en el que hay dos pares VRRP maestros dará como resultado que se ejecute la doble lógica maestra de VRRP, el igual de prioridad superior seguirá siendo maestro y el de prioridad inferior se convertirá en copia de seguridad. Este caso de ejemplo puede "aparecer" para demostrar una anomalía en la funcionalidad de "sin sustitución".

### Característica Start Delay

Para solucionar los problemas asociados al retraso en la convergencia de niveles inferiores de la pila de red durante un suceso de activación de interfaz, así como otros factores que contribuyen, se ha incorporado una nueva característica denominada “Startup Delay” en el parche 1801p. La característica hace que el estado de VRRP de una máquina que se ha "recargado" para permanecer en el estado INIT hasta después de un retraso predefinido pueda ser configurada por el operador de red. La flexibilidad en este valor de retardo permite al operador de red personalizar las características de su red y dispositivos en condiciones reales.

### Detalles del mandato

```
vrrp {
start-delay <0-600>
}
```

`start-delay` puede tener un valor comprendido entre 0 (valor predeterminado) y 600 segundos.

### Configuración de ejemplo

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
