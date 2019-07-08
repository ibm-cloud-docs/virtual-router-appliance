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

# Añadir funciones de cortafuegos a Virtual Router Appliance (con estado y sin estado)
La aplicación de reglas de cortafuegos en cada interfaz es un método de cortafuegos que se utiliza con Virtual Router Appliance (VRA). Cada interfaz tiene tres instancias posibles de cortafuegos: In, Out y Local. Se pueden aplicar reglas a cada instancia. 

La acción predeterminada es Drop, y las reglas que permiten el tráfico específico se aplican de la regla 1 a la N. Cuando aparece una coincidencia, el cortafuegos aplica la acción específica de la regla de coincidencia.

De las tres instancias de cortafuegos siguientes, se puede aplicar solo una.

**IN:** el cortafuegos filtra paquetes que entran en la interfaz y atraviesan el sistema. Deberá permitir que ciertos rangos de IP tengan acceso frontal y de fondo por motivos de gestión (ping, supervisión, etc.).

**OUT:** el cortafuegos filtra paquetes que salen de la interfaz. Estos paquetes pueden atravesar el sistema VRA o se pueden originar en el sistema.

**LOCAL:** el cortafuegos filtra paquetes destinados al propio sistema VRA mediante la interfaz del sistema. Debe establecer restricciones en los puertos de acceso al dispositivo de direcciones IP externas que no están restringidas.

Utilice los pasos siguientes para establecer una regla de cortafuegos de ejemplo para desactivar el protocolo de mensajes de control de Internet (ICMP) (ping - mensaje de respuesta de eco de IPv4) en las interfaces de Virtual Router Appliance (se trata de una acción sin estado, más adelante se analizará una acción con estado):

1. Escriba `show configuration commands` en el indicador de mandatos para ver qué configuraciones están establecidas. Verá una lista de todos los mandatos que tiene establecidos en el dispositivo (puede ser útil si decide migrar y desea ver todas las configuraciones). Observe el mandato `set firewall all-ping enable`, que indica que ICMP todavía está habilitado en el dispositivo.

2. Escriba `configure`.

3. Escriba `set firwall all-ping disable`.

4. Escriba `commit`.

Si ahora intenta hacer ping al dispositivo VRA, ya no recibirá respuesta.

Para asignar reglas de cortafuegos al tráfico direccionado, las reglas deben aplicarse a las interfaces de VRA, o se deben crear zonas y entonces aplicar las reglas a las zonas.

Para este ejemplo, se van a crear zonas para las VLAN que se han utilizado hasta ahora.

 VLAN | Zona 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (para la producción)
bond0.2254 | private (para el desarrollo)
bond1.1280 | reservada para uso futuro
bond1.1894 | reservada para uso futuro

## Crear reglas de cortafuegos
Antes de crear las zonas, es una buena idea crear las reglas de cortafuegos que se van a aplicar a las zonas. Crear las reglas antes que las zonas permite aplicarlas inmediatamente, en lugar de tener que crear la zona, luego las reglas y luego volver a la zona para aplicar las reglas.

Los siguientes mandatos permiten:

* Crear una regla de cortafuegos denominada `dmz2private` con la acción predeterminada de descartar cualquier paquete.
* Habilitar el registro del tráfico aceptado y denegado por la regla denominada `dmz2private`.

1. Escriba `configure` en el indicador de mandatos.

2. Especifique los mandatos siguientes:

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. Especifique el siguiente conjunto de mandatos, de forma que las iptables permitan que vuelva el tráfico de una sesión establecida. De forma predeterminada, las iptables no permiten esto, por eso se necesita una regla explícita.

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. Especifique los mandatos siguientes para permitir que TCP/UDP atraviese el puerto 22, el predeterminado para SSH.
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. Especifique los mandatos siguientes para permitir que TCP/UDP atraviese el puerto 80, el predeterminado para HTTP.

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. Escriba `commit` para garantizar que se adopten todas las reglas cuando haya terminado.

7. Visualice la nueva configuración escribiendo `show firewall name dmz2private` en el indicador de mandatos.

8. Cree una regla de cortafuegos para aplicarla a la zona dmz indicando los mandatos siguientes en el símbolo del sistema. La regla se llamará public. 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## Crear zonas

Las zonas son representaciones lógicas de una interfaz. En esta sección vamos a:

* Crear una zona y una política denominada dmz con una acción predeterminada para descartar los paquetes destinados a esta zona
* Definir que la política dmz utilice la interfaz `bond1`
* Definir que la política prod utilice la interfaz `bond1.2007`
* Crear una política de zona denominada `private` con una acción predeterminada para descartar los paquetes destinados a esta zona
* Definir que la política denominada `private` utilice la interfaz `bond0.2254`

1. Especifique los mandatos siguientes en el símbolo del sistema:

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. Utilice los mandatos siguientes para establecer la política de cortafuegos para las zonas:

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
Puede aplicar una regla de cortafuegos a una interfaz específica si no desea aplicarla a una política de zona. Utilice los mandatos siguientes para aplicar una regla a una interfaz.

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
