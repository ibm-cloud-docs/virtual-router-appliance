---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, setup, 5400

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

# Configuración de reglas NAT en Vyatta 5400
{: #setting-up-nat-rules-on-vyatta-5400}

Este tema contiene ejemplos de las reglas de NAT (conversión de direcciones de red) utilizadas en Vyatta.

## Regla de NAT One-to-many (enmascaramiento)
{: #one-to-many-nat-rule-masquerade-}

Especifique los mandatos siguientes en el símbolo del sistema:

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

Las solicitudes de conexión de máquinas en la red `10.xxx.xxx.xxx` se correlacionan con la IP en bond1 y reciben un puerto temporal asociado cuando son salientes. La intención es asignar números de regla de enmascaramiento one-to-many más altos, de forma que no entren en conflicto con las reglas de NAT inferiores que pueda tener.

Debe configurar el servidor para que pase el tráfico de Internet a través de VRA, de modo que su pasarela predeterminada sea la dirección IP privada de la LAN virtual (VLAN) gestionada. Por ejemplo, para `bond0.2254`, la pasarela es `10.52.69.201`. Esta debería ser la dirección de pasarela para que el servidor pase el tráfico de Internet.
{: note}

Utilice el siguiente mandato para resolver problemas de NAT:

  '''
  run show nat source translations detail 
'''
  {: tip}

## Regla de NAT One-to-one
{: #one-to-one-nat-rule}

El mandato siguiente muestra cómo configurar una regla de NAT one-to-one. Observe que se han definido números de regla más bajos que la regla de enmascaramiento. De esta forma, las reglas one-to-one tendrán prioridad sobre las reglas one-to-many.

Las direcciones IP que se correlacionan en reglas one-to-one no se pueden enmascarar. Si convierte una dirección IP de entrada, debe convertir dicha IP de salida para que el tráfico vaya en ambos sentidos.
{: tip}

Los mandatos siguientes son para una regla de origen y una de destino. Escriba `show nat` en modalidad de configuración para ver el tipo de regla de NAT.

  Utilice el siguiente mandato para resolver problemas de NAT: `run show nat source translations detail`.
  {: tip}

Especifique los mandatos siguientes en el símbolo del sistema después de comprobar que está en modalidad de configuración:

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

Si el tráfico entra en la IP `50.97.203.227` en bond1, esa IP se correlacionará con la IP `10.52.69.202` (en cualquier interfaz definida). Si el tráfico sale con la IP `10.52.69.202` (en cualquier interfaz definida), se convertirá a la IP `50.97.203.227` y saldrá en la interfaz bond1.

Las direcciones IP que se correlacionan en reglas one-to-one no se pueden enmascarar. Si convierte una dirección IP de entrada, debe convertir la misma IP de salida para que su tráfico vaya en ambos sentidos.
{: note}

## Adición de rangos de IP a través de VRA
{: #adding-ip-ranges-through-your-vra}

En función de la configuración de VRA, es posible que desee aceptar direcciones IP de IBM© Cloud específicas.

Los nuevos despliegues de vRouter vienen con las direcciones IP de red de servicios de IBM Cloud definidas en una regla de cortafuegos denominada `SERVICE-ALLOW`.

El ejemplo siguiente es un ejemplo de `SERVICE-ALLOW`. No es un conjunto completo de reglas de IP privada.

```
~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~
```

Una vez que haya definido las reglas de cortafuegos, las puede asignar como crea conveniente. A continuación se enumeran dos ejemplos.

Aplicada a una zona: `set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Aplicada a una interfaz de enlace: `set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
