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

# Ricarica il SO
Il processo di ricaricamento del SO, quando avviato nel portale del cliente, eliminerà tutte le configurazioni presenti nel dispositivo e lo riporterà alla sua configurazione originale. Tutte le modifiche effettuate dall'ultimo caricamento del sistema saranno cancellate. Di conseguenza, se hai effettuato modifiche (ad esempio al gruppo VRRP su un'altra macchina), la macchina ricaricata avrà probabilmente un conflitto quando ritorna online.

Per ricaricare il tuo SO, esegui la seguente procedura:

1. Accedi a [Customer Portal ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){: new_window} utilizzando le tue credenziali univoche.
2. Seleziona **Device List** dall'elenco a discesa dei dispositivi.
3. Fai clic sul server che desideri ricaricare.
4. Seleziona **OS Reload** dal menu a discesa **Actions** in alto a sinistra nella pagina.
5. Nella schermata OS Reload, scegli le funzioni e le opzioni che vuoi applicare come parte del ricaricamento. 
6. Fai clic sul pulsante **Reload Above Configuration** per procedere alla finestra a comparsa **Review**. Fai clic su **Cancel** per annullare le modifiche al dispositivo e esci dalla schermata.
7. Verifica che tutti i dettagli nella sezione New Configuration siano corretti. Fai clic su **Next** per passare alla finestra a comparsa Confirm.
8. Fai clic sul pulsante **Confirm OS Reload** per confermare e avvia il ricaricamento del SO. Fai clic su **Cancel** per annullare l'azione.

Dopo aver avviato il processo di ricaricamento del SO, il dispositivo andrà offline e il processo di ricaricamento del SO inizierà. Il tempo impiegato dal ricaricamento del SO varia in base alla configurazione nuova e corrente del dispositivo. Durante il processo di configurazione, il tempo minimo per il ricaricamento del SO, viene visualizzato in ogni schermata. Il periodo di tempo è una stima fatta dal sistema. Se il ricaricamento impiega più di 24 ore, contattare il team di supporto. Quando il dispositivo torna online, funzionerà come specificato nella nuova configurazione del ricaricamento del SO. 

**NOTA:** tutti i dati precedentemente salvati nel dispositivo andranno persi, ma possono essere ripristinati se era stato eseguito un backup del dispositivo prima del suo ricaricamento. Se non era stato eseguito il backup dei dati, non possono essere ripristinati.
