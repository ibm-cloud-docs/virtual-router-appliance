---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: troubleshooting, vfp, problems, nat, dnat, gre

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Resolución de problemas de la interfaz de VFP
{: #troubleshooting-your-vfp-interface}

Este tema contiene información sobre la resolución de problemas de las interfaces VFP.

* Una interfaz de VFP no es una interfaz "real", como por ejemplo `dp0bond0` (o incluso un VIF o TUN). Es un marcador para los procesos de cortafuegos y NAT para colgar una interfaz de modo que puedan procesar el tráfico correctamente. Puede seguir direccionando el tráfico sobre una VFP como una interfaz normal, pero `tshark` y otros mandatos del supervisor no mostrarán el tráfico.
* Con NAT, debe utilizar un rango de subredes más específico para que tráfico se direccione al VFP, en lugar de utilizar la ruta del kernel que ha creado IPsec. Si no se establece una ruta estática, se seguirá la ruta del kernel. Puede probarlo con `show ip route x.x.x.x`.
* DNAT debe procesarse correctamente al salir de la VFP, pero el tráfico de retorno todavía necesita un conjunto de rutas estáticas. Busque el tráfico que no sea NAT de salida de la interfaz IPsec, `dp0bond1` o `dp0bond0` (o cualquier interfaz que utilice el tráfico IPsec).
* La utilización de protocolos de direccionamiento a través de un VFP no se ha probado.
* La utilización de un túnel GRE a través de un VFP tampoco se ha probado.
