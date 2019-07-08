---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: upgrade, os

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
{:important: .important}

# Actualización del SO
{: #upgrading-the-os}

Se puede realizar la actualización del sistema operativo VRA con el mandato ``add system image`` en un archivo ISO local cargado por nuestro equipo de soporte. Se puede obtener una lista de las versiones de actualización de Vyatta disponibles mediante el sistema de incidencia de soporte de IBM©.

Para empezar el proceso de actualización, abra una incidencia con el sistema de incidencia de soporte de IBM solicitando una actualización y que se cargue al sistema una nueva imagen ISO. Recibirá un correo electrónico de soporte de IBM indicando el lugar en el que se ha cargado el archivo ISO. En el ejemplo, se encuentra en el directorio ``tmp``.

El proceso de actualización que se muestra a continuación es un VRA único. Si está utilizando VRA en modo de alta disponibilidad, debe ejecutar el mismo mandato de actualización en ambos sistemas. Además, se recomienda que actualice primero la máquina `BACKUP`, y verifique que funciona correctamente. A continuación, acceda a la máquina `MASTER` rechácela utilizando el mandato `reset vrrp`. Finalmente, actualice el `MASTER` original cuando `BACKUP` haya tomado el control.
{: important}

Para actualizar el VRA, realice el procedimiento siguiente.

1. Ejecute el mandato ``add system image <Local ISO File>``.
2. Pulse **Intro** para aceptar el nombre predeterminado de la imagen ISO, o especifique uno propio.
3. Elija si desea guardar el directorio de configuración y el archivo de configuración actuales.
4. Elija si desea guardar las claves de host SSH de la configuración actual.
5. Pulse **Intro** para aceptar la consola del sistema predeterminada o especifique una propia.
6. Pulse **Intro** para aceptar la velocidad de la consola predeterminada o especifique una propia.
7. Especifique el mandato `reboot` y escriba `Sí` para rearrancar la máquina.
8. Si está utilizando VRA en modo de alta disponibilidad, ejecute los pasos del 1 al 7 de nuevo en la segunda máquina.

El ejemplo siguiente ilustra el proceso de actualización.

```
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Welcome to the Brocade vRouter image installer.
Checking MD5 checksums of files on the ISO image...OK.
What would you like to name this image? [5.2R5S4.08091424]: 5.2R5S4
This image will be named: 5.2R5S4
Would you like to save the current configuration
directory and config file? (Yes/No) [Yes]:
Saving current configuration...
Would you like to save the SSH host keys from your
current configuration? (Yes/No) [Yes]:
Saving SSH keys...
Copying squashfs image...
Copying kernel and initrd images...
Copying saved configuration to config partition.
Copying saved SSH host keys.
Enter the desired system console [tty0]:
Enter the console speed [38400]:
Running post-install script...
Done.

vyatta@vra01:~$ show system image
The system currently has the following image(s) installed:

   1: 5.2R5S3.06301309 (running image)
   2: 5.2R5S4 (default boot)

vyatta@vra01:~$ reboot
Proceed with reboot? (Yes/No) [No] y
The system is going down for reboot NOW!

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```
