---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Direccionar VLAN
Virtual Router Appliance es capaz de direccionar varios VLAN en la misma interfaz de red (por ejemplo, `dp0bond0` o `dp0bond1`). Esto se consigue estableciendo el puerto de conmutador en modalidad troncal y configurando las interfaces virtuales (VIF) en el dispositivo.

Por ejemplo:

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Los mandatos anteriores crean dos interfaces virtuales en la interfaz `dp0bond0`. La interfaz `dp0bond0.1432` procesa el tráfico de VLAN 1432 y la interfaz `dp0bond0.1693` procesa el tráfico de la VLAN 1693.
