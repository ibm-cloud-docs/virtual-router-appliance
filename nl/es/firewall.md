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

# Gestionar los cortafuegos de IBM
{: #manage-your-ibm-firewalls}

Virtual Router Appliance (VRA) tiene la capacidad de procesar reglas de cortafuegos para proteger las VLAN direccionadas a través del dispositivo. Los cortafuegos de VRA pueden dividirse en dos pasos:

1. Definición de uno o varios conjuntos de reglas.
2. Aplicación de un conjunto de reglas para una interfaz o zona. Una zona consiste en una o más interfaces de red.

Es importante probar las reglas de cortafuegos después de la creación para garantizar que las reglas funcionan según lo previsto y que las nuevas reglas no restringen el acceso administrativo al dispositivo.

Durante la manipulación de reglas en la interfaz `dp0bond1`, se recomienda conectar con el dispositivo utilizando `dp0bond0`. La conexión a la consola mediante Intelligent Platform Management Interface (IPMI) también es una opción.

## Sin estado vs con estado
De forma predeterminada, el cortafuegos es sin estado, pero puede configurarse como con estado en caso de que sea necesario. Un cortafuegos sin estado necesitará reglas para el tráfico en ambas direcciones, mientras que los cortafuegos con estado realizan el seguimiento de las conexiones y permiten automáticamente el tráfico de retorno de los flujos aceptados. Para configurar un cortafuegos con estado, debe dictar qué reglas desea que funcionen con estado.

Para habilitar el seguimiento "con estado" del tráfico `tcp`, `udp` o `icmp`, ejecute los mandatos siguientes:

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Tenga en cuenta que los mandatos `global-state-policy` sólo realizarán el seguimiento del estado del tráfico que haya coincidido con una regla de cortafuegos que establezca explícitamente el protocolo correspondiente. Por ejemplo:

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

Puesto que `GLOBAL_STATELESS` no especifica `protocol tcp`, el mandato `global-state-policy tcp` no se aplica a esta regla. 

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

En este caso, `protocol tcp` se define explícitamente. El mandato `global-state-policy tcp` permitirá el seguimiento con estado del tráfico que coincida con la regla 1 de `GLOBAL_STATEFUL_TCP`


Para que las reglas de cortafuegos individuales sean "con estado":

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
De esta forma, se habilitará el seguimiento con estado de todo el tráfico del que se puede realizar el seguimiento con estado y que coincida con la regla 1 de `TEST`, independientemente de que existan mandatos `global-state-policy`. 

## ALG para seguimiento con estado asistido
Algunos protocolos, como FTP, utilizan sesiones más complejas que la operación normal del cortafuegos no puede rastrear. 
Existen módulos preconfigurados que permiten que estos protocolos se puedan gestionar con estado.
Se sugiere inhabilitar estos módulos ALG, a menos que sean necesarios para la correcta utilización de los respectivos protocolos.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Conjunto de reglas de cortafuegos
Las reglas de cortafuegos se agrupan en conjuntos con nombre para facilitar la aplicación de regla en varias interfaces. Cada conjunto de reglas tiene una acción predeterminada asociada. Considere el ejemplo siguiente:
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

En el conjunto de reglas, `ALLOW_LEGACY`, hay dos reglas definidas. La primera regla descarta cualquier tráfico que proviene de un grupo de dirección denominado `network-group1`. La segunda regla descarta y registra cualquier tráfico destinado para el puerto telnet (`tcp/23`) del grupo de dirección denominado `network-group2`. La acción predeterminada indica que se acepta cualquier otra acción.

## Cómo permitir el acceso de centro de datos
IBM© ofrece varias subredes IP para proporcionar servicios y soporte a los sistemas que se ejecutan en el centro de datos. Por ejemplo, los servicios de resolución de DNS se ejecutan en `10.0.80.11` y `10.0.80.12`. Otras subredes se utilizan durante el suministro y soporte. Encontrará los rangos de IP utilizados en los centros de datos en [este tema](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges).

Puede permitir el acceso de centro de datos colocando las reglas `SERVICE-ALLOW` adecuadas al principio de los conjuntos de reglas de cortafuegos con la acción `accept`. El lugar en el que el conjunto de reglas debe aplicarse depende del diseño de cortafuegos y direccionamiento implementado.

Se recomienda colocar reglas de cortafuegos en la ubicación que causa la duplicación de trabajo. Por ejemplo, permitir la entrada de subredes de fondo en `dp0bond0` será menos trabajo que permitir la salida de subredes de fondo en cada interfaz virtual de VLAN.

### Reglas de cortafuegos por interfaz
Un método para configurar el cortafuegos en VRA es aplicar conjuntos de reglas de cortafuegos en cada interfaz. En este caso, una interfaz puede ser de plano de datos (`dp0s0`) o virtual (`dp0bond0.303`). Cada interfaz tiene tres asignaciones de cortafuegos posibles:

`in`: el cortafuegos se comprueba en paquetes que entran a través la interfaz. Los paquetes se pueden atravesar o destinar para VRA.

`out`: el cortafuegos se comprueba en paquetes que salen a través de la interfaz. Estos paquetes pueden estar atravesando VRA o pueden estar originados en VRA.

`local`: el cortafuegos se comprueba en paquetes que están destinados directamente a VRA.

Una interfaz puede tener varios conjuntos de reglas aplicados en cada dirección. Se aplican por orden de configuración. Tenga en cuenta que esto no es posible en el tráfico de cortafuegos que proviene de un dispositivo VRA que utiliza cortafuegos por interfaz.

A modo de ejemplo, para asignar el conjunto de reglas `ALLOW_LEGACY` a la opción `in` para la interfaz `bp0s1`, utilice el mandato de configuración siguiente: 

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
Control plane policing (CPP) proporciona protección contra los ataques en Virtual Router Appliance y le permite configurar políticas de cortafuegos asignadas a las interfaces deseadas y aplicar dichas políticas a los paquetes que entran y salen de VRA.

CPP se implementa cuando la palabra clave `local` se utiliza en las políticas de cortafuegos que se asignan a cualquier tipo de interfaz VRA, como las interfaces de plano de datos o bucle de retorno. A diferencia de las reglas de cortafuegos aplicadas a los paquetes que atraviesan VRA, la acción predeterminada de las reglas de cortafuegos para el tráfico de entrada y salida del plano de control es `Allow`. Los usuarios deben añadir reglas de descarte explícitas si el comportamiento predeterminado no es el deseado.

VRA proporciona un conjunto de regla CPP básica como plantilla. Puede fusionarlo con su configuración ejecutando: 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

Después de fusionar este conjunto de regla, se añade y se aplica un nuevo conjunto de reglas de cortafuegos denominado `CPP` en la interfaz de bucle de retorno. Se recomienda modificar este conjunto de reglas para que se adapte a su entorno.

Tenga en cuenta que las reglas de CPP no pueden ser con estado, y solo se aplicarán al tráfico entrante.

## Cortafuegos basados en zonas
Otro concepto de cortafuegos de Virtual Router Appliance son los cortafuegos basados en zonas. En una operación de cortafuegos basado en zonas, se asigna una interfaz a una zona (solo una zona por interfaz) y los conjuntos de reglas de cortafuegos se asignan a los límites entre las zonas, con la idea de que todas las interfaces de una zona tengan el mismo nivel de seguridad y permiso para direccionar libremente. El tráfico solo se analiza cuando pasa de una zona a otra. Las zonas descartan cualquier tráfico que llega que no esté permitido de manera explícita.

Una interfaz puede pertenecer a una zona o tener una configuración de cortafuegos por interfaz; pero no puede realizar ambas.

Imagínese el siguiente caso de ejemplo de oficina con tres departamentos, cada uno con su propia VLAN: 

* Departamento A - VLAN 10 y 20 (interfaces dp0bond1.10 y dp0bond1.20)
* Departamento B - VLAN 30 y 40 (interfaces dp0bond1.30 y dp0bond1.40)
* Departamento C - VLAN 50 (interfaz dp0bond1.50)

Se puede crear una zona para cada departamento y las interfaces de dicho departamento pueden añadirse a la zona. El ejemplo siguiente lo ilustra: 
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

El mandato `commit` rellena cada zona como una interfaz y las reglas de descarte predeterminadas descartan el tráfico que intenta entrar en la zona desde el exterior. En el ejemplo, VLAN 10 y 20 pueden pasar tráfico porque están en la misma zona (`DEPARTMENTA`), pero VLAN 10 y VLAN 30 no pueden pasar el tráfico porque VLAN 30 está en una zona distinta (`DEPARTMENTB`).

Las interfaces de cada zona pueden pasar el tráfico libremente y las reglas pueden definirse para la interacción entre zonas. Un conjunto de reglas se configura desde el punto de vista de dejar una zona a otra. 

El mandato siguiente muestra un ejemplo de cómo configurar una regla:

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING`

Este mandato asocia la transición de DEPARTMENTC a DEPARTMENTB con el conjunto de reglas denominado `ALLOW_PING`. El tráfico que entra en la zona DEPARTMENTB desde la zona DEPARTMENTC se comprueba en este conjunto de reglas.

Es importante comprender que no existe ninguna sentencia que declare que esta asignación de la zona DEPARTMENTC entrando en la zona DEPARTMENTB se pueda realizar a la inversa. Si no hay reglas que permitan el tráfico de la zona DEPARTMENTB a la zona DEPARTMENTC, el tráfico (respuestas de ICMP) no volverá a los hosts de DEPARTMENTC.

`ALLOW_PING` se aplica como cortafuegos `out` en las interfaces de la zona DEPARTMENTB (dp0bond1.30 y dp0bond1.40). Como lo instala la política de zona, solo se comprobará con el conjunto de reglas el tráfico procedente de las interfaces de la zona de origen (dp0bond1.50).

## Registro de la sesión y el paquete
VRA admite dos tipos de registros:

1. Registro de sesión.  Utilice el mandato ``security firewall session-log`` para configurar el registro de sesión del cortafuegos.
  
	En UDP, ICMP, y todos los flujos que no sean de TCP, una sesión realiza una transición a cuatro estados durante el tiempo de vida del flujo. Para cada transición, puede configurar VRA para que registre un mensaje. TCP tiene un mayor número de transiciones de estado y cada una puede configurarse para realizar un registro.  

2. Registro por paquete. Incluya la palabra clave ``log`` en el cortafuegos o la regla NAT para registrar cada paquete de red que coincida con la regla.

	El registro por paquete se produce en las vías de acceso de reenvío de paquetes y genera grandes cantidades de salidas. Puede reducir significativamente el rendimiento de VRA y aumentar considerablemente el espacio de disco utilizado para los archivos de registro. Se recomienda utilizar el registro de paquete solo con fines de depuración. Para fines operativos, debe utilizarse el registro de sesión con estado.
