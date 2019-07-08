---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: firewall, manage, stateless, stateful, alg, firewall, rules, CPP, Logging

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

# Gestisti i tuoi firewall IBM
{: #manage-your-ibm-firewalls}

VRA (Virtual Router Appliance) ha la capacità di elaborare le regole del firewall per proteggere le VLAN instradate tramite il dispositivo. I firewall nella VRA possono essere suddivisi in due passi:

1. Definizione di una o più serie di regole.
2. Applicazione di una serie di regole a un'interfaccia o zona. Una zona è formata da una o più interfacce di rete.

È importante verificare le regole del firewall dopo la creazione e assicurarsi che funzionino come previsto e che le nuove regole non limitino l'accesso amministrativo al dispositivo.

Mentre modifichi le regole nell'interfaccia `dp0bond1`, ti consigliamo di collegarti al dispositivo utilizzando `dp0bond0`. Il collegamento alla console tramite IPMI (Intelligent Platform Management Interface) è anche un'opzione.

## Assenza di stato in confronto ad avere uno stato
{: #stateless-vs-stateful}

Per impostazione predefinita, il firewall è senza stato, ma può essere configurato come con stato, se necessario. Un firewall senza stato avrà bisogno delle regole per il traffico in entrambe le direzioni, mentre i firewall con stato tracciano le connessioni e automaticamente consentono il traffico dei flussi accettati. Per configurare un firewall con stato, devi determinare quali regole vuoi che operino con stato.

Per abilitare il tracciamento 'con stato' del traffico `tcp`, `udp` o `icmp`, immetti i seguenti comandi:

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Nota che i comandi `global-state-policy` tracceranno solo lo stato del traffico che corrisponde a una regola del firewall che imposta esplicitamente il protocollo corrispondente. Ad esempio:

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

Poiché `GLOBAL_STATELESS` non specifica `protocol tcp`, il comando `global-state-policy tcp` non si applica a questa regola.

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

In questo caso `protocol tcp` è definito esplicitamente. Il comando `global-state-policy tcp` abilita il tracciamento con stato del traffico che corrisponde alla regola 1 di `GLOBAL_STATEFUL_TCP`


Per rendere le singole regole del firewall 'con stato':

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
Questo abilita il tracciamento con stato di tutto il traffico che può venire tracciato con stato e che corrisponde alla regola 1 di `TEST`, indipendentemente dall'esistenza dei comandi `global-state-policy`.

## ALG per il tracciamento con stato assistito
{: #alg-for-assisted-stateful-tracking}

Alcuni protocolli come FTP utilizzano sessioni più complesse di quanto può tracciare la normale operazione firewall con stato.
Esistono dei moduli preconfigurati che abilitano questi protocolli in modo da essere gestiti con lo stato.

Si consiglia di disabilitare questi moduli ALG, a meno che non siano necessari per il corretto utilizzo dei rispettivi protocolli.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Serie di regole del firewall
{: #firewall-rule-sets}

Le regole del firewall vengono raggruppate tra loro in serie denominate per far applicare le regole a più interfacce facilmente. Ogni regola ha un'azione predefinita associata ad essa. Considera il seguente esempio:
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

Nella serie di regole, `ALLOW_LEGACY`, sono presenti due regole definite. La prima regola elimina tutto il traffico originato da un gruppo di indirizzi denominato `network-group1`. La seconda regola scarta e registra tutto il traffico destinato alla porta telnet (`tcp/23`) dal gruppo di indirizzi denominato `network-group2`. L'azione predefinita indica che non viene accettato altro.

## Consentire l'accesso al data center
{: #allowing-data-center-access}

IBM© offre diverse sottoreti IP per fornire servizi e supporto ai sistemi in esecuzione nel data center. Ad esempio, i servizi del resolver DNS sono in esecuzione in `10.0.80.11` e `10.0.80.12`. Le altre sottoreti sono utilizzate durante il provisioning e il supporto. Puoi trovare gli intervalli IP utilizzati nei data center in [questo argomento](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges).

Puoi consentire l'accesso ai data center inserendo le regole `SERVICE-ALLOW` appropriate all'inizio delle serie di regole del firewall con un'azione `accept`. Dove la serie di regole che deve essere applicata dipende dall'instradamento e dalla progettazione del firewall che stanno venendo implementati.

Si consiglia di inserire le regole del firewall nell'ubicazione che causa la minima duplicazione del lavoro. Ad esempio, consentendo l'ingresso delle sottoreti di backend in `dp0bond0` dovrebbe comportare meno lavoro rispetto a consentire l'uscita delle sottoreti di backend verso ogni interfaccia virtuale della VLAN.

### Regole del firewall pre-interfaccia
{: #per-interface-firewall-rules}

Un metodo di configurazione del firewall su una VRA è di applicare le serie di regole del firewall per ogni interfaccia. In questo caso un'interfaccia può essere un'interfaccia del dataplane (`dp0s0`) o un'interfaccia virtuale (`dp0bond0.303`). Ogni interfaccia ha tre assegnazioni firewall possibili:

`in` - Il firewall è controllato con i pacchetti immessi tramite l'interfaccia. Questi pacchetti possono essere in attraversamento o destinati alla VRA.

`out` - Il firewall è controllato con i pacchetti in uscita tramite l'interfaccia. Questi pacchetti possono essere in attraversamento o originati dalla VRA.

`local` - Il firewall è controllato con i pacchetti destinati direttamente alla VRA.

Un'interfaccia può avere più serie di regole applicate in ogni direzione. Vengono applicate nell'ordine di configurazione. Tieni presente che non è possibile utilizzare il firewall per il traffico originato dal dispositivo VRA utilizzando i firewall pre-interfaccia.

Ad esempio, per assegnare la serie di regole `ALLOW_LEGACY` all'opzione `in` per l'interfaccia `bp0s1`, è necessario utilizzare il comando di configurazione:

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
{: #control-plane-policing-cpp-}

CPP (Control Plane Policing) fornisce la protezione da attacchi alla VRA (Virtual Router Appliance) consentendoti di configurare le politiche del firewall assegnate alle interfacce desiderate e applicando queste politiche ai pacchetti in entrata nella VRA.

CPP viene implementato quando la parola chiave `local` viene utilizzata nelle politiche del firewall assegnate a tutti i tipi di interfaccia VRA, come i loopback o le interfacce del piano dati. Diversamente dalle regole del firewall applicate ai pacchetti in attraversamento nella VRA, l'azione predefinita delle regole del firewall per il piano di controllo del traffico in entrata o in uscita è `Allow`. Gli utenti devono aggiungere regole di rilascio esplicite se il comportamento predefinito non è desiderato.

La VRA fornisce una serie di regole CPP di base come template. Puoi unirle nella tua configurazione eseguendo:

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

Dopo che questa serie di regole è stata unita, viene aggiunta e applicata una nuova serie di regole denominata `CPP` all'interfaccia di loopback. Si consiglia di modificare questa serie di regole per soddisfare il tuo ambiente.

Tieni presente che le regole CPP non possono essere con stato e saranno applicate solo al traffico in ingresso.

## Firewall zona
{: #zone-firewalling}

Un altro concetto di firewall in VRA (Virtual Router Appliance) è il firewall basato sulla zona. Nell'operazione di firewall basato sulla zona, viene assegnata un'interfaccia a una zona (solo una zona per interfaccia) e vengono assegnate le serie di regole del firewall ai limiti tra le zone con l'idea che tutte le interfacce all'interno di una zona abbiano lo stesso livello di sicurezza e possono instradare liberamente. Il traffico viene soltanto analizzato quando passa da una zona a un'altra. Le zone rilasciano tutto il traffico in entrata che non è consentito esplicitamente.

Un'interfaccia può appartenere a una zona o a una configurazione del firewall pre-interfaccia; un'interfaccia non ad entrambi.

Immagina il seguente scenario di ufficio con tre dipartimenti, ognuno con la propria VLAN:

* Dipartimento A - VLAN 10 e 20 (interfaccia dp0bond1.10 e dp0bond1.20)
* Dipartimento B - VLAN 30 e 40 (interfaccia dp0bond1.30 e dp0bond1.40)
* Dipartimento C - VLAN 50 (interfaccia dp0bond1.50)

Una zona può essere creata per ogni dipartimento e le interfacce per tale dipartimento possono essere aggiunte alla zona. Il seguente esempio illustra questo:
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

Il comando `commit` popola ogni zona come un'interfaccia e le regole di rilascio predefinite eliminano tutto il traffico che tenta di entrare nella zona dall'esterno. In questo esempio, le VLAN 10 e 20 possono passare il traffico perché sono nella stessa zona (`DEPARTMENTA`) ma VLAN 10 e VLAN 30 non possono passare il traffico perché VLAN 30 è in una zona differente (`DEPARTMENTB`).

Le interfacce all'interno di ogni zona possono passare il traffico gratuitamente e possono essere definite le regole per l'interazione tra le zone. Una serie di regole viene configurata da punto di vista del lasciare una zona per un'altra.

Il seguente comando mostra un esempio di come configurare una regola:

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

Questo comando associa la transizione da DEPARTMENTC a DEPARTMENTB alla serie di regole denominata `ALLOW_PING`. Il traffico in entrata nella zona DEPARTMENTB dalla DEPARTMENTC viene controllato da questa serie di regole.

È importante comprendere che questa assegnazione dalla zona DEPARTMENTC alla zona DEPARTMENTB non crea alcuna istruzione per l'inverso. Se non sono presenti regole che consentono il traffico dalla zona DEPARTMENTB alla DEPARTMENTC, il traffico (risposte a ICMP) non torna agli host in DEPARTMENTC.

`ALLOW_PING` sarà applicato come un firewall `out` nelle interfacce della zona DEPARTMENTB (dp0bond1.30 e dp0bond1.40). Come viene installato dalla politica della zona, solo il traffico originato dalle interfacce della zona di origine (dp0bond1.50) viene controllato con la serie di regole.

## Registrazione pacchetto e sessione
{: #session-and-packet-logging}

La VRA supporta due tipi di registrazione:

1. Registrazione sessione.  Utilizza il comando ``security firewall session-log`` per configurare la registrazione della sessione del firewall.

	Per UDP, ICMP e tutti i flussi non TCP, la tua sessione transiterà tra quattro stati durante la durata del flusso. Per ogni transizione, puoi configurare la VRA per registrare un messaggio. TCP ha molte transizioni di stato, ognuna che può essere configurata per la registrazione.  Il seguente è un esempio di configurazione di session-log per il tuo firewall:

```
set security firewall session-log icmp established
set security firewall session-log tcp established
set security firewall session-log udp established
```

2. Registrazione per pacchetto. Includi la parola chiave ``log`` nel firewall o nella regola NAT per registrare ogni pacchetto di rete che corrisponde alla regola.

	La registrazione per pacchetto si verifica nei percorsi di inoltro del pacchetto e genera una grande quantità di output. È molto importante notare che la registrazione per pacchetto può ridurre enormemente la velocità di trasmissione della VRA, causando problemi di prestazioni, e aumentando notevolmente lo spazio su disco utilizzato per i file di log. Consigliamo di utilizzare la registrazione per pacchetto solo per scopi di debug. Per tutti i fini operativi, dovrebbe essere utilizzata la registrazione della sessione con stato invece della registrazione per pacchetto.
	{: important}
