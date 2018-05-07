---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HA e VRRP
VRA (Virtual Router Appliance) supporta VRRP (Virtual Router Redundancy Protocol) come un protocollo ad alta disponibilità. La distribuzione dei dispositivi viene eseguita in modo attivo/passivo, in cui una macchina è il master e l'altra è il backup. Tutte le interfacce su entrambe le macchine saranno un membro dello stesso "sync-group", per cui se un'interfaccia riscontra un errore, le altre interfacce nello steso gruppo riscontrano lo stesso errore e il dispositivo non è più il master. Il backup corrente rileverà che il master non trasmette più messaggi keepalive/heartbeat e prende il controllo dei VIP (VRRP virtual IP) e diventa il master.

VRRP non è la parte più importante della configurazione durante il provisioning dei gateway. La funzionalità dell'alta disponibilità si basa sui messaggi di heartbeat, per cui accertarsi che non sono bloccati è fondamentale.

## Indirizzi VIP (VRRP virtual IP)

VRRP virtual IP o VIP, è l'indirizzo IP mobile modificato dal dispositivo master al backup quando si verifica il failover. Quando un VRA esegue la distribuzione, avrà una connessione di rete associata privata e pubblica e IP reali assegnati ad ogni interfaccia. Un VIP viene assegnato ad entrambe le interfacce, sia se il dispositivo è autonomo che se è in una coppia HA. Il traffico con un IP di destinazione nelle sottoreti nelle VLAN associate al VRA sarà inviato direttamente a questi VIP VRRP.

Gli indirizzi VIP (VRRP virtual IP) per tutti i gruppi di gateway non dovrebbero essere modificati e l'interfaccia non dovrebbe essere disabilitata. Questi indirizzi IP sono il metodo con cui il traffico viene instradato al gateway quando viene associata una VLAN. Se l'indirizzo IP non è presente, il traffico non può essere inoltrato da softLayer BCR/FCR al gateway stesso.

## Gruppo VRRP

Un gruppo VRRP è formato da un cluster di interfacce o da interfacce virtuali che forniscono la ridondanza per un'interfaccia primaria o “master” nel gruppo. Ogni interfaccia nel gruppo è generalmente su un router separato. La ridondanza è gestita dal processo VRRP in ogni sistema. Il gruppo VRRP ha un identificativo numerico e possono essere assegnati fino a 20 indirizzi IP virtuali.  

L'ID del gruppo VRRP viene assegnato da SoftLayer e non dovrebbe essere modificato. Quando viene eseguito il provisioning di un gruppo di gateway dietro a Front Customer Router (FCR)/Backend Customer Router (BCR) per la prima volta, si riceverà un gruppo VRRP di 1. I provisioning successivi del gruppo di gateway aumenteranno questo valore per evitare conflitti, il gruppo successivo sarà il gruppo 2, quindi il gruppo 3 e così via. È quindi calcolato e assegnato dal processo di provisioning di SoftLayer. Modificando questo valore si rischiano conflitti con altri gruppi attivi e quindi il conflitto master/master, che probabilmente causerà un'interruzione di entrambi i gruppi di gateway.

Se esegui la migrazione da una configurazione precedente, si consiglia di ricontrollare il tuo codice di configurazione per assicurarti che l'ID del gruppo VRRP non sia assegnato staticamente.

L'ID del gruppo VRRP viene conservato nel database SoftLayer, per cui lo stesso valore ID del gruppo sarà utilizzato durante un aggiornamento o ricaricamento SO. Tutti gli ID del gruppo VRRP modificato dall'utente saranno sovrascritti con il valore assegnato dal sistema durante un ricaricamento SO.  

## Priorità VRRP

La prima macchina in un gruppo di gateway avrà una priorità di 254 e questo valore viene diminuito per il successivo dispositivo di cui viene eseguito il provisioning. Non dovresti mai impostare una priorità di 255, perché definisce il "proprietario dell'indirizzo" VIP e può causare comportamenti non voluti quando la macchina viene messa online con una configurazione diversa da quella del master attivo in esecuzione.

## Precedenza VRRP

La precedenza dovrebbe sempre essere disabilitata su tutte le interfacce VRRP, in modo che un nuovo dispositivo o uno nel processo di ricaricamento del proprio SO non tenta di subentrare al cluster.

## Intervallo di avviso VRRP

Per segnalare che è ancora in servizio, l'interfaccia master o VIF invia pacchetti di “heartbeat” denominati “advertisements” al segmento LAN, utilizzando gli indirizzi multicast assegnati IANA per il VRRP (`224.0.0.18` per IPv4 e `FF02:0:0:0:0:0:0:12` per IPv6).

Per impostazione predefinita, l'intervallo è impostato su 1. Questo valore può essere incrementato, ma non si consiglia di impostare un valore superiore a 5. In una rete occupata, potrebbe impiegarci molto più di un secondo perché tutti gli avvisi VRRP arrivino al dispositivo di backup dal master, per cui dovrebbe essere utilizzato un valore di 2 per le reti con elevato traffico.

## Sincronizzazione VRRP (sync-group)

Le interfacce in un gruppo di sincronizzazione VRRP (“sync group”) sono sincronizzate in modo che, se il backup di una delle interfacce nel gruppo ha esito negativo, si verifica un failover del backup per tutte le interfacce. Ad esempio, in molti casi, se un'interfaccia in un router master riscontra un errore, si verifica il failover del router di backup dell'intero router.

Questo valore è diverso da quello del gruppo VRRP, perché definisce quali interfacce su un dispositivo riscontreranno un failover quando un'interfaccia in tale gruppo registra un errore. Si consiglia che tutte le interfacce che appartengano allo stesso sync-group, altrimenti, alcune interfacce saranno il master e avranno gli IP del gateway, mentre altre saranno il backup e il traffico non sarà più inoltrato correttamente. Le configurazioni attiva/attiva non sono supportate.

## Compatibilità rfc VRRP

La compatibilità rfc abilita o disabilita gli indirizzi RFC 3768 MAC per VRRP in un'interfaccia. Questa dovrebbe essere abilitata nelle interfacce native, ma non su tutti i VIF configurati. Alcuni switch virtuali (principalmente vmware) hanno problemi con questa impostazione abilitata e causeranno l'eliminazione e non l'invio del traffico all'IP del gateway dalla macchina host hypervisor. Lascia questa impostazione autonoma e non configurarla per i VIF.

## Autenticazione VRRP
Se viene impostata una password per l'autenticazione VRRP, deve inoltre essere definito il tipo di autenticazione. Se la password è impostata e il tipo di autenticazione non è definito, il sistema genererà un errore quando tenti di eseguire il commit della configurazione.

Allo stesso modo, non puoi eliminare la password VRRP senza anche eliminare il tipo di autenticazione VRRP. Se fai ciò, il sistema genera un errore quando tenti di eseguire il commit della configurazione. Se elimini sia il tipo che la password di autenticazione VRRP, l'autenticazione VRRP viene disabilitata.

IETF ha deciso che tale autenticazione non deve essere utilizzata per VRRP versione 3. Per ulteriori informazioni, fai riferimento a RFC 5798 VRRP.

## Supporto versione 2/3
Il VRA supporta sia i protocolli VRRP versione 2 (predefinito) che versione 3. Nella versione 2, non puoi avere IPv4 e IPv6 insieme. Ma nella versione 3, puoi avere IPv4 e IPv6 contemporaneamente.

## VPN ad elevata disponibilità con VRRP
Il VRA fornisce la possibilità di mantenere la connettività tramite un tunnel IPsec utilizzando una coppia di VRA (Virtual Router Appliance) con VRRP. Quando un router riscontra un errore o viene chiuso per manutenzione, il nuovo router master VRRP ripristina la connettività IPsec tra le reti remota e locale.

Quando configuri la VPN ad elevata disponibilità con VRRP, se viene aggiunto un indirizzo IP virtuale VRRP a un'interfaccia VRA, devi reinizializzare il daemon IPsec perché il servizio IPsec sia in ascolto solo per le connessioni all'indirizzo presenti nel VRA quando viene inizializzato il daemon del servizio Internet Key Exchange (IKE).

Immetti il seguente comando nei router master e di backup, in modo che quando si verifica un failover, i daemon IPsec vengano riavviati in un un nuovo dispositivo master dopo la transizione del VIP, come illustrato nel seguente esempio:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Firewall ad elevata disponibilità con VRRP

Quando due dispositivi sono in una coppia HA, devi prestare attenzione al tuo dispositivo master perché non ne devi bloccare l'accesso da altri dispositivi. La porta 443 deve essere consentita per entrambi i dispositivi perché la sincronizzazione di configurazione funzioni e VRRP deve poter essere inviato e ricevuto, incluso l'intervallo multicast di 224.0.0.0/24.

## Sottoreti VLAN associate a VRRP

Per ogni configurazione VIF con VRRP avrai bisogno di un indirizzo IP virtuale (il primo IP utilizzabile nella sottorete, l'IP del gateway a cui viene instradata ogni cosa in tale sottorete) e anche di un indirizzo IP dell'interfaccia reale per il VIF su entrambi i dispositivi. Per mantenere gli IP utilizzabili, si consiglia che gli IP dell'interfaccia reali utilizzino un intervallo fuori banda come 192.168.0.0/30, dove un dispositivo ha l'indirizzo .1 e l'altro dispositivo .2. Puoi avere più VIP (VRRP virtual IP), ma hai bisogno solo di un indirizzo reale per ogni VIF.

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

* Un vrrp sync-group è differente da un vrrp group. Quando un'interfaccia che appartiene a un sync-group modifica lo stato, tutti gli altri membri dello stesso sync-group passeranno allo stesso stato. 
* Il numero del vrrp-group delle interfacce VLAN (VIF) non deve essere lo stesso di quello delle interfacce native o delle altre VLAN. Tuttavia, è fortemente consigliato di conservare tutti gli indirizzi virtuali della stessa VLAN in un vrrp-group, come visto nella VLAN 10.
* Gli indirizzi dell'interfaccia reali nelle VLAN native (ad esempio dp0bond1: 169.110.20.28/29) non sono sempre nella stessa sottorete come i loro VIP (169.110.21.26/29). 

## Failover VRRP manuale
Se devi forzare un failover VRRP, può essere ottenuto immettendo quanto segue nel dispositivo che al momento è il master VRRP:

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

L'ID del gruppo è l'ID del gruppo VRRP delle interfacce native e, se menzionato precedentemente, potrebbe essere differente nella tua coppia.

## Sincronizzazione della connessione
Quando due dispositivi VRA sono in una coppia HA, può essere utile tracciare le connessioni con stato tra i due dispositivi in modo che se si verifica un failover, lo stato corrente di tutte le connessioni nel dispositivo in errore sia replicato nel dispositivo di backup, per cui non è necessario ricompilare da zero tutte le sessioni attive correnti (come le connessioni SSL), il che può comportare un'interruzione dell'esperienza dell'utente.

Questa operazione si chiama sincronizzazione della traccia di connessione.

Per configurarla, devi dichiarare quale è il metodo di failover, quale interfaccia utilizzerai per inviare le informazioni sulla traccia di connessione e l'IP del peer remoto:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

L'altro VRA avrà la stessa configurazione, ma un diverso `remote-peer`.

Tieni presente che questo può saturare un link se è presente un elevato numero di connessioni in entrata nelle altre interfacce e sarà in competizione con tutto il traffico nel link dichiarato.
