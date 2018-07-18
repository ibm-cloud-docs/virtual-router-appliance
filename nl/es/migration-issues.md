---

copyright:
  years: 2017
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Problemas comunes de migración de Vyatta 5400
La tabla siguiente ilustra los problemas comunes o los cambios de comportamiento que se pueden producir tras la migración de un dispositivo Vyatta 5400 a IBM Virtual Router Appliance. En algunos casos, se incluyen métodos alternativos para solucionar los problemas.

## Política global-state basada en interfaz para cortafuegos con estado

### Problemas
El comportamiento al definir el "estado de State-policy" en cortafuegos con estado del release 5.1 ha cambiado. En las versiones anteriores al release 5.1, si se establecía `state - global -state -policy` en un cortafuegos con estado, el vRouter añadía automáticamente una regla `Allow` implícita para devolver la comunicación de la sesión automáticamente.

En el release 5.1 y posteriores, se debe añadir un valor de regla `Allow` en Virtual Router Appliance. El valor con estado funciona por interfaces en dispositivos Vyatta 5400 y por protocolos en dispositivos VRA.

### Métodos alternativos
Si se aplica la regla `firewall-in` en una interfaz Ingress/Inside, se debe aplicar la regla `Firewall-out` a la interfaz Egress/Outside. De lo contrario, el tráfico de retorno se descartará en la interfaz Egress/Outside.        

## State-Enable en reglas de cortafuegos

### Problemas
Si `global-state-policy` no está configurado, este cambio de comportamiento no se ve afectado.

Además, si utiliza la opción `state enable` en cada regla en lugar de `global-state-policy`, el cambio de comportamiento no se ve afectado.

## Política basada en zonas: manejo de local-zone

### Problemas
No hay ninguna pseudointerfaz "local-zone" que asignar a zone-policy. 

### Método alternativo
Este comportamiento se puede simular aplicando un cortafuegos basado en zonas a las interfaces físicas y un cortafuegos de interfaz a la interfaz de bucle de retorno. El cortafuegos de la interfaz de bucle de retorno filtra todo lo que entre y sale del direccionador. 

Por ejemplo:

set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## Orden de operación para cortafuegos, NAT, direccionamiento y DNS

### Problemas
En un caso de ejemplo en el que se despliega una NAT de origen de enmascaramiento en IBM Virtual Router Appliance, no se puede utilizar el cortafuegos para determinar el acceso de los hosts a Internet. Esto se debe a que la dirección de NAT posterior será la misma.

En los dispositivos Vyatta 5400, esta operación es posible porque el cortafuegos se realizó antes que la NAT, permitiendo así la restricción de los hosts para acceder a Internet.

### Métodos alternativos
Se requiere un nuevo esquema de direccionamiento para VRA:
![routing dns](./images/routing-dns.png)

## Tabla de direccionamiento basado en políticas

### Problemas
La palabra "Table" es opcional en las configuraciones en el direccionamiento basado en políticas de v5400, pero en VRA, si la acción es `accept`, el campo **Table** es obligatorio. Si la acción es `drop` en la configuración de VRA, el campo Table es opcional.

### Métodos alternativos
"Table Main" es una opción disponible en el direccionamiento basado en políticas de Vyatta 5400. El equivalente en VRA es "routing-instance default".

## Direccionamiento basado en políticas en la interfaz de túnel

### Problemas
En las políticas de PBR (direccionamiento basado en políticas) de Virtual Router Appliance, se pueden aplicar políticas a las interfaces de plano de datos para el tráfico de entrada, pero no a las interfaces de bucle de retorno, túnel, puente, OpenVPN, VTI e IP sin número.

### Métodos alternativos
Actualmente, no hay soluciones alternativas para este problema.

## TCP-MSS

### Problemas
IBM Virtual Router Appliance utiliza nftables y no admite TCP-MSS.

### Métodos alternativos
Actualmente, no hay soluciones alternativas para este problema.

## OpenVPN

### Problemas
OpenVPN no empieza a funcionar cuando se utiliza el parámetro `push-route` en Virtual Router Appliance.

### Métodos alternativos
Utilice el parámetro `openvpn-option` en lugar de `push-route`.

## GRE/VTI sobre IPSEC + OSPF

### Problemas
* Cuando VIF tiene varias subredes configuradas, el tráfico no puede ir a través de dichas subredes en VRA.                                
* El direccionamiento InterVlan no funciona en VRA.

### Métodos alternativos
Utilice reglas allow implícitas para aceptar el tráfico en las interfaces de VIF.

## IPSec

### Problemas
IPSec (basada en prefijos) no funciona con el filtro IN.

### Métodos alternativos
Utilice IPSec (basada en VTI).

## IPSEC 'match-none"

### Problemas
Con los dispositivos Vyatta 5400, se permite la siguiente regla de cortafuegos:

set firewall name allow rule 10 ipsec 

Sin embargo, con IBM Virtual Router Appliance, no hay IPSec.

### Métodos alternativos
Posibles reglas alternativas para dispositivos VRA:

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### Problemas
Con los dispositivos Vyatta 5400, se permiten las siguientes reglas de cortafuegos:

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

Sin embargo, con IBM Virtual Router Appliance, no hay IPSec.

### Métodos alternativos
Añada los protocolos `ESP` y `AH` (protocolos IP 50 y 51 respectivamente).

La regla `action` puede ser `accept` o `drop`, tal como se muestra a continuación:

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## IPSEC de sitio a sitio con DNAT

### Problemas
IPSec (basada en prefijos) no funciona con DNAT.                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1) 
===S-S-IPsec=== (12.0.0.1) 
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

El fragmento de código anterior es un breve ejemplo de configuración de la conversión de DNAT después de que un paquete IPSec se haya descifrado en Vyatta 5400. En el ejemplo, hay dos vyattas, `vyatta1 (11.0.0.1)` y `vyatta2 (12.0.0.1)`. La interconexión de IPsec se establece entre `11.0.0.1` y `12.0.0.1`. En este caso, el cliente se dirige a `172.16.1.245` desde `10.103.0.1` de extremo a extremo.El comportamiento esperado de este caso de ejemplo es que la dirección de destino `172.16.1.245` se convierta en `10.71.68.245` en la cabecera de paquete.

Inicialmente, el dispositivo Vyatta 5400 realizaba la DNAT en la IPSec de entrada, finalizaba la interfaz y devolvía el tráfico correctamente al túnel IPsec utilizando la tabla de seguimiento de conexiones. 

En Virtual Router Appliance, la configuración no funciona de la misma manera. Se crea la sesión; sin embargo, el tráfico de retorno ignora el túnel IPsec después de que la tabla de seguimiento de conexiones invierta el cambio de DNAT. A continuación, VRA envía el paquete en la conexión sin cifrado de IPsec.  El dispositivo en sentido ascendente no espera este tráfico, y lo más probable es que lo descarte. A pesar de que se interrumpe la conectividad continua, se trata de un comportamiento intencionado.   

### Métodos alternativos
Para tener en cuenta el caso de ejemplo de red anterior, IBM ha creado una RFE. 

Mientras se evalúa la RFE, recomendamos el siguiente método alternativo:

**Mandatos de configuración de interfaz**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**Mandatos de configuración de VPN**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**Mandatos de configuración de NAT**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**Mandatos de configuración de protocolos**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**Mandatos de configuración de PBR**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### Problemas
PPTP ya no se admite en Virtual Router Appliance.                                                                                                                                                   

### Métodos alternativos
En su lugar, utilice el protocolo L2TP.

## Script para el reinicio de IPSec

### Problemas
Cuando se añade una dirección virtual de VRRP a IBM Virtual Router Appliance en una VPN de alta disponibilidad, debe reinicializar el daemon IPsec. Esto se debe a que el servicio de IPsec solo escucha las conexiones en las direcciones que están presentes en VRA cuando se inicializa el daemon del servicio IKE.

En un par de VRA con VRRP, es posible que el direccionador en espera no tenga la dirección virtual de VRRP que está presente en el dispositivo durante la inicialización si el direccionador maestro no tiene presente dicha dirección. Por lo tanto, para reinicializar el daemon IPsec cuando se produce una transición de estado de VRRP, ejecute el mandato siguiente en los direccionadores maestro y de copia de seguridad:

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Recent count y Recent time

### Problemas

La intención de la regla siguiente consiste en limitar las conexiones SSH a 3 cada 30 segundos para SSH con cualquier dirección: 

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

En IBM Virtual Router Appliance, esta regla tiene los siguientes problemas:

* La opción de recent count y recent time ha quedado en desuso.

* Debido al problema anterior, la regla no puede funcionar como se esperaba y bloqueará todas las conexiones SSH a la interfaz aplicada.

### Métodos alternativos
En su lugar, utilice CPP.

## Problemas de set system conntrack

### Problemas
```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## Set system conntrack timeout

### Problemas
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## Cortafuegos basado en tiempo

### Problemas
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### Problemas
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(en 18.x, no se puede aplicar tcp-mss utilizando la opción de sólo PBR, es para establecer la interfaz directamente, que creo que no es equivalente a pbr).
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## Aplicación específica o puerto interrumpido en la VPN Ipsec S-S

### Problemas

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
Finalizaciones posibles:
   <Enter> Ejecutar el mandato actual
   port    Cualquier puerto TCP o UDP
   prefix  Prefijo IPv4 o IPv6 remoto                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Cambio significativo en el comportamiento de registro 

### Problemas
Existe un cambio significativo en el comportamiento de registro entre el dispositivo Vyatta 5400 e IBM Virtual Router Appliance, desde el registro por sesión al registro por paquete.

* Registro de sesión: Registra las transiciones de estado de sesión con estado

* Registro de paquete: Registra todos los paquetes que coinciden con la regla. Puesto que el registro de paquete se registra en el archivo de registro en "unidades de paquete", hay una marcada reducción del rendimiento y la presión de la capacidad de disco.

* La capacidad de registro de vRouter se puede utilizar para capturar la actividad del cortafuegos. Al igual que cualquier función de registro, sólo debe habilitarla si desea resolver un problema concreto, e inhabilitar el registro tan pronto como pueda.

El cortafuegos con estado que gestiona las sesiones de cortafuegos/NAT escribe en "unidades de sesión". Se recomienda utilizar el registro de sesión. A continuación, se describe cada ejemplo:
 
**Registro de sesión**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Cortafuegos de registro de paquete**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>` 
