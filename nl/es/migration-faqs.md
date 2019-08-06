---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Preguntas frecuentes sobre el fin de soporte para Vyatta 5400
{: #vyatta-5400-end-of-support-faqs}

Las siguientes son las preguntas más frecuentes al migrar desde Vyatta 5400.

## ¿Por qué el “Fin de soporte” (EoS) de Vyatta 5400 es el 31 de marzo de 2019?
{: faq}

En septiembre de 2017, se anunció que el EoS de Vyatta 5400 sería el 20 de febrero de 2018 basándose en la política de ciclo de vida de IBM para el soporte: seis meses más tarde de la fecha de disponibilidad general (GA) de la nueva versión, el {{site.data.keyword.vra_full}} (VRA).

Para respetar los tiempos de migración de los clientes, se amplió la fecha de EoS de Vyatta 5400 hasta el 31 de marzo de 2019. Debido a que el software de Debian 7 ya no está soportado por la comunidad de código abierto de Debian, no hay planes futuros para ampliar el soporte a los proveedores por parte de AT&T.

[Pulse aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) para ver el anuncio formal de fin de soporte.
{: important}

## ¿Qué significa el “fin de soporte” para mí como cliente?
{: faq}

Después de la fecha de Fin de soporte, AT&T dejará de proporcionar parches de código y de aceptar escalabilidades de soporte procedentes de IBM.

De forma similar, el soporte de IBM Cloud dejará de resolver problemas de configuración o de red en los despliegues de Vyatta 5400.  El soporte se limitará a las solicitudes de nivel de hardware (unidad de disco duro, RAM, etc.), alimentación y conectividad fuera de banda (IPMI).

Recomendamos encarecidamente a los clientes que tomen medidas inmediatas para migrar a una solución alternativa, como por ejemplo {{site.data.keyword.vra_full}} (VRA; basado en Vyatta 5600) o Juniper vSRX.  [Pulse
aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para empezar.

## ¿Qué sucede si todavía ejecuto mis cargas de trabajo de IBM Cloud utilizando un Vyatta 5400 después del 31 de marzo?
{: faq}

Vyatta 5400 seguirá funcionando después del 31 de marzo. Sin embargo, su empresa y sus entornos de aplicaciones podrían verse expuestos a amenazas de seguridad y otras manipulaciones indebidas debido a las vulnerabilidades latentes en el software de Vyatta 5400.

Si se encuentra con un problema de red que desactiva su entorno empresarial y de aplicaciones, y al rastrearlo ve que la causa raíz está en Vyatta 5400, escale el asunto a nuestro gestor de ofertas de 5400, porque ya no habrá soporte disponible de IBM ni de AT&T. Puede acceder al equipo de Gestión de ofertas a través del correo electrónico `nwom@us.ibm.com`.

## ¿El hardware del servidor nativo subyacente – todavía está soportado?
{: faq}

Se da soporte a sustituciones de hardware, pero si el proceso de resolución de problemas indica que su problema tiene relación con el sistema operativo de Vyatta, se le indicará que migre inmediatamente a una oferta de hardware soportada. [Pulse
aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para empezar.

## Como cliente que tiene un Vyatta 5400, ¿qué tengo que hacer de cara al 31 de marzo de 2019?
{: faq}

Los clientes que tienen un Vyatta 5400 deben migrar a VRA (Vyatta 5600), Juniper vSRX o a Fortigate Security Appliance (FSA) 10G. El VRA (Vyatta 5600) sigue con soporte completo. No hay ninguna fecha de fin de soporte actual o prevista para el VRA por parte de IBM Cloud ni de AT&T. [Pulse
aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para empezar.

  Los VRA y los vSRX son dispositivos gestionados por el cliente.
  {: note}

## ¿IBM ofrece soporte para migrar de Vyatta 5400 a VRA, vSRX o FSA 10G?
{: faq}

El servicio de conversión de configuración de Vyatta 5400 a VRA (5600) sigue disponible:

* Para los clientes existentes, IBM Cloud proporciona una oferta gratuita para ayudarle a refactorizar la configuración existente de Vyatta 5400
a los formatos de {{site.data.keyword.vra_full}} (VRA), Juniper vSRX o Fortigate Security Appliance (FSA) 10G. Para enviar una solicitud para el servicio de conversión de configuración, envíe un mensaje de correo electrónico a nwom@us.ibm.com con el asunto: `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx. `.

  Asegúrese de insertar la opción de aplicación en sustitución de `aaaaaaaa` ({{site.data.keyword.vra_full}}, Juniper vSRX o Fortigate Security Appliance (FSA) 10G), y su número de cuenta en sustitución de `xxxxxx` en la línea de asunto.
  {: note}

* Wanclouds, asociado nuestro en este proceso de configuración de conversión, ha completado varios cientos de encargos de migración de forma satisfactoria. Se encargarán de transformar su Vyatta 5400 existente para crear una funcionalidad similar en la plataforma Vyatta 5600. Proporcionan sus servicios en dos niveles que se describen a continuación:

  <img src="images/tiers.png" alt="dibujo" style="width: 700px;"/>

[Pulse
aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para empezar.

## ¿Existen servicios adicionales de migración de pago disponibles por parte de IBM Business Partners para la migración de Vyatta 5400?
{: faq}

Tenemos varios socios comerciales que proporcionan soporte de pago para las migraciones de Vyatta 5400. [Pulse aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) para obtener más información.

## ¿Hay algún contacto de soporte de gestión de ofertas de Vyatta 5400 en IBM donde pueda hacer preguntas relacionadas con la migración de Vyatta 5400?
{: faq}

Póngase en contacto con IBM Vyatta 5400 y VRA Network Offering Management para estas preguntas, en la dirección `nwom@us.ibm.com`. También puede ponerse en contacto con ellos utilizando Slack con el espacio de trabajo de IBM Watson Cloud Platform: `#vyatta-migration`

## ¿Qué recursos adicionales hay disponibles para ayudarme con esta migración?
{: faq}

Revise los siguientes recursos de documentación de {{site.data.keyword.vra_full}} para obtener más información:

  * [Iniciación a {{site.data.keyword.vra_full}}](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [Acerca de VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Visión general de la migración de Vyatta 5400](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
