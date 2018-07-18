---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Cómo configurar IPSec en Vyatta

Se hará referencia al dispositivo Brocade 5400 vRouter (Vyatta) como "local" por lo que respecta al túnel IPSec (Internet Protocol Security). Cada uno de los mandatos siguientes realizará diferentes funciones para configurar IPSec sitio a sitio. Tenga en cuenta que este ejemplo de IPSec sitio a sitio muestra el túnel en la red pública de SoftLayer; utilice **bond0** para las conexiones privadas de IPSec sitio a sitio.

1. Indique al túnel el propósito de **interface bond1:**

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Configure la primera fase del túnel de dos fases. Los siguientes mandatos permiten:

  * Crear un nuevo grupo **ike** denominado **test** y utilizar **dh-group** como tipo de intercambio de claves.
  * Especificar el tipo de cifrado a utilizar; si no se configura, el dispositivo utilizará **aes128** como un valor predeterminado.
  * Utilizar la función hash **sha-1**.<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Configure la segunda fase del túnel de dos fases. Los siguientes mandatos permiten:

  * Inhabilitar PFS (secreto-perfecto-adelante), porque no todos los dispositivos pueden utilizarlo. (En el mandato, esp es la segunda parte del cifrado).
  * Especificar el tipo de cifrado a utilizar; si no se configura, el dispositivo utilizará **aes128** como un valor predeterminado.
  * Utilizar la función hash **sha-1**.<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Configure los parámetros de cifrado de IPSec sitio a sitio. Los siguientes mandatos permiten:

  * Especificar la IP remota y que IPSec utilice la clave secreta compartida previamente.
  * Utilizar la IP remota y la clave secreta TestPSK.
  * Establecer el grupo predeterminado **esp** del túnel en TestESP
  * Indicar a IPSec que utilice ike-group TestIKE, que se ha definido anteriormente.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Cree la correlación para el túnel IPSec. Los siguientes mandatos, basados en el ejemplo del material, permiten:

  * Indicar al túnel que se correlacione con la IP remota de 169.54.254.117 y con la dirección IP local de bond1, 50.97.240.219.
  * Direccionar sólo las direcciones IP con la subred de 10.54.9.152/29 que están en la interfaz del servidor local al servidor remoto 169.54.254.117.
  * Direccionar el tráfico remoto del túnel 1 169.54.254.117 a la subred remota de 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

El siguiente paso es configurar el dispositivo remoto, que es Brocade 5400 vRouter 6.6.5 R.

  * Utilice el dispositivo recién configurado (que se ha configurado en la modalidad operativa) para especificar los mandatos de configuración de muestra de mandatos. Aparecerá una lista de mandatos utilizados para configurar el dispositivo.
  * Copie los mandatos en un editor de texto. Los mandatos utilizados para configurar el dispositivo local se utilizarán para configurar el servidor remoto con modificaciones de la IP para apuntar al dispositivo Brocade 5400 vRouter 6.6.5R en SoftLayer.

La configuración remota utilizada anteriormente se muestra a continuación. Los cambios necesarios para la configuración local se muestran en negrita.

Configuración remota:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (Se ha intercambiado la IP igual de sitio a sitio)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (Se ha intercambiado la dirección IP local)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (No se han intercambiado el prefijo local y el prefijo remoto)

* Copie y pegue los mandatos nuevos en el servidor remoto (asegúrese de estar en modalidad de configuración), escriba commit y guarde.
* Escriba run show vpn ike sa para ver si el túnel está ahora establecido.

Este es un resumen de lo que se ha hecho: direccionar solo las direcciones IP con la subred de '10.54.9.152/29' que residen en la interfaz local (bond1, 50.97.240.219) a solo las subredes 192.168.1.2/32 en el servicio remoto que residen en la interfaz con la dirección IP de 169.54.254.117.
