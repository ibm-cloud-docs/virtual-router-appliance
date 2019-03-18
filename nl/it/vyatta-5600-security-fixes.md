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

# Patch software di AT&T Vyatta 5600 vRouter
{: #at-t-vyatta-5600-vrouter-software-patches}

**A partire dal: 14 febbraio 2019**

Questo documento elenca le patch per le versioni attualmente supportate di Vyatta Network OS 5600. Con le versioni 5.2 e meno recenti, le patch sono denominate utilizzando un numero S. Con le versioni 17.1 e più recenti, le patch sono denominate con una lettera minuscola, escludendo “i”, “o”, “l” e “x”.

Quando sono trattati più numeri CVE in un singolo aggiornamento, viene elencato il punteggio CVSS più alto.

## 1801v

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-45175 | Critico | Dump del core Rsyslogd quando vengono configurati i VRF |
| VRVDR-45057 | Critico | Interfaccia tunnel VTI IPsec in stato A/D dopo avvio iniziale, SA IPsec rimane attivo |
| VRVDR-44985 | Principale | Registrazione DNAT Firewall di input / ordine dell'operazione |
| VRVDR-44944 | Critico | vyatta-config-vti.pl: utilizzo file temporaneo non sicuro |
| VRVDR-44941 | Secondario| Rotta statica mancante nel kernel a causa di breve instabilità interfaccia VTI |
| VRVDR-44914 | Critico | Arresto anomalo ALG RPC su entrambi i membri della coppia HA |
| VRVDR-44668 | Principale | Con il flusso del traffico di produzione-monitoraggio statistiche del flusso di rete che riportano i rallentamenti e gli arresti |
| VRVDR-44667 | Secondario | L'ordine di interfaccia non è coerente tra le esecuzioni di 'show flow-monitoring' |
| VRVDR-44657 | Principale | Il conflitto re-key IKEv1 fa in modo che l'interfaccia VTI rimanga inattiva quando i tunnel sono attivi |
| VRVDR-44560 | Principale| Più rallentamenti CPU rcu_sched puntano al driver ip_gre |
| VRVDR-44517 | Secondario | Arresto anomalo del piano dati con errore grave in rte_ipv6_fragment_packet |
| VRVDR-44282 | Principale | Problema nell'eliminazione della maschera /32 quando nel gruppo di indirizzi sono presenti sia l'indirizzo con la maschera /32 che quello senza |
| VRVDR-44278 | Secondario | "show address-group all ipv4 optimal" non genera output |
| VRVDR-44239 | Principale | Richiesta di miglioramento dei dettagli della GUI Web per l'elenco a discesa del protocollo quando sono richiesti tutti ('all') i protocolli |
| VRVDR-44076 | Principale | perdita di memoria nel flusso-monitoraggio che porta a seg-fault e interruzione del piano dati |
| VRVDR-44007 | Critico | Errore di segmentazione del piano dati in npf_dataplane_session_establish |
| VRVDR-43909 | Secondario| Connsync causa la disattivazione delle interfacce dopo "restart vrrp" |
| VRVDR-42679 | Principale | syslog - arresto anomalo in zactor_is |
| VRVDR-42020 | Principale | Blocco RIB nell'aggiunta più e più volte della stessa rotta |
| VRVDR-18095 | Secondario | Statistiche di monitoraggio del flusso non acquisite come parte di 'show tech-support' |


**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-45148 | N/D | DSA-4371-1 | CVE-2019-3462: Debian DSA-4371-1 – aggiornamento della sicurezza apt |
| VRVDR-45043 | 8,8 | DSA-4369-1 | CVE-2018-19961, CVE-2018-19962, CVE-2018- 19965, CVE-2018-19966, CVE-2018-19967: DSA 4369-1 - aggiornamento della sicurezza Xen|
| VRVDR-45042 | N/D | DSA-4368-1 | CVE-2019-6250: Debian DSA-4368-1 : zeromq3 - aggiornamento della sicurezza |
| VRVDR-45035 | N/D | DSA-4367-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - aggiornamento della sicurezza |
| VRVDR-44956 | 7,5 | DSA-4359-1 | CVE-2018-16864, CVE-2018-16865, CVE-2018- 16866: Debian DSA-4367-1 : systemd - aggiornamento della sicurezza CVE-2018-12086, CVE-2018-18225, CVE-2018- 18226, CVE-2018-18227, CVE-2018-19622, CVE- 2018-19623, CVE-2018-19624, CVE-2018-19625, CVE-2018-19626, CVE-2018-19627, CVE-2018- 19628: Debian DSA-4359-1 : wireshark - aggiornamento della sicurezza |
| VRVDR-44747 | N/D | DSA-4350-1 | CVE-2018-19788: Debian DSA-4350-1 : policykit-1 - aggiornamento della sicurezza |
| VRVDR-44634 | 8,8 | DSA-4349-1 | CVE-2017-11613, CVE-2017-17095, CVE-2018- 10963, CVE-2018-15209, CVE-2018-16335, CVE- 2018-17101, CVE-2018-18557, CVE-2018-5784, CVE-2018-7456, CVE-2018-8905:Debian DSA-4349- 1 : tiff - aggiornamento della sicurezza |
| VRVDR-44633 | 7,5 | DSA-4348-1| CVE-2018-0732, CVE-2018-0734, CVE-2018-0735, CVE-2018-0737, CVE-2018-5407: Debian DSA-4348- 1 : openssl - aggiornamento della sicurezza |
| VRVDR-44611 | 9,8 | DSA-4347-1| CVE-2018-18311, CVE-2018-18312, CVE-2018- 18313, CVE-2018-18314: Debian DSA-4347-1 : perl - aggiornamento della sicurezza |
| VRVDR-44348| 9,8 | DSA-4338-1 | CVE-2018-10839, CVE-2018-17962, CVE-2018- 17963: Debian DSA-4338-1: aggiornamento della sicurezza qemu|
| VRVDR-43264| 5,6 | DSA-4274-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4274- 1: aggiornamento della sicurezza xen |

## 1801u

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-44406 | Critico | Con più sottoreti nello stesso VIF, rilevata bassa velocità del traffico in transito rispetto alle prestazioni di 5400 |
| VRVDR-44253 | Secondario | Il blocco MSS sull'interfaccia di bonding arresta il funzionamento dopo il riavvio |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-44277 | N/D | DSA-4332-1 | CVE-2018-16395, CVE-2018-16396: Debian DSA-4332-1 : ruby2.3 - aggiornamento della sicurezza |
| VRVDR-44276 | N/D | DSA-4331-1 | CVE-2018-16839, CVE-2018-16842: Debian DSA-4331-1 : curl - aggiornamento della sicurezza |

## 1801t

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-44172 | Di blocco | Errore “interfaces [openvpn] is not valid” notificato nei test mss-clamp |
| VRVDR-43969 | Secondario | La GUI Vyatta 18.x sta notificando un utilizzo della memoria di controllo dello stato non corretto |
| VRVDR-43847  | Principale | Velocità effettiva lenta per le conversazioni TCP sull'interfaccia di bonding. |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-43842 | N/D | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – aggiornamento della sicurezza |

## 1801s

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-44041 | Principale | Tempo di risposta lento oid SNMP ifDescr |
 
**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-44074| 9,1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – aggiornamento della sicurezza|
| VRVDR-44054 | 8,8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – aggiornamento della sicurezza |
| VRVDR-44038 | N/D | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – aggiornamento della sicurezza |
| VRVDR-44033 | N/D | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – aggiornamento della sicurezza | 
|VRVDR-43922 | 7,8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – aggiornamento della sicurezza |
| VRVDR-43908 | 9,8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - aggiornamento della sicurezza |
| VRVDR-43884 | 7,5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - aggiornamento della sicurezza |

## 1801r

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-43738 | Principale | I pacchetti di ICMP irraggiungibile restituiti tramite la sessione SNAT non vengono recapitati |
| VRVDR-43538 | Principale | Errori di dimensione eccessiva ricevuti sull'interfaccia di bonding. | 
| VRVDR-43519 | Principale | Vyatta-keepalived è in esecuzione senza alcuna configurazione presente | 
| VRVDR-43517 | Principale | Il traffico non riesce quando l'endpoint di VFP/IPsec basato sulle politiche si trova sul vRouter stesso | 
| VRVDR-43477 | Principale | Il commit della configurazione IPsec VPN restituisce l'avvertenza “Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], codice di errore ricevuto 65280 |
| VRVDR-43379 | Secondario | Le statistiche NAT sono visualizzate in modo non corretto. |
 
**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-43837 | 7,5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1: libarchive-zip-perl –aggiornamento della sicurezza |
| VRVDR-43693 | N/D | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1: mgetty –aggiornamento della sicurezza | 
| VRVDR-43578 | N/D | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1: curl -aggiornamento della sicurezza |
| VRVDR-43326 | N/D | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1: openssh -aggiornamento della sicurezza | 
| VRVDR-43198 | N/D | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1: aggiornamento della sicurezza Linux (FragmentSmack) |
| VRVDR-43110 | N/D | DSA-4265-1 | Debian DSA-4265-1 : xml-security-c -aggiornamento della sicurezza | 
| VRVDR-43057 | N/D | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : libmspack -aggiornamento della sicurezza | 
| VRVDR-43026 | 9,8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 -aggiornamento della sicurezza VRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 :fuse -aggiornamento della sicurezza |

## 1801q

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-43531 | Principale |L'avvio su 1801p causa un errore grave del kernel entro circa 40 secondi |
| VRVDR-43104 | Critico | Falso ARP gratuito sulla rete DHCP quando IPsec è abilitato |
| VRVDR-41531 | Principale | IPsec continua a provare a usare l'interfaccia VFP dopo che ne è stato annullato il bind. |
| VRVDR-43157 | Secondario | Quando si verifica un bounce del tunnel, la trap SNMP non viene generata correttamente. |
| VRVDR-43114 | Critico | Al riavvio, un router in una coppia HA con una priorità più alta del suo peer non rispetta la sua configurazione “preempt false” e diventa il master immediatamente dopo l'avvio |
| VRVDR-42826 | Secondario | Con il remote-id “0.0.0.0” la negoziazione peer non riesce a causa di una mancata corrispondenza di PSK (pre-shared-key) |
| VRVDR-42774 | Critico| Il driver X710 (i40e) sta inviando i frame di controllo del flusso a un tasso molto elevato |
| VRVDR-42635 | Secondario | La modifica della politica di BGP redistribute route-map non diventa effettiva |
| VRVDR-42620 | Secondario | Vyatta-ike-sa-daemon genera l'errore “Command failed: establishing CHILD_SA passthrough-peer” mentre il tunnel sembra essere attivo |
| VRVDR-42483 | Secondario | Autenticazione TACACS in errore |
| VRVDR-42283 | Principale | Lo stato VRRP cambia in FAULT per tutte le interfacce quando un ip interfaccia vif viene eliminato |
| VRVDR-42244 | Secondario | Flow-monitoring esporta solo 1000 campioni al raccoglitore |
| VRVDR-42114 | Critico | Il servizio HTTPS NON DEVE esporre TLSv1 |
| VRVDR-41829 | Principale | Dump del core del piano dati fino a che il sistema non risponde con soak test SIP ALG |
| VRVR-41683 | Di blocco | L'indirizzo del server di nomi DNS appreso su VRF non è riconosciuto in modo congruente |
| VRVDR-41628 | Secondario | Rotta/prefisso da router-advertisement attivo in kernel e piano dati ma ignorato da RIB |
 
**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-43288 | 5,6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – aggiornamento della sicurezza Linux |
| VRVDR-43111 | N/D | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – aggiornamento della sicurezza Linux |

## 1801n

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-42588 | Secondario | Configurazione del protocollo di instradamento sensibile inavvertitamente trapelata nel log di sistema |
| VRVDR-42566 | Critico | Dopo l'upgrade da 17.2.0h a 1801m, un giorno dopo su entrambi i membri HA si sono verificati molteplici riavvii |
| VRVDR-42490 | Principale | VTI-IPSEC IKE SA in errore circa un minuto dopo la transizione VRRP |
| VRVDR-42335 | Principale | IPSEC: la modalità di funzionamento del remote-id “hostname” cambia da 5400 a 5600 |
| VRVDR-42264 | Critico | Nessuna connettività sul tunnel SIT – “kernel: sit: non-ECT da 0.0.0.0 con TOS=0xd” |
| VRVDR-41957 | Secondario | I pacchetti con NAT bidirezionali troppo grandi per GRE non riescono a restituire ICMP Tipo 3 Codice 4 |
| VRVDR-40283 | Principale | Le modifiche di configurazione generano molti messaggi di log |
| VRVDR-39773 | Principale | L'utilizzo di una route-map con il comando BGP vrrp-failover può causare il ritiro di tutti i prefissi |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-42505 | N/D | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - aggiornamento della sicurezza |
| VRVDR-42427 | N/D | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - aggiornamento della sicurezza |
| VRVDR-42383 | N/D | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - aggiornamento della sicurezza |
| VRVDR-42088 | 5,5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – aggiornamento della sicurezza |
| VRVDR-41924 | 8,8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – aggiornamento della sicurezza |

## 5.2R6S12

Data di rilascio: 21 giugno 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-42084 | Di blocco | Interfaccia Vfp contrassegnata come “non-dataplane interface” in “show dataplane route” quando viene riapplicata la configurazione nat/ipsec |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-42317 | 5,4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – aggiornamento della sicurezza |
| VRVDR-42284 | 7,5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – aggiornamento della sicurezza |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – aggiornamento della sicurezza |
| VRVDR-41680 | 7,8 | DSA-4188-1 | Debian DSA-4188-1: linux – aggiornamento della sicurezza (Spectre) |

## 1801m

Data di rilascio: 15 giugno 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-42256 | Critico | Nessun traffico in uscita se viene eliminato l'ultimo CHILD_SA stabilito |
| VRVDR-42084 | Di blocco | Le sessioni NAT collegate alle interfacce VFP per i tunnel PB IPsec non vengono create per i pacchetti che arrivano sul router anche se il router è configurato per farlo |
| VRVDR-42018 | Secondario | Quando viene eseguito “restart vpn”, viene generato un errore “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” |
| VRVDR-42017 | Secondario | Quando “show vpn ipsec sa” è in esecuzione sul backup VRRP, viene generato l'errore “ConnectionRefusedError” correlato a vyatta-op-vpn- ipsec-vici riga 563 |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5,4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – aggiornamento della sicurezza |
| VRVDR- 42284 | 7,5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – aggiornamento della sicurezza |

## 5.2R6S11

Data di rilascio: 11 giugno 2018

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-42109 | Critico | Solo 1 pacchetto di risposta ICMP ricevuto con SNAT+FW su 5.2R6S7 |
| VRVDR-42084 | Di blocco | Le sessioni NAT collegate alle interfacce VFP per i tunnel PB IPsec non vengono create per i pacchetti che arrivano sul router anche se il router è configurato per farlo |
| VRVDR-42027 | Principale | SFLOW con ifIndex di input non corretto |
| VRVDR-41558 | Principale | Le date/ore notificate nelle tracce di pacchetti non sono congruenti con clock di sistema e data/ora effettivi |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7,5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – aggiornamento della sicurezza |
| VRVDR- 42013 | N/D | DSA-4210-1 | CVE-2018-3639: Esecuzione speculativa, variante 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9,8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – aggiornamento della sicurezza |
| VRVDR- 41946 | N/D | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – aggiornamento della sicurezza |
| VRVDR- 41795 | 6,5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – aggiornamento della sicurezza |

## 1801k

Data di rilascio: 8 giugno 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-42084 | Di blocco | Le sessioni NAT collegate alle interfacce VFP per i tunnel PB IPsec non vengono create per i pacchetti che arrivano sul router anche se il router è configurato per farlo |
| VRVDR-41944 | Principale | Dopo il fail-over VRRP, il ristabilimento di alcuni tunnel VTI non riesce finché non viene immesso un “vpn restart” o una reimpostazione del peer (peer reset) |
| VRVDR-41906 | Principale | Il rilevamento PMTU non riesce poiché i messaggi ICMP tipo 3 codice 4 vengono inviati dall'IP di origine errato |
| VRVDR-41558 | Principale | Le date/ore notificate nelle tracce di pacchetti non sono congruenti con clock di sistema e data/ora effettivi |
| VRVDR-41469 | Principale | Un collegamento di interfaccia è inattivo – il collegamento non sta trasportando traffico |
| VRVDR-41420 | Principale | Stato/link collegamento LACP “u/D” con modifica della modalità active-backup a LACP |
| VRVDR-41313 | Critico | IPsec – instabilità dell'interfaccia VTI |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7,5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – aggiornamento della sicurezza |
| VRVDR- 42013 | N/D | DSA-4210-1 | CVE-2018-3639: Esecuzione speculativa, variante 4: speculative store bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9,8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – aggiornamento della sicurezza |
| VRVDR- 41946 | N/D | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – aggiornamento della sicurezza |
| VRVDR- 41795 | 6,5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – aggiornamento della sicurezza |

## 1801j

Data di rilascio: 18 maggio 2018

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41481 | Secondario | VRRP sull'interfaccia di bonding non invia l'avviso VRRP |
| VRVDR-39863 | Principale | Failover di VRRP quando il cliente rimuove l'istanza di instradamento (routing-instance) con GRE associato e l'indirizzo locale (local-address) del tunnel fa parte di VRRP |
| VRVDR-27018 | Critico | Il file di configurazione in esecuzione è leggibile globalmente |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR-41680 | 7,8 | DSA-4188-1 | Debian DSA-4188-1: linux – aggiornamento della sicurezza |

## 5.2R6S10

Data di rilascio: 17 maggio 2018

**Problemi risolti**
| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41543 | Principale | “update config-sync” sta notificando degli errori quando la barra rovesciata “\” viene utilizzata nelle descrizioni della configurazione
| VRVDR-27018 | Critico | Il file di configurazione in esecuzione è leggibile globalmente |

## 1801h

Data di rilascio: 11 maggio 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41664 | Critico | Il piano dati elimina i pacchetti ESP con dimensioni MTU |
| VRVDR-41536 | Secondario | Limite start-init del servizio dnsmasq raggiunto quando si aggiungono più di 4 voci host statiche se l'inoltro dns è abilitato |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7,8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: aggiornamento della sicurezza linux |

## 5.2R6S

Data di rilascio: 8 maggio 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40803 | Secondario | Le interfacce VIF non sono presenti nell'output “show vrrp” dopo un riavvio |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9,8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – aggiornamento della sicurezza |

## 1801g

Data di rilascio: 4 maggio 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41620 | Principale | Il traffico dell'interfaccia vTI smette di inviare traffico dopo l'aggiunta di un nuovo vIF |
| VRVDR-40965 | Principale | Il collegamento non viene ripristinato dopo un arresto anomalo del piano dati |

## 1801f

Data di rilascio: 23 aprile 2018

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41537 | Secondario | Il ping non sta funzionando sul tunnel IPsec su 1801d |
| VRVDR-41283 | Secondario | Configd smette di elaborare le rotte statiche durante l'avvio se la configurazione ha disabilitato le rotte statiche |
| VRVDR-41266 | Principale | Per la rotta statica che trapela a VRF non c'è transito di traffico attraverso il tunnel mGRE dopo il riavvio |
| VRVDR-41255 | Principale | Quando lo slave diventa inattivo, occorrono più di 60s perché lo stato del link del master rifletta tale condizione |
| VRVDR-41252 | Principale | Con VTI senza binding in zone-policy, la regola di rilascio (drop) viene tralasciata, a seconda dell'ordine di commit delle regole di zona |
| VRVDR-41221 | Critico | Upgrade di vRouter da 1801b a 1801c a 1801d con un tasso di errore del 10% |
| VRVDR-40967 | Principale | la disabilitazione dell'inoltro IPv6 previene l'instradamento di pacchetti IPv4 originati da VTI |
| VRVDR-40858 | Principale | Interfaccia VTI che mostra MTU 1428 che causa problemi TCP PMTU |
| VRVDR-40857 | Critico | Vhost-bridge non si presenta per la VLAN con tag con i nomi di interfaccia di una certa lunghezza |
| VRVDR-40803 | Secondario | Le interfacce VIF non sono presenti nell'output “show vrrp” dopo un riavvio |
| VRVDR-40644 | Principale | IKEv1: le ritrasmissioni QUICK_MODE non sono gestite correttamente |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9,8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – aggiornamento della sicurezza |
| VRVDR- 41331 | 6,5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – aggiornamento della sicurezza
| VRVDR- 41330 | 6,5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – aggiornamento della sicurezza |
| VRVDR- 41215 | 6,1 |CVE-2018-1059 | CVE-2018-1059 – Accesso alla memoria host fuori limite del vhost DPDK dai guest VM |

##5.2R6S8
Data di rilascio: 16 aprile 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-41283 | Secondario |Configd smette di elaborare le rotte statiche durante l'avvio se la configurazione ha disabilitato le rotte statiche |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6,5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – aggiornamento della sicurezza

##1801e
Data di rilascio: 28 marzo 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-39985 | Secondario | I pacchetti TCP DF più grandi di MTU di tunnel GRE sono eliminati senza la restituzione di alcuna frammentazione ICMP necessaria | 
| VRVDR-41088 | Critico | ASN esteso (4 byte) non rappresentato internamente come tipo senza segno |
| VRVDR-40988 | Critico | Il vhost non si avvia quando viene utilizzato con un certo numero di interfacce |
| VRVDR-40927 | Critico | DNAT: SDP in SIP 200 OK non convertito quando segue una risposta 183 |
| VRVDR-40920 | Principale | Con 127.0.0.1 come indirizzo di ascolto (listen-address), snmpd non si avvia |
| VRVDR-40920 | Critico | ARP non funziona sull'interfaccia SR-IOV di bonding |
| VRVDR-40294 | Principale | Il piano dati non ripristina le code precedenti dopo la rimozione dello slave dal gruppo di bonding |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/D | DSA-4140-1 | DSA 4140-1: aggiornamento della sicurezza libvorbis |

##5.2R6S7
Data di rilascio: 15 marzo 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-38801 | Principale | Un pacchetto multisegmento ricevuto tramite IPsec VTI causa la disattivazione dell'interfaccia di bonding

##5.2R6S6
Data di rilascio: 12 marzo 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40281 | Principale | Dopo l'upgrade dalla 5.2 a una versione più recente, errore “vbash: show: command not found” in modalità operativa |
| VRVDR-40135 | Principale | I pacchetti Spanning Tree non vengono ricevuti su una porta di bridge di interfaccia VIF |
| VRVDR-39991 | Principale | Il firewall con stato elimina i pacchetti tra due sottoreti sulla stessa interfaccia | 
| VRVDR-36481 | Principale | L'upgrade/downgrade da 5.2R4 a 17.1.0/5.2R3 mostra /opt/vyatta/sbin/vyatta-install-image.functions: riga 372: is_onie_boot: command not found |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8,8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – aggiornamento della sicurezza |
| VRVDR- 39907 | 7,8 | CVE-2017-5717 | Inserimento destinazione ramo / CVE-2017-5717 / Spectre, noto anche come Variante #2 |

##1801d
Data di rilascio: 8 marzo 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40940 | Principale | Arresto anomalo del piano dati correlato a NAT/firewall |
| VRVDR-40886 | Principale | La combinazione di “icmp name <value>” con un numero di altre configurazioni per la regola causerà il mancato caricamento del firewall |
| VRVDR-39879 | Principale | La configurazione del collegamento per i frame jumbo non riesce |

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9,8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – aggiornamento della sicurezza | 
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Inserimento destinazione ramo / CVE-2017-5715 / Spectre, noto anche come variante #2 |

##1801c
Data di rilascio: 7 marzo 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40281 | Principale | Dopo l'upgrade dalla 5.2 a una versione più recente, errore “-vbash: show: command not found” in modalità operativa |

##1801b
Data di rilascio: 21 febbraio 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40622 | Principale | Il corretto rilevamento delle immagini cloud-init non riesce se l'indirizzo IP è stato ottenuto dal server DHCP |
| VRVDR-40613 | Critico | L'interfaccia di bonding non si attiva se uno dei link fisici è inattivo |
| VRVDR-40328 | Principale | L'avvio delle immagini cloud-init richiede molto tempo |

##1801a
Data di rilascio: 7 febbraio 2018.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-40324 | Principale | Medie di carico superiori a 1,0 con nessun carico sul router con l'interfaccia di bonding |

##5.2R6S5
Data di rilascio: 19 gennaio 2018.

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5,6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – aggiornamento della sicurezza (Meltdown) |
| VRVDR- 38265 | 8,8 | DSA-3970-1 | CVE-2017-1 |

##5.2R6S4
Data di rilascio: 15 dicembre 2017.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-39529 | Principale | Il failover del server DHCP non sta sincronizzando i database |
| VRVDR-39399 | Critico | Rilascio da parte di Vyatta dello stato di rete FAULT in show vrrp/più interfacce instabili/errore di segmentazione |
| VRVDR-39112 | Principale | Il traffico DNAT che corrisponde a ZBF rilascia solo il primo pacchetto nel flusso |
| VRVDR-38075 | Secondario | Quando “restart vpn” viene emesso dal responder, l'initiator non ristabilisce la connessione |
| VRVDR-37934 | Critico | Arresto anomalo di BGPd quando aggregate-address summary-only è configurato/mancano le rotte statiche |
| VRVDR-37717 | Secondario | Rinominare i campi “Description” e “License” di hard-enf nell'output della versione |
| VRVDR-37689 | Principale | Elevata frequenza di interrupt PF NIC |
| VRVDR-37633 | Critico | Keepalived si blocca |

## 5.2R6S3
Data di rilascio: 4 dicembre 2017.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-39207 | Critico | ARP in errore sull'interfaccia VIF di bonding |


##5.2R6S2
Data di rilascio: 2 novembre 2017.

**Problemi risolti**

| Numero problema | Priorità | Riepilogo |
| --- | --- | --- |
| VRVDR-39177 | Principale | Opzione domain-name del server OpenVPN non applicata con –push dhcp-option |
| VRVDR-39129 | Critico | Il parametro push-route del server OpenVPN causa un errore di avvio di OpenVPN |

##5.2R6S1
Data di rilascio: 12 ottobre 2017.

**Vulnerabilità della sicurezza risolte**

| Numero problema | Punteggio CVSS | Informativa | Riepilogo |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9,8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- aggiornamento della sicurezza |

Le informazioni qui contenute non sono un'offerta, un impegno, una rappresentazione o una garanzia da AT&T e sono soggette a modifica. Non ne è previsto l'uso o la diffusione al di fuori delle aziende AT&T, fatto salvo un accordo scritto.

© 2018 AT&T Intellectual Property. Tutti i diritti riservati. I logo di AT&T e Globe sono marchi registrati di AT&T Intellectual Property. Tutti gli altri marchi sono di proprietà dei rispettivi proprietari.
