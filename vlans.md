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

# Routing Your VLANs
{: #routing-your-vlans}

The Virtual Router Appliance is able to route multiple VLANs over the same network interface (for example, `dp0bond0` or `dp0bond1`). This is accomplished by setting the switch port into trunk mode and configuring virtual interfaces (VIFs) on the device.

For example:  

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

The commands above create two virtual interfaces on the `dp0bond0` interface. The interface `dp0bond0.1432` processes traffic for VLAN 1432 while the interface `dp0bond0.1693` processes traffic for VLAN 1693.