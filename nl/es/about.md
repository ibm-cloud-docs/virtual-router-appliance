---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Acerca de VRA

IBM Virtual Router Appliance (VRA) proporciona el sistema operativo Vyatta 5600 más reciente para servidores nativos **x86**. Se suministra como configuración de alta disponibilidad o autónoma.

Virtual Router Appliance le permite direccionar el tráfico de red privada y pública selectivamente, a través de un direccionador de empresa completo que tiene cortafuegos, modelado de tráfico, direccionamiento basado en políticas, VPN y otras características. El VRA ofrece rendimiento con facilidad de configuración. Tiene las ventajas de mantenimiento de ejecutarse en un servidor de hardware normal. El dispositivo de hardware VRA tiene un tamaño que permite manejar la carga de direccionamiento de varias VLAN, y se puede solicitar con enlaces de red y matrices RAID redundantes. Todas las características de VRA las gestiona el cliente. 

**NOTA:** FortiGate Security Appliance (FSA) 10Gbps es cortafuegos de hardware de alto rendimiento (10Gbps) de un solo arrendatario (dedicado) con características de última generación como, por ejemplo, antivirus, prevención de intrusiones y filtrado de web. Puede ser una alternativa a VRA (Virtual Router Appliance) para conseguir objetivos similares. Para obtener más información, consulte la [FSA documentación](/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Cortafuegos
Para proteger el entorno de amenazas externas, Virtual Router Appliance puede aprovecharse como cortafuegos. Puede añadir reglas de cortafuegos para permitir o denegar el tráfico de red de entrada o salida a los puertos en los que se ejecuta la aplicación y puede filtrar el tráfico dentro de sus propias redes. También se puede configurar Virtual Router Appliance para que realice un filtrado IPv4 e IPv6 con estado para proteger los datos críticos.

## Pasarela de red privada virtual (VPN)
Conecte su oficina o centro de datos local a IBM Cloud mediante la ejecución en túnel VPN, proporcionando Virtual Router Appliance como dispositivo de pasarela de red. Puede utilizar un túnel VPN de sitio a sitio de IPsec para una comunicación segura de la oficina o el centro de datos de su empresa con la red de IBM Cloud. Otras opciones de VPN son: VPN IPsec de acceso remoto (del cliente al sitio), OpenVPN, GRE, L2TP y DMVPN.

Consulte las guías de configuración de VPN de Brocade en la [sección Documentación de VRA suplementaria](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation).

## Conversión de dirección de red (NAT)
Con Virtual Router Appliance, puede proporcionar aplicaciones y servidores de base de datos sin interfaces de red pública mientras permite que los servidores puedan acceder a Internet mediante Source NAT. También puede ocultar los servidores detrás del dispositivo de pasarela con Destination NAT para mejorar la seguridad.

## Direccionamiento de empresa

Para las aplicaciones multinivel en diferentes redes aisladas, Virtual Router Appliance le permite crear conectividad entre estas redes. Puede configurar el direccionamiento dinámico utilizando BGP, lo que le permitirá anunciar su propio espacio de IP pública en los direccionadores de IBM Cloud. BGP también ofrece más flexibilidad para las configuraciones de red privada personalizadas cuando se utilizan varios túneles y soluciones de enlace directo.

Consulte las guías de configuración de BGP de Brocade en la [sección Documentación de VRA suplementaria](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation).
