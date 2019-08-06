---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: updates, changes, additions

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


# Actualizaciones recientes de {{site.data.keyword.vra_full}}
{: #recent-updates-for-ibm-virtual-router-appliance}

Descubra las características nuevas y actualizadas de {{site.data.keyword.vra_full}} (VRA).

## Agosto de 2018
{: #august-2018}

### Sistema operativo Brocade Versión 18.x
{: #brocade-operating-system-version-18-x}

La versión 18.x del sistema operativo Brocade está ahora disponible para {{site.data.keyword.vra_full}}. Entre otras características nuevas, esta versión proporciona la solución para la brecha de seguridad de Spectre.

Las nuevas características de VRA 18.x se describen en los temas siguientes:

* [Configuración de un túnel IPsec que funciones con cortafuegos de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [Configuración de una interfaz de VFP con IPsec y cortafuegos de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [Utilización de NAT con IPsec basado en prefijo](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [Resolución de problemas de la interfaz de VFP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

Si va a migrar de Vyatta 5400, la mejor manera de actualizar a 18.x es a través del [procedimiento normal](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) de recarga completa de sistema operativo.

Puesto que no hay una correlación sencilla de uno a uno en la funcionalidad entre Vyatta 5400 y el dispositivo direccionador virtual, resulta útil crear una configuración de línea base para VRA. Un IBM Partner, WanClouds, puede ayudarle con este proceso y proporcionarle orientación sobre la creación de una funcionalidad similar a la Vyatta 5400 en su VRA.

Para obtener más información sobre los problemas comunes que puede encontrar durante este proceso de actualización, consulte nuestra [documentación adicional](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
