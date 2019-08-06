---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, issues, faqs, migration, upgrading

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

# Problemi di migrazione comuni di Vyatta 5400
{: #vyatta-5400-common-migration-issues}

La seguente tabella illustra i problemi comuni o le modifiche della modalità di funzionamento che potresti riscontrare dopo la migrazione da un dispositivo Vyatta 5400 a una {{site.data.keyword.vra_full}}. In alcuni casi, include delle soluzioni temporanee per risolvere i problemi.

## Politica di stato globale basata sull'interfaccia per il firewall con stato
{: #interface-based-global-state-policy-for-stateful-firewall}

### Problemi
{: #issues}

La modalità di funzionamento quando si imposta "State of State-policy" per i firewall con stato dalla release 5.1 è stata modificata. Nella versione antecedente alla release 5.1, se impostavi `state - global -state -policy` di un firewall con stato, il vRouter aggiungeva automaticamente una regola `Allow` implicita per la comunicazione di ritorno della sessione automaticamente

Nella release 5.1 e versioni successive, devi aggiungere un'impostazione di regola `Allow` sulla {{site.data.keyword.vra_full}}. L'impostazione con stato funziona per le interfacce sui dispositivi Vyatta 5400 e per i protocolli sui dispositivi VRA.

### Soluzioni alternative
{: #workarounds}
Se la regola `firewall-in` viene applicata su un'interfaccia Ingress/Inside, la regola `Firewall-out` deve essere applicata sull'interfaccia Egress/Outside. Altrimenti, il traffico di ritorno verrà rilasciato all'interfaccia Egress/Outside.        

## Abilitazione dello stato nelle regole firewall
{: #state-enable-in-firewall-rules}

### Problemi
{: #issues-2}
Se `global-state-policy` non è configurato, questa modifica della modalità di funzionamento non è interessata.

Inoltre, se stai utilizzando l'opzione `state enable` in ciascuna regola invece di `global-state-policy`, la modifica della modalità di funzionamento non è interessata.

## Politica basata sulla zona: gestione della zona locale
{: #zone-based-policy-local-zone-handling}

### Problemi
{: #issues-3}
Non c'è alcuna pseudointerfaccia "local-zone" da assegnare alla politica di zona.

### Soluzioni alternative
{: #workarounds-3}
Questa modalità di funzionamento può essere simulata applicando un firewall basato sulle zone alle interfacce fisiche e un firewall interfaccia all'interfaccia loopback. Il firewall nell'interfaccia loopback filtra tutto quanto entra nel, ed esce dal, router.

Ad esempio:

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## Ordine delle operazioni per firewall, NAT, instradamento e DNS
{: #order-of-operation-for-firewalls-nat-routing-and-dns}

### Problemi
{: #issues-4}
In uno scenario in cui la NAT di origine Masquerade viene distribuita su una {{site.data.keyword.vra_full}}, non puoi utilizzare il firewall per determinare l'accesso per gli host a internet. Ciò è dovuto al fatto che l'indirizzo post-NAT sarà lo stesso.

Per i dispositivi Vyatta 5400, questa operazione era possibile perché il firewalling veniva eseguito prima della NAT, consentendo la limitazione dell'accesso a Internet agli host.

### Soluzioni alternative
{: #workarounds-4}
Un nuovo schema di instradamento è richiesto per la VRA:
![routing dns](./images/routing-dns.png)

## Tabella PBR (Policy Based Routing)
{: #policy-based-routing-table}

### Problemi
{: #issues-5}

La parola "Table" nelle configurazioni è facoltativa nel PBR (Policy Based Routing) v5400 ma, per VRA, se l'azione è `accept`, il campo **Table** è obbligatorio. Se l'azione è `drop` nella configurazione VRA, il campo Table è facoltativo.

### Soluzioni alternative
{: #workarounds-5}
"Table Main" è un'opzione disponibile nel PBR (Policy Based Routing) Vyatta 5400. L'equivalente in VRA è "routing-instance default".

## PBR (Policy Based Routing) nell'interfaccia tunnel
{: #policy-based-routing-on-tunnel-interface}

### Problemi
{: #issues-6}
Sulla {{site.data.keyword.vra_full}}, le politiche PBR (Policy Based Routing) possono essere applicate alle interfacce di piano dati per il traffico in entrata ma non alle interfacce senza numero IP, VTI, OpenVPN, bridge, tunnel e loopback.

### Soluzioni alternative
{: #workarounds-6}
Non ci sono attualmente delle soluzioni temporanee per questo problema.

## TCP-MSS
{: #tcp-mss}

### Problemi
{: #issues-7}

{{site.data.keyword.vra_full}} utilizza nftables e non supporta TCP-MSS.

### Soluzioni alternative
{: #workarounds-7}

Non ci sono attualmente delle soluzioni temporanee per questo problema.

## OpenVPN
{: #openvpn}

### Problemi
{: #issues-8}

OpenVPN non inizia a funzionare quando si utilizza il parametro `push-route` sulla {{site.data.keyword.vra_full}}.

### Soluzioni alternative
{: #workarounds-8}

Utilizza il parametro `openvpn-option` invece di `push-route`.

## GRE/VTI su IPSEC + OSPF
{: #gre-vti-over-ipsec-ospf}

### Problemi
{: #issues-9}

* Quando VIF ha più sottoreti configurate, il traffico non può attraversare tali sottoreti nella VRA.                                             
* L'instradamento InterVlan non sta funzionando sulla VRA.

### Soluzioni alternative
{: #workarounds-9}

Utilizza le regole di consenso implicito per accettare il traffico attraverso le interfacce VIF.

## IPSec
{: #ipsec}

### Problemi
{: #issues-10}

IPSec (basato sui prefissi) non funziona con il filtro IN.

### Soluzioni alternative
{: #workarounds-10}

Utilizza IPSec (BASATO SU VTI).

## IPSEC "match-none"
{: #ipsec-match-none-}

### Problemi
{: #issues-11}

Con i dispositivi Vyatta 5400, è consentita la seguente regola firewall:

set firewall name allow rule 10 ipsec

Tuttavia, con {{site.data.keyword.vra_full}}, non c'è alcun IPSec.

### Soluzioni alternative
{: #workarounds-11}

Regole alternative possibili per i dispositivi VRA:

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"
{: #ipsec-match-ipsec-}

### Problemi
{: issues-12}

Con i dispositivi Vyatta 5400, sono consentite le seguenti regole firewall:

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

Tuttavia, con {{site.data.keyword.vra_full}}, non c'è alcun IPSec.

### Soluzioni alternative
{: workarounds-12}

Aggiungi i protocolli `ESP` e `AH` (rispettivamente i protocolli IP 50 e 51).

La regola `action` può essere `accept` o `drop`, come mostrato di seguito:

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## IPSEC site-to-site con DNAT
{: #site-to-site-ipsec-with-dnat}

### Problemi
{: #issues-13}

IPSec (basato sui prefissi) non funziona con DNAT.                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1)
===S-S-IPsec=== (12.0.0.1)
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

Il frammento di codice in alto è un piccolo esempio di configurazione per la conversione DNAT dopo che un pacchetto IPSec è stato decrittografato in un Vyatta 5400. Nell'esempio, ci sono due vyatta, `vyatta1 (11.0.0.1)` e `vyatta2 (12.0.0.1)`. Il peering IPsec viene stabilito tra `11.0.0.1` e `12.0.0.1`. In questo caso, il client ha come destinazione `172.16.1.245` originato dall'end-to-end `10.103.0.1`.La modalità di funzionamento prevista di questo scenario è che l'indirizzo di destinazione `172.16.1.245` verrà convertito in `10.71.68.245` nell'intestazione del pacchetto.

Inizialmente, il dispositivo Vyatta 5400 stava eseguendo DNAT sull'IPSec in entrata, terminando l'interfaccia e restituendo il traffico normalmente nel tunnel IPsec utilizzando la tabella di traccia della connessione.

Su una {{site.data.keyword.vra_full}}, la configurazione non funziona nello stesso modo. La sessione viene creata, tuttavia il traffico di restituzione esclude il tunnel IPsec dopo che la tabella di traccia della connessione ha invertito la modifica DNAT.La VRA invia quindi il pacchetto in transito senza la crittografia IPsec. Il dispositivo di upstream non sta prevedendo questo traffico e molto probabilmente lo rilascerà.Sebbene la connettività end-to-end sia interrotta, questa è la modalità di funzionamento prevista.   

### Soluzioni alternative
{: #workarounds-13}

Al fine di conformarsi allo scenario di rete sopra indicato, IBM ha creato una RFE.

Mentre tale RFE è attualmente in fase di valutazione, consigliamo la seguente soluzione temporanea:

**Comandi di configurazione dell'interfaccia**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**Comandi di configurazione VPN**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**Comandi di configurazione NAT**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**Comandi di configurazione dei protocolli**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**Comandi di configurazione PBR**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP
{: #pptp}

### Problemi
{: #issues-13}

PPTP non è più supportato nella {{site.data.keyword.vra_full}}.                                                                                                                                                   

### Soluzioni alternative
{: #workarounds-13}

Utilizza invece il protocollo L2TP.

## Script per il riavvio di IPSec
{: #script-for-ipsec-restart}

### Problemi
{: issues-14}

Quando viene aggiunto un indirizzo virtuale VRRP a una {{site.data.keyword.vra_full}} su una VPN ad alta disponibilità, devi reinizializzare il daemon IPsec. Ciò è dovuto al fatto che il servizio IPsec è in ascolto solo per le connessioni agli indirizzi presenti nella VRA quando viene inizializzato il daemon IKE.

Per una coppia di VRA con VRRP, il router di standby potrebbe non avere l'indirizzo virtuale VRRP che è presente sul dispositivo durante l'inizializzazione se il router master non ha tale indirizzo presente. Pertanto, per reinizializzare il daemon IPsec quando si verifica una transizione dello stato VRRP esegui questo comando sui router master e di backup:

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Conteggio recente e tempo recente
{: #recent-count-and-recent-time}

### Problemi
{: #issues-15}

L'intento della seguente regola è limitare le connessioni SSH a 3 ogni 30 secondi per SSH utilizzando qualsiasi indirizzo:

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

Sulla {{site.data.keyword.vra_full}}, questa regola ha i seguenti problemi:

* L'opzione per il conteggio recente e il tempo recente è stata dichiarata obsoleta.

* A causa del problema precedente, la regola non può funzionare come previsto e bloccherà tutte le connessioni SSH all'interfaccia applicata.

### Soluzioni alternative
{: #workarounds-15}

Utilizza invece CPP.

## Imposta problemi di conntrack di sistema
{: #set-system-conntrack-issues}

### Problemi
{: #issues-16}

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## Imposta timeout di conntrack di sistema
{: #set-system-conntrack-timeout}

### Problemi
{: #issues-17}

```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## Firewall basato sul data/ora
{: #time-based-firewall}

### Problemi
{: #issues-18}

```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS
{: #tcp-mss}

### Problemi
{: #issues-19}

```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## Applicazione o porta specifiche interrotte nella VPN Ipsec S-S
{: #specific-application-or-port-broken-in-s-s-ipsec-vpn}

### Problemi
{: #issues-19}

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Modifica significativa nella modalità di funzionamento della registrazione
{: #significant-change-in-logging-behavior}

### Problemi
{: #issues-20}

Si è verificato un importante cambiamento nella modalità di funzionamento della registrazione tra il dispositivo Vyatta 5400 e {{site.data.keyword.vra_full}}: da registrazione per sessione a registrazione per pacchetto.

* Registrazione sessione: registra le transizioni di stato delle sessioni con stato

* Registrazione pacchetto: registra tutti i pacchetti che corrispondono alla regola. Poiché la registrazione pacchetto viene registrata nel file di log in "unità pacchetto", si verifica una marcata diminuzione di velocità effettiva e pressione sulla capacità del disco.

* La funzionalità di registrazione del vRouter può essere utilizzata per acquisire l'attività del firewall. Come per qualsiasi altra funzione di registrazione, devi abilitarla solo se stai risolvendo uno specifico problema e disabilitarla appena puoi.

Il firewall con stato che gestisce le sessioni Firewall / NAT scrive in "unità sessione". Si consiglia di utilizzare la registrazione sessione. Ogni esempio di impostazione è descritto di seguito:

**Sessione / registrazione**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Firewall di registrazione pacchetto**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>`
