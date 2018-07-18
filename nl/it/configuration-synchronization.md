---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sincronizza le configurazioni dell'alta disponibilità
Due VRA (Virtual Router Appliances) in una coppia HA (alta disponibilità) devono avere le loro configurazioni sufficientemente sincronizzate in modo che i dispositivi si comportino in modo simile. Questo viene eseguito tramite `configuration sync-maps` e puoi scegliere quale parte della configurazione sarà sincronizzata. Se effettui una modifica in una macchina, trasmetterà la configurazione contrassegnata all'altro dispositivo.

**NOTA:** questo sincronizza e salva la configurazione in esecuzione del dispositivo locale sul dispositivo remoto. Tuttavia, come parte del processo di commit non salva la configurazione sul dispositivo locale. 

Le configurazioni che sono univoche in un sistema non vengono sincronizzate nell'altro. Gli indirizzi IP reali e i MAC non dovrebbero venire sincronizzati, per l'istanza. Il nodo stesso di configurazione `system config-sync` e il nodo `service https` non possono essere sincronizzati affatto.

Il seguente esempio illustra config-sync:

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

Le prime due righe creano lo stesso sync-map reale. Qui, la stanza di configurazione per `security firewall` sarà impostata su `sync-map`. Di conseguenza, tutte le modifiche effettuate nel nodo di configurazione saranno trasmesse al dispositivo remoto. Tuttavia, le modifiche effettuate a `security user` non saranno inviate, perché non corrispondono alla regola. Puoi eseguire `sync-map` come specifico o generale a tua scelta.

Le successive tre righe designano la password e l'utente, l'IP e quale sync-map da trasmettere `config-sync` del router remoto. Tutte le modifiche che corrispondo alle regole per `TEST`, andranno a `remote-router 192.168.1.22`, utilizzando queste informazioni di accesso. Tieni presente che viene effettuata una chiamata `REST` per eseguire questa operazione utilizzando l'API VRA, in modo che il server HTTPS debba essere in esecuzione (e sbloccato) nel router remoto.

Config-sync si verifica se esegui il commit di una modifica. Controlla se sono presenti messaggi di errore che provengono dal dispositivo remoto. Se la configurazione non è sincronizzata, dovrai correggerla nel dispositivo remoto per renderlo nuovamente operativo.

Puoi anche visualizzare le differenze di configurazione utilizzando il comando `show config-sync difference`.
