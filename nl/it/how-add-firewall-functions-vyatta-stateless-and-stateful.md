---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Aggiunta delle funzioni Firewall a Vyatta 5400 (senza stato e con stato)
{: #adding-firewall-functions-to-vyatta-5400-stateless-and-stateful-}

L'applicazione delle serie di regole del firewall ad ogni interfaccia è un metodo di protezione firewall quando utilizzi i dispositivi Brocade 5400 vRouter (Vyatta). Ogni interfaccia ha tre istanze firewall possibili - In, out e local - e ogni istanza dispone di regole che possono essere applicate ad essa. L'azione predefinita è Rilascia, con le regole che permettono l'applicazione di traffico specifico nel modo dalla regola 1 alla N. Non appena viene effettuata una corrispondenza, il firewall applicherà l'azione specifica della regola di corrispondenza.

Per ognuna delle seguenti tre istanze firewall, può esserne applicata **solo una**.

* **IN:** il firewall filtra i pacchetti in entrata nell'interfaccia e che attraversano il sistema Brocade. Dovrai consentire ad alcuni intervalli di IP SoftLayer di avere accesso al frontend e al backend per la gestione (ping, monitoraggio e così via).
* **OUT:** il firewall filtra i pacchetti che lasciano l'interfaccia. Questi pacchetti possono essere in attraversamento del sistema Brocade o originati dal sistema.
* **LOCAL:** il firewall filtra i pacchetti destinati al sistema Brocade vRouter stesso tramite l'interfaccia del sistema. Dovresti stabilire delle limitazioni nelle porte di accesso nel dispositivo Brocade vRouter dagli indirizzi IP esterni che non sono limitati.

Utilizza la seguente procedura per configurare una regola del firewall di esempio per disattivare ICMP (Internet Control Message Protocol) *(ping - messaggio di risposta echo IPv4)* alle tue interfacce di Brocade 5400 vRouter (questa è un'azione senza stato; un'azione con stato sarà esaminata successivamente):

1\. Immetti i comandi *show configuration* nel prompt dei comandi per visualizzare quali configurazioni sono impostate. Visualizzerai un elenco di tutti i comandi che hai impostato sul tuo dispositivo (che può essere utile se decidi di migrare e vuoi visualizzare tutte le tue configurazioni). Nota il comando *set firewall all-ping 'enable'*, che indica che ICMP è ancora abilitato per il tuo dispositivo.

2\. Immetti *configure*.

3\. Immetti *set firwall all-ping 'disable'*.

4\. Immetti *commit*.

Se ora provi ad eseguire il ping del tuo dispositivo Brocade 5400 vRouter, non riceverai più una risposta.

Per assegnare le regole del firewall al traffico instradato, le regole devono essere applicate alle **interfacce** di Brocade 5400 vRouter o devono essere **create delle zone** e quindi applicate ad esse.

In questo esempio, saranno create le zone per le VLAN che sono state utilizzate finora.

**VLAN = Zone**

bond1 = dmz

bond1.2007 = prod (per la produzione)

bond0.2254 = privata (per lo sviluppo)

bond1.1280 = riservata per utilizzo futuro

bond1.1894 = riservata per utilizzo futuro

**Crea le regole del firewall**

Prima della creazione effettiva delle zone, è una buona idea creare le regole del firewall che devono venire applicate alle zone. La creazione delle regole prima delle zone ti consente di applicarle immediatamente, rispetto alla creazione della zona, alla creazione delle regole e a dover ritornare alla zona per l'applicazione della regola.

**NOTA:** le zone e le serie di regole hanno un'istruzione di azione predefinita. Quando utilizzi le zone-politiche, l'azione predefinita viene impostata dall'istruzione zona-politica ed è rappresentata dalla regola 10.000. È anche importante ricordare che l'azione predefinita di una serie di regole del firewall è impostata su **drop** per tutto il traffico.

I seguenti comandi effettueranno queste azioni:

* Crea una regola del firewall denominata **dmz2private** con l'azione predefinita di rilascio di tutti i pacchetti.
* Abilita la registrazione del traffico accettato e negato per la regola denominata **dmz2private**.


1\. Immetti *configure* nel prompt dei comandi.

2\. Immetti i seguenti comandi nel prompt:

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

La serie successiva di comandi è per abilitare le **iptables** per consentire il traffico per una sessione stabilita da restituire. Per impostazione predefinita, le **iptables** non consentono ciò, questo perché è necessaria una regola esplicita.

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

La terza serie di comandi consente a TCP/UDP di passare per la porta 22, valore predefinito per SSH.

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

La serie finale di comandi consente a TCP/UDP di passare per la porta 80, valore predefinito per HTTP.

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. Immetti *commit* per assicurarti che tutte le regole vengano apportate quando terminato.

4\. Visualizza la tua configurazione immettendo *show firewall name dmz2private* nel prompt dei comandi.

La prossima regola del firewall che creiamo sarà applicata alla nostra zona **dmz**. La regola sarà denominata **public**. Immetti i seguenti comandi nel prompt.

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**NOTA:** le regole del firewall devono fluire all'esterno tramite **prod** a **dmz**. Utilizza il seguente comando per aiutarti nella risoluzione dei problemi del flusso di rete: *sudo tcpdump -i any host 10.52.69.202*.

**Crea le zone**

Le zone sono rappresentazioni logiche di un'interfaccia. I seguenti comandi effettueranno queste azioni:

* Crea una zona e una politica denominate **dmz** con un'azione predefinita di rilascio dei pacchetti destinati a questa zona.
* Imposta la politica **dmz** in modo che utilizzi l'interfaccia **bond1**.
* Imposta la politica **prod** in modo che utilizzi l'interfaccia **bond1.2007**.
* Crea una politica della zona denominata **private** con un'azione predefinita di rilascio dei pacchetti destinati a questa zona.
* Imposta la politica denominata **private** in modo che utilizzi l'interfaccia **bond0.2254**.

1\. Immetti i seguenti comandi nel prompt:

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. Utilizza i seguenti comandi per impostare la politica del firewall per le zone:

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

Tieni presente che puoi applicare una regola del firewall a un'interfaccia specifica se non vuoi applicarla a una politica della zona. Utilizza i seguenti comandi per applicare una regola a un'interfaccia.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## Firewall con lo stato

Un firewall *con lo stato* conserva una tabella dei flussi precedentemente visualizzati e i pacchetti possono essere accettati o scartati in base alla loro relazione con i pacchetti precedenti. Come regola generale, i firewall con stato sono generalmente preferiti dove il traffico dell'applicazione è prevalente. 

<span style="text-decoration: underline">*Brocade 5400 vRouter non traccia lo stato delle connessioni con la configurazione predefinita. Il firewall è senza stato finché non viene riscontrata una delle seguenti condizioni:*</span>

* La configurazione di un firewall e di tutte le regole hanno impostato un parametro state
* La configurazione di un firewall state-policy
* La configurazione di NAT
* L'abilitazione del servizio proxy web trasparente
* L'abilitazione di una configurazione di bilanciamento del carico WAN

## Firewall senza stato

Un firewall *senza stato* considera ogni pacchetto in isolamento. I pacchetti possono essere accettati o rilasciati solo in base ai criteri ACL (access control list) di base come ad esempio i campi di destinazione e di origine nelle intestazioni IP o TCP/UDP (Transmission Control Protocols/User Datagram Protocol). Un Brocade 5400 vRouter senza stato non archivia le informazioni di connessione e non ha requisiti per ricercare ogni relazione del pacchetto con i flussi precedenti, di cui consuma piccole quantità di memoria e tempo CPU. Le prestazioni di inoltro non elaborate sono pertanto migliori in un sistema senza stato. Brocade raccomanda di conservare il router senza stato per delle prestazioni migliori se non hai bisogno di funzioni specifiche senza stato.
