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

# Risoluzione dei problemi della tua interfaccia VFP
{: #troubleshooting-your-vfp-interface}

Questo argomento contiene informazioni per la risoluzione dei problemi per le interfacce VFP,

* Un'interfaccia VFP non è una "vera" interfaccia, come ad esempio `dp0bond0` (o anche una VIF o una TUN). È un segnaposto per consentire ai processi firewall e NAT di fissare un'interfaccia in modo da poter elaborare correttamente il traffico. Puoi comunque instradare il traffico su un VFP come un'interfaccia regolare, ma `tshark` e altri comandi di monitoraggio non riveleranno alcun traffico.
* Con NAT, devi utilizzare un intervallo di sottoreti più specifico per fare in modo che il traffico venga instradato al VFP piuttosto che all'instradamento kernel creato da IPsec. Se non è impostato un instradamento statico, verrà seguito l'instradamento kernel. Puoi verificare ciò utilizzando `show ip route x.x.x.x`.
* La DNAT dovrebbe essere elaborata correttamente in uscita dal VFP ma il traffico di ritorno richiede ancora che sia impostato un instradamento statico. Cerca il traffico non NAT in uscita dall'interfaccia IPsec, `dp0bond1` o `dp0bond0` (o qualsiasi interfaccia che utilizza il traffico IPsec).
* L'utilizzo dei protocolli di instradamento su un VFP non è stato testato.
* Anche l'utilizzo di un tunnel GRE su un VFP non è stato testato.
