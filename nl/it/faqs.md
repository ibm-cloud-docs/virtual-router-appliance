---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# FAQ per IBM Virtual Router Appliance
{: #faqs-for-ibm-virtual-router-appliance}

Le seguenti sono le FAQ (frequently asked question) quando si utilizza IBM© Virtual Router Appliance (VRA).

## Cosa è una VRA? 
{:faq}

Una VRA (Virtual Router Appliance) consente a un cliente IBM Cloud di instradare selettivamente il traffico di rete pubblico e privato tramite un router aziendale completo con firewall, modellamento del traffico, instradamento basato sulla politica, VPN e un host di altre funzioni. Tutte le funzioni VRA sono gestite dal cliente. VRA fornisce a un cliente IBM Cloud un grado di controllo riservato normalmente alle reti in loco.

## Cosa è un'applicazione gateway? 
{:faq}

Un'apparecchiatura Applicazione gateway ti consente di utilizzare il portale web o l'API per scegliere i segmenti di rete (VLAN) per l'instradamento tramite una VRA. Puoi modificare le selezioni VLAN in qualsiasi momento. L'applicazione gateway gestisce inoltre l'alta disponibilità (HA) della VRA, configurando una seconda VRA da utilizzare se si verifica un malfunzionamento della prima.

## Alcune volte vedo dei riferimenti a termini come "Vyatta" e "vRouter." Come si correlano alla VRA?
{:faq}

Vyatta era un software basato sul PC open source, che è stato completamente acquisito e passato a un'origine chiusa. Oggi, "Vyatta" e "Vyatta OS" descrivono gli adeguamenti del software commerciali da tale progetto di origine chiuso. IBM VRA incorpora elementi di Vyatta OS, insieme a un notevole miglioramento del servizio e della funzione disponibile esclusivamente tramite IBM Cloud.

"vRouter" era un cambiamento di marchio di breve durata di Vyatta e di sua proprietà. Quando visto nella documentazione, può essere considerato un sinonimo di Vyatta.

## È Vyatta 5400 ancora supportato?
{:faq}

IBM non supporterà più Vyatta 5400 a partire dal 31 marzo 2019.

## Quali miglioramenti per Virtual Router Appliance (Vyatta 5600) rispetto a Vyatta 5400?
{:faq}

Vyatta 5600 offre i seguenti miglioramenti rispetto a Vyatta 5400:

- Velocità di trasmissione maggiore, fino a 10Gbps per core CPU
- Oltre 3X di velocità di trasmissione per sessione VPN IPsec (standard di codifica avanzati)
- Capacità incrementata per i router, i flussi e le connessioni simultanee.
- Supporto per gli standard aggiornato, inclusi Layer 2 Tunneling Protocol, versione 3 (L2TPv3), Internet Key Exchange, versione 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2) e l'incapsulamento 802.1Q tunneling (Q-in-Q)

## Come è l'offerta AT&T vRouter 5600?
{:faq}

AT&T (precedentemente Brocade) ha annunciato la fine del ciclo di vita e del supporto della loro offerta Brocade vRouter 5600. Mentre Brocade vRouter 5600 fornisce la capacità tecnologica sottostante per IBM Virtual Router Appliance, questo annuncio non si applica ai clienti IBM. I clienti IBM continueranno ad avere supporto utilizzando questa nuova offerta.

## Come viene fornita la VRA? 
{:faq}

Ottieni una VRA ordinando un gateway di rete. Questo semplice processo di permette di scegliere un data center e un server VRA adatto, così come se volessi distribuire una coppia HA di VRA. I server, i sistemi operativi e l'apparecchiatura Applicazione gateway sono tutti forniti automaticamente. Quando il provisioning è completo, puoi utilizzare l'interfaccia dell'applicazione gateway per instradare le VLAN tramite la VRA. Puoi configurare il tuo server VRA direttamente utilizzando SSH (shell sicura) con le password fornite nella sezione dei dettagli hardware del portale del cliente.

## È la mia password protetta? 
{:faq}

Sì. A tutte le VRA sono assegnate password casuali visibili solo al titolare dell'account. Le password sono facilmente modificabili, perché sono chiavi pubbliche SSH e gestiscono le limitazioni dell'accesso all'IP.

## Posso ottenere una VRA senza un'applicazione gateway? 
{:faq}

Sì, ma può gestire solo il traffico tra le interfacce privata e pubblica della VRA. Le VLAN e HA richiedono l'apparecchiatura Applicazione gateway.

## Tutto il traffico di rete viene inviato tramite la VRA? 
{:faq}

No. L'applicazione gateway ti permette di selezionare i segmenti di rete pubblica e privata (VLAN) che desideri instradare tramite la VRA. Puoi modificare e tralasciare le selezioni VLAN in qualsiasi momento. VRA ti permette inoltre di definire le regole basate sull'IP che si applicano alle sottoreti o agli intervalli IP. Queste regole funzionano solo se le VLAN che contengono queste sottoreti sono instradate tramite la VRA.

## Può una VRA o un firewall dedicato impedire il provisioning di un nuovo server? 
{:faq}

Sì. Quando possibile, non dovresti bloccare la tua rete finché non l'hai popolata con i server che hai pianificato di utilizzare.

Al supporto IBM è proibito dalla politica di esaminare o modificare la VRA o la configurazione del firewall dedicato senza un coinvolgimento esplicito del cliente, per cui in molti casi il supporto non può sapere che una VRA è responsabile di provisioning del server in stallo o non riusciti.

È responsabilità del cliente assicurarsi che la VRA o il firewall siano configurati in modo da consentire i provisioning del server automatizzati prima di inserire l'ordine del server. I provisioning che sono bloccati da una VRA o un firewall gestito dal cliente, è responsabilità del cliente risolverli. Tali ritardi del provisioning non sono soggetti a SLA o crediti. I sistemi ordinati possono essere restituiti all'inventario (dopo che sono stati cancellati i dati del cliente) se il cliente non risponde velocemente.

Allo stesso modo, se una VRA o un firewall vengono esclusi dopo l'inserimento di un ordine, è ancora probabile che l'ordine non riesca. Potrebbe esserci una finestra ridotta durante la quale saranno tentati i nuovi tentativi di automazione. È meglio che l'intero processo di provisioning proceda senza interferenze di rete.

## Quali prodotti firewall offre IBM?
{:faq}

Puoi trovare un confronto dettagliato di tutti i prodotti firewall offerti in IBM Cloud esaminando [questo argomento](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-exploring-firewalls). 

## Può una VRA confondere gli sforzi del supporto clienti? 
{:faq}

Sì, per i motivi descritti precedentemente. VRA è una "scatola nera:" in cui le VLAN entrano ed escono e IBM non ha idea di quali clienti stiano facendo cosa.

Il supporto fa sempre del suo meglio, ma con VRA e il firewall dedicato: a) la privacy del cliente prevale sulla connettività e b) il nostro staff di supporto standard non è equipaggiato per analizzare le configurazioni VRA/firewall molto complesse o malformate.

Come primo passo di diagnostica, potremmo aver bisogno che metti le tue VLAN VRA e firewall in bypass. Se, in questo stato, i provisioning che hanno avuto esito negativo iniziano a funzionare, dobbiamo assumere che il problema risiede nella tua configurazione VRA/firewall.

## Quale effetto avrà VRA sulle mie prestazioni di rete? 
{:faq}

Tieni presente che anche se non possono vederti, un cloud pubblico condivide le reti con altri clienti. Nel migliore dei casi la velocità di trasmissione della VRA viene determinata dalla capacità di rete disponibile in punto nel tempo, con in più la distanza che devono percorrere i dati.

A parte queste variabili, VRA può inoltrare 80 Gbps di traffico non modificato tra più interfacce, utilizzando la formula approssimativa che ogni 10 Gbps di velocità di trasmissione richiedono un core del processore completo (non includendo l'hyperthreading). Dato che i server correnti escono al massimo di 40 Gbps (2 x 10 Gbps pubblici + 2 x 10 Gb privati), un server con 8 o più core dovrebbero avere abbastanza capacità di calcolo per gestire più funzioni VRA comuni approssimativamente al meglio delle loro prestazioni di rete.

## Cosa posso fare se perdo la mia password VRA?
{:faq}

Se hai accesso al sistema, imposta una nuova password eseguendo il seguente comando:

```
set system login user [account] authentication plaintext-password [password]  
```

Se non hai accesso al sistema, puoi riavviare il dispositivo e utilizzare l'opzione di recupero della password nel menu GRUB per il ripristino della password utente principale.

## Cosa faccio se sono stato bloccato dal firewall?
{:faq}

Il costrutto `reboot at [time]` può essere utile durante la verifica di regole firewall potenzialmente pericolose.

Se la regola funziona, utilizza il comando `reboot cancel` per annullare il riavvio. Se la regola ti blocca l'accesso, attendi semplicemente che si verifichi il riavvio pianificato.

Se non hai accesso al sistema, potrebbe venire utilizzato un riavvio per ripristinare l'accesso. Al riavvio, il sistema leggerà il file di configurazione che non dovrebbe essere stato modificato dalle voci precedenti che sono state eliminate.

Se puoi accedere utilizzando IPMI, puoi eseguire le seguenti azioni per ripristinare l'accesso:

1. Disabilita la regola in errore eseguendo:

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Sgancia la regola denominata completa impostata dall'interfaccia necessaria eseguendo:

	```
	delete interfaces dataplane [interface] firewall [type][firewall name]
	commit
	```

**NOTA:** l'utilizzo non corretto di questi comandi può cancellare la tua configurazione dell'interfaccia.

## Come posso abilitare gli accessi root per la VRA?
{:faq}

Per abilitare l'accesso root tramite SSH, esegui il seguente comando:

`set service ssh allow-root`

Nota che consentire l'accesso root utilizzando SSH non è considerato sicuro. Un'alternativa per accedere alla shell root è di accedere come un altro utente e elevare la root localmente con `su -` o di consentire i comandi sudo per 'superusers'. 

Ad esempio, per configurare vyatta come un superuser:

`set system login vyatta level superuser`
