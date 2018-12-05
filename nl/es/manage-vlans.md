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

# Gestionar redes VLAN
Puede realizar una serie de acciones de la [pantalla Detalles de dispositivo de pasarela](access-gateway-details.html).

## Asociar una VLAN a un dispositivo de pasarela

Una VLAN necesita asociarse a un dispositivo de pasarela antes de poder ser direccionada. Una asociación VLAN es la vinculación de una VLAN elegible a la pasarela de red, de forma que es posible direccionarla a un dispositivo de pasarela en el futuro. El proceso de asociación no direcciona de manera automática una VLAN al dispositivo de pasarela; la VLAN continua utilizando direccionadores frontales y fondo hasta que se direcciona a la pasarela. 

Pueden asociarse las VLAN a una sola pasarela a la vez y no deben tener un cortafuegos. Realice el procedimiento siguiente para asociar una VLAN a una pasarela de red.

1. [Acceda a la pantalla Detalles de dispositivo de pasarela](access-gateway-details.html) en el Portal de clientes. 
2. Seleccione la VLAN deseada de la lista desplegable **Asociar una VLAN**.
3. Pulse el botón **Asociar** para asociar la VLAN.

Después de asociar una VLAN a un dispositivo de pasarela, aparece en la sección VLAN asociadas de la pantalla Detalles de dispositivo de pasarela. En esta sección, la VLAN puede direccionar a la pasarela o puede asociarse de la pasarela. Pueden asociarse VLAN elegibles adicionales al dispositivo de pasarela en cualquier momento repitiendo los pasos anteriores.

## Direccionar una VLAN asociada

Las VLAN asociadas están vinculadas al dispositivo de pasarela, pero el tráfico de entrada y salida de la VLAN no alcanza la pasarela hasta que se ha direccionada la VLAN. Después de direccionar una VLAN asociada, todo el tráfico frontal y de fondo se direcciona a través del dispositivo de pasarela en contraposición con los direccionadores de cliente. 

Realice el procedimiento siguiente para direccionar una VLAN asociada:

1. [Acceda a la pantalla Detalles de dispositivo de pasarela](access-gateway-details.html) en el Portal de clientes. 
2. Localice la VLAN deseada en la sección VLAN asociadas.
3. Seleccione **Direccionar VLAN** del menú desplegable Acciones.
4. Pulse **Sí** para direccionar la VLAN. 

Después de direccionar una VLAN, todo el tráfico frontal y de fondo se desplaza de los direccionadores de cliente a la pasarela de red. Los controles adicionales relacionados con el tráfico y el propio dispositivo de pasarela pueden utilizarse al acceder a la herramienta de gestión de la pasarela. Es posible que se retire el direccionamiento a través de la pasarela de red en cualquier momento [ignorando el dispositivo de pasarela](#bypass-gateway-appliance-routing-for-a-vlan).

## Ignorar el direccionamiento de dispositivo de pasarela para una VLAN

Después de que se haya direccionado una VLAN, todo el tráfico frontal y de fondo viaja a través de la pasarela de red. En cualquier momento, se puede ignorar el dispositivo de pasarela para que el tráfico vuelva a los direccionadores de cliente frontales y de fondo (FCR y BCR). 

Ignorar una VLAN permite que esta permanezca asociada a la pasarela de red. Si la VLAN no debe continuar asociándose con la pasarela de dispositivo, consulte [Desasociar una VLAN de un dispositivo de pasarela](#disassociate-a-vlan-from-a-gateway-appliance). 

Realice el procedimiento siguiente para ignorar el direccionamiento de pasarela para una VLAN:

1. [Acceda a la pantalla Detalles de dispositivo de pasarela](access-gateway-details.html) en el Portal de clientes. 
2. Localice la VLAN deseada en la sección VLAN asociadas.
3. Seleccione **Ignorar VLAN** del menú desplegable Acciones.
4. Pulse **Sí** para ignorar la pasarela. 

Después de ignorar la pasarela de red, todo el tráfico frontal y de fondo se direcciona mediante el FCR y el BCR asociados con la VLAN. La VLAN permanecerá asociada al dispositivo de pasarela y podrá direccionarse al mismo en cualquier momento.

## Desasociar una VLAN de un dispositivo de pasarela

Las VLAN pueden vincularse a un dispositivo de pasarela a la vez mediante [asociación](#associate-a-vlan-to-a-gateway-appliance). La asociación permite que la VLAN se direccione al dispositivo de pasarela en cualquier momento. Es necesaria la disociación si una VLAN debe asociarse a otro dispositivo de pasarela, o si la VLAN ya no debe estar asociada al mismo. La disociación elimina el "enlace" de la VLAN al dispositivo de pasarela, permitiéndole asociarse a otra pasarela, si es necesario. 

Realice el procedimiento siguiente para desasociar una VLAN del dispositivo de pasarela:

1. [Acceda a la pantalla Detalles de dispositivo de pasarela](access-gateway-details.html) en el Portal de clientes. 
2. Localice la VLAN deseada en la sección VLAN asociadas.
3. Seleccione **Desasociar** del menú desplegable **Acciones**. 
4. Pulse **Sí** para desasociar la VLAN. 

Después de desasociar una VLAN del dispositivo de pasarela, la VLAN puede asociarse a otra pasarela. También puede volver a asociarse al dispositivo de pasarela en cualquier momento. Después de desasociar una VLAN de un dispositivo de pasarela, el tráfico no puede direccionarse mediante la pasarela. Las VLAN deben asociarse a un dispositivo de pasarela antes de poder ser direccionadas.

## Direccionar varias VLAN en la misma interfaz de red
Virtual Router Appliance es capaz de direccionar varios VLAN en la misma interfaz de red (por ejemplo, `dp0bond0` o `dp0bond1`). Esto se consigue estableciendo el puerto de conmutador en modalidad troncal y configurando las interfaces virtuales (VIF) en el dispositivo.

Por ejemplo: 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Los mandatos anteriores crean dos interfaces virtuales en la interfaz `dp0bond0`. La interfaz `dp0bond0.1432` procesa el tráfico de VLAN 1432 y la interfaz `dp0bond0.1693` procesa el tráfico de la VLAN 1693.
