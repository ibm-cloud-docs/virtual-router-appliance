---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# Visión general sobre la migración
{: #migration-overview}

Los clientes de sistemas Vyatta 5400 anteriores deben migrar a {{site.data.keyword.vra_full}} (VRA) lo antes posible, ya que el soporte de Vyatta 5400 se dejará de ofrecer a partir del 31 de marzo de 2019. Para leer el anuncio completo de fin de soporte y para obtener más información, pulse [aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## Cómo puede beneficiarle actualizarse
{: #how-upgrading-can-benefit-you}

Además de una variedad de características y funciones nuevas que aporta {{site.data.keyword.vra_full}}, también ofrece una serie de mejoras. Consulte nuestras [Preguntas frecuentes](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) para obtener más detalles.

La actualización a un nuevo dispositivo tendrá una configuración distinta que en Vyatta 5400. Como resultado, la configuración de Vyatta 5400 (archivo) no se transferirá a su nuevo dispositivo.
{: note}

## Documentación de migración
{: #migration-documentation}

Para ayudarle a migrar desde Vyatta 5400, hemos preparado la documentación y el soporte siguientes:

| Documentación de migración | Descripción |
| ------------- | ------------- |
| [Actualización de Vyatta 5400 y Reutilización de su dirección IP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Instrucciones para actualizar Vyatta 5400 a un {{site.data.keyword.vra_full}} equivalente, reutilizando las direcciones IP de Vyatta 5400. |
| [Migración de Vyatta 5400 a Juniper vSRX o Fortigate Security Appliance (FSA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Instrucciones para migrar a Juniper vSRX o a Fortigate Security Appliance. Esta opción no le permitirá reutilizar el dispositivo Vyatta 5400 existente ni conservar las direcciones IP asociadas. |
| [Problemas comunes de migración](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Información sobre los problemas o cambios de comportamiento más frecuentes que se pueden encontrar después de migrar de un dispositivo Vyatta 5400 a un {{site.data.keyword.vra_full}}. En muchos casos, se incluyen métodos alternativos para solucionar estos problemas. |

Si simplemente necesita pedir un nuevo {{site.data.keyword.vra_full}}, [haga clic aquí](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started) para obtener más detalles.
{: note}
