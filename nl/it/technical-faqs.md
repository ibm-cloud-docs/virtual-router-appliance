---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: faqs, vlan, traffic, firewall, SSH,

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# FAQ tecniche per {{site.data.keyword.vra_full}}
{: #technical-faqs-for-ibm-virtual-router-appliance}

Le seguenti domande frequenti affrontano la configurazione di {{site.data.keyword.vra_full}} (VRA) e la migrazione a VRA da Vyatta 5400.

## Come consento il traffico associato a internet dagli host che si trovano su una VLAN privata?
{: faq}

Questo traffico deve ottenere un IP di origine pubblico, quindi un NAT di origine deve mascherare l'IP privato con quello pubblico della VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

La precedente configurazione esegue soltanto SNAT dal traffico originato dei server nella rete `10.0.0.0/8` privata.

Questo assicura che non interferirà con i pacchetti che già dispongono di un indirizzo di origine instradabile su internet.

## Come posso filtrare il traffico associato a internet e consentire solo protocolli/destinazioni specifici?
{: faq}

Questa è una domanda comune quando devono essere combinati il NAT di origine e un firewall.

Tieni a mente l'ordine delle operazioni nella VRA quando progetti le tue serie di regole.

In breve, le regole del firewall vengono applicate *dopo* SNAT.

Per bloccare tutto il traffico in uscita in un firewall, ma consentire dei flussi SNAT specifici, devi spostare la logica di filtro nel tuo SNAT.

Ad esempio, per consentire solo il traffico associato a internet HTTPS per un host, la regola SNAT dovrebbe essere:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` potrebbe essere un indirizzo pubblico per la VRA.

Consigliamo vivamente di utilizzare l'indirizzo pubblico VRRP della VRA, in modo che poter distinguere il traffico pubblico VRA dall'host.

Supponiamo che `150.1.2.3` è l'indirizzo VRA VRRP e `150.1.2.5` è l'indirizzo dp0bond1 reale.

Il firewall con stato applicato su `dp0bond1 out` dovrebbe essere:

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

Nota che la combinazione del NAT di origine e del firewall raggiunge l'obiettivo di progettazione richiesto.

Assicurati che le regole siano appropriate per la tua progettazione e che nessun'altra regola consenta il traffico che dovrebbe essere bloccato.

## Come proteggo la VRA stesso con un firewall basato sulla zona?
{: faq}

La VRA non ha una `local zone`.

Puoi invece utilizzare la funzionalità CPP (Control Plane Policing) poiché viene applicata come un firewall `local` su loopback.

Nota che questo è un firewall senza stato e dovrai consentire esplicitamente il traffico in restituzione delle sessioni in uscita originate sulla VRA stessa.

## Come limito SSH e blocco le connessioni provenienti da internet?
{: faq}

È considerata una procedura consigliata non consentire le connessioni SSH da internet e di utilizzare altri modi di accesso all'indirizzo privato, come VPN SSL.

Per impostazione predefinita, VRA accetta SSH su tutte le interfacce.
Per poter ascoltare solo le connessioni SSH sull'interfaccia privata, è necessario impostare la seguente configurazione:

```
set service ssh listen-address '10.1.2.3'
```

Tieni a mente che l'indirizzo IP deve essere sostituito con l'indirizzo che appartiene alla VRA.
