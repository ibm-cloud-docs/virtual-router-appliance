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

# Recarga del SO
{: #reloading-the-os}

Al iniciarse en el Portal de clientes, el proceso de recarga del SO borrará todas las configuraciones presentes en el dispositivo y lo restaurará a su configuración original. Cualquier cambio realizado desde la última carga del sistema se borrará. Como resultado, si ha realizado cambios (por ejemplo en el grupo de VRRP de otra máquina), es probable que la máquina cargada tenga un conflicto cuando vuelva a estar en línea.

Para volver a cargar el SO, realice el procedimiento siguiente:

1. Inicie sesión en el [Portal de clientes ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){: new_window} utilizando las credenciales exclusivas.
2. Seleccione **Lista de dispositivos** de la lista desplegable Dispositivos.
3. Pulse en el servidor que desea volver a cargar.
4. Seleccione **Recarga de SO** del menú desplegable **Acciones** en la parte superior izquierda de la página.
5. En la pantalla Recarga de SO, pulse **Editar** para la Categoría que requiere una actualización. Seleccione **AT&T** como proveedor y la versión de sistema operativo que desee volver a cargar.
6. Pulse el botón **Recargar la configuración indicada** para proceder a la ventana emergente **Revisar**. Pulse **Cancelar** para cancelar los cambios en el dispositivo y salga de la pantalla.
7. Verifique que todos los detalles de la sección Nueva configuración son correctos. Pulse **Siguiente** para avanzar a la pantalla emergente Confirmación.
8. Pulse el botón **Confirmar recarga de SO** para confirmar e iniciar la recarga de SO. Pulse **Cancelar** para cancelar la acción.

Tras iniciarse el proceso de recarga de SO, el dispositivo se pondrá fuera de línea y comenzará el proceso de recarga. El tiempo que tarda en completarse una recarga de SO varía en función de la configuración actual y la nueva configuración del dispositivo. Durante el proceso de configuración, se muestra en cada pantalla el tiempo mínimo de la recarga de SO. El periodo de tiempo es una estimación hecha por el sistema. Si tarda más de 24 horas, póngase en contacto con nuestro equipo de soporte. Cuando el dispositivo vuelva a estar en línea, funcionará según se haya especificado en la nueva configuración para la recarga de SO. 

**NOTA:** Se perderán todos los datos anteriormente guardados en el dispositivo, pero se pueden restaurar si se realizó una copia de seguridad antes de su recarga. Si no se efectuó ninguna copia de seguridad, no se podrán recuperar los datos.
