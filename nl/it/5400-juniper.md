---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

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

# Migrazione di una Vyatta 5400 a un Juniper vSRX o una FSA (Fortgate Security Appliance) 10Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Oltre a poter eseguire l'upgrade del tuo dispositivo Vyatta 5400 a una IBM Virtual Router Appliance, puoi anche eseguire la migrazione al alcune delle opzioni più sicure, affidabili e vantaggiose disponibili sul mercato: Juniper vSRX e Fortigate Security Appliance.
Tuttavia, non potrai riutilizzare il tuo dispositivo Vyatta 5400 esistente o mantenere i tuoi indirizzi IP associati.

Le seguenti procedure forniscono le istruzioni per eseguire l'upgrade di un dispositivo Vyatta 5400 autonomo o di due dispositivi Vyatta 5400 che operano in una coppia ad alta disponibilità (HA, High Availability) a Juniper vSRX o FSA.

## Upgrade di un Vyatta 5400 autonomo
{: #upgrading-a-stand-alone-vyatta-5400}

Per eseguire l'upgrade di un singolo Vyatta 5400 a un'applicazione Juniper vSRX o FSA - 10G con la minima quantità di tempo di inattività, ti consigliamo di attenerti alla seguente procedura.

1. Assicurati di avere eseguito un backup del tuo 5400 e di avere archiviato i dati in due ubicazioni differenti. Questo include le chiavi SSH, i certificati SSL, gli script e qualsiasi altro file necessario per recuperare la tua configurazione Vyatta 5400 corrente, se necessario. Per eseguire tale operazione, attieniti a [queste istruzioni](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contatta nwom@us.ibm.com per richiedere la conversione della tua configurazione

3. Ordina il nuovo dispositivo, attenendoti alle istruzioni per [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) o per [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

  Questa è una buona opportunità per valutare l'ordinazione di una soluzione ad alta disponibilità.
  {: note}

4. Carica la configurazione che hai ricevuto da nwom@us.ibm.com sul tuo dispositivo appena ordinato.

5. Pianifica del tempo per eseguire la migrazione delle tue VLAN al tuo nuovo dispositivo.

  Questo passo richiede un breve periodo di inattività.
  {: note}

6. Verifica e conferma che il tuo nuovo dispositivo stia funzionando correttamente.

7. Apri un ticket di supporto per annullare il tuo dispositivo Vyatta 5400.

## Upgrade di una coppia di Vyatta 5400 ad alta disponibilità
{: #upgrading-a-vyatta-5400-high-availability-pair}

Per eseguire l'upgrade di due Vyatta 5400 in una coppia HA, attieniti alla seguente procedura:

1. Assicurati di avere eseguito un backup del tuo 5400 e di avere archiviato i dati in due ubicazioni differenti. Questo include le chiavi SSH, i certificati SSL, gli script e qualsiasi altro file necessario per recuperare la tua configurazione Vyatta 5400 corrente, se necessario. Per eseguire tale operazione, attieniti a [queste istruzioni](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Contatta nwom@us.ibm.com e richiedi la conversione della tua configurazione

3. Ordina il nuovo dispositivo, attenendoti alle istruzioni per [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) o per [FSA -10G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

4. Carica la configurazione che hai ricevuto da nwom@us.ibm.com sul tuo dispositivo appena ordinato.

5. Pianifica del tempo per eseguire la migrazione delle tue VLAN al tuo nuovo dispositivo.

  Questo passo richiede un breve periodo di inattività.
  {: note}

6. Verifica e conferma che il tuo nuovo dispositivo stia funzionando correttamente.

7. Apri un ticket di supporto per annullare i tuoi dispositivi Vyatta 5400.
