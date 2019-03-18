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

# Routage de vos réseaux locaux virtuels (VLAN)
{: #routing-your-vlans}

Virtual Router Appliance (VRA) peut router plusieurs VLAN via la même interface réseau (par exemple, `dp0bond0` ou `dp0bond1`). Cette opération s'effectue en définissant le port de commutation en mode de liaison et en configurant des interfaces virtuelles (VIF) sur l'unité.

Par exemple :

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Les commandes ci-dessus créent deux interfaces virtuelles sur l'interface `dp0bond0`. L'interface `dp0bond0.1432` traite le trafic du VLAN 1432 alors que l'interface `dp0bond0.1693` traite le trafic du VLAN 1693.
