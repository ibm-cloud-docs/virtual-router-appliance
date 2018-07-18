---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informazioni
IBM Virtual Router Appliance (VRA) ti consente di instradare selettivamente il traffico di rete pubblico e privato tramite un router aziendale completo con firewall, modellamento del traffico, instradamento basato sulla politica, VPN e un host di altre funzioni. Virtual Router Appliance fornisce le prestazioni, la facilità di configurazione e i vantaggi sulla manutenzione dell'esecuzione su un server hardware normale. L'hardware è dimensionato per gestire il carico di instradamento per più VLAN e può essere ordinato con i link di rete ridondanti e gli array RAID ridondanti. Tutte le funzioni VRA sono gestite dal cliente. 

IBM Virtual Router Appliance fornisce l'ultimo sistema operativo Vyatta 5600 per i server bare metal x86. Viene offerto con l'alta disponibilità (HA) o con un'offerta indipendente.

**NOTA:** FortiGate Security Appliance (FSA) 10Gbps è un tenant singolo (dedicato), un firewall hardware (10Gbps) a velocità elevata con funzioni di prossima generazione, come AntiVirus (AV), Intrusion Prevention (IPS) e il filtro web. Potrebbe essere un'alternativa a VRA per raggiungere obiettivi simili. Per ulteriori informazioni, fai riferimento a [FSA documentation](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Firewall
Per proteggere il tuo ambiente da minacce esterne, Virtual Router Appliance può essere utilizzato come un firewall. Puoi aggiungere le regole del firewall per consentire o negare il traffico di rete in entrata o in uscita per le porte su cui è in esecuzione la tua applicazione e puoi filtrare il traffico all'interno delle tue proprie reti. Virtual Router Appliance può anche essere configurato per eseguire il filtro IPv4 e IPv6 con stato per proteggere i tuoi dati critici.

## Gateway VPN (Virtual Private Network)
Collega il tuo data center o ufficio in loco a IBM Cloud utilizzando il tunneling VPN fornendo la tua VRA (Virtual Router Appliance) come un dispositivo gateway di rete. Puoi utilizzare un tunnel VPN site-to-site IPsec per la comunicazione sicura dal tuo data center aziendale o dal tuo ufficio alla tua rete IBM Cloud. Altre opzioni VPN sono; Remote access IPsec VPN (client-to-site), OpenVPN, GRE, L2TP e DMVPN.

Dai un'occhiata alle guide di configurazione della VPN Brocade nella nostra [sezione Supplemental VRA documentation](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)

## NAT (Network Address Translation)
Con Virtual Router Appliance, puoi eseguire il provisioning dei server del database e dell'applicazione senza le interfacce di rete pubbliche mentre continui a consentire ai tuoi server di accedere a internet utilizzando Source NAT. Puoi anche nascondere i tuoi server dietro il dispositivo gateway con Destination NAT per una sicurezza avanzata.

## Instradamento a livello aziendale

Per le applicazioni a più livelli su reti isolate separate, Virtual Router Appliance ti consente di creare la connettività tra queste reti con maggiore flessibilità. Potrai configurare l'instradamento dinamico utilizzando BGP, che ti consentirà di annunciare il tuo proprio spazio IP pubblico nei router IBM Cloud. BGP offrirà anche ulteriore flessibilità per le configurazioni della rete privata personalizzata quando utilizzi vari tunnel e soluzioni di collegamento diretto.

Dai un'occhiata alle guide di configurazione di Brocade BGP nella nostra [sezione Supplemental VRA documentation](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)
