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

# Acerca de 
IBM Virtual Router Appliance (VRA) le permite direccionar selectivamente la ruta del tráfico de red privada y pública a través de un direccionador de empresa completo con cortafuegos, reforma de tráfico, direccionamiento basado en políticas, VPN y un host de otras características. Virtual Router Appliance proporciona rendimiento, facilidad de configuración y ventajas de mantenimiento en la ejecución en un servidor de hardware normal. Esta hardware tiene un tamaño que permite manejar la carga de direccionamiento de varias VLAN, y se le pueden pedir enlaces de red y matrices RAID redundantes. Todas las características de VRA están gestionadas por el cliente.  

IBM Virtual Router Appliance proporciona el último sistema operativo Vyatta 5600 para servidores nativos x86. Se suministra como un producto de alta disponibilidad o autónomo. 

**NOTA:** FortiGate Security Appliance (FSA) 10Gbps es cortafuegos de hardware de alto rendimiento (10Gbps) de arrendatario único (dedicado) con características de última generación como, por ejemplo, antivirus, prevención de intrusiones y filtrado web. Puede ser una alternativa a VRA (Virtual Router Appliance) para conseguir objetivos similares. Para obtener más información, consulte la [FSA documentación](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Cortafuegos
Para proteger el entorno de amenazas externas, Virtual Router Appliance puede aprovecharse como cortafuegos. Puede añadir reglas de cortafuegos para permitir o denegar el tráfico de red de entrada o salidas a los puertos en los que se está ejecutando la aplicación.Puede filtrar el tráfico dentro de su propia red. También es posible configurar Virtual Router Appliance para realizar un filtrado IPv4 e IPv6 con estado para proteger los datos críticos.

## Pasarela de red privada virtual (VPN)
Conecte su centro de datos local a IBM Cloud mediante la ejecución en túnel VPN, proporcionando Virtual Router Appliance como dispositivo de pasarela de red. Puede utilizar un túnel VPN de sitio a sitio de IPsec para una comunicación segura del centro de datos de su empresa con la red de IBM Cloud. 

## Conversión de dirección de red (NAT)
Con Virtual Router Appliance, puede proporcionar aplicaciones y servidores de base de datos sin interfaces de red pública mientras permite que los servidores puedan acceder a Internet mediante Source NAT. También puede ocultar los servidores detrás del dispositivo de pasarela con Destination NAT para mejorar la seguridad.

## Direccionamiento de empresa 
Para las aplicaciones multinivel en diferentes redes aisladas, Virtual Router Appliance le permite crear conectividad entre estas redes con mayor flexibilidad. 
