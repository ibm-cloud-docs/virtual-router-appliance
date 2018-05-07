---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Aggiungi le funzioni firewall a VRA (Virtual Router Appliance) (senza stato e con stato)
L'applicazione delle serie di regole del firewall ad ogni interfaccia è un metodo di protezione firewall quando utilizzi VRA (Virtual Router Appliance). Ogni interfaccia ha tre istanze firewall possibili - In, out e local - e ogni istanza dispone di regole che possono essere applicate ad essa. 

L'azione predefinita è Rilascia, con le regole che permettono l'applicazione di traffico specifico nel modo dalla regola 1 alla N. Non appena viene effettuata una corrispondenza, il firewall applicherà l'azione specifica della regola di corrispondenza.

Per ognuna delle seguenti tre istanze firewall, può esserne applicata solo una.

**IN:** il firewall filtra i pacchetti in entrata nell'interfaccia e che attraversano il sistema. Dovrai consentire ad alcuni intervalli di IP di accedere al frontend e al backend per la gestione (ping, monitoraggio e così via).

**OUT:** il firewall filtra i pacchetti che lasciano l'interfaccia. Questi pacchetti possono attraversare il sistema VRA o essere originati nel sistema.

**LOCAL:** il firewall filtra i pacchetti destinati al sistema VRA stesso utilizzando l'interfaccia del sistema. Dovresti stabilire delle limitazioni nelle porte di accesso nel dispositivo dagli indirizzi IP esterni che non sono limitati.

Utilizza la seguente procedura per configurare una regola del firewall di esempio per disattivare ICMP (Internet Control Message Protocol) (ping - messaggio di risposta echo IPv4) alle tue interfacce del VRA (Virtual Router Appliance) (questa è un'azione senza stato; un'azione con stato sarà esaminata successivamente):

1. Immetti `show configuration commands` nel prompt dei comandi per visualizzare quali configurazioni sono impostate. Visualizzerai un elenco di tutti i comandi che hai impostato sul tuo dispositivo (che può essere utile se decidi di migrare e vuoi visualizzare tutte le tue configurazioni). Nota il comando `set firewall all-ping enable`, che indica che ICMP è ancora abilitato per il tuo dispositivo.

2. Immetti `configure`.

3. Immetti `set firwall all-ping disable`.

4. Immetti `commit`.

Se ora provi ad eseguire il ping del tuo dispositivo VRA, non riceverai più una risposta.

Per poter assegnare le regole del firewall al traffico instradato, le regole devono essere applicate alle interfacce del VRA o devono essere create delle zone e quindi applicate ad esse.

In questo esempio, saranno create le zone per le VLAN che sono state utilizzate finora.

 VLAN | Zona 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (per la produzione)
bond0.2254 | privata (per lo sviluppo)
bond1.1280 | riservata per utilizzo futuro
bond1.1894 | riservata per utilizzo futuro

## Crea le regole del firewall
Prima della creazione effettiva delle zone, è una buona idea creare le regole del firewall che devono venire applicate alle zone. La creazione delle regole prima delle zone ti consente di applicarle immediatamente, rispetto alla creazione della zona e quindi delle regole, ritornando alla zona per l'applicazione della regola.

I seguenti comandi effettueranno queste azioni:

* Crea una regola del firewall denominata `dmz2private` con l'azione predefinita di rilascio di tutti i pacchetti.
* Abilita la registrazione del traffico accettato e negato per la regola denominata `dmz2private`.

1. Immetti `configure` nel prompt dei comandi.

2. Immetti i seguenti comandi:

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. Immetti la serie successiva di comandi in modo che quelle iptables consentiranno il traffico per una sessione stabilita da restituire. Per impostazione predefinita, le ipables non consentono ciò, questo perché è necessaria una regola esplicita.

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. Immetti i seguenti comandi per consentire a TCP/UDP di passare per la porta 22, valore predefinito per SSH.
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. Immetti i seguenti comandi per consentire a TCP/UDP di passare per la porta 80, valore predefinito per HTTP.

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. Immetti `commit` per assicurarti che tutte le regole vengano apportate quando terminato.

7. Visualizza la nuova configurazione immettendo `show firewall name dmz2private` nel prompt dei comandi.

8. Crea una regola del firewall che deve essere applicata alla zona dmz immettendo i seguenti comandi nel prompt. La regola sarà denominata public. 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## Crea le zone

Le zone sono rappresentazioni logiche di un'interfaccia. In questa sezione, effettuerai le seguenti azioni:

* Crea una zona e una politica denominate dmz con un'azione predefinita di rilascio dei pacchetti destinati a questa zona.
* Imposta la politica dmz in modo che utilizzi l'interfaccia `bond1` 
* Imposta la politica prod in modo che utilizzi l'interfaccia `bond1.2007` 
* Crea una politica della zona denominata `private` con un'azione predefinita di rilascio dei pacchetti destinati a questa zona
* Imposta la politica denominata `private` in modo che utilizzi l'interfaccia `bond0.2254` 

1. Immetti i seguenti comandi nel prompt:

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. Utilizza i seguenti comandi per impostare la politica del firewall per le zone:

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
Puoi applicare una regola del firewall a un'interfaccia specifica se non vuoi applicarla a una politica della zona. Utilizza i seguenti comandi per applicare una regola a un'interfaccia.

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
