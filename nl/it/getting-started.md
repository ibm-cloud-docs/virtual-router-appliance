---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Introduzione a {{site.data.keyword.vra_full}}
{: #getting-started}

IBM© VRA ({{site.data.keyword.vra_full}}) fornisce il sistema operativo Vyatta 5600 più recente per i server bare metal x86. Viene offerta come una configurazione ad alta disponibilità (HA, High Availability) oppure autonoma e ti consente di instradare traffico di rete privato e pubblico in modo selettivo mediante un router aziendale completo con firewall, modellamento del traffico, instradamento basato sulle politiche, VPN e altre funzioni.

I requisisti server minimi di VRA sono 8 GB di RAM e un core CPU per ogni 10 Gbps di capacità di rete. Ad esempio, un sistema con uplink privato e pubblico 10 Gbps duale richiede almeno quattro core. Inoltre, se la tua intenzione è di configurare i servizi VPN con la codifica, potresti voler aggiungere ulteriori core. L'aggiunta di ulteriori core ai servizi VPN garantirà che la VRA non si bloccherà a causa di uno o più carichi pesanti quando instradi e contemporaneamente crittografi/decrittografi i dati.

## Ordinazione di una VRA ({{site.data.keyword.vra_full}})
{: #order-vra}

Per ordinare una VRA, attieniti alla seguente procedura:

1. Dal tuo browser, apri la pagina Gateway Appliances nella [Console IU di IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} e accedi nel tuo account. 

  Puoi anche andare a questa pagina selezionando il menu di navigazione nella parte superiore sinistra del [Catalogo di IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com) e selezionando **Infrastruttura classica > Rete > Applicazione gateway**.
  {: tip}

2. Dalla sezione **Fornitore del gateway**, seleziona l'opzione **AT&T** (quando è selezionata, sul pulsante compare un segno di spunta blu). Dal menu a discesa su quello stesso pulsante, scegli la tua larghezza di banda (20Gbps o 2Gbps).

  	<img src="images/ordering_vra.png" alt="disegno" style="width: 500px;"/>

3. Dalla sezione **Applicazione gateway**, immetti le tue informazioni sul tuo **Nome host** e **Dominio**. Questi campi saranno precompilati con le informazioni predefinite, quindi assicurati che i valori siano corretti. Seleziona l'opzione **Alta disponibilità**, se la desideri, e seleziona quindi l'**Ubicazione** del data center desiderata e il **Pod** specifico che desideri dal menu a discesa.

  Verranno qui visualizzati solo i pod che già hanno una VLAN associata. Se desideri eseguire il provisioning dell'Applicazione gateway in un pod che non vedi elencato, creare prima una VLAN lì.
  {: note}

4. Dalla sezione **Configurazione**, scegli il tuo processore selezionando le tue chiavi SSH e la tua RAM (se necessario).

  <img src="images/ordering_vra_2.png" alt="disegno" style="width: 600px;"/>
  
  Il processore appropriato viene scelto per te in base alla versione di licenza che hai selezionato nel passo due. Puoi tuttavia scegliere delle configurazioni RAM differenti.
  {: note}

5. Dalla sezione **Dischi di archiviazione**, scegli le opzioni che soddisfano i tuoi requisiti di archiviazione. 

  Sono disponibili le opzioni RAID0 e RAID1 per una protezione aggiunta contro la perdita di dati poiché sono delle "riserve a caldo" (componenti di backup che possono essere attivati immediatamente quando si verifica il malfunzionamento di un componente primario).
  {: note}

  Puoi avere fino a quattro dischi per ogni VRA. La "dimensione del disco" con una configurazione RAID è la dimensione del disco utilizzabile, in quanto delle configurazioni RAID viene eseguito il mirroring.
  {: note}

  Riserva più spazio rispetto alle impostazioni predefinite del disco se pensi di eseguire le diagnostiche di rete che generano i log dettagliati.
  {: tip}

6. Dalla sezione **Interfaccia di rete**, seleziona le tue **Velocità porta di uplink**. La selezione predefinita è una singola interfaccia ma ci sono anche opzioni ridondanti e solo private. Scegli quella che meglio si adatta alle tue esigenze.

  La sezione **Moduli aggiuntivi** dell'interfaccia di rete ti consente di selezionare un indirizzo IPv6, se necessario, e ti mostra le eventuali opzioni predefinite incluse aggiuntive. 
  
8. Esamina le tue selezioni, controlla di aver letto gli accordi di servizio di terze parti e fai quindi clic su **Create**. L'ordine viene verificato automaticamente.

Dopo l'approvazione del tuo ordine, il provisioning della tua {{site.data.keyword.vra_full}} viene avviato automaticamente. Quando il processo di provisioning è completo, la nuova VRA sarà visualizzata nella pagina di elenco Gateway Appliances. Fai clic sul nome del gateway per aprire la pagina Gateway Details. Troverai gli indirizzi IP, la password e il nome di accesso del dispositivo.  

  <img src="images/gateway_details.png" alt="disegno" style="width: 500px;"/>

Ricordati che, dopo aver ordinato e configurato la tua VRA dal Catalogo di IBM Cloud, devi configurare anche il dispositivo stesso con le stesse impostazioni.
{: tip}

## Ruolo dell'applicazione gateway e VLAN
{: #vlans-and-the-gateway-appliance-s-role}

Una VLAN (LAN virtuale) è un meccanismo che suddivide une rete fisica in molti segmenti virtuali. Per comodità, il traffico da più VLAN selezionate può essere consegnato tramite un solo cavo di rete, un processo normalmente chiamato "trunking."

La {{site.data.keyword.vra_full}} viene consegnata in due parti: il(i) server VRA e l'apparecchiatura Applicazione gateway. L'Applicazione gateway ti fornisce un'interfaccia (GUI e API) per selezionare le VLAN che desideri associare alla tua VRA. L'associazione di una VLAN a un'applicazione gateway reinstrada (o "trunks") tale VLAN e tutte le sue sottoreti alla tua VRA, fornendoti il controllo sul filtro, l'inoltro e la protezione. Ogni VLAN associata all'Applicazione gateway viene consentita sulle porte di switch a cui è connessa la VRA e le eventuali sottoreti su tale VLAN vengono instradate staticamente all'IP VRRP pubblico della tua VRA (se le sottoreti sono sottoreti pubbliche) oppure instradate staticamente all'IP VRRP privato della tua VRA (se le sottoreti sono sottoreti private). Questo instradamento viene eseguito al router dietro cui si trova la VRA che sarà, rispettivamente per il traffico pubblico e quello privato, FCR (Frontend Customer Router) o BCR (Backend Customer Router). 

Tieni presente che VRRP è disabilitato per impostazione predefinita e deve essere abilitato affinché il traffico VLAN funzioni, anche su wyatta autonomi. Questa è una conseguenza delle sottoreti sulle VLAN associate che vengono instradate all'IP VRRP o all'indirizzo virtuale assegnato alla VRA. Per ulteriori informazioni, consulta [Indirizzi VIP (VRRP virtual IP)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses).
{: important}

I server in una VLAN associata possono essere raggiunti solo da altre VLAN passando attraverso la tua {{site.data.keyword.vra_full}}; non è possibile aggirare la VRA a meno che non escludi o annulli l'associazione alla VLAN.

Per impostazione predefinita, una nuova applicazione gateway viene associata a due VLAN "transit" non rimovibili, una per ogni VLAN pubblica e privata. Queste vengono normalmente utilizzate per la gestione e possono essere protette separatamente dai comandi VRA.

Le VLAN di transito sono per i dispositivi di rete quali i firewall o i programmi di bilanciamento del carico in modo che possano instradare il traffico mantenendo al tempo stesso gli altri dispositivi, quali i server o i contenitori, isolati da internet.

Le VLAN "gateway", invece, sono dove sono ospitati i dispositivi, quali i server e i contenitori.

La VRA può solo gestire le VLAN associate a esso tramite l'applicazione gateway.

Per informazioni su come gestire le VLAN dalla schermata dei dettagli delle applicazioni gateway, fai riferimento a [Gestione delle VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
