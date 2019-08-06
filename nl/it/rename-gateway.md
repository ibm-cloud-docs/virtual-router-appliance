---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: gateway, rename

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

# Ridenominazione di una VRA
{: #renaming-a-vra}

I gateway di rete sono forniti di nomi univoci che aiutano gli utenti a identificarli. In qualsiasi momento, un gateway può essere modificato. Si consiglia di utilizzare una convenzione di denominazione coerente per facilitare l'identificazione dei gateway.

Esegui la seguente procedura per ridenominare un gateway di rete:

1. [Accedi alla schermata Gateway Appliance Details](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) nel portale del cliente.
2. Fai clic sul menu a discesa **Actions** e seleziona **Rename Gateway**.
3. Immetti il nuovo nome del gateway nel campo **Gateway Name**.
4. Fare clic su **OK** per salvare la modifica.

Dopo aver modificato un nome dell'applicazione gateway, esso sarà immediatamente modificato all'inizio della schermata Gateway Appliance Details. Il nome del gateway può essere modificato nuovamente in qualsiasi momento ripetendo i precedenti passi.

La modifica del nome della VRA nel portale del cliente non modifica automaticamente il nome host nella VRA ({{site.data.keyword.vra_full}}) o in qualsiasi voce DNS di cui puoi disporre. Questo richiederà un intervento manuale se necessario.
{: note}
