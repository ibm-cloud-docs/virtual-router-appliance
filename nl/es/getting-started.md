---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Iniciación a {{site.data.keyword.vra_full}}
{: #getting-started}

{{site.data.keyword.vra_full}} (VRA) proporciona el sistema operativo Vyatta 5600 más reciente para servidores nativos x86. Se ofrece como una configuración de alta disponibilidad (HA) o autónoma, y le permite direccionar el tráfico de red privada y pública selectivamente, a través de un direccionador de empresa completo que tiene cortafuegos, modelado de tráfico, direccionamiento basado en políticas, VPN y otras características.

Los requisitos mínimos para un servidor de VRA son 8 GB de RAM y un núcleo de CPU para cada 10 Gbps de capacidad de red. Por ejemplo, un sistema con enlaces públicos y privados de 10 Gbps duales requiere al menos cuatro núcleos. Además, si pretende configurar servicios VPN con cifrado, es posible que desee añadir núcleos adicionales. Añadiendo núcleos adicionales para los servicios de VPN, se garantiza que no se sobrecargue el VRA con cargas pesadas al direccionar y, simultáneamente, cifrar y descifrar datos.

## Pedido de un {{site.data.keyword.vra_full}}
{: #order-vra}

Para hacer un pedido de un VRA, siga el procedimiento siguiente:

1. En el navegador, abra la página Dispositivos de pasarela en la [consola de la IU de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} e inicie sesión en su cuenta. 

  También puede llegar a esta página seleccionando el menú de navegación en la parte superior izquierda del [Catálogo de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com) y seleccionando **Infraestructura clásica > Red > Dispositivo de pasarela**. 
  {: tip}

2. En la sección **Proveedor de pasarela**, seleccione la opción **AT&T** (cuando está seleccionada, aparece una marca de selección azul sobre el botón). En el menú desplegable de ese mismo botón, elija el ancho de banda (20 Gbps o 2 Gbps).

  	<img src="images/ordering_vra.png" alt="dibujo" style="width: 500px;"/>

3. En la sección **Dispositivo de pasarela**, especifique la información de **Nombre de host** y nombre de **Dominio**. Estos campos ya estarán cumplimentados con información predeterminada, asegúrese de que los valores son correctos. Si lo desea, marque la opción **Alta disponibilidad** y seleccione la **Ubicación** deseada del centro de datos y el **Pod** específico que prefiera en el menú desplegable.

  Sólo se mostrarán los pods que ya tengan una VLAN asociada. Si desea suministrar su Dispositivo de pasarela en un pod que no aparece en la lista, primero cree una VLAN en el mismo.
  {: note}

4. En la sección **Configuración**, seleccione el procesador seleccionando las claves de RAM y SSH (si es necesario).

  <img src="images/ordering_vra_2.png" alt="dibujo" style="width: 600px;"/>
  
  Se elige el procesador adecuado en base a la versión de licencia que ha seleccionado en el paso dos. Sin embargo, puede elegir configuraciones de RAM distintas.
  {: note}

5. En la sección **Discos de almacenamiento**, elija las opciones que se ajusten a sus requisitos de almacenamiento. 

  Hay las opciones RAID0 y RAID1 disponibles para una mayor protección contra la pérdida de datos, así como los repuestos dinámicos (componentes de copia de seguridad que se pueden poner en servicio inmediatamente cuando falla un componente primario).
  {: note}

  Puede tener hasta cuatro discos por cada VRA. El "Tamaño de disco" en una configuración de RAID es el tamaño de disco utilizable, ya que las configuraciones de RAID están duplicadas.
  {: note}

  Reserve más de los valores predeterminados de disco si desea ejecutar diagnósticos de red que generen registros detallados.
  {: tip}

6. En la sección **Interfaz de red**, seleccione las **Velocidades de puerto de enlace ascendente**. La selección predeterminada es una única interfaz, pero también hay las opciones redundante y privada. Elija la que mejor se ajuste a sus necesidades.

  La sección **Complementos** de interfaz de red le permite seleccionar una dirección IPv6 si es necesario, y muestra las opciones predeterminadas adicionales que haya. 
  
8. Revise las selecciones que ha realizado, compruebe que ha leído los Acuerdos de servicio de terceros y pulse **Crear**. El pedido se verifica automáticamente.

Cuando se haya aprobado el pedido, se inicia automáticamente el suministro de {{site.data.keyword.vra_full}}. Cuando se completa el proceso de suministro, el nuevo VRA se muestra en la página de lista Dispositivos de pasarela. Pulse el nombre de la pasarela para abrir la página Detalles de pasarela. Encontrará las direcciones IP, el nombre de usuario de inicio de sesión y la contraseña del dispositivo.  

  <img src="images/gateway_details.png" alt="dibujo" style="width: 500px;"/>

Recuerde que una vez que haya pedido y configurado el VRA desde el Catálogo de IBM Cloud, también debe configurar el propio dispositivo con los mismos valores.
{: tip}

## Rol del dispositivo de pasarela y de VLAN
{: #vlans-and-the-gateway-appliance-s-role}

Una VLAN (LAN virtual) es un mecanismo que segrega una red física en muchos segmentos virtuales. Para mayor comodidad, el tráfico de varias VLAN seleccionadas puede suministrarse mediante un solo cable de red, proceso comúnmente denominado "conexión troncal".

{{site.data.keyword.vra_full}} se entrega en dos partes: los servidores de VRA y el elemento fijo de dispositivo de pasarela. El dispositivo de pasarela le proporciona una interfaz (GUI y API) para seleccionar las VLAN que desea asociar con su VRA. La asociación de una VLAN con un dispositivo de pasarela redirige (o "realiza una conexión troncal") dicha VLAN y todas sus subredes a VRA, proporcionándole control sobre el filtrado, reenvío y protección. Para cada VLAN asociada con el Dispositivo de pasarela, esa VLAN está permitida en los puertos de conmutador a los que está conectado el VRA, y cualquier subred en dicha VLAN se direcciona estáticamente a la IP de VRRP pública del VRA (si la subred es una subred pública) o a la IP de VRRP privada del VRA (si la subred es una subred privada). Este direccionamiento se realiza en el direccionador detrás del VRA, que será el FCR (Frontend Customer Router) o el BCR (Backend Customer Router) para el tráfico público y privado respectivamente. 

Tenga en cuenta que VRRP está inhabilitado de forma predeterminada y que debe estar habilitado para que el tráfico de VLAN funcione, incluso en vyattas autónomos. Esto es consecuencia de que las subredes en la VLAN asociada están direccionadas a la IP de VRRP o a la dirección virtual asignada al VRA. Para obtener más información, consulte [Direcciones IP virtuales (VIP) de VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses).
{: important}

Los servidores de una VLAN asociada solo pueden alcanzarse a partir de otras VLAN a través del {{site.data.keyword.vra_full}}; no es posible omitir el VRA a menos que se ignore o se desasocie la VLAN.

De forma predeterminada, un nuevo dispositivo de pasarela se asocia a dos VLAN de "tránsito" no extraíbles, una para la pública y otra para la privada. Normalmente se utilizan para fines de administración y los mandatos VRA pueden separarlas de forma segura.

Las VLAN de tránsito son para dispositivos de red como cortafuegos o equilibradores de carga para que puedan direccionar el tráfico mientras mantiene otros dispositivos, como servidores o contenedores, aislados de Internet.

En comparación, las VLAN de "pasarela" es donde están alojados los dispositivos, como los servidores y los contenedores.

VRA solo puede gestionar VLAN asociadas con el mismo mediante el dispositivo de pasarela.

Para obtener información acerca de cómo gestionar VLAN en la pantalla Detalles de dispositivos de pasarela, consulte [Gestión de VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
