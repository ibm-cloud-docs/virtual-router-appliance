---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Migración de Vyatta 5400 a Juniper vSRX o Fortigate Security Appliance (FSA) de 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Además de poder actualizar el dispositivo Vyatta 5400 a un IBM Virtual Router Appliance, también puede migrar a algunas de las opciones más eficientes, fiables y seguras en el mercado; Juniper vSRX y Fortigate Security Appliance.
No obstante, no podrá reutilizar el dispositivo Vyatta 5400 existente ni conservar las direcciones IP asociadas.

Los procedimientos siguientes proporcionan instrucciones para actualizar un Vyatta 5400 autónomo, o dos dispositivos Vyatta 5400 que funcionan en un par de Alta Disponibilidad (HA) a Juniper vSRX o bien a FSA.

## Actualización de un Vyatta 5400 autónomo
{: #upgrading-a-stand-alone-vyatta-5400}

Para actualizar un Vyatta 5400 a un dispositivo Juniper vSRX o FSA - 10G con el menor tiempo de inactividad posible, le recomendamos que siga el procedimiento siguiente.

1. Asegúrese de haber realizado una copia de seguridad de los 5400 y de haber almacenado los datos en dos ubicaciones diferentes. Esto incluye claves SSH, certs SSL, scripts y cualquier otro archivo necesario para recuperar la configuración de Vyatta 5400 actual, si es necesario. Para hacerlo, siga [estas instrucciones](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Póngase en contacto con nwom@us.ibm.com para solicitar que se convierta su configuración

3. Pida el nuevo dispositivo, siguiendo las instrucciones para [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) o para [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

  Esta es una buena oportunidad para considerar hacer un pedido de una solución de alta disponibilidad.
  {: note}

4. Cargue la configuración que ha recibido de nwom@us.ibm.com en el dispositivo que ha recibido.

5. Planifique el tiempo necesario para migrar las VLAN al nuevo dispositivo.

  Este paso requiere un breve tiempo de inactividad.
  {: note}

6. Pruebe y confirme que el nuevo dispositivo está funcionando correctamente.

7. Abra una incidencia de soporte para cancelar el dispositivo Vyatta 5400.

## Actualización de un Par de alta disponibilidad Vyatta 5400
{: #upgrading-a-vyatta-5400-high-availability-pair}

Para actualizar dos Vyatta 5400 en un par de HA, siga el procedimiento siguiente:

1. Asegúrese de haber realizado una copia de seguridad de los 5400 y de haber almacenado los datos en dos ubicaciones diferentes. Esto incluye claves SSH, certs SSL, scripts y cualquier otro archivo necesario para recuperar la configuración de Vyatta 5400 actual, si es necesario. Para hacerlo, siga [estas instrucciones](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Póngase en contacto con nwom@us.ibm.com y solicite que se convierta su configuración.

3. Pida el nuevo dispositivo, siguiendo las instrucciones para [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) o para [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

4. Cargue la configuración que ha recibido de nwom@us.ibm.com en el dispositivo que ha recibido.

5. Planifique el tiempo necesario para migrar las VLAN al nuevo dispositivo.

  Este paso requiere un breve tiempo de inactividad.
  {: note}

6. Pruebe y confirme que el nuevo dispositivo está funcionando correctamente.

7. Abra una incidencia de soporte para cancelar sus dispositivos Vyatta 5400.
