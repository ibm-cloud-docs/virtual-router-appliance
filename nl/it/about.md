---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

keywords: vra, about, firewall, VPN, NAT, Routing

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

# Informazioni sulla VRA
{: #about-the-vra}

IBM© VRA ({{site.data.keyword.vra_full}}) fornisce il sistema operativo Vyatta 5600 più recente per i server bare metal **x86**. Viene offerta come configurazione autonoma o ad alta disponibilità (HA).

La {{site.data.keyword.vra_full}} ti consente di instradare il traffico di rete privato e pubblico in modo selettivo, tramite un router aziendale completo, con firewall, modellamento del traffico, instradamento basato sulle politiche, VPN e altre funzioni. La VRA offre prestazioni con una facilità di configurazione. Offre i vantaggi di manutenzione dell'esecuzione su un normale server hardware. L'applicazione hardware VRA è dimensionata per gestire il carico di instradamento per più VLAN e può essere ordinata con i link di rete ridondanti e gli array RAID ridondanti. Tutte le funzioni della VRA sono gestite da te, il cliente.

**Alternative:** FSA (FortiGate Security Appliance) 10Gbps è un firewall hardware a tenant singolo (dedicato) ed elevata velocità effettiva (10Gbps) con funzioni di prossima generazione, come AntiVirus (AV), Intrusion Prevention (IPS) e il filtro web. Potrebbe essere un'alternativa a VRA per raggiungere obiettivi simili. Per ulteriori informazioni, fai riferimento a [FSA documentation](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started).

## Firewall
{: #firewall}

Per proteggere il tuo ambiente da minacce esterne, {{site.data.keyword.vra_full}} può essere utilizzato come un firewall. Puoi aggiungere le regole del firewall per consentire o negare il traffico di rete in entrata o in uscita per le porte su cui è in esecuzione la tua applicazione e puoi filtrare il traffico all'interno delle tue reti. Anche la {{site.data.keyword.vra_full}} può essere configurata per eseguire il filtro IPv4 e IPv6 con stato per proteggere i tuoi dati critici.

## Gateway VPN (Virtual Private Network)
{: #virtual-private-network-vpn-gateway}

Collega il tuo data center o ufficio in loco a IBM Cloud utilizzando il tunneling VPN fornendo la tua VRA ({{site.data.keyword.vra_full}}) come un dispositivo gateway di rete. Puoi utilizzare un tunnel VPN site-to-site IPsec per la comunicazione sicura dal tuo data center aziendale o dal tuo ufficio alla tua rete IBM Cloud. Altre opzioni VPN sono; Remote access IPsec VPN (client-to-site), OpenVPN, GRE, L2TP e DMVPN.

Dai un'occhiata alle guide di configurazione della VPN Brocade nella nostra [sezione Supplemental VRA documentation](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).

## NAT (Network Address Translation)
{: #network-address-translation-nat-}

Con {{site.data.keyword.vra_full}}, puoi eseguire il provisioning dei server del database e dell'applicazione senza le interfacce di rete pubbliche mentre continui a consentire ai tuoi server di accedere a internet utilizzando Source NAT. Puoi anche nascondere i tuoi server dietro il dispositivo gateway con Destination NAT per una sicurezza avanzata.

## Instradamento a livello aziendale
{: #enterprise-grade-routing}

Per le applicazioni a più livelli su diverse reti isolate, la {{site.data.keyword.vra_full}} ti offre dei modi flessibili per creare la connettività tra queste reti. Puoi configurare l'instradamento dinamico utilizzando BGP, che ti consente di annunciare il tuo spazio IP pubblico nei router IBM Cloud. BGP offre anche ulteriore flessibilità per le configurazioni della rete privata personalizzata quando utilizzi vari tunnel e soluzioni di collegamento diretto.

Dai un'occhiata alle guide di configurazione di Brocade BGP nella nostra [sezione Supplemental VRA documentation](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).
