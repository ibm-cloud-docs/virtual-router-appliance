---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Cómo ver Vyatta en el portal

Se ha desplegado una única pasarela de 1 Gbps en el centro de datos de San José; tiene acceso redundante tanto a la infraestructura pública como a la privada. Utilice el separador **Dispositivos** bajo el menú ** Red > Dispositivos de pasarela** para ver la configuración real del hardware desplegado.

Anote la **VLAN pública 2546**, la **VLAN privada 2576** y el enlace **Mostrar detalles de Hardware de red**. Las redes son dedicadas y solo contienen dispositivos de pasarela. Están controladas por la plataforma de suministro (no se pueden suministrar servidores normales mediante este separador).

Para ver las asociaciones de red, pulse el menú **Red, Dispositivos de pasarela** en el portal web.

SoftLayer configura todos los puertos de conmutador de red y los adaptadores Ethernet de Brocade 5400 vRouter (Vyatta) como conexiones troncales. Nuestras asociaciones solo actualizan la configuración del puerto para permitir que se seleccionen LAN virtuales adicionales (VLAN).<sup>1</sup>

El menú desplegable **Asociar una VLAN** le permite seleccionar una VLAN específica conocida por, o "asociada" a, el dispositivo Brocade 5400 vRouter desde dentro del centro de datos. **No puede** asociar una VLAN que esté protegida por un cortafuegos de hardware o un dispositivo FortiGate, ya que esto rompería el control de la VLAN. Si no se listan VLAN adicionales en el menú desplegable, puede solicitar más enviando una incidencia a Softlayer.

Puede ver las VLAN asociadas con el dispositivo Brocade 5400 vRouter en VLAN asociadas en la Figura 4. El estado predeterminado es Ignorado (de forma privada, SoftLayer ha identificado los puertos de conmutador de conexión troncal para incluir 1894 y 2254 de la red privada y 2007 y 1280 de la red pública).<sup>2</sup>

En la pantalla **Detalles** (** Red, Dispositivos de pasarela**) en el menú desplegable de **VLAN asociadas, Acciones**, hay una opción para **Direccionar VLAN**. Si ha seleccionado esta opción, ha solicitado de forma efectiva al FCR y al BCR SoftLayer que eliminen la pasarela predeterminada y que, en su lugar, añadan al dispositivo Brocade 5400 vRouter una ruta para las subredes mediante las VLAN de tránsito.

El dispositivo Brocade vRouter es ahora la única ruta válida de entrada y salida de la VLAN. Lo importante es recordar que no sólo es para el tráfico de la aplicación, sino también para los servicios adicionales de SoftLayer que ahora tengan que pasar a través del dispositivo.<sup>4</sup>

Aún así, es necesario realizar la configuración de la parte de Brocade 5400 vRouter; cualquier elemento de la VLAN con las subredes ya no se puede alcanzar. Esto también significa que los nuevos servidores suministrados como el portal web tampoco pueden acceder a la VLAN.

**Notas:**

<sup>1</sup>Una asociación VLAN es el proceso de vincular una VLAN elegible a una pasarela de red, de forma que se pueda direccionar a la pasarela en el futuro. El proceso de asociación no direcciona de manera automática una VLAN a una pasarela; la VLAN continúa utilizando direccionadores frontales y de fondo hasta que se direcciona manualmente a la pasarela. Las VLAN pueden asociarse a una pasarela a la vez y no deben tener cortafuegos para ser consideradas elegibles para la asociación.

<sup>2</sup>La pasarela se puede ignorar en cualquier momento para que el tráfico vuelva a los direccionadores de cliente frontales y de fondo (FCR y BCR). Ignorar una VLAN permite que esta permanezca asociada a la pasarela de red.

<sup>3</sup>Las subredes portátiles se pueden adquirir y añadir a las pasarelas primarias.

<sup>4</sup>Los dispositivos Brocade 5400 vRouter se suministran normalmente con la dirección IP de servicio de SoftLayer predefinida.
