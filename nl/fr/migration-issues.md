---

copyright:
  years: 2017
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Problèmes de migration courants pour Vyatta 5400
Le tableau ci-après illustre les problèmes ou les changements de comportement courants que vous pouvez rencontrer après la migration à partir d'un périphérique Vyatta 5400 vers un dispositif IBM Virtual Router Appliance. Dans certains cas, il propose des solutions de contournement. 

## Règle Global-State basée sur l'interface pour le pare-feu avec état

### Problèmes
Le comportement lors de la définition de "State of State-policy" pour les pare-feu avec état à partir de l'édition 5.1 a changé. Dans les versions antérieures à l'édition 5.1, si vous définissiez `state - global -state -policy` pour un pare-feu avec état, le dispositif vRouter ajoutait automatiquement une règle `Allow` implicite pour la communication en retour de la session. 

A partir de l'édition 5.1, vous devez ajouter un paramètre de règle `Allow` sur le dispositif VRA (Virtual Router Appliance). Le paramètre avec état fonctionne par interface sur les périphériques Vyatta 5400 et par protocole sur les dispositifs VRA. 

### Solutions de contournement
Si la règle `firewall-in` est appliquée à une interface d'entrée/interne, la règle `Firewall-out` doit être appliquée sur l'interface de sortie/externe. Sinon, le trafic de retour sera supprimé au niveau de l'interface de sortie/externe.         

## Utilisation de State-Enable dans les règles de pare-feu

### Problèmes
Si `global-state-policy` n'est pas configuré, ce changement de comportement n'est pas affecté. 

De plus, si vous utilisez l'option `state enable` dans chaque règle au lieu de `global-state-policy`, le changement de comportement n'est pas affecté. 

## Règle basée sur une zone : traitement local-zone

### Problèmes
Il n'existe aucune pseudo-interface "local-zone" à affecter à la règle basée sur une zone.  

### Solution de contournement
Ce comportement peut être simulé en appliquant un pare-feu basé sur une zone à des interfaces physiques et un pare-feu d'interface à l'interface de bouclage. Le pare-feu situé dans l'interface de bouclage filtre tout ce qui rentre et sort à partir du routeur.  

Par exemple :

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

## Ordre des opérations pour les pare-feu, NAT, le routage et DNS

### Problèmes
Dans un scénario où Masquerade Source NAT est déployé sur un dispositif VRA (IBM Virtual Router Appliance), vous ne pouvez pas utiliser le pare-feu afin de déterminer les droits d'accès pour les hôtes à Internet. Cela est dû au fait que l'adresse post-NAT sera identique.

Pour les périphériques Vyatta 5400, cette opération était possible car la protection par pare-feu a été réalisée avant NAT, autorisant la restriction d'accès des hôtes à Internet.

### Solutions de contournement
Un nouveau schéma de routage est requis pour le dispositif VRA :
![routing dns](./images/routing-dns.png)

## Table de routage basée sur une règle

### Problèmes
Le mot "Table" dans les configurations est facultatif dans le routage basé sur la règle pour v5400, mais pour le dispositif VRA, si l'action est `accept` la zone **Table** est obligatoire. Si l'action est `drop` sur la configuration de dispositif VRA, la zone Table est facultative.

### Solutions de contournement
"Table Main" est une option disponible dans le routage basé sur la règle pour Vyatta 5400. L'équivalent dans le dispositif VRA est "routing-instance default".

## Routage basé sur une règle sur l'interface de tunnel

### Problèmes
Sur le dispositif VRA (Virtual Router Appliance), des règles PBR (Policy Based Routing) peuvent être appliquées aux interfaces de plan de données pour le trafic entrant, mais pas pour les interfaces non numérotée de bouclage, de tunnel, de pont, OpenVPN, VTI et d'adresse IP.

### Solutions de contournement
Il n'existe actuellement aucune solution de contournement pour ce problème.

## TCP-MSS

### Problèmes
IBM Virtual Router Appliance utilise nftables et ne prend pas en charge TCP-MSS.

### Solutions de contournement
Il n'existe actuellement aucune solution de contournement pour ce problème.

## OpenVPN

### Problèmes
OpenVPN ne se lance pas lorsque le paramètre `push-route` est utilisé sur le dispositif Virtual Router Appliance.

### Solutions de contournement
Utilisez le paramètre `openvpn-option` à la place de `push-route`.

## GRE/VTI sur IPSEC + OSPF

### Problèmes
* Lorsque plusieurs sous-réseaux sont configurés pour VIF, le trafic ne peut pas traverser ces sous-réseaux dans le dispositif VRA.
* InterVlan Routing ne fonctionne pas sur le dispositif VRA.

### Solutions de contournement
Utilisez les règles allow implicites pour accepter le trafic sur des interfaces VIF.

## IPSec

### Problèmes
IPSec (basé sur le préfixe) ne fonctionne pas avec le filtre IN.

### Solutions de contournement
Utilisez IPSec (basé sur VTI).

## IPSEC 'match-none"

### Problèmes
Avec des périphériques Vyatta 5400, la règle de pare-feu suivante est autorisée :

set firewall name allow rule 10 ipsec 

Toutefois, avec IBM Virtual Router Appliance, il n'existe aucun IPSec.

### Solutions de contournement
Les autres règles possibles pour les périphériques VRA sont les suivantes :

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### Problèmes
Avec des périphériques Vyatta 5400, les règles de pare-feu suivantes sont autorisées :

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

Toutefois, avec IBM Virtual Router Appliance, il n'existe aucun IPSec.

### Solutions de contournement
Ajoutez les protocoles `ESP` et `AH` (respectivement, protocoles IP 50 et 51).

La règle `action` peut être `accept` ou `drop`, comme illustré ci-dessous :

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

## IPSEC de site à site avec DNAT

### Problèmes
IPSec (basé sur le préfixe) ne fonctionne pas avec DNAT.

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1) 
===S-S-IPsec=== (12.0.0.1) 
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

Le fragment de code ci-dessus est un petit exemple d'installation et de configuration pour la conversion DNAT après qu'un paquet IPSec a été déchiffré dans un Vyatta 5400. L'exemple illustre deux vyattas, `vyatta1 (11.0.0.1)` et `vyatta2 (12.0.0.1)`. L'appairage IPsec est établi entre 11.0.0.1` et `12.0.0.1`. Dans ce cas, le client cible l'adresse '172.16.1.245' à partir de l'adresse source '10.103.0.1' de bout en bout. Le comportement attendu de ce scénario est que l'adresse de destination `172.16.1.245` est convertie en `10.71.68.245` dans l'en-tête de paquet.

Initialement, le périphérique Vyatta 5400 effectuait une action DNAT sur l'IPSec entrant, en mettant fin à l'interface et en renvoyant gracieusement le trafic dans le tunnel IPsec à l'aide du tableau de suivi de connexion.

Sur un dispositif VRA (Virtual Router Appliance), la configuration ne fonctionne pas de la même façon. La session est créée, mais, le trafic de retour ignore le tunnel IPsec après que la table conntrack inverse le changement DNAT. Le dispositif VRA envoie ensuite le paquet sur le réseau sans chiffrement IPsec. Le périphérique en amont ne s'attend pas à ce trafic et va probablement le supprimer. Tant que la connectivité de bout en bout est rompue, ce comportement est prévu.   

### Solutions de contournement
Pour prendre en charge le scénario de mise en réseau ci-dessus, IBM a créé une RFE. 

Durant l'évaluation de cette RFE, nous recommandons la solution de contournement suivante :

**Commandes de configuration d'interface**

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

**Commandes de configuration de réseau privé virtuel**

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

**Commandes de configuration de conversion d'adresses réseau**

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

**Commandes de configuration des protocoles**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
.set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**Commandes de configuration de routeur PBR**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### Problèmes
PPTP n'est plus pris en charge dans le dispositif Virtual Router Appliance.

### Solutions de contournement
Utilisez le protocole L2TP à la place.

## Script de redémarrage IPSec

### Problèmes
Chaque fois qu'une adresse virtuelle VRRP est ajoutée à un IBM Virtual Router Appliance sur un réseau privé virtuel à haute disponibilité, vous devez réinitialiser le démon IPsec. En effet, le service IPsec écoute uniquement les connexions aux adresses qui sont présentes sur le dispositif VRA lorsque le démon de service IKE est initialisé.

Pour une paire de dispositifs VRA avec VRRP, le routeur de secours peut ne pas avoir l'adresse virtuelle VRRP qui est présente sur le périphérique durant l'initialisation si cette adresse n'est pas présente sur le routeur principal. Par conséquent, pour réinitialiser le démon IPsec lorsqu'une transition d'état VRRP se produit, exécutez la commande suivante sur le routeur principal et le routeur de sauvegarde :

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Comptage récent et période récente

### Problèmes

L'objectif de la règle suivante est de limiter le nombre de connexions SSH à 3 toutes les 30 secondes pour SSH avec n'importe quelle adresse :

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

Sur IBM Virtual Router Appliance, cette règle présente les problèmes suivants :

* L'option de comptage récent et de période récente est devenue obsolète.

* En raison du problème précédent, la règle ne peut pas fonctionner comme prévu et elle bloque toutes les connexions SSH avec l'interface appliquée.

### Solutions de contournement
Utilisez CPP à la place.

## Problèmes liés à set system conntrack

### Problèmes
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

## Problèmes liés à set system conntrack timeout

### Problèmes
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

## Pare-feu basé sur le temps

### Problèmes
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

### Problèmes
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

## Application spécifique ou port rompu dans VPN Ipsec S-S

### Problèmes

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix
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
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Changement significatif de comportement de consignation

### Problèmes
Il existe un changement de comportement significatif entre le périphérique Vyatta 5400 et IBM Virtual Router Appliance, qu'il s'agisse de la journalisation de session ou de la journalisation de paquet.

* Journalisation de session : enregistrement des transitions d'état de session avec état

* Journalisation de paquet : enregistrement de tous les paquets qui correspondent à la règle. Dans la mesure où la journalisation de paquet est enregistrée dans le fichier journal par "unités de paquet", l'on constate une baisse notable du débit et de la pression de la capacité disque.

* La fonction de journalisation du routeur vRouter peut être utilisée pour capturer les activités de pare-feu. Comme pour toutes les fonctions de journalisation, vous ne devez l'activer que lorsque vous tentez d'identifier et de résoudre un problème spécifique, puis vous devez la désactiver dès que possible.

Le pare-feu avec état qui gère les sessions de pare-feu/NAT écrit des données dans des "unités de session". Il est recommandé d'utiliser la journalisation de session. Chaque exemple de définition est décrit ci-dessous :

**Session / logging**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Packet logging Firewall**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>` 
