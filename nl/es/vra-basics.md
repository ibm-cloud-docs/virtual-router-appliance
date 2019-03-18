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

# Acceso y configuración de IBM Virtual Router Appliance
{: #accessing-and-configuring-the-ibm-virtual-router-appliance}

VRA puede configurarse con una sesión de consola remota mediante SSH o iniciando sesión en la GUI de web. De forma predeterminada, la web GUI no está disponible en la Internet pública. Para habilitar la web GUI, primero inicie sesión mediante SSH.

**NOTA:** La configuración de VRA fuera de su shell e interfaz puede producir resultados inesperados y, por lo tanto, no se recomienda.

## Acceso al dispositivo mediante SSH
La mayoría de los sistemas operativos basados en Unix, como Linux, BSD y Mac OSX, incluyen clientes OpenSSH con las instalaciones predeterminadas. Los usuarios de Windows pueden descargar un cliente SSH, como PuTTy.

Se recomienda inhabilitar el SSH en la IP pública y permitirlo solo en la IP privada. Las conexiones a las IP privadas requieren que el cliente esté conectado a la red privada. Puede iniciar sesión con una de las opciones de VPN predeterminadas (PPTP VPN, SSL-VPN e IPsec) ofrecidas en el portal de clientes, o utilizando una solución VPN personalizada configurada en VRA.

Utilice la cuenta de Vyatta de la página **Detalles del dispositivo** para iniciar sesión mediante SSH. También se proporciona la contraseña de root, pero el inicio de sesión como root se inhabilita de forma predeterminada por razones de seguridad.

`ssh vyatta@[IP address] `

**NOTA:** Se recomienda mantener inhabilitados los inicios de sesión como root. Inicie sesión utilizando una cuenta de Vyatta y ascienda a root solo cuando sea necesario.

Las claves SSH también se pueden suministrar durante el despliegue para evitar que la cuenta de Vyatta inicie sesión. Después de verificar la capacidad para acceder a VRA utilizando la clave SSH, puede inhabilitar el inicio de sesión de usuario/contraseña estándar ejecutando los mandatos siguientes:

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Acceso al dispositivo mediante la Web GUI

Inicie sesión en VRA utilizando las instrucciones de SSH anteriores y ejecute los mandatos siguientes para habilitar el servicio HTTPS:

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

Una vez completados los mandatos, indique `https://<ip.address>`en la barra de dirección de su navegador, sustituyendo la dirección de IP por su VRA. Es posible que se le solicite aceptar el certificado emitido de VRA. Si es así, inicie sesión en la interfaz web con las credenciales de Vyatta cuando se le solicite.

## Modalidades
**Modalidad de configuración:** Se invoca con el mandato `configure`. En esta modalidad se realiza la configuración del sistema VRA.

**Modo operativo:** La modalidad inicial al registrarse en un sistema VRA. En esta modalidad, se pueden ejecutar los mandatos `show` para consultar información sobre el estado del sistema. El sistema también puede reiniciarse en esta modalidad.

El shell de VRA es una interfaz modal con varias modalidades de operación. La modalidad primaria/predeterminada es **Operativa**, y está será la modalidad que se presente al iniciar sesión. En la modalidad **Operativa**, puede visualizar información y emitir mandatos que afecta a la ejecución actual del sistema como, por ejemplo, establecer la fecha/hora o rearrancar el dispositivo.

El mandato `configure` sitúa al usuario en la modalidad **Configuración**, en la que se puede editar la configuración. Tenga en cuenta que los cambios de configuración no tienen lugar de forma inmediata; deben estar confirmados y guardados. Cuando se especifican los mandatos, entran en un búfer de configuración. Una vez se hayan especificado todos los mandatos necesarios, ejecute el mandato `commit` para que se apliquen los cambios.

Para guardar los mandatos de forma permanente, ejecute el mandato `save` después del mandato `commit`.

Los mandatos de modalidad operativa pueden ejecutarse desde la modalidad de configuración iniciando el mandato con `run`. Por ejemplo:


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

La marca hash (`#`) indica la modalidad Configuración. Al iniciar un mandato con `run` se indica al shell VRA que se presenta un mandato operacional. El ejemplo anterior también ilustra la capacidad a "grep" en la salida de mandatos.

## Exploración de mandatos

El shell de mandato de VRA incluye funciones de finalización de separador. Si desea saber qué mandatos están disponibles, pulse la tecla tabulador para obtener una lista con una breve explicación. Esto funciona en el indicador de shell y mientras se escribe un mandato. Por ejemplo:  

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Configuración de ejemplo
Las configuraciones se organizan en un patrón jerárquico de nodos. Considere este bloque de ruta estática:

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

Esto lo generarían los mandatos:

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

Este ejemplo ilustra que es posible que un nodo (estático) tenga varios nodos hijo. Para eliminar la ruta de `192.168.1.0/24` debe utilizarse el mandato `delete protocols static route 192.168.1.0/24`. Si `192.168.1.0/24` se deja fuera del mandato, los nodos de ruta se marcan para supresión.

Recuerde que la configuración no se cambia hasta que se emite el mandato `commit`. Para comparar la configuración de ejecución actual con los cambios presentes en el búfer de configuración, utilice el mandato `compare`. Para vaciar el búfer de configuración, utilice `discard`.

## Control de accesos basado en roles y usuarios (RBAC)
Las cuentas de usuario pueden configurarse con tres niveles de acceso:

* Administrador
* Operador
* Superusuario

Los usuarios de nivel de operador pueden ejecutar mandatos `show` para ver el estado de ejecución del sistema y emitir mandatos `reset` para reiniciar los servicios en el dispositivo. Los permisos a nivel de operador no implican acceso de solo lectura.

Los usuarios de nivel de administrador tienen acceso completa a todas las configuraciones y operaciones del dispositivo. Los usuarios de administrador puede visualizar las configuraciones de administración en ejecución, cambiar los valores de configuración para el dispositivo, rearrancar el dispositivo y cerrarlo.

Los usuarios de nivel de superusuario pueden ejecutar mandatos con privilegios raíz a través del mandato `sudo` además de poseer privilegios de administrador.

Se pueden configurar los usuarios para los estilos de contraseña o autenticación de clave pública, o ambos. La autenticación de clave pública se utiliza con SSH y permite a los usuarios iniciar sesión utilizando un archivo de claves en su sistema. Para crear un usuario operador con una contraseña:

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**NOTA:** Cuando no se especifica ningún nivel, se considera el usuario a nivel de administrador. En este caso, las contraseñas de usuario se muestran como cifradas en el archivo de configuración.

El control de acceso basado en roles (RBAC) es un método de restricción de acceso para parte de la configuración de los usuarios autorizados. RBAC permite a los administradores definir reglas para un grupo de usuarios que restringe los mandatos que pueden ejecutar.

RBAC se realiza creando un grupo asignado al conjunto de reglas de gestión de control de acceso (ACM), añadiendo un usuario al grupo, creando un conjunto de reglas que coincida con el grupo de las vías de acceso del sistema y configurando el sistema para permitir o denegar las vías de acceso que se aplican al grupo.

De forma predeterminada, no hay ningún conjunto de reglas ACM definido en Virtual Router Appliance, y ACM está inhabilitado. Si desea utilizar RBAC para proporcionar control de acceso granular, debe habilitar la ACM y añadir las siguientes reglas ACM predeterminadas, además de sus propias reglas definidas:

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

Consulte la [documentación adicional](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation) antes de intentar habilitar las reglas. Los valores erróneos en las reglas de ACM pueden provocar la denegación de acceso o errores en la inoperancia del sistema.
