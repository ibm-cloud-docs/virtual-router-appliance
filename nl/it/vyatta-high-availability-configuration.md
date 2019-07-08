---

copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:important: .important}
{:tip: .tip}

# Configurazione dell'elevata disponibilità di Vyatta 5400
{: #vyatta-5400-high-availability-configuration}

L'alta disponibilità di Vyatta è supportata tramite l'utilizzo di VRRP, Virtual Routing Redundancy Protocol. Ogni gruppo di gateway avrà due indirizzi IP VRRP primari, uno per il lato privato e uno per il lato pubblico delle reti.

I Vyatta solo privati avranno solo il VRRP privato.
{: note}

Questi indirizzi IP sono gli IP di destinazione per l'infrastruttura di rete Softlayer per instradare tutte le sottoreti delle VLAN associate ai membri del gateway. Solo un Vyatta alla volta avrà questi IP VRRP in esecuzione, gli altri membri del gruppo di gateway li avranno disattivati in modo amministrativo.

La configurazione può essere sincronizzata tra due Vyatta con i comandi di configurazione "config-sync". Questa configurazione consentirà a un membro di trasmettere la configurazione di opzioni specifiche a un altro Vyatta in un gruppo e di farlo in modo selettivo. Puoi trasmettere solo le regole del firewall o solo la configurazione IPsec o una qualsiasi combinazione di serie di regole.

Si consiglia di non tentare di trasmettere gli indirizzi IP o altre configurazioni di rete poiché config-sync eseguirà immediatamente il commit delle tue modifiche agli altri Vyatta, portando quelle interfacce online. Se vuoi abilitare dinamicamente le interfacce e i servizi, dovresti utilizzare gli script di transizione per eseguire questa azione al failover. Inoltre, si consiglia di utilizzare più indirizzi IP VRRP per i tuoi IP del gateway sulle VLAN associate, questo renderà più facile gestire un failover.

La tua configurazione VRRP di base su entrambe le macchine sarà simile a questa configurazione di esempio:

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP richiede che sia associato un indirizzo IP reale a un'interfaccia virtuale prima che possa essere inviato un qualsiasi avviso VRRP. In molti casi puoi semplicemente aggiungere un IP dalla sottorete primaria, ma questo può andare in conflitto con le forniture future o potresti voler instradare una VLAN che già ha ogni IP della sottorete primaria assegnato a un server. Per aggirare questo problema, puoi utilizzare una coppia di indirizzi IP fuori banda su entrambe i Vyatta che possono comunicare tra loro. Questo è un esempio:

Nella prima Vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

Nella seconda Vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

In questo caso, entrambi i Vyatta avranno un loro IP che non andrà in conflitto con la sottorete assegnata. Puoi scegliere quasi ogni intervallo privato che desideri; scegli soltanto una piccola sottorete che non andrà in conflitto con tutti gli altri instradamenti di cui disponi, come un intervallo di sottoreti in un tunnel IPsec o un indirizzo 10.0.0.0/8 in conflitto con quello di Softlayer.

Vorrai anche aggiungere un nome "sync-group". Tutti gli indirizzi VRRP dovrebbero far parte dello stesso sync-group. Questo perché un malfunzionamento in un'interfaccia comporterà che anche tutte le interfacce nello stesso sync-group siano in errore. Altrimenti, alla fine alcune potrebbero essere MASTER e altre in BACKUP. Utilizza lo stesso nome nella configurazione VLAN nativa bond0 e bond1.

La configurazione VRRP bond0 e bond1 potrebbe avere una riga per rfc3768-compatibility. Questa non è necessaria per VRRP in un VIF, solo la VLAN nativa di bond0 e bond1.
{: tip}

In una coppia di gateway appena forniti, config-sync avrà solo una configurazione minima:


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

Dovrai aggiungere le regole per determinare su quali rami della configurazione migrare. Ad esempio, se vuoi che il firewall e la configurazione ipsec vengano trasmessi, aggiungi questi comandi:


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Dopo aver eseguito il commit di questi comandi, tutte le modifiche al firewall o alla configurazione ipsec saranno trasmesse a un altro Vyatta al commit.

sync-group e sync-map sono due cose diverse. La configurazione sync-map è per le regole per trasmettere le modifiche della configurazione a un altro Vyatta. L'altro, sync-group, è per gli IP VRRP con un failover come un gruppo invece di uno alla volta. La configurazione di uno non influenza l'altro.
{: tip}
