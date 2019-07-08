---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

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

# Upgrade di Vyatta 5400 e riutilizzo dei suoi indirizzi IP
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

Questa opzione ti consente di riutilizzare il dispositivo Vyatta 5400 esistente come una VRA (Virtual Router Appliance) e di tenere i tuoi indirizzi IP associati. Le seguenti procedure forniscono le istruzioni per eseguire l'upgrade di un dispositivo Vyatta 5400 autonomo o di due dispositivi Vyatta 5400 che operano in una coppia ad alta disponibilità (HA, High Availability).

## Upgrade di un Vyatta 5400 autonomo
{: #upgrading-a-stand-alone-vyatta-5400}

Per eseguire l'upgrade di un singolo Vyatta 5400 a una IBM Virtual Router Appliance, attieniti alla seguente procedura:

1. Assicurati di avere eseguito un backup del tuo 5400 e di avere archiviato i dati in due ubicazioni differenti. Questo include le chiavi SSH, i certificati SSl, gli script e qualsiasi altro file necessario per recuperare la tua configurazione Vyatta 5400 corrente, se necessario.
2. Ricarica il tuo Vyatta 5400 a una Virtual Router Appliance predefinita utilizzando le istruzioni in [questo argomento](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os).

  Un ricaricamento del sistema operativo genererà una nuova password per gli ID utente Wyatta e root.
  {: note}

4. Annota la nuova password della Virtual Router Appliance.
5. Configura la VRA appena ricaricata con le tue impostazioni desiderate utilizzando [queste istruzioni](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).

## Upgrade di una coppia di Vyatta 5400 ad alta disponibilità
{: #upgrading-a-vyatta-5400-high-availability-pair}

Per eseguire l'upgrade di due Vyatta 5400 in una coppia HA, attieniti alla seguente procedura:

1. Identifica il dispositivo Vyatta 5400 di backup e ricaricalo prima come una Virtual Router Appliance attenendoti alla procedura sopra indicata.

  Idealmente, una configurazione HA funzionale mostra tutte le interfacce come BACKUP o MASTER su uno specifico nodo. In caso di dubbio, utilizza il comando `show vrrp` per confermare.
  {: note}
2. Configura la VRA appena ricaricata con le tue impostazioni desiderate utilizzando [queste istruzioni](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
3. Il Vyatta 5400 master e la Virtual Router Appliance di backup saranno in una coppia VRRP. Modifica la Virtual Router Appliance di backup perché diventi quella master utilizzando il comando VRRP `reset VRRP master`.

  Questa azione causerà un'interruzione nelle eventuali sessioni esistenti e lo stato delle sessioni verrà elencato come Lost. Verranno reimpostate anche le eventuali sessioni NAT esistenti.
  {: note}

5. Verifica che la nuova Virtual Router Appliance master stia funzionando come desiderato.
6. Esegui la procedura di ricaricamento sopra indicata sul Vyatta 5400 di backup corrente per eseguirne l'upgrade a una VRA.
7. Dopo il secondo ricaricamento, configura la Virtual Router Appliance di backup come desiderato, utilizzando [queste istruzioni](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
