---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Preguntas frecuentes
Las siguientes son las preguntas más frecuentes cuando se trabaja con IBM Virtual Router Appliance (VRA).

## ¿Qué es VRA? 
Virtual Router Appliance (VRA) le permite a un cliente de IBM Cloud direccionar de forma selectiva tráfico de red público y privado a través de un direccionador de empresa completo con cortafuegos, reforma de tráfico, direccionamiento basado en políticas, VPN y un host de otras funciones. Todas las características de VRA están gestionadas por el cliente. VRA le ofrece a un cliente de IBM Cloud un grado de control que normalmente se reserva para las redes sociales.

## ¿Qué es un dispositivo de pasarela? 
Un elemento fijo de dispositivo de pasarela le permite utilizar el portal web o la API para seleccionar los segmentos de red (VLAN) para direccionar un VRA. Puede cambiar las selecciones VLAN en cualquier momento. El dispositivo de pasarela también maneja la alta disponibilidad de VRA, configurando un segundo VRA como sustituto por si el primero falla.

## A veces veo referencias a término cómo "Vyatta" y "vRouter". ¿Cuál es la relación con VRA?
Vyatta era un software direccionador basado en PC de código abierto que se adquirió en su totalidad y se convirtió en un software de código cerrado. A día de hoy, "Vyatta" y "Vyatta OS" describen adaptaciones de software comercial derivado de ese proyecto de código cerrado. IBM VRA incorpora elementos de Vyatta OS, junto con importantes mejoras de características y servicio disponibles exclusivamente a través de IBM Cloud.

"vRouter" fue un intento efímero de cambiar la imagen corporativa de Vyatta por parte del entonces propietario. Cuando aparece en la documentación, puede considerarse como sinónimo de Vyatta.

## ¿Todavía se ofrece soporte a Vyatta 5400?
IBM dejará de ofrecer soporte a Vyatta 5400 a partir del 20 de febrero de 2018.

## ¿Qué mejoras tiene Virtual Router Appliance (Vyatta 5600) respecto a Vyatta 5400?
Vyatta 5600 ofrece las mejoras siguientes respecto a Vyatta 5400:

- Un rendimiento más rápido, de hasta 10Gbps por núcleo de CPU
- Mayor rendimiento de más de 3X por sesión VPN IPsec (estándar de cifrado avanzado)
- Mayor capacidad para direccionadores, flujos y conexiones simultáneas
- Soporte a las estándares soportados, incluida la encapsulación de Layer 2 Tunneling Protocol, versión 3 (L2TPv3), Internet Key Exchange, versión 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2) y 802.1Q tunneling (Q-in-Q)

## ¿Qué ocurre con la oferta de vRouter 5600 AT&T?
AT&T (anteriormente Brocade) ha anunciado la finalización de Brocade vRouter 5600. Si bien Brocade vRouter 5600 proporciona la capacidad de tecnología subyacente para IBM Virtual Router Appliance, este anuncio no se aplica a los clientes de IBM. Los clientes de IBM seguirán recibiendo soporte para poder utilizarlo.

## ¿Cómo se suministra VRA? 
Puede obtener VRA solicitando una pasarela de red. Este proceso simplificado le permite eligir un centro de datos y un servidor VRA adecuados, así como si desea desplegar una alta disponibilidad de VRA. Los servidores, los sistemas operativos y el elemento fijo de dispositivos de pasarela se suministran de manera automática. Cuando se complete, puede utilizar la interfaz de un dispositivo de pasarela para direccionar VLAN mediante VRA. Puede configurar VRA directamente mediante SSH (Secure Shell) con las contraseñas proporcionadas en la sección Detalles de hardware del Portal de clientes.

## ¿Es segura mi contraseña? 
Sí. Se asignan contraseñas aleatorias a todos los VRA que son visibles solo para el titular de la cuenta. Las contraseñas se pueden cambiar fácilmente, de igual forma que las claves públicas SSH y las restricciones de acceso IP de administrador.

## ¿Puedo obtener VRA sin un dispositivo de pasarela? 
Sí, pero solo puede gestionar el tráfico entre las interfaces públicas y privadas de VRA. VLAN y la alta disponibilidad requieren el elemento fijo del dispositivo de pasarela.

## ¿Se envía todo el tráfico a través de VRA? 
No. El dispositivo de pasarela le permite seleccionar los segmentos de red pública y privada (VLAN) que desea direccionar mediante VRA. Puede cambiar e ignorar las selecciones VLAN en cualquier momento. VRA también le permite definir las reglas que se aplican a las subredes o rangos de IP. Las reglas funcionan solo si las VLAN que contienen dichas subredes se direccionan mediante VRA.

## ¿Puede VRA o un cortafuegos dedicado impedir nuevas provisiones de servidor? 
Sí. Siempre que sea posible, no debería cerrar la red hasta que la haya rellenado con los servidores que tiene pensado utilizar.

El soporte de IBM tiene prohibido por política examinar o alterar VRA o la configuración del cortafuegos dedicado sin la participación explícita de un cliente, por lo que en la mayoría de los casos el soporte no puede saber si un VRA es responsable de provisiones de servidores detenidas o erróneas.

Es responsabilidad del cliente asegurarse de que VRA o el cortafuegos están configurados para permitir provisiones de servidor automatizado antes de que se realice el pedido de servidor. Es responsabilidad del cliente resolver las disposiciones que están bloqueadas por un VRA gestionado por un cliente o un cortafuegos. Los retrasos de dicho suministro no están sujetos a SLA o créditos. Los sistemas ordenados pueden devolverse al inventario (después se borran los datos de cliente) si el cliente no responde rápidamente.

Del mismo modo, si un VRA/cortafuegos se ignora después de realizar un pedido, sigue siendo probable que el pedido falle. Es posible que haya una ventana estrecha durante la que se reintentará automatización. Es mejor que el todo el proceso de suministro proceda sin interferencias en la red.

## ¿Qué productos de cortafuegos ofrece IBM?
Para ver una comparación detallada de todos los productos de cortafuegos que se ofrecen en IBM Cloud, consulte este [tema ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/infrastructure/fortigate-10g/explore-firewalls.html#explore-firewalls){: new_window}. 

## ¿Puede confundir VRA la tarea del soporte al cliente? 
Sí, por las razones descritas anteriormente. VRA es una "caja negra": las VLAN están dentro y se pueden sacar, e IBM no tiene ni idea de qué están haciendo los clientes entre los paquetes.

El soporte siempre hace todo lo posible, pero con VRA y el cortafuegos dedicado: a) la privacidad de cliente supera la conectividad y b) nuestro personal de soporte no está equipado para analizar las configuraciones de cortafuegos/VRA muy complejas o mal formadas.

Como primer paso de diagnóstico, es posible que necesite poner su VRA o VLAN de cortafuegos en modo ignorar. Si, en este estado, las disposiciones que han fallado empiezan a funcionar, debemos suponer que el problema se encuentra en la configuración del cortafuegos/VRA.

## ¿Qué efecto tendrá VRA en mi rendimiento de red? 
Tenga en cuenta que aunque no pueden verlo, una nube pública comparte las redes con otros clientes. El mejor rendimiento de VRA se determina por la capacidad de red disponible en un momento determinado, además de la distancia que deben recorrer los datos.

Dejando estas variables a un lado, VRA es capaz de reenviar 80 Gbps de tráfico no modificado en varias interfaces, utilizando la fórmula de modo que cada 10 Gbps de rendimiento requiere un núcleo de procesador completo (sin incluir hyper-thread). Dado que los servidores actuales alcanzan el máximo a los 40 Gbps (2 x 10 Gbps público + 2 x 10 Gb privado), un servidor con 8 núcleos o más tiene suficiente margen dinámico de cálculo para manejar varias características de VRA comunes en el mejor rendimiento de red

## ¿Qué debo hacer si pierdo la contraseña de VRA?
Si hay acceso al sistema, establezca una nueva contraseña ejecutando el mandato siguiente:

```
set system login user [account] authentication plaintext-password [password]  
```

Si no hay acceso al sistema, puede rearrancar el dispositivo y utilizar la opción de recuperación de contraseña en el menú GRUB para restablecer la contraseña de usuario root.

## ¿Qué debo hacer si el cortafuegos se ha bloqueado?
La construcción `reboot at [time]` puede ser útil al probar reglas de cortafuegos potencialmente peligrosas.

Si la regla funciona, utilice el mandato `reboot cancel` para cancelar el rearranque. Si la regla le bloquea el acceso, espere al próximo rearranque planificado..

Si no hay acceso al sistema, entonces se utilizará un rearranque para recuperar el acceso. Tras el rearranque, el sistema leerá el archivo de configuración que permanece sin cambios a según las entradas previas descartadas.

Si hay acceso mediante IPMI, puede realizar las acciones siguientes para recuperarlo:

1. Inhabilitar la regla errónea ejecutando:

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Separe el conjunto de reglas con nombre de la interfaz necesaria ejecutando:

	```
	delete interfaces dataplane [interface] firewall [type][firewall name]
	commit
	```

**NOTA:** El uso incorrecto de estos mandatos puede borrar la configuración de la interfaz.

## ¿Cómo puedo inhabilitar los inicios de sesión como root para VRA?

Para habilitar el acceso como usuario root mediante SSH, ejecute el mandato siguiente:

`set service ssh allow-root`

Tenga en cuenta que permitir el acceso como usuario root mediante SSH no se considera seguro. Una alternativa para acceder a un shell root es iniciar sesión como otro usuario y elevar a root localmente con `su -` o permitir mandatos sudo a los 'superusuarios'. 

Por ejemplo, para configurar vyatta como superusuario:

`set system login vyatta level superuser`
