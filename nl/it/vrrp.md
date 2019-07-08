---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, vrrp, vip

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

# Gestione di Alta disponibilità e VRRP
{: #working-with-high-availability-and-vrrp}

VRA (Virtual Router Appliance) supporta VRRP (Virtual Router Redundancy Protocol) come un protocollo ad alta disponibilità. La distribuzione dei dispositivi viene eseguita in modo attivo/passivo, in cui una macchina è quella master e l'altra quella di backup. Tutte le interfacce su entrambe le macchine saranno un membro dello stesso "sync-group", per cui se un'interfaccia riscontra un errore, le altre interfacce nello steso gruppo riscontrano lo stesso errore e il dispositivo non è più il master. Il backup corrente rileverà che il master non trasmette più messaggi keepalive/heartbeat e prende il controllo dei VIP (VRRP virtual IP) e diventa il master.

VRRP non è la parte più importante della configurazione durante il provisioning dei gateway. La funzionalità dell'alta disponibilità si basa sui messaggi di heartbeat, per cui accertarsi che non sono bloccati è fondamentale.

## Indirizzi VIP (VRRP virtual IP)
{: #vrrp-virtual-ip-vip-addresses}

Il VIP (virtual IP) VRRP per `dp0bond1` o `dp0bond0`, oppure VIP, è l'indirizzo IP mobile che viene modificato dal dispositivo master a quello di backup quando viene eseguito il failover. Quando una VRA esegue la distribuzione, avrà una connessione di rete associata privata e pubblica e IP reali assegnati ad ogni interfaccia. Un VIP viene assegnato ad entrambe le interfacce, indipendentemente dal fatto che il dispositivo sia autonomo o in una coppia HA. Il traffico che ha un IP di destinazione nelle sottoreti nelle VLAN associate alla VRA sarà inviato direttamente a questi VIP VRRP tramite un instradamento statico su FCR/BCR.

Gli indirizzi VIP (VRRP virtual IP) per tutti i gruppi di gateway non dovrebbero mai essere modificati né dovrebbe essere disabilitata l'interfaccia VRRP. Questi indirizzi IP sono il metodo con cui il traffico viene instradato al gateway quando viene associata una VLAN. Di conseguenza, se sono inattivi, anche il traffico VLAN sarà inattivo. Se l'indirizzo IP non è presente, il traffico non può essere inoltrato da softLayer BCR/FCR alla VRA stessa. Questo indirizzo virtuale o VIP non è attualmente modificabile. Questa limitazione potrebbe cambiare in futuro, ma attualmente non sono supportati né la migrazione di una VRA tra POD/FCR/BCR né la modifica del VIP.

Il seguente è un esempio delle configurazioni predefinite dei VIP `dp0bond0` e `dp0bond1` per una specifica VRA. Nota: i tuoi indirizzi IP e i vrrp-group potrebbero essere diversi dall'esempio riportato di seguito:

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 virtual-address '10.127.170.2/26'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 virtual-address '159.8.98.214/29'
```

Consulta [Aggiungi più sottoreti a una singola VLAN](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans#add-multiple-subnets-to-a-single-vlan) o [Sottoreti VLAN associate a VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp) per ulteriori informazioni sulla configurazione dell'indirizzo virtuale per le VIF.

**Per impostazione predefinita, VRRP è impostato come disabilitato.** Ciò garantisce che i nuovi provisioning e caricamenti non causino interruzioni sul dispositivo Master.Perché il traffico VLAN funzioni, è necessario riabilitare VRRP dopo il completamento del provisioning o di un ricaricamento. Il seguente esempio indica in dettaglio la configurazione predefinita:

```
set interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

Per abilitare VRRP su queste due interfacce dopo un provisioning o un ricaricamento, **che è necessario sia per le coppie autonome che per quelle HA**, esegui questi comandi ed esegui quindi il commit della modifica:

```
delete interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
delete interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

## Gruppo VRRP
{: #vrrp-group}

Un gruppo VRRP è formato da un cluster di interfacce o da interfacce virtuali che forniscono la ridondanza per un'interfaccia primaria o “master” nel gruppo. Ogni interfaccia nel gruppo è generalmente su un router separato. La ridondanza è gestita dal processo VRRP in ogni sistema. Il gruppo VRRP ha un identificativo numerico e possono essere assegnati fino a 20 indirizzi VIP (Virtual IP).  

L'ID del gruppo VRRP viene assegnato da SoftLayer e non dovrebbe essere modificato. Quando viene eseguito il provisioning di un gruppo di gateway dietro a FCR (Front Customer Router)/BCR (Backend Customer Router) per la prima volta, si riceverà un gruppo VRRP di 1. I provisioning successivi del gruppo di gateway aumenteranno questo valore per evitare conflitti, il gruppo successivo sarà il gruppo 2, quindi il gruppo 3 e così via. È quindi calcolato e assegnato dal processo di provisioning di SoftLayer. Modificando questo valore si rischiano conflitti con altri gruppi attivi e quindi il conflitto master/master, che probabilmente causerà un'interruzione di entrambi i gruppi di gateway.

Se esegui la migrazione da una configurazione precedente, si consiglia di ricontrollare il tuo codice di configurazione per assicurarti che l'ID del gruppo VRRP non sia assegnato staticamente.

L'ID del gruppo VRRP viene conservato nel database SoftLayer, per cui lo stesso valore ID del gruppo sarà utilizzato durante un aggiornamento o ricaricamento SO. Tutti gli ID del gruppo VRRP modificato dall'utente saranno sovrascritti con il valore assegnato dal sistema durante un ricaricamento SO.  

## Priorità VRRP
{: #vrrp-priority}

La prima macchina in un gruppo di gateway avrà una priorità di 254 e questo valore viene diminuito per il successivo dispositivo di cui viene eseguito il provisioning. Non dovresti mai impostare una priorità di 255, perché definisce il "proprietario dell'indirizzo" VIP e può causare comportamenti non voluti quando la macchina viene messa online con una configurazione diversa da quella del master attivo in esecuzione.

## Precedenza VRRP
{: #vrrp-preemption}

La precedenza dovrebbe sempre essere disabilitata su tutte le interfacce VRRP, in modo che un nuovo dispositivo o uno nel processo di ricaricamento del proprio SO non tenta di subentrare al cluster.

## Intervallo di avviso VRRP
{: #vrrp-advertise-interval}

Per segnalare che è ancora in servizio, l'interfaccia master o VIF invia pacchetti di “heartbeat” denominati “advertisements” al segmento LAN, utilizzando gli indirizzi multicast assegnati IANA per il VRRP (`224.0.0.18` per IPv4 e `FF02:0:0:0:0:0:0:12` per IPv6).

Per impostazione predefinita, l'intervallo è impostato su 1. Questo valore può essere incrementato, ma non si consiglia di impostare un valore superiore a 5. In una rete occupata, potrebbe impiegarci molto più di un secondo perché tutti gli avvisi VRRP arrivino al dispositivo di backup dal master, per cui dovrebbe essere utilizzato un valore di 2 per le reti con elevato traffico.

## Sincronizzazione VRRP (sync-group)
{: #vrrp-synchronization-sync-group-}

Le interfacce in un gruppo di sincronizzazione VRRP (“sync group”) sono sincronizzate in modo che, se il backup di una delle interfacce nel gruppo ha esito negativo, si verifica un failover del backup per tutte le interfacce. Ad esempio, in molti casi, se un'interfaccia in un router master riscontra un errore, si verifica il failover del router di backup dell'intero router.

Questo valore è diverso da quello del gruppo VRRP, perché definisce quali interfacce su un dispositivo riscontreranno un failover quando un'interfaccia in tale gruppo registra un errore. Si consiglia che tutte le interfacce che appartengano allo stesso sync-group, altrimenti, alcune interfacce saranno quelle master e avranno gli IP del gateway, mentre altre saranno quelle di backup e il traffico non sarà più inoltrato correttamente. Le configurazioni attiva/attiva non sono supportate.

## Compatibilità rfc VRRP
{: #vrrp-rfc-compatibility}

La compatibilità rfc abilita o disabilita gli indirizzi RFC 3768 MAC per VRRP in un'interfaccia. Questa dovrebbe essere abilitata nelle interfacce native, ma non su tutti i VIF configurati. Alcuni switch virtuali (principalmente vmware) hanno problemi con questa impostazione abilitata e causeranno l'eliminazione e non l'invio del traffico all'IP del gateway dalla macchina host hypervisor. Lascia questa impostazione autonoma e non configurarla per i VIF.

## Autenticazione VRRP
{: #vrrp-authenticaton}

Se viene impostata una password per l'autenticazione VRRP, deve inoltre essere definito il tipo di autenticazione. Se la password è impostata e il tipo di autenticazione non è definito, il sistema genererà un errore quando tenti di eseguire il commit della configurazione.

Allo stesso modo, non puoi eliminare la password VRRP senza anche eliminare il tipo di autenticazione VRRP. Se fai ciò, il sistema genera un errore quando tenti di eseguire il commit della configurazione. Se elimini sia il tipo che la password di autenticazione VRRP, l'autenticazione VRRP viene disabilitata.

IETF ha deciso che tale autenticazione non deve essere utilizzata per VRRP versione 3. Per ulteriori informazioni, fai riferimento a RFC 5798 VRRP.

## Supporto versione 2/3
{: #version-2-3-support}

La VRA supporta sia i protocolli VRRP versione 2 (predefinito) che versione 3. Nella versione 2, non puoi avere IPv4 e IPv6 insieme. Ma nella versione 3, puoi avere IPv4 e IPv6 contemporaneamente.

## VPN ad alta disponibilità con VRRP
{: #high-availability-vpn-with-vrrp}

La VRA fornisce la possibilità di mantenere la connettività tramite un tunnel IPsec utilizzando una coppia di VRA (Virtual Router Appliance) con VRRP. Quando un router riscontra un errore o viene chiuso per manutenzione, il nuovo router master VRRP ripristina la connettività IPsec tra le reti remota e locale.

Quando configuri la VPN ad alta disponibilità con VRRP, se viene aggiunto un indirizzo virtuale VRRP a un'interfaccia VRA, devi reinizializzare il daemon IPsec perché il servizio IPsec sia in ascolto solo per le connessioni agli indirizzi presenti nella VRA quando viene inizializzato il daemon del servizio Internet Key Exchange (IKE).

Immetti il seguente comando nei router master e di backup, in modo che quando si verifica un failover, i daemon IPsec vengano riavviati in un un nuovo dispositivo master dopo la transizione del VIP, come illustrato nel seguente esempio:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Firewall ad alta disponibilità con VRRP
{: #high-availability-firewalls-with-vrrp}

Quando due dispositivi sono in una coppia HA, devi prestare attenzione al tuo dispositivo master perché non ne devi bloccare l'accesso da altri dispositivi. La porta 443 deve essere consentita per entrambi i dispositivi perché la sincronizzazione di configurazione funzioni e VRRP deve poter essere inviato e ricevuto, incluso l'intervallo multicast di 224.0.0.0/24.

## Sottoreti VLAN associate a VRRP
{: #associated-vlan-subnets-with-vrrp}

Per ogni configurazione VIF con VRRP avrai bisogno di un indirizzo VIP (Virtual IP) (il primo IP utilizzabile nella sottorete, l'IP del gateway a cui viene instradata ogni cosa in tale sottorete) e anche di un indirizzo IP dell'interfaccia reale per il VIF su entrambi i dispositivi. Per mantenere gli IP utilizzabili, si consiglia che gli IP dell'interfaccia reali utilizzino un intervallo fuori banda come 192.168.0.0/30, dove un dispositivo ha l'indirizzo .1 e l'altro dispositivo .2. Puoi avere più VIP (virtual IP) VRRP, ma hai bisogno solo di un indirizzo reale su ogni VIF.

Questo è un esempio di una configurazione VRRP con due VLAN private e tre sottoreti:

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* Un VRRP sync-group è differente da un gruppo VRRP. Quando un'interfaccia che appartiene a un sync-group modifica lo stato, tutti gli altri membri dello stesso sync-group passeranno allo stesso stato.
*  Il numero del vrrp-group delle interfacce VLAN (VIF) dovrebbe quasi sempre essere lo stesso di quello della sua interfaccia nativa corrispondente. Ad esempio, `dp0bond1.789` dovrebbe sempre avere lo stesso numero di vrrp-group di `dp0bond1`, a meno che le due interfacce non condividano lo stesso sync-group. Avere un numero di gruppi differente sulla VIF e nell'interfaccia nativa può causare una condizione "split brain" in caso di accoppiamento a gruppi di sincronizzazione differenti. Quando due interfacce condividono lo stesso sync-group, anche se sono su vrrp-group differenti, eseguiranno entrambe la transizione tra master e backup contemporaneamente, evitando una condizione di split brain. D'altro canto, se l'interfaccia nativa è configurata con un numero di vrrp-group differente e un sync-group differente come interfaccia VLAN, poiché le sottoreti configurate sulle interfacce VLAN vengono instradate staticamente al virtual-address sull'interfaccia nativa, se l'interfaccia VLAN si presenta come quella di backup e l'interfaccia nativa è quella master, il router invierà comunque il traffico dell'interfaccia VLAN alla VRA dove l'interfaccia nativa è quella master anche se l'interfaccia VLAN è secondaria e non attiva sulla VRA. Questa specifica situazione causerà una condizione di "split brain" e un'interruzione. Questo è il motivo per cui è molto importante essere consapevoli di come sono configurati i sync-group insieme ai vrrp-group. È anche molto importante non utilizzare gli stessi numeri di vrrp-group tra più coppie HA VRA nella stessa VLAN di transito, poiché quattro Vyatta che utilizzano lo stesso gruppo potrebbero, potenzialmente, fare in modo che tre VRA assumano lo stato di Backup, mentre solo una è Master, con una conseguente interruzione. 
* Gli indirizzi dell'interfaccia reali nelle VLAN native (ad esempio dp0bond1: 169.110.20.28/29) non sono sempre nella stessa sottorete come i loro VIP (169.110.21.26/29).


## Failover VRRP manuale
{: #manual-vrrp-failover}

Se devi forzare un failover VRRP, può essere ottenuto eseguendo quanto segue nel dispositivo che al momento è il master VRRP:

`vyatta@vrouter$ reset VRRP master interface dp0bond0 group 1`

L'ID del gruppo è l'ID del gruppo VRRP delle interfacce native e, come menzionato precedentemente, potrebbe essere differente nella tua coppia.

## Sincronizzazione della connessione
{: #connection-synchronization}

Quando due dispositivi VRA sono in una coppia HA, può essere utile tracciare le connessioni con stato tra i due dispositivi in modo che se si verifica un failover, lo stato corrente di tutte le connessioni nel dispositivo in errore sia replicato nel dispositivo di backup, per cui non è necessario ricompilare da zero tutte le sessioni attive correnti (come le connessioni SSL), il che può comportare un'interruzione dell'esperienza dell'utente.

Questa operazione si chiama sincronizzazione della traccia di connessione.

Per configurarla, devi dichiarare quale è il metodo di failover, quale interfaccia utilizzerai per inviare le informazioni sulla traccia di connessione e l'IP del peer remoto:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

L'altra VRA avrà la stessa configurazione, ma un diverso `remote-peer`.

Tieni presente che questo può saturare un link se è presente un elevato numero di connessioni in entrata nelle altre interfacce e sarà in competizione con tutto il traffico nel link dichiarato.

## Funzione Start Delay (Ritardo dell'avvio) VRRP
{: #vrrp-start-delay-feature}

Vyatta OS versione 1801p e successive include un nuovo comando `vrrp`.

`vrrp` specifica un protocollo di elezione che assegna dinamicamente la responsabilità per un router virtuale a uno dei router VRRP su una LAN. Il router VRRP che controlla l'indirizzo o gli indirizzi IPv4 o IPv6 associati a un router virtuale è denominato Master e inoltra i pacchetti inviati a questi indirizzi IPv4 o IPv6. Il processo di elezione fornisce un failover dinamico nella responsabilità di inoltro se il Master dovesse diventare non disponibile. Tutta la messaggistica di protocollo viene eseguita utilizzando i datagrammi multicast IPv4 o IPv6; come risultato, il protocollo può operare su una varietà di tecnologie LAN multiaccesso che supportano il multicast IPv4/IPv6.

Per ridurre al minimo il traffico di rete, solo il Master per ogni router virtuale invia messaggi di avviso VRRP periodici. Un router di Backup non tenterà di eseguire la prelazione del Master a meno che non abbia una priorità più alta. Questo elimina l'interruzione del servizio a meno che non diventi disponibile un percorso maggiormente preferito. È anche possibile vietare amministrativamente tutti i tentativi di prelazione. Se il Master diventa non disponibile, il Backup con la priorità più alta eseguirà la transizione a Master dopo un breve ritardo, fornendo una transizione controllata della responsabilità del router virtuale con una minima interruzione del servizio.

Nelle distribuzioni fornite da IBM© Cloud, il valore di Start Delay (Ritardo dell'avvio) è impostato sul valore predefinito. Potresti volerlo modificare a tua discrezione mentre verifichi i tuoi metodi di failover e alta disponibilità.
{: note}

### Confronto tra prelazione e non prelazione
{: #preemption-vs-no-preemption}

Il protocollo `vrrp` definisce la logica che decide quale peer VRRP su una rete ha la priorità più alta e, pertanto, il miglior peer per eseguire il ruolo come Master. Con una configurazione predefinita, VRRP sarà abilitato ad eseguire la prelazione il che significa che un nuovo peer con una priorità più alta sulla rete forzerà il failover del ruolo Master.

Quando la prelazione è disabilitata, un peer con una priorità più alta eseguirà il failover del ruolo Master solo se il peer con priorità più bassa esistente non è più disponibile sulla rete. La disabilitazione della prelazione è a volte utile negli scenari reali poiché gestisce meglio le situazioni in cui il peer con priorità più alta potrebbe avere iniziato a essere periodicamente instabile a causa di problemi di affidabilità con il peer stesso o una delle sue connessioni di rete. È anche utile per evitare il failover prematuro a un nuovo peer con una priorità più alta che non ha completato la convergenza di rete.

### Presupposti e limitazioni della prelazione
{: #assumptions-and-limitations-of-preemption}

Se i peer VRRP sono stati configurati per disabilitare la prelazione, ci sono alcuni casi in cui “sembrerà” che si sta verificando la prelazione ma, in realtà, lo scenario è solo un failover VRRP standard. Come descritto in precedenza, VRRP utilizza i datagrammi multicast IP come mezzo per confermare la disponibilità del router Master attualmente eletto. Poiché è un protocollo di livello 3 che sta rilevando il malfunzionamento di un peer VRRP, è importante che il rilevamento del failover in VRRP sia ritardato finché VRRP e i livelli da 1 a 2 dello stack di rete non siano pronti e convergenti. In alcuni casi, l'interfaccia di rete che esegue VRRP potrebbe confermare al protocollo che l'interfaccia è attiva ma altri servizi sottostanti, come Spanning Tree o Bonding, potrebbero non essere stati completati. Di conseguenza, la connettività IP tra i peer non può essere stabilita. Se ciò si verifica, VRRP su un nuovo peer superiore diventerà Master, in quanto non è in grado di rilevare i messaggi VRRP dal peer Master corrente sulla rete. Dopo la convergenza, il breve periodo di tempo in cui ci sono due peer VRRP Master determinerà l'esecuzione della logica Master duale di VRRP, il peer con la priorità più alta rimarrà Master e quello con la priorità più bassa diventerà Backup. Può “sembrare” che questo scenario illustri un malfunzionamento nella funzionalità “nessuna prelazione”.

### Funzione Start Delay (Ritardo dell'avvio)
{: #start-delay-feature}

Per rispondere ai problemi associati al ritardo nella convergenza dei livelli più bassi dello stack di rete durante un evento di interfaccia attiva, nonché ad altri fattori che contribuiscono, nella patch 1801p è stata introdotta una nuova funzione denominata “Startup Delay”. La funzione fa in modo che lo stato VRRP su una macchina che è stata “ricaricata” rimanga nello stato INIT fino a dopo un ritardo predefinito, che può essere configurato dall'operatore di rete. La flessibilità in questo valore di ritardo consente all'operatore di rete di personalizzare le caratteristiche della sua rete e dei suoi dispositivi in condizioni reali.

### Informazioni dettagliate sui comandi
{: #command-details}

```
vrrp {
start-delay <0-600>
}
```

`start-delay` può avere un valore compreso tra 0 (valore predefinito) e 600 secondi.

### Configurazione di esempio
{: #example-configuration}

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
