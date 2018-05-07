---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Preguntas técnicas más frecuentes (FAQ)
Las siguientes preguntas frecuentes tratan sobre la configuración de IBM Virtual Router Appliance (VRA) y la migración a VRA desde Vyatta 5400.

## ¿Cómo permito el tráfico vinculado a Internet de hosts que están en una VLAN privada?
Este tráfico necesita obtener una dirección IP de origen público, por lo que se necesita un origen NAT para hacer pasar la IP privada por la IP pública de VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

La configuración anterior solo realiza SNAT de tráfico procedente de los servidores en la privada `10.0.0.0/8`.

De este modo, se garantiza que no interfiera con los paquetes que ya tienen una dirección de origen direccionable de Internet.

## ¿Cómo puedo filtrar el tráfico vinculado a Internet y permitir sólo protocolos/destinos específicos?
Esta es una pregunta habitual cuando se deben combinar un cortafuegos y NAT de origen.

Tenga en cuenta el orden de las operaciones en VRA cuando diseñe los conjuntos de reglas.

Las reglas de cortafuegos se aplican *después de* la SNAT.

Para bloquear todo el tráfico saliente en un cortafuegos, pero permitir flujos específicos de SNAT, deberá trasladar la lógica de filtrado a la SNAT.

Por ejemplo, para permitir sólo el tráfico vinculado a Internet HTTPS para un host, la regla de SNAT sería:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` sería una dirección pública para VRA. 

Es muy recomendable utilizar la dirección pública de VRRP de VRA, de modo que pueda diferenciar entre el tráfico público de VRA y el host.

Supongamos que `150.1.2.3` es la dirección de VRA de VRRP y `150.1.2.5` es la dirección dp0bond1 real.

El cortafuegos aplicado en `dp0bond1 out` sería:

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

Tenga en cuenta que la combinación de NAT de origen y cortafuegos logra el objetivo de diseño necesario. 

Asegúrese de que las reglas sean apropiadas para el diseño y de que no haya otras reglas que permitan el tráfico que se debe bloquear. 

## ¿Cómo protejo el propio VRA con un cortafuegos basado en zonas?
VRA no tiene `zona local`.

En su lugar, puede utilizar la funcionalidad Control Plane Policing (CPP), ya que se aplica como cortafuegos `local` en bucle de retorno.

Tenga en cuenta que se trata de un cortafuegos sin estado y deberá permitir explícitamente el tráfico de retorno de sesiones de salida que se origina en VRA.

## ¿Cómo puedo restringir SSH y bloquear conexiones procedentes de Internet?
Se considera una práctica recomendada no permitir conexiones SSH de Internet y utilizar otros medios de acceso a la dirección privada, como VPN con SSL.

De forma predeterminada, VRA acepta SSH en todas las interfaces.
Para escuchar solamente conexiones SSH en la interfaz privada, debe establecerse la siguiente configuración:

```
set service ssh listen-address '10.1.2.3'
```

Tenga en cuenta que la dirección IP se debe sustituir por la dirección de VRA.
