---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, sync, synchronize, config-sync

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

# Sincronizar configuraciones de alta disponibilidad
{: #synchronizing-high-availability-configurations}

Dos VRA (Virtual Router Appliance) en un par de alta disponibilidad (HA) deben tener las configuraciones sincronizadas adecuadamente para que ambos dispositivos se comporten de manera similar. Esto se realiza mediante el mandato `configuration sync-maps` y puede elegir qué parte de la configuración se sincronizará. Si realiza un cambio en una máquina, impulsará la configuración marcada a otro dispositivo.

De esta forma, se sincroniza y se guarda la configuración de ejecución del dispositivo local en el dispositivo remoto. Sin embargo, como paso del proceso de confirmación, no se guarda la configuración en el dispositivo local.
{: note}

Las configuraciones exclusivas de un sistema no deben estar sincronizadas con el otro. Las direcciones IP y MAC reales no deben estar sincronizadas, por ejemplo. El mismo nodo de configuración `system config-sync` y el nodo `service https` no pueden sincronizarse.

El ejemplo siguiente muestra la sincronización de la configuración:

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

Las dos primeras líneas crean la correlación de sincronización real. Aquí, la stanza de configuración `security firewall` se establecerá en `sync-map`. Como resultado, cualquier cambio realizado en el nodo de configuración se enviará al dispositivo remoto. Sin embargo, los cambios realizados en `security user` no se enviarán, ya que no coinciden con la regla. Puede hacer que `sync-map` sea tan específico o general como desee.

Las tres líneas siguientes designan el usuario `config-sync` del direccionador remoto y la contraseña, la IP y la correlación de sincronización que debe enviarse. Cualquier cambio que coincida con las reglas para `TEST`, irá a `remote-router 192.168.1.22`, utilizando esta información de inicio de sesión. Tenga en cuenta que se hace una llamada `REST` para realizar esta acción utilizando la API VRA, de modo que el servidor HTTPS debe estar ejecutándose (y desbloqueado) en el direccionador remoto.

Para sincronizar la configuración de una contraseña, como por ejemplo secreto precompartido para una VPN IPSec, el sistema en espera debe tener configurado el grupo `secretos` y el usuario de sincronización de configuración (config-sync) debe estar en ese grupo.

```
set system login group secrets
set system login user vyatta authentication plaintext-password '****'
set system login user vyatta group secrets
```

La sincronización de configuración se lleva a cabo cuando confirma un cambio. Esté atento a los mensajes de error del dispositivo remoto. Si la configuración no está sincronizada, deberá solucionar el problema en el dispositivo remoto para volver a hacerla operativa.

También puede ver las diferencias de configuración utilizando el mandato `show config-sync difference`.
