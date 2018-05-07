---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configura IPSec su Vyatta 5400 

Si farà riferimento al dispositivo Brocade 5400 vRouter (Vyatta) come a "locale" in considerazione al tunnel IPSec (Internet Security Protocol). Ognuno dei seguenti comandi eseguirà funzioni differenti per configurare site-to-site IPsec. Nota questo esempio di site-to-site IPsec illustra il tunnel sulla rete pubblica di SoftLayer; utilizza **bond0** per le connessioni site-to-site IPsec private.

1. "Informa" il tunnel dello scopo di **interface bond1:**

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Configura la prima fase del tunnel a due fasi. I seguenti comandi effettueranno queste azioni:

  * Crea un nuovo gruppo **ike** denominato **test** e utilizza **dh-group** come il tipo di scambio chiave.
  * Specifica il tipo di codifica da utilizzare; se non viene configurato, il dispositivo utilizzerà **aes128** come valore predefinito
  * Utilizza la funzione hash **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Configura la seconda fase del tunnel a due fasi. I seguenti comandi effettueranno queste azioni:

  * Disabilita PFS (perfect forward secrecy) perché non tutti i dispositivi possono utilizzarlo. (L'esp nel comando è la seconda parte della codifica.)
  * Specifica il tipo di codifica da utilizzare; se non viene configurato, il dispositivo utilizzerà **aes128** come valore predefinito
  * Utilizza la funzione hash **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Configura i parametri di codifica site-to-site IPsec. I seguenti comandi effettueranno queste azioni:

  * Specifica l'IP del lato remoto e che IPSec utilizzerà il segreto precondiviso
  * Utilizza l'IP remoto e la chiave del segreto TestPSK
  * Imposta il gruppo **esp** predefinito per il tunnel su TestESP
  * "Informa" IPSec di utilizzare ike-group TestIKE, che era stato definito precedentemente<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Crea l'associazione al tunnel IPSec. I seguenti comandi eseguiranno queste azioni, in base all'esempio nel materiale:

  * "Informa" il tunnel di associare l'IP remoto 169.54.254.117 all'indirizzo IP remoto di bond1, 50.97.240.219
  * Instrada solo gli indirizzi IP con la sottorete 10.54.9.152/29 che si trovano nell'interfaccia del server locale al server remoto 169.54.254.117
  * Instrada il traffico remoto tunnel 1 169.54.254.117 alla sottorete remota 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

Il prossimo passo è di configurare il dispositivo lato remoto, che è un Brocade 5400 vRouter 6.6.5 R

  * Utilizza il dispositivo appena configurato (che è stato configurato nella modalità operativa) per immettere il comando show configuration commands. Sarà presentato un elenco di comandi per configurare il dispositivo.
  * Copia i comandi in un editor di testo. I comandi utilizzati per configurare il dispositivo locale saranno utilizzati per configurare il server remoto con le modifiche all'IP in modo che punti al dispositivo Brocade 5400 vRouter 6.6.5R in SoftLayer.

La configurazione lato remoto utilizzata precedentemente è la seguente. Le modifiche necessarie per la configurazione lato locale sono in grassetto.

Configurazione lato remoto:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (L'IP del peer site-to-site è stato scambiato)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (L'indirizzo IP locale è stato scambiato)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (Il prefisso locale e remoto sono stati scambiati)

* Copia e incolla i nuovi comandi nel server remoto (assicurati di essere nella modalità di configurazione) immetti commit e salva.
* Immetti run show vpn ike sa per vedere se il tunnel è ora stato stabilito.

Questo è un riassunto di ciò che è stato fatto: instradati solo gli indirizzi IP con la sottorete '10.54.9.152/29' che risiedono su un'interfaccia locale (bond1, 50.97.240.219) alle sole sottoreti 192.168.1.2/32 nel sevizio remoto che risiedono nell'interfaccia con l'indirizzo IP 169.54.254.117.
