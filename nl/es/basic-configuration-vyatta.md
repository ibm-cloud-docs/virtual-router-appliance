---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuración básica de Vyatta 5400

Realice el procedimiento siguiente para configurar Vyatta 5400.

La LAN virtual (VLAN) pública 1224, que ahora está asociada y direccionada, debe configurarse para poder pasar tráfico. Se necesitan dos elementos de información para completar la configuración:

  * El enlace del lado público del dispositivo Brocade 5400 vRouter
  * La pasarela predeterminada de 1224

Tenga en cuenta que la opción de cálculo se encuentra en la VLAN del lado público, no en la VLAN pública del dispositivo Brocade 5400 vRouter.

## En el portal de Softlayer

1\. En el portal web de SoftLayer, bajo **Dispositivos**, localice el enlace en el separador **Configuración** del dispositivo Brocade 5400 vRouter. Supongamos que <span style="text-decoration: underline">eth1=bond1</span> en el dispositivo.

2\. Seleccione **Red > Gestión de IP > VLAN** en el portal web para buscar la pasarela predeterminada para la VLAN 1224.

3\. Pulse la VLAN en la lista y pulse la **subred** en la que vea su pasarela. Tome nota de la información de CIDR (direccionamiento entre dominios sin clases) que se encuentra tras la barra inclinada. 

## En la GUI de Vyatta

4\. Desde el panel de control, pulse el separador **Configuration**.

5\. Expanda **Interfaces > Bonding > bond1** en la parte izquierda de la pantalla; resalte **vif**.

6\. Especifique el nombre de las VLAN en el campo **vif** (para nuestro objetivo, 1224) y pulse el botón **Create**. El proceso tardará unos segundos en completarse.

7\. Seleccione el elemento **vif 1224** recién creado bajo el icono de **vif**.

8\. Marque el recuadro bajo **dhcp** y escriba la dirección IP de pasarela predeterminada y la notación de CIDR de máscara de la VLAN que desea que pase por Brocade 5400 vRouter (por ejemplo, VLAN 1224).

9\. Pulse el botón **Set** y pulse **Commit**.

10\. Pulse **Save** en la barra de menús del medio, si no, la configuración vuelve a sus valores predeterminados la siguiente vez que se reinicia el sistema.

**NOTA:** la retrotracción de la configuración puede ser una característica muy útil si deshace la configuración probando los cambios. Mientras no guarde los cambios, puede reiniciar el servidor desde el portal web y restaurar la configuración anterior.

11\. Pulse el separador Statistics y abra la nueva interfaz para verificar y supervisar el tráfico.

## Configurar la VLAN privada utilizando la CLI

Hay dos modalidades de mandatos en la CLI (interfaz de línea de mandatos): operativa y de configuración. 

La modalidad operativa proporciona acceso a los mandatos operativos para:

  * Visualizar y borrar información
  * Habilitar e inhabilitar la depuración
  * Configurar los valores de terminal
  * Cargar y guardar la configuración
  * Reiniciar el sistema

La modalidad de configuración proporciona acceso a los mandatos para:

  * Crear
  * Modificar
  * Suprimir
  * Confirmar
  * Mostrar información de configuración
  * Navegar por la jerarquía de configuración

Cuando inicia sesión en el sistema, el sistema está en modalidad operativa; deberá cambiar a la modalidad configuración para utilizar estos mandatos.

**NOTA:** Puede saber en qué modalidad está, operativa o de configuración, en función del símbolo del sistema. Si el símbolo del sistema es #, está en modalidad operativa; si el símbolo es $, está en modalidad de configuración.

Utilice los pasos siguientes para configurar la VLAN privada mediante la CLI. Recuerde que los valores necesarios para configurar la VLAN son:

  * Nombre de la VLAN que desea direccionar a través del dispositivo Brocade 5400 vRouter (2254)
  * Pasarela y máscara (formato CIDR) de la VLAN que se va a direccionar (10.52.69.201/29)
  * Nombre de enlace privado del dispositivo Brocade 5400 vRouter (bond0)

1. Establezca una conexión SSH a Brocade 5400 vRouter (dirección IP pública o privada) utilizando **vyatta** como **nombre de usuario**; indique la contraseña cuando se le solicite.

   **NOTA:** debe crear un nuevo usuario dentro de Brocade 5400 vRouter e inhabilitar el usuario inicial predeterminado `vyatta`.

2. Configure la vif:

  * Escriba *configure* en el indicador de mandatos para entrar en la modalidad de configuración.
  * Escriba *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* en el indicador de mandatos para establecer la vif.
  * Escriba *commit* en el indicador de mandatos para confirmar los valores.
  * Escriba *save* para guardar los valores.
  * Escriba *exit* para volver a la modalidad operativa.

3\. Escriba *show interfaces* para comprobar los valores que acaba de confirmar.

4\. Direccione las VLAN restantes a través del dispositivo Brocade 5400 vRouter.
