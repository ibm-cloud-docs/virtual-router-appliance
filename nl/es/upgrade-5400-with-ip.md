---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# Actualización de Vyatta 5400 y Reutilización de sus direcciones IP
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

Esta opción le permite reutilizar el dispositivo Vyatta 5400 existente como un Virtual Router Appliance equivalente (VRA), y conservar las direcciones IP asociadas. Los procedimientos siguientes proporcionan instrucciones para actualizar un Vyatta 5400 autónomo, o dos dispositivos Vyatta 5400 que funcionan en un par de Alta Disponibilidad (HA).

## Actualización de un Vyatta 5400 autónomo
{: #upgrading-a-stand-alone-vyatta-5400}

Para actualizar un Vyatta 5400 a IBM Virtual Router Appliance, siga el procedimiento siguiente:

1. Asegúrese de haber realizado una copia de seguridad de los 5400 y de haber almacenado los datos en dos ubicaciones diferentes. Esto incluye claves SSH, certs SSL, scripts y cualquier otro archivo necesario para recuperar la configuración de Vyatta 5400 actual, si es necesario.
2. Vuelva a cargar Vyatta 5400 en un Virtual Router Appliance predeterminado utilizando las instrucciones de [este tema](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os).

  Una recarga de sistema operativo generará una nueva contraseña para los ID de usuario root y de Vyatta.
  {: note}

4. Tome nota de la nueva contraseña del Virtual Router Appliance.
5. Configure el VRA que acaba de volver a cargar con los valores que desee utilizando [estas instrucciones](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).

## Actualización de un Par de alta disponibilidad Vyatta 5400
{: #upgrading-a-vyatta-5400-high-availability-pair}

Para actualizar dos Vyatta 5400 en un par de HA, siga el procedimiento siguiente:

1. Identifique el dispositivo Vyatta 5400 de reserva y vuelva a cargarlo como un Virtual Router Appliance, utilizando el procedimiento anterior.

  Idealmente, una configuración de HA funcional muestra todas las interfaces como BACKUP o como MASTER en un nodo en particular. Ante la duda, utilice el mandato `show vrrp` para confirmarlo.
  {: note}
2. Configure el VRA que acaba de volver a cargar con los valores que desee utilizando [estas instrucciones](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
3. El Vyatta 5400 maestro y el Virtual Router Appliance de reserva estarán en un par VRRP. Cambie el Virtual Router Appliance de reserva actual para que pase a maestro utilizando el mandato de VRRP: `reset VRRP master`.

  Esta acción provocará una interrupción en las sesiones existentes y el estado de las sesiones se listará como Perdido. Cualquier sesión de NAT existente también se restablecerá.
  {: note}

5. Verifique que el nuevo Virtual Router Appliance maestro esté funcionando como se espera.
6. Realice el procedimiento de recarga anterior en el Vyatta 5400 de reserva actual para actualizarlo a VRA.
7. Después de la segunda recarga, configure el Virtual Router Appliance de reserva como desee, utilizando [estas instrucciones](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
