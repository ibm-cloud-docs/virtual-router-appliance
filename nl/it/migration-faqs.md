---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

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

# Domande frequenti sulla fine del supporto di Vyatta 5400
{: #vyatta-5400-end-of-support-faqs}

Le seguenti sono le domande frequenti quando si esegue la migrazione da Vyatta 5400.

## Perché la “Fine del supporto” (EoS, End of Support) di Wyatta 5400 il 31 marzo 2019?
{: faq}

Nel mese di settembre del 2017, per Vyatta 5400 legacy è stata annunciata la fine del supporto per il 20 febbraio 2018 sulla base della politica del ciclo di vita di IBM per il supporto: sei mesi dopo la data di luglio della GA (General Availability) per la versione successiva, IBM VRA (Virtual Router Appliance).

Per onorare le linee temporali della migrazione del cliente, la data di fine del supporto di Vyatta 5400 è stata estesa fino al 31 marzo 2019. Poiché il software Debian 7 ora non è più supportato dalla community Open Source di Debian, non ci sono piani futuri di estendere il supporto del fornitore da AT&T.

[Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) per visualizzare l'annuncio formale di fine del supporto.
{: important}

## Che significa “Fine del supporto” per me in quanto cliente?
{: faq}

Dopo la data di fine del supporto, AT&T non fornirà più alcuna patch del codice né accetterà escalation di supporto da IBM.

Analogamente, il supporto di IBM Cloud non risolverà più i problemi di configurazione o di rete sulle distribuzioni Vyatta 5400. Il supporto sarà limitato alle richieste di livello hardware (disco fisso, RAM e così via), alimentazione e connettività fuori banda (IPMI).

Consigliamo vivamente ai clienti di procedere immediatamente alla migrazione a una soluzione alternativa, come VRA (Virtual Router Appliance, basata su Vyatta 5600) o Juniper vSRX.  [Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) per iniziare.

## Cosa succede se sto ancora eseguendo i miei carichi di lavoro IBM Cloud utilizzando un Vyatta 5400 dopo il 31 marzo?
{: faq}

Il tuo Vyatta 5400 funzionerà dopo il 31 marzo. Tuttavia, i tuoi ambienti di business e applicativi potrebbero essere esposti a potenziali minacce alla sicurezza e altre violazioni di manomissione dovute alle più recenti vulnerabilità nel software Vyatta 5400.

Se riscontri un problema di rete che disattiva il tuo ambiente di rete e applicativo, e tracci la causa scatenante in Wyatta 5400, esegui una escalation di questo problema al nostro responsabile delle offerte 5400 poiché il supporto non sarà più disponibile da IBM o AT&T. Puoi contattare il team di gestione delle offerte tramite email all'indirizzo `nwom@us.ibm.com`.

## Il mio hardware Bare Metal Server sottostante è ancora supportato?
{: faq}

Le sostituzioni hardware sono supportate ma, se la risoluzione del problema indica una correlazione al sistema operativo Wyatta, ti verrà chiesto di eseguire la migrazione a un'offerta hardware supportata immediatamente. [Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) per iniziare.

## In quanto cliente proprietario di Vyatta 5400, cosa devo fare entro il 31 marzo 2019?
{: faq}

I clienti che hanno un Vyatta 5400 devono eseguire una migrazione a VRA (Vyatta 5600), Juniper vSRX o FSA (Fortigate Security Appliance) 10G. La VRA (Vyatta 5600) è ancora pienamente supportata. Non c'è una data di fine di supporto attuale o prevista per la VRA da IBM Cloud o AT&T. [Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) per iniziare.

  VRA e vSRX sono dispositivi gestiti dal cliente.
  {: note}

## È disponibile il supporto da IBM per la migrazione da Vyatta 5400 a VRA, vSRX o FSA 10G?
{: faq}

Il servizio di conversione della configurazione da Vyatta 5400 a VRA (5600) è ancora disponibile.

* Per i clienti esistenti, IBM Cloud sta fornendo un'offerta gratuita per assistere nel refactoring della configurazione Vyatta 5400 esistente nei formati VRA (Virtual Router Appliance), Juniper vSRX o FSA (Fortigate Security Appliance) 10G. Per inoltrare una richiesta per il servizio di conversione della configurazione, invia una e-mail a nwom@us.ibm.com con l'oggetto: `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx. `.

  Assicurati di inserire la tua scelta dell'applicazione al posto di `aaaaaaaa` (Virtual Router Appliance, Juniper vSRX o FSA (Fortigate Security Appliance) 10G) e il tuo numero di account specifico al posto di `xxxxxx` nella tua riga dell'oggetto.
  {: note}

* Wanclouds, il nostro partner in questo processo di configurazione della conversione, ha completato diverse centinaia di impegni di migrazione con esito positivo. Trasformeranno il tuo Vyatta 5400 esistente per creare una funzionalità simile sulla piattaforma Vyatta 5600. Forniscono i loro servizi in due livelli, descritti di seguito:

  <img src="images/tiers.png" alt="disegno" style="width: 700px;"/>

[Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) per iniziare.

## Ci sono altri servizi di migrazione a pagamento disponibili dai Business Partner di IBM per la migrazione di Vyatta 5400?
{: faq}

Abbiamo diversi Business Partner che forniscono un supporto a pagamento per le migrazioni di Vyatta 5400. [Fai clic qui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) per ulteriori informazioni.

## È disponibile un contatto di supporto della gestione dell'offerta Vyatta 5400 in IBM a cui posso fare delle domande relative alla mia migrazione di Wyatta 5400?
{: faq}

Contatta i responsabili della gestione dell'offerta di rete IBM VRA e Vyatta 5400 con le domande all'indirizzo `nwom@us.ibm.com`. Puoi anche contattarli utilizzando slack con lo spazio di lavoro IBM Watson Cloud Platform: `#vyatta-migration`

## Quali risorse aggiuntive sono disponibili per aiutarmi con questa migrazione?
{: faq}

Per ulteriori informazioni, esamina le seguenti risorse di documentazione della Virtual Router Appliance:

  * [Introduzione a IBM Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [Informazioni sulla VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Panoramica della migrazione di Vyatta 5400](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
