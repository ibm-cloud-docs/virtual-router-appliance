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

# Accesso e configurazione 
Il VRA può essere configurato utilizzando una sessione della console remota tramite SSH o accedendo alla GUI web. Per impostazioni predefinita, la GUI web non è disponibile da internet pubblicamente. Per abilitare la GUI web, accedi prima tramite SSH.

**NOTA:** la configurazione di VRA all'esterno della sua shell e interfaccia può produrre risultati non previsti e quindi non è consigliato.

## Accesso al dispositivo utilizzando SSH 
Molti sistemi operativi basati su UNIX, come Linux, BSD e Mac OSX, hanno i client OpenSSH inclusi nelle loro installazioni predefinite. Gli utenti di Windows possono scaricare un client SSH, come ad esempio PuTTy.

Si consiglia che l'SSH all'IP pubblico sia disabilitato e che sia consentito solo all'IP privato. Le connessioni agli IP privati richiedono che il tuo client sia collegato alla rete privata. Puoi accedere utilizzando una delle seguenti opzioni VPN predefinite (PPTP, SSL-VPN e IPsec) offerte nel portale del cliente o utilizzando una soluzione VPN personalizzata configurata nel VRA.

Utilizza l'account Vyatta dalla pagina **Device Details** per accedere tramite SSH. Viene anche fornita la password root ma l'accesso root viene disabilitato in modo predefinito per motivi di sicurezza. 

`ssh vyatta@[IP address] `

**NOTA:** si consiglia di lasciare gli accessi root disabilitati. Accedi tramite l'account Vyatta ed eleva a root quando necessario.

Possono essere fornite anche le chiavi SSH durante la distribuzione per evitare il bisogno dell'accesso dell'account Vyatta. Dopo aver verificato la tua capacità di accedere al tuo VRA utilizzando la tua chiave SSH, puoi disabilitare gli accessi utente/password standard immettendo i seguenti comandi:

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Accesso al dispositivo utilizzando la GUI Web 

Accedi al VRA utilizzando le precedenti istruzioni SSH, quindi immetti i seguenti comandi per abilitare il servizio HTTPS: 

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

Dopo il completamento di questi comandi, immetti `https://<ip.address>` nella barra di indirizzi del tuo browser, sostituendo l'indirizzo IP con quello del tuo VRA. Ti potrebbe venire richiesto di accettare il certificato emesso in automatico di VRA. Accetta, quindi accedi all'interfaccia web con le tue credenziali Vyatta quando richiesto.

## Modalità
**Modalità di configurazione:** richiamata con l'utilizzo del comando `configure`, questa modalità è dove viene eseguita la configurazione del sistema VRA.

**Modalità operativa:** la modalità iniziale dopo l'accesso a un sistema VRA. In questa modalità, i comandi `show` possono essere immessi per eseguire query sullo stato del sistema. Il sistema può anche essere riavviato da questa modalità.

La shell del VRA è un'interfaccia modale, con molte modalità operative. La modalità primaria/predefinita è **Operational** e sarà la modalità presentata all'accesso. Nella modalità **Operational**, puoi visualizzare le informazioni e immettere i comandi che influenzano l'esecuzione corrente del sistema, come l'impostazione di data/ora o il riavvio del dispositivo.

Il comando `configure` colloca l'utente nella modalità **Configuration**, dove possono essere effettuate modifiche alla configurazione. Tieni presente che queste modifiche non avvengono immediatamente; ne deve essere eseguito il commit e il salvataggio. Come vengono immessi i comandi, vanno in un buffer di configurazione. Quando sono stati immessi tutti i comandi necessari, esegui il comando `commit` per rendere le modifiche effettive.

Per salvare i comandi in modo permanente, esegui il comando `save` dopo `commit`.

I comandi della modalità operativa possono essere eseguiti dalla modalità di configurazione facendo cominciare il comando con `run`. Ad esempio:


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

Il segno cancelletto (`#`) indica la modalità di configurazione. Facendo iniziare un comando con `run` si indica alla shell VRA che è stato presentato un comando operativo. Il precedente esempio illustra inoltre la capacità di "grep" nell'output di comandi.

## Esplorazione comando

La shell del comando VRA include le funzionalità di completamento della scheda. Se sei curioso su quali comandi sono disponibili, premi il tasto Tab per un elenco e una breve spiegazione. Questo funziona al prompt della shell e mentre si digita un comando. Ad esempio:

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Configurazione di esempio
Le configurazioni sono disposte in un pattern gerarchico di nodi. Considera questo blocco di instradamento statico:

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

Sarà generato dai comandi:

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

Questo esempio illustra che per un nodo (statico) è possibile avere più nodi secondari. Per rimuovere la rotta per `192.168.1.0/24` deve essere utilizzato il comando `delete protocols static route 192.168.1.0/24`. Se `192.168.1.0/24` è stato tralasciato dal comando entrambi i nodi di instradamento saranno contrassegnati per l'eliminazione.

Ricorda che la configurazione non è in effetti modificata finché non viene immesso il comando `commit`. Per confrontare la configurazione in esecuzione corrente di tutte le modifiche presentate nel buffer di configurazione, utilizza il comando `compare`. Per scaricare il buffer di configurazione, utilizza `discard`.

## Utenti e RBAC (Role-based Access Control)
Gli account utente possono essere configurati con tre livelli di accesso:

* Amministratore
* Operatore
* Superuser

Gli utenti di livello operatore possono eseguire i comandi `show` per visualizzare lo stato di esecuzione del sistema e immettere i comandi `reset` per riavviare i servizi nel dispositivo. Le autorizzazioni di livello operatore non comportano l'accesso in sola lettura.

Gli utenti di livello amministratore hanno accesso completo a tutte le configurazioni e operazioni del dispositivo. Gli utenti amministratore possono visualizzare le configurazioni, modificare le impostazioni di configurazione del dispositivo, riavviarlo e arrestarlo.

Gli utenti di livello superutente possono eseguire i comandi con i privilegi root tramite il comando `sudo` in aggiunta all'avere i privilegi a livello amministratore.

Gli utenti possono essere configurati per stili di autenticazione password o chiave pubblica o entrambi. L'autenticazione chiave pubblica viene utilizzata con SSH e consente agli utenti di accedere utilizzando un file di chiavi al loro sistema. Per creare un utente operatore con una password:

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**NOTA:** quando non viene specificato un livello, un utente è considerato al livello amministratore. Le password utente in questo caso sono visualizzate come codificate nel file di configurazione.

RBAC (Role-based Access Control) è un metodo per limitare l'accesso a parte della configurazione agli utenti autorizzati. RBAC consente agli amministratori di definire le regole per un gruppo di utenti che limitano i comandi che possono eseguire.

RBAC viene eseguito creando un gruppo assegnato alla serie d regole Access Control Management (ACM), aggiungendo un utente al gruppo, creando una serie di regole per far corrispondere il gruppo ai percorsi nel sistema, quindi configurando il sistema per consentire o negare che questi percorsi siano applicati al gruppo.

Per impostazione predefinita, non è presente una serie di regole ACM predefinita in VRA (Virtual Router Appliance) e ACM è disabilitato. Se desideri utilizzare RBAC per fornire il controllo di accesso granulare, devi abilitare ACM e aggiungere le seguenti regole ACM predefinite in aggiunta alle tue proprie regole definite:

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

Fai riferimento alla [documentazione](vra-docs.html) supplementare prima di tentare di abilitare le regole ACM. Impostazioni della regola ACM inaccurate possono causare negazioni dell'accesso al dispositivo o errori di funzionamento del sistema.
