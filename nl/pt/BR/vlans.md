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

# Rotear VLANs
O Virtual Router Appliance é capaz de rotear múltiplas VLANs na mesma interface de rede (por exemplo, `dp0bond0` ou `dp0bond1`). Isso é feito configurando a porta do comutador no modo de tronco e configurando interfaces virtuais (VIFs) no dispositivo.

Por exemplo:

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Os comandos acima criam duas interfaces virtuais na interface `dp0bond0`. A interface `dp0bond0.1432` processa o tráfego para a VLAN 1432 enquanto a interface `dp0bond0.1693` processa o tráfego para a VLAN 1693.
