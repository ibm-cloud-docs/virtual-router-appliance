---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configurazione di base di Vyatta 5400
{: #basic-configuration-of-vyatta-5400}

Esegui la seguente procedura per configurare il tuo Vyatta 5400.

La LAN virtuale pubblica (VLAN) 1224, che è ora associata e instradata, deve essere configurata prima che possa trasmettere il traffico. Sono necessarie due informazioni per completare la configurazione:

  * Il vincolo del lato pubblico dell'applicazione Brocade 5400 vRouter
  * Il gateway predefinito di 1224

Tieni presente che è la VLAN del lato pubblico dove si trova l'opzione di calcolo - non la VLAN pubblica dell'applicazione Brocade 5400 vRouter.

## Nel portale Softlayer

1\. All'interno del portale web SoftLayer, in **Devices**, individua il vincolo nella tabella **Configuration** del dispositivo Brocade 5400 vRouter. Supponiamo che sia <span style="text-decoration: underline">eth1=bond1</span> nel tuo dispositivo.

2\. Seleziona **Network > IP Management > VLANs** dal portale web per trovare il gateway predefinito per la VLAN 1224.

3\. Fai clic sulla tua VLAN nell'elenco e su **Subnet** dove visualizzi il tuo gateway. Prendi nota dei dettagli del CIDR (Classless Inter-Domain Routing) posizionati dopo la barra. 

## Nella GUI Vyatta

4\. Dal dashboard, fai clic sulla scheda **Configuration**.

5\. Espandi **Interfaces > Bonding > bond1** nel lato sinistro della schermata; evidenzia **vif**.

6\. Immetti il "nome" delle VLAN nel campo **vif** (1224 per i nostri scopi) e fai clic sul pulsante **Create** . Il processo impiegherà alcuni secondi per il completamento.

7\. Seleziona l'appena creato **vif 1224** nell'indicatore di espansione **vif**.

8\. Seleziona la casella in **dhcp** e immetti l'indirizzo IP del gateway predefinito e la notazione CIDR della maschera della VLAN che vuoi trasmettere tramite Brocade 5400 vRouter (ad esempio, VLAN 1224).

9\. Fai clic sul pulsante **Set** e su **Commit**.

10\. Fai clic su **Save** nella barra del menu centrale altrimenti la configurazione esegue il rollback alle sue impostazioni predefinite la prossima volta che viene riavviato il sistema.

**NOTA:** il rollback della configurazione può essere una funzione molto utile se interrompi la tua configurazione mentre verifichi le modifiche. Finché le modifiche non vengono salvate puoi riavviare il server dal portale web e ripristinare la tua configurazione precedente.

11\. Fai clic sulla scheda delle statistiche e apri la nuova interfaccia per verificare e monitorare il traffico.

## Configura la VLAN privata utilizzando la CLI

Esistono due modalità di comando nella CLI (interfaccia riga di comando): operativa e configurazione. 

La modalità operativa fornisce l'accesso ai comandi operativi per:

  * Visualizzazione cancellazione delle informazioni
  * Abilitazione e disabilitazione del debug
  * Configurazione delle impostazioni del terminale
  * Caricamento e salvataggio della configurazione
  * Riavvio del sistema

La configurazione fornisce l'accesso ai comandi per:

  * Creazione
  * Modifica
  * Eliminazione
  * Commit
  * Visualizzazione delle informazioni di configurazione
  * Navigazione tramite la gerarchia di configurazione

Quando accedi al sistema, questo è nella modalità operativa; dovrai passare alla configurazione per i comandi.

**NOTA:** puoi vedere in quale modalità sei, operativa o configurazione, in base al prompt. Sei in modalità operativa se il prompt è # e nella configurazione se il prompt è $.

Utilizza la seguente procedura per configurare la VLAN privata utilizzando la CLI. Ricorda che i valori necessari per configurare le VLAN sono:

  * Il nome della VLAN che deve essere instradata tramite il dispositivo Brocade 5400 vRouter (2254)
  * Il gateway e la maschera (formato CIDR) della VLAN che deve essere instradata (10.52.69.201/29)
  * Il nome del vincolo privato del dispositivo Brocade 5400 vRouter (bond0)

1. Accedi tramite SSH al tuo Brocade 5400 vRouter (indirizzo IP pubblico o privato) utilizzando **vyatta** come **Username**; fornisci la password quando richiesta.

   **NOTA:** devi creare un nuovo utente in Brocade 5400 vRouter e disabilitare l'utente iniziale predefinito `vyatta`.

2. Configura il vif:

  * Immetti *configure* nel prompt dei comandi per entrare nella modalità di configurazione.
  * Immetti *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* nel prompt dei comandi per configurare il vif.
  * Immetti *commit* nel prompt dei comandi per eseguire il commit delle impostazioni.
  * Immetti *save* per salvare le impostazioni.
  * Immetti *exit* per tornare alla modalità operativa.

3. Immetti *show interfaces* per controllare le impostazioni di cui hai appena eseguito il commit.

4. Instrada ogni VLAN rimanente tramite il dispositivo Brocade 5400 vRouter.
