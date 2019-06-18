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


# Iniciación a IBM Virtual Router Appliance
{: #getting-started}

Para empezar con IBM© Virtual Router Appliance (VRA), navegue a la página en el Portal de clientes:

1. En el navegador, abra el [Portal de clientes ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){: new_window} e inicie sesión en su cuenta.
2. En la navegación del Portal de clientes, seleccione **Red > Dispositivos de pasarela**.
3. En la página de lista **Dispositivos de pasarela**, pulse **Pasarela de orden**.
4. En la página **Orden**, seleccione el centro de datos deseado desde el menú desplegable y elija el tipo de hardware de servidor deseado.

    **NOTA:** los requisitos mínimos de servidor de VRA son de 8 GB de RAM y un núcleo de CPU para cada 10 Gbps de capacidad de red. Por ejemplo, un sistema con enlaces públicos y privados de 10 Gbps duales requiere al menos cuatro núcleos. Reserve más de los valores predeterminados de disco si desea ejecutar diagnósticos de red que generen registros detallados. Finalmente, si pretende configurar servicios VPN con cifrado, es posible que desee añadir núcleos adicionales. Al añadir núcleos adicionales para servicios VPN, se garantiza que no se sobrecargue el VRA con una carga pesada al direccionar y, simultáneamente, cifrar y descifrar datos.

5. En la página de pedido, seleccione la opción **Par de alta disponibilidad** si lo desea, seleccione el tamaño de memoria, seleccione la versión adecuada del sistema operativo VRA y luego seleccione la velocidad de enlace ascendente de red.

6. Revise todas las selecciones y pulse **Añadir a pedido**, y el pedido se verificará automáticamente.
7. En la página **PAGO**, si ya dispone de VLAN en el centro de datos seleccionado, seleccione las VLAN de fondo que deben protegerse. Proporcione un nombre de host y un nombre de dominio para VRA. Compruebe todos los recuadros de los términos de los servicios de IBM Cloud y el acuerdo de servicio de terceros. Pulse **Enviar pedido**.

Cuando se haya aprobado el pedido, el suministro de VRA se inicia automáticamente. Cuando se completa el proceso de suministro, el nuevo VRA se muestra en la página de lista **Dispositivos de pasarela**. Pulse en el nombre de pasarela para abrir la página **Detalles de pasarela** y, a continuación, en cada miembro de pasarela para abrir la página **Detalles de dispositivo**. Encontrará las direcciones IP, el nombre de usuario de inicio de sesión y la contraseña del dispositivo.  

**NOTA:** es importante recordar que una vez que haya pedido y configure el VRA desde el portal de clientes de IBM Cloud, también debe configurar el propio dispositivo con los mismos valores.

## Rol del dispositivo de pasarela y de VLAN
Una VLAN (LAN virtual) es un mecanismo que segrega una red física en muchos segmentos virtuales. Por su comodidad, el tráfico de varias VLAN seleccionadas puede suministrarse mediante un solo cable de red, proceso comúnmente denominado "conexión troncal".

VRA se entrega en dos partes: los servidores de VRA y el elemento fijo de dispositivo de pasarela. El dispositivo de pasarela le proporciona una interfaz (GUI y API) para seleccionar las VLAN que desea asociar con su VRA. La asociación de una VLAN con un dispositivo de pasarela redirige (o "realiza una conexión troncal") dicha VLAN y todas sus subredes a VRA, proporcionándole control sobre el filtrado, reenvío y protección. Los servidores de una VLAN asociada solo pueden alcanzarse a partir de otras VLAN pasando por VRA; no es posible omitir VRA a menos que ignore o desasocie la VLAN.

De forma predeterminada, un nuevo dispositivo de pasarela se asocia a dos VLAN de "tránsito" no extraíbles, una para la pública y otra para la privada. Normalmente se utilizan para fines de administración y los mandatos VRA pueden separarlas de forma segura.

Las VLAN de tránsito son para dispositivos de red como cortafuegos o equilibradores de carga para que puedan direccionar el tráfico mientras mantiene otros dispositivos, como servidores o contenedores, aislados de Internet.

En comparación, las VLAN de "pasarela" es donde están alojados los dispositivos, como los servidores y los contenedores.

VRA solo puede gestionar VLAN asociadas con el mismo mediante el dispositivo de pasarela.

Para obtener información acerca de cómo gestionar VLAN en la pantalla Detalles de dispositivos de pasarela, consulte [Gestión de VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
