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

# Instrada le VLAN
VRA (Virtual Router Appliance) è in grado di instradare più VLAN nella stessa interfaccia di rete (ad esempio, `dp0bond0` o `dp0bond1`). Questo è possibile impostando la porta switch nella modalità trunk e configurando le interfacce virtuali (VIF) sul dispositivo.

Ad esempio:

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

I precedenti comandi creano due interfacce virtuali nell'interfaccia `dp0bond0`. L'interfaccia `dp0bond0.1432` elabora il traffico per la VLAN 1432 mentre `dp0bond0.1693` per la VLAN 1693.
