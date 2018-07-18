---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Come visualizzare Vyatta nel portale

Un singolo gateway da 1 Gbps è stato distribuito nel data center di San Jose; ha un accesso ridondante sia all'infrastruttura pubblica che a quella privata. Utilizza la scheda **Devices** nel menu **Network > Gateway Appliances** per visualizzare la configurazione effettiva dell'hardware distribuito.

Nota i link **2546 Public VLAN**, **2576 Private VLAN** e **Network Hardware Show Details**. Le reti sono dedicate e contengono solo dispositivi gateway. Sono sotto il controllo della piattaforma di provisioning (non puoi eseguire il provisioning di server normali tramite questa scheda).

Per visualizzare le associazioni di rete, fai clic sul menu **Network, Gateway Appliances** nel portale web.

Le porte di switch di rete e gli adattatori Ethernet su Brocade 5400 vRouter (Vyatta) sono tutti configurati da SoftLayer come trunk. Le nostre associazioni aggiornano semplicemente la configurazione della porta per consentire le VLAN (virtual VLAN) virtuali aggiuntive selezionate.<sup>1</sup>

Il menu a discesa **Associate a VLAN** ti consente di selezionare una specifica VLAN perché sia nota, o "associata", al tuo dispositivo Brocade 5400 vRouter dall'interno del tuo data center. **Non puoi** associare una VLAN protetta da un firewall hardware o un'applicazione FortiGate poiché ciò interromperebbe il controllo della VLAN. Se nel menu a discesa non è elencata alcuna VLAN aggiuntiva, puoi ordinarne di più inoltrando un ticket a SoftLayer.

Puoi visualizzare le VLAN associate all'applicazione Brocade 5400 vRouter in Associated VLANs nella Figura 4. Lo stato (Status) predefinito è Bypassed (SoftLayer ha identificato in background le porte di switch trunk per includere 1894 e 2254 dalla rete privata e 2007 e 1280 dalla rete pubblica).<sup>2</sup>

Nella schermata **Details** (**Network, Gateway Appliances**) nel menu a discesa **Associated VLAN, Actions**, c'è un'opzione per instradare la VLAN (**Route VLAN**). Se selezioni questa opzione, hai in effetti richiesto a FCR e BCR SoftLayer di rimuovere il gateway predefinito e di aggiungere invece un instradamento per le sottoreti utilizzando le VLAN di transito al dispositivo Brocade 5400 vRouter.

L'applicazione Brocade vRouter è ora il solo instradamento valido in ingresso e in uscita della VLAN. Il fattore chiave da ricordare è che questo non vale solo per il tuo traffico dell'applicazione ma anche per qualsiasi servizio SoftLayer aggiuntivo che ha ora bisogno di transitare per il tuo dispositivo.<sup>4</sup>

La configurazione deve ancora essere eseguita sul lato di Brocade 5400 vRouter, eppure qualsiasi cosa si trovi sulla VLAN con le sottoreti non è più raggiungibile. Questo significa anche che neanche i nuovi server di cui viene eseguito il provisioning come portale web possono raggiungere la VLAN.

**Note:**

<sup>1</sup>L'associazione della VLAN è il processo di collegare una VLAN idonea a un gateway di rete in modo che possa essere instradata al gateway in futuro. Il processo di associazione non instrada automaticamente una VLAN a un gateway; la VLAN continua ad utilizzare i router del cliente di back-end e front-end finché non viene instradata normalmente al gateway. Le VLAN possono essere associate a un singolo gateway alla volta e non devono avere un firewall per essere considerate idonee per l'associazione.

<sup>2</sup>Il gateway può essere escluso in qualsiasi momento in modo che il traffico ritorni ai router del cliente di front-end e back-end (FCR e BCR). L'esclusione di una VLAN le consente di rimanere associata al gateway di rete.

<sup>3</sup>Le sottoreti portatili possono essere acquistate e aggiunte ai gateway primari.

<sup>4</sup>Il provisioning dei dispositivi Brocade 5400 vRouter viene di norma eseguito con l'indirizzo IP del servizio SoftLayer predefinito.
