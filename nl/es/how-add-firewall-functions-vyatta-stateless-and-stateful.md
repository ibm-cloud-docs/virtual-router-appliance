---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Añadir funciones de cortafuegos a Vyatta 5400 (con y sin estado)
{: #adding-firewall-functions-to-vyatta-5400-stateless-and-stateful-}

La aplicación de reglas de cortafuegos en cada interfaz es un método de cortafuegos que se utiliza con dispositivos Brocade 5400 vRouter (Vyatta). Cada interfaz tiene tres instancias posibles de cortafuegos: In, Out y Local. Se pueden aplicar reglas a cada instancia. La acción predeterminada es Drop, y las reglas que permiten el tráfico específico se aplican de la regla 1 a la N. Cuando aparece una coincidencia, el cortafuegos aplica la acción específica de la regla de coincidencia.

De las tres instancias de cortafuegos siguientes, se puede aplicar **solo una**.

* **IN:** el cortafuegos filtra paquetes que entran en la interfaz y atraviesan el sistema Brocade. Deberá permitir que ciertos rangos de IP de SoftLayer tengan acceso frontal y de fondo por motivos de gestión (ping, supervisión, etc.).
* **OUT:** el cortafuegos filtra paquetes que salen de la interfaz. Estos paquetes pueden atravesar el sistema Brocade o se pueden originar en el sistema.
* **LOCAL:** el cortafuegos filtra paquetes destinados al propio sistema Brocade vRouter mediante la interfaz del sistema. Debe establecer restricciones en los puertos de acceso al dispositivo Brocade vRouter de direcciones IP externas que no están restringidas.

Utilice los pasos siguientes para establecer una regla de cortafuegos de ejemplo para desactivar el protocolo de mensajes de control de Internet (ICMP) *(ping - mensaje de respuesta de eco de IPv4)* en las interfaces de Brocade 5400 vRouter (se trata de una acción sin estado, más adelante se analizará una acción con estado):

1\. Escriba los mandatos *show configuration* en el indicador de mandatos para ver qué configuraciones están establecidas. Verá una lista de todos los mandatos que tiene establecidos en el dispositivo (puede ser útil si decide migrar y desea ver todas las configuraciones). Observe el mandato *set firewall all-ping 'enable'*, que indica que ICMP todavía está habilitado en el dispositivo.

2\. Escriba *configure*.

3\. Escriba *set firwall all-ping 'disable'*.

4\. Escriba *commit*.

Si ahora intenta hacer ping al dispositivo Brocade 5400 vRouter, ya no recibirá respuesta.

Para asignar reglas de cortafuegos al tráfico direccionado, las reglas deben aplicarse a las **interfaces** de Brocade 5400 vRouter o se deben **crear zonas** y entonces aplicar las reglas a las zonas.

Para este ejemplo, se van a crear zonas para las VLAN que se han utilizado hasta ahora.

**VLAN = Zona**

bond1 = dmz

bond1.2007 = prod (para la producción)

bond0.2254 = private (para el desarrollo)

bond1.1280 = reservada para uso futuro

bond1.1894 = reservada para uso futuro

**Crear reglas de cortafuegos**

Antes de crear las zonas, es una buena idea crear las reglas de cortafuegos que se van a aplicar a las zonas. Crear las reglas antes que las zonas permite aplicarlas inmediatamente, en lugar de tener que crear la zona, luego crear las reglas y luego volver a la zona para aplicar las reglas.

**NOTA:** las zonas y los conjuntos de reglas tienen una sentencia de acción predeterminada. Al utilizar valores zone-policy, la acción predeterminada se establece mediante la sentencia de zone-policy y está representada por la regla 10.000. Cabe recordar que la acción predeterminada de un conjunto de reglas de cortafuegos se define en **drop** para todo el tráfico.

Los siguientes mandatos permiten:

* Crear una regla de cortafuegos denominada **dmz2private** con la acción predeterminada de descartar cualquier paquete.
* Habilitar el registro del tráfico aceptado y denegado por la regla denominada **dmz2private**.


1\. Escriba *configure* en el indicador de mandatos.

2\. Especifique los mandatos siguientes en el símbolo del sistema:

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

El siguiente conjunto de mandatos hace que las **iptables** permitan que vuelva el tráfico de una sesión establecida. De forma predeterminada, las **iptables** no permiten esto, por eso se necesita una regla explícita.

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

El tercer conjunto de mandatos permite que TCP/UDP atraviese el puerto 22, el predeterminado para SSH.

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

El último conjunto de mandatos permite que TCP/UDP atraviese el puerto 80, el predeterminado para HTTP.

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. Escriba *commit* para garantizar que se adopten todas las reglas cuando haya terminado.

4\. Visualice la configuración escribiendo *show firewall name dmz2private* en el indicador de mandatos.

La siguiente regla de cortafuegos que vamos a crear, se aplicará a la zona **dmz**. La regla se llamará **public**. Especifique los mandatos siguientes en el símbolo del sistema.

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**NOTA:** las reglas de cortafuegos deben circular a través de **prod** y **dmz**. Utilice el siguiente mandato para resolver problemas de flujo de red: *sudo tcpdump -i any host 10.52.69.202*.

**Crear zonas**

Las zonas son representaciones lógicas de una interfaz. Los siguientes mandatos permiten:

* Crear una zona y una política denominada **dmz** con una acción predeterminada para descartar los paquetes destinados a esta zona.
* Definir que la política **dmz** utilice la interfaz **bond1**.
* Definir que la política **prod** utilice la interfaz **bond1.2007**.
* Crear una política de zona denominada **private** con una acción predeterminada para descartar los paquetes destinados a esta zona.
* Definir que la política denominada **private** utilice la interfaz **bond0.2254**.

1\. Especifique los mandatos siguientes en el símbolo del sistema:

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. Utilice los mandatos siguientes para establecer la política de cortafuegos para las zonas:

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

Tenga en cuenta que puede aplicar una regla de cortafuegos a una interfaz específica si no desea aplicarla a una política de zona. Utilice los mandatos siguientes para aplicar una regla a una interfaz.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## Cortafuegos con estado

Los cortafuegos *con estado* mantienen una tabla de flujos vistos anteriormente, y los paquetes se pueden aceptar o descartar según su relación con los paquetes anteriores. Como regla general, se prefieren los cortafuegos con estado cuando el tráfico de la aplicación es frecuente. 

<span style="text-decoration: underline">*Brocade 5400 vRouter no realiza el seguimiento del estado de las conexiones con la configuración predeterminada. El cortafuegos es sin estado hasta que se cumple una de las siguientes condiciones:*</span>

* Se configura un cortafuegos y alguna regla tiene un parámetro state establecido
* Se configura un valor state-policy de cortafuegos
* Se configura NAT
* Se habilita el servicio proxy web transparente
* Se habilita una configuración de equilibrio de carga de WAN

## Cortafuegos sin estado

Los cortafuegos *sin estado* consideran cada paquete de cortafuegos de forma aislada. Los paquetes se pueden aceptar o descartar únicamente según criterios básicos de lista de control de accesos (ACL), como los campos de origen y destino en las cabeceras de TCP/UDP (Transmission Control Protocols/User Datagram Protocol) e IP. Los dispositivos Brocade 5400 vRouter sin estado no almacenan información de conexión y no se requiere que comprueben la relación de cada paquete con los flujos anteriores; ambas acciones consumen pequeñas cantidades de memoria y tiempo de CPU. Por lo tanto, el rendimiento de reenvío bruto es mejor opción en un sistema sin estado. Brocade recomienda mantener el direccionador sin estado para un mejor rendimiento, si no necesita las características específicas de la opción con estado.
