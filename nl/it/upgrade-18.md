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

# Considerazioni sull'upgrade per la versione del sistema operativo 18.01

Questo file contiene un elenco di cose da sapere quando si sta eseguendo l'upgrade della VRA (Vyatta 5600) dal sistema operativo Brocade 5.X al sistema operativo 18.01, che include la correzione per la violazione della sicurezza Spectre. Ci sono diversi punti di cui essere consapevoli quando si esegue questo upgrade.

## Chi deve leggere questo argomento

* Tutti gli utenti con una VRA corrente che non sta eseguendo il sistema operativo 1801. (La versione corrente è 18.01g)

* Chiunque intenda installare una nuova versione 18.01f per una Virtual Router Appliance (ad esempio, chiunque stia eseguendo un upgrade dalla 17.2X).

* Se hai un Vyatta 5400, queste informazioni non ti riguardano.

## Perché hai bisogno di queste informazioni

La versione 1801f della VRA contiene una correzione di sicurezza per la vulnerabilità Spectre; tuttavia, è necessario apportare una modifica al programma di installazione stesso prima che la patch possa essere installata. È necessario un passo intermedio per l'installazione della versione 1801C.

## Procedura di upgrade normale
Per eseguire l'upgrade alla versione 18.01f, devi prima scaricare e installare la patch 18.01C per aggiornare il programma di installazione della VRA:

1. Scarica la patch 1801c da questa ubicazione, quindi attieniti alla [procedura normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) per installarla.

2. Dopo che hai eseguito il riavvio, scarica la patch 18.01f da questa ubicazione e installala utilizzando la [procedura normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

Dovresti ora eseguire la versione 18.01f, con la limitazione di sicurezza Spectre corretta.

## Procedura di ricaricamento completo
In alternativa, puoi anche eseguire un ricaricamento completo della VRA al livello 18.01f:

*AVVERTENZA:* questa procedura ti consente di tralasciare il passo intermedio di download e installazione di due patch ma perderai tutti i tuoi dati durante un ricaricamento completo utilizzando la ISO.

1. Ottieni l'ISO 18.01f da questa ubicazione.
2. Attieniti alla [procedura normale](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) per installarla ed eseguire il riavvio.

Dovresti ora eseguire la versione 1801f, con la limitazione di sicurezza Spectre corretta.

## Indicazioni ulteriori

Poiché non esiste una semplice associazione uno-a-uno della funzionalità tra Vyatta 5400 e la Virtual Router Appliance, la creazione di una configurazione baseline per la VRA è utile. Un partner IBM©, WanClouds, può aiutarti con questo processo e fornire indicazioni sulla creazione di una funzionalità simile a Vyatta 5400 sulla tua VRA.

Per ulteriori informazioni sui problemi comuni riscontrati in questo processo di upgrade, consulta la nostra [documentazione aggiuntiva](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
