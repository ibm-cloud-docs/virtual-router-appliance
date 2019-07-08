---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: nat, setup, 5400

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

# Configurazione delle regole NAT su Vyatta 5400
{: #setting-up-nat-rules-on-vyatta-5400}

Questo argomento contiene gli esempi delle regole NAT (Network Address Translation) utilizzate su Vyatta.

## Regola NAT one-to-many (mascheramento)
{: #one-to-many-nat-rule-masquerade-}

Immetti i seguenti comandi nel prompt:

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

La richiesta di connessione dalle macchine nella rete `10.xxx.xxx.xxx` viene associata all'IP in bond1 e riceve una porta temporanea associata quando va in uscita. L'intenzione è di assegnare numeri superiori alla regola di mascheramento one-to-many in modo che non siano in conflitto con le regole NAT inferiori di cui puoi disporre.

Devi configurare il server per passare il suo traffico internet tramite la VRA in modo che il suo gateway predefinito sia l'indirizzo IP privato della LAN virtuale gestita (VLAN). Ad esempio, per `bond0.2254` il gateway è `10.52.69.201`. Questo dovrebbe essere l'indirizzo del gateway per il server che sta passando il traffico internet.
{: note}

Utilizza il seguente comando per aiutarti nella risoluzione dei problemi NAT:

  '''
  run show nat source translations detail 
'''
  {: tip}

## Regola NAT uno-a-uno
{: #one-to-one-nat-rule}

I seguenti comandi illustrano come configurare una regola NAT uno-a-uno. Nota che i numeri della regola sono configurati in modo da essere inferiori a quelli della regola di mascheramento. Questo in modo che le regole uno-a-uno abbiano la precedenza sulle regole one-to-many.

Gli indirizzi IP associati uno-a-uno non possono essere mascherati. Se traduci un IP in entrata, devi tradurre quell'IP in uscita in modo che il traffico vada in entrambe le direzioni.
{: tip}

I seguenti comandi sono per una regola di origine e di destinazione. Immetti `show nat` nella modalità di configurazione per visualizzare il tipo di regola NAT.

  Utilizza il seguente comando per aiutarti nella risoluzione dei problemi NAT: `run show nat source translations detail`.
  {: tip}

Immetti i seguenti comandi nel prompt dopo esserti assicurato di essere nella modalità di configurazione:

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

Se il traffico arriva su un IP `50.97.203.227` su bond1, quell'IP sarà associato all'IP `10.52.69.202` (su ogni interfaccia definita). Se il traffico esce con l'IP di `10.52.69.202` (su ogni interfaccia definita), sarà tradotto con l'IP `50.97.203.227` e diretto all'esterno nell'interfaccia bond1.

Gli indirizzi IP associati uno-a-uno non possono essere mascherati. Se traduci un IP in entrata, devi tradurre quello stesso IP in uscita in modo che il suo traffico vada in entrambe le direzioni.
{: note}

## Aggiunta di intervalli di IP tramite la tua VRA
{: #adding-ip-ranges-through-your-vra}

A seconda della tua configurazione VRA, potresti voler accettare indirizzi IP IBM© Cloud specifici.

La nuove distribuzioni vRouter sono fornite con gli indirizzi IP di rete dei servizi di IBM Cloud definiti in una regola del firewall denominata `SERVICE-ALLOW`.

Il seguente è un esempio di `SERVICE-ALLOW`. Questa non è una serie di regole di IP privati completa.

```
~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~
```

Dopo aver definito le regole del firewall, puoi assegnarle a tua discrezione. Di seguito sono elencati due esempi.

Applicazione a una zona: `set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Applicazione a un'interfaccia di collegamento: `set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
