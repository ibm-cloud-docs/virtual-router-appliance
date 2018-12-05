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

# Correctifs logiciels AT&T Vyatta 5600 vRouter

**A partir du 2 novembre 2018**

Ce document répertorie les correctifs qui s'appliquent aux versions actuellement prises en charge pour le système d'exploitation de réseau Vyatta 5600. Jusqu'à la version 5.2, les correctifs sont nommés à l'aide d'un nombre S. A compter de la version 17.1, les correctifs sont nommés avec une minuscule, sauf “i”, “o”, “l” et “x”.

Lorsque plusieurs numéros CVE sont traités dans une seule mise à jour, le score CVSS le plus élevé est répertorié. 

## 1801t

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-44172 | Blocage | Erreur “interfaces [openvpn] is not valid” signalée lors de tests mss-clamp |
| VRVDR-43969 |Mineure| L'interface graphique Vyatta 18.x génère un statut incorrect de la commande de vérification d'utilisation de la mémoire |
| VRVDR-43847  | Majeure | Faible débit pour les conversations TCP sur l'interface de liaison |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-43842 | N/A | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1 : mise à jour de sécurité strongswan |

## 1801s

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-44041 | Majeure | Temps de réponse lent pour l'ID objet ifDescr SNMP |
 
**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-44074| 9.1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1 : mise à jour de sécurité libssh |
| VRVDR-44054 | 8.8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1 : mise à jour de sécurité spice |
| VRVDR-44038 | N/A | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1 : mise à jour de sécurité wireshark |
| VRVDR-44033 | N/A | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1 : mise à jour de sécurité net-snmp | 
|VRVDR-43922 | 7.8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1 : mise à jour de sécurité linux |
| VRVDR-43908 | 9.8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1 : mise à jour de sécurité python3.5 |
| VRVDR-43884 | 7.5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1 : mise à jour de sécurité python2.7 |

## 1801r

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-43738 | Majeure | Les paquets inaccessibles ICMP renvoyés via une session SNAT ne sont pas distribués |
| VRVDR-43538 | Majeure | Réception d'erreurs de surdimensionnement sur l'interface de liaison | 
| VRVDR-43519 | Majeure | Vyatta-keepalived s'exécute alors qu'il n'existe aucune configuration | 
| VRVDR-43517 | Majeure | Echec du trafic lorsque le noeud final d'IPsec VFP/basé sur les stratégies réside sur le routeur vRouter proprement dit | 
| VRVDR-43477 | Majeure | La validation de la configuration VPN IPsec renvoie l'avertissement “Warning: unable to [VPN toggle net.ipv4.conf.intf.disable_policy], code d'erreur 65280 reçu |
| VRVDR-43379 |Mineure| Affichage incorrect des statistiques NAT |
 
**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-43837 | 7.5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1 : mise à jour de sécurité libarchive-zip-perl |
| VRVDR-43693 | N/A | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1 : mise à jour de sécurité mgetty | 
| VRVDR-43578 | N/A | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1 : mise à jour de sécurité curl |
| VRVDR-43326 | N/A | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1 : mise à jour de sécurité openssh | 
| VRVDR-43198 | N/A | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1 : mise à jour de sécurité linux (FragmentSmack) |
| VRVDR-43110 | N/A | DSA-4265-1 | Debian DSA-4265-1 : mise à jour de sécurité xml-security-c | 
| VRVDR-43057 | N/A | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : mise à jour de sécurité libmspack | 
| VRVDR-43026 | 9.8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 -security updateVRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 : mise à jour de sécurité fuse |

## 1801q

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-43531 | Majeure |L'amorçage sur 1801p génère une panique du noyau pendant environ 40 secondes |
| VRVDR-43104 |Critique | Faux protocoles de résolution d'adresse gratuits sur le réseau DHCP lorsque IPsec est activé |
| VRVDR-41531 | Majeure | IPsec continue d'essayer d'utiliser une interface VFP après avoir supprimé sa liaison |
| VRVDR-43157 |Mineure| Lorsque le tunnel prend le contrôle, l'alerte SNMP n'est pas correctement générée |
| VRVDR-43114 |Critique | Au redémarrage, un routeur d'une paire à haute disponibilité ayant une priorité plus élevée que celle de son homologue n'honore pas sa propre configuration “preempt false” et devient le maître juste après le démarrage |
| VRVDR-42826 |Mineure| Avec l'ID distant “0.0.0.0”, la négociation homologue échoue en raison d'une non concordance de clé pré-partagée |
| VRVDR-42774 |Critique | Le pilote X710 (i40e) envoie des trames de contrôle de flux à un débit très élevé |
| VRVDR-42635 |Mineure| La modification de la stratégie route-map de redistribution BGP est sans effet |
| VRVDR-42620 |Mineure| Vyatta-ike-sa-daemon émet l'erreur “Command failed: establishing CHILD_SA passthrough-peer” alors que le tunnel semble actif |
| VRVDR-42483 |Mineure| Echec de l'authentification TACACS |
| VRVDR-42283 | Majeure | L'état VRRP devient FAULT pour toutes les interfaces lorsqu'une adresse IP d'interface VIF est supprimée |
| VRVDR-42244 |Mineure| La surveillance de flux exporte uniquement 1000 échantillons vers le collecteur |
| VRVDR-42114 |Critique | Le service HTTPS ne DOIT PAS exposer TLSv1 |
| VRVDR-41829 | Majeure | Vidage mémoire du plan de données jusqu'à ce que le système ne réponde plus avec un test d'imprégnation SIP ALG |
| VRVDR-41683 | Blocage | L'adresse de serveur de noms DNS apprise au cours de l'acheminement et du routage VRF n'est pas systématiquement reconnue |
| VRVDR-41628 |Mineure| Route/préfixe de l'annonce de routeur actif dans le noyau et le plan de données mais ignoré par RIB |
 
**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-43288 | 5.6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 : mise à jour de sécurité Linux |
| VRVDR-43111 | N/A | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 : mise à jour de sécurité Linux|

## 1801n

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-42588 |Mineure| Fuite accidentelle de la configuration du protocole de routage sensible dans le journal système |
| VRVDR-42566 |Critique | Après la mise à niveau de 17.2.0h vers 1801m, plusieurs réamorçages le jour d'après se sont produits sur les deux membres à haute disponibilité |
| VRVDR-42490 | Majeure | Echec des SAS IKE VTI-IPSEC environ une minute après la transition VRRP |
| VRVDR-42335 | Majeure | IPSEC : changement du comportement “hostname” remote-id de 5400 à 5600 |
| VRVDR-42264 |Critique | Pas de connectivité sur le tunnel S – “kernel: sit: non-ECT from 0.0.0.0 with TOS=0xd” |
| VRVDR-41957 |Mineure| Paquets NAT bidirectionnels trop volumineux pour GRE. Echec renvoi ICMP Type 3 Code 4 |
| VRVDR-40283 | Majeure | Les modifications de configuration génèrent beaucoup de messages de journal |
| VRVDR-39773 | Majeure | L'utilisation d'une mappe de route avec la commande BGP vrrp-failover peut provoquer le retrait de tous les préfixes|

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-42505 | N/A | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1 : mise à jour de sécurité xen |
| VRVDR-42427 | N/A | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1 : mise à jour de sécurité xen |
| VRVDR-42383 | N/A | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1 : mise à jour de sécurité libgcrypt20 |
| VRVDR-42088 | 5.5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1 : mise à jour de sécurité xen |
| VRVDR-41924 | 8.8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1 : mise à jour de sécurité xen |

## 5.2R6S12

Publié le 21 juin 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-42084 | Blocage | Interface VFP marquée comme “non-dataplane interface” dans “show dataplane route” lorsque la config NAT/IPSec est réappliquée |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1 : mise à jour de sécurité perl |
| VRVDR-42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1 : mise à jour de sécurité gnupg2 |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1 : mise à jour de sécurité linux |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1 : mise à jour de sécurité linux (Spectre) |

## 1801m

Publié le 15 juin 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-42256 |Critique |Pas de trafic sortant si le dernier élément CHILD_SA établi est supprimé |
| VRVDR-42084 | Blocage | Les sessions NAT liées à des interfaces VFP pour les tunnels PB IPsec ne sont pas créées pour les paquets qui arrivent sur le routeur même si celui-ci est configuré pour cela |
| VRVDR-42018 |Mineure|Lorsque la commande “restart vpn” est exécutée, une erreur “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” est inconnue |
| VRVDR-42017 |Mineure|Lorsque la commande “show vpn ipsec sa” s'exécute sur la sauvegarde VRRP, l'erreur “ConnectionRefusedError” liée à vyatta-op-vpn- ipsec-vici, ligne 563, est émise |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5.4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1 : mise à jour de sécurité perl|
| VRVDR- 42284 | 7.5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1 : mise à jour de sécurité gnupg2|

## 5.2R6S11

Publié le 11 juin 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-42109 |Critique | 1 seul paquet de réponse ICMP reçu avec SNAT+FW sur 5.2R6S7 |
| VRVDR-42084 | Blocage | Les sessions NAT liées à des interfaces VFP pour les tunnels PB IPsec ne sont pas créées pour les paquets qui arrivent sur le routeur même si celui-ci est configuré pour cela |
| VRVDR-42027 | Majeure | SFLOW utilise un index d'interface (ifIndex) d'entrée incorrect |
| VRVDR-41558 | Majeure | Les données d'horodatage signalées dans les traces de paquet ne sont pas cohérentes avec l'heure et l'horloge système réelles |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1 : mise à jour de sécurité wireshark |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639 : exécution spéculative, variante 4 : Speculative Store Bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1 : mise à jour de sécurité procps |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1 : mise à jour de sécurité curl |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1 : mise à jour de sécurité wget |

## 1801k

Publié le 8 juin 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-42084 | Blocage | Les sessions NAT liées à des interfaces VFP pour les tunnels PB IPsec ne sont pas créées pour les paquets qui arrivent sur le routeur même si celui-ci est configuré pour cela |
| VRVDR-41944 | Majeure | Après le basculement VRRP, certains tunnels VTI n'ont pas pu être rétablis jusqu'à ce qu'une commande “vpn restart” ou une réinitialisation d'homologue soit émise|
| VRVDR-41906 | Majeure | Echec de la détection PMTU car des messages ICMP type 3 code 4 sont envoyés à partir d'une adresse IP source incorrecte |
| VRVDR-41558 | Majeure | Les données d'horodatage signalées dans les traces de paquet ne sont pas cohérentes avec l'heure et l'horloge système réelles |
| VRVDR-41469 | Majeure | Une liaison d'interface descendante - la liaison n'achemine pas de trafic |
| VRVDR-41420 | Majeure | Liaison/état de liaison LACP "u/D" avec changement du mode de sauvegarde active en LACP |
| VRVDR-41313 |Critique | Instabilité d'interface IPsec – VTI |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7.5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1 : mise à jour de sécurité wireshark |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639 : exécution spéculative, variante 4 : Speculative Store Bypass / Spectre v4 / Spectre-NG |
| VRVDR- 42006 | 9.8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1 : mise à jour de sécurité procps |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1 : mise à jour de sécurité curl |
| VRVDR- 41795 | 6.5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1 : mise à jour de sécurité wget |

## 1801j

Publié le 18 mai 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41481 |Mineure| VRRP sur l'interface de liaison n'envoie pas d'annonce VRRP |
| VRVDR-39863 | Majeure | VRRP bascule lorsque le client retire une instance de routage à laquelle GRE est associé et que l'adresse locale du tunnel fait partie de VRRP |
| VRVDR-27018 |Critique | Le fichier de configuration active est lisible à l'échelle mondiale |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR-41680 | 7.8 | DSA-4188-1 | Debian DSA-4188-1 : mise à jour de sécurité linux |

## 5.2R6S10

Publié le 17 mai 2018

**Problèmes résolus**
| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41543 | Majeure | La commande “update config-sync” génère des erreurs lorsqu'une barre oblique inversée “\” est utilisée dans les descriptions de configuration
| VRVDR-27018 |Critique | Le fichier de configuration active est lisible à l'échelle mondiale |

## 1801h

Publié le 11 mai 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41664 |Critique | Le plan de données supprime les paquets ESP de taille MTU |
| VRVDR-41536 |Mineure| Limite start-init atteinte pour le service Dnsmasq lors de l'ajout de plus de 4 entrées d'hôte statique si le réacheminement DNS est activé |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7.8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1 : mise à jour de sécurité linux |

## 5.2R6S

Publié le 8 mai 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40803 |Mineure| Les interfaces VIF ne sont pas présentes dans la sortie “show vrrp” après un réamorçage |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1 : mise à jour de sécurité perl |

## 1801g

Publié le 4 mai 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41620 | Majeure | L'interface vTI cesse d'envoyer du trafic après l'ajout d'une nouvelle interface vIF |
| VRVDR-40965 | Majeure | La liaison n'est pas restaurée après une panne de plan de données |

## 1801f

Publié le 23 avril 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41537 |Mineure| Les commandes Ping ne fonctionnent pas sur le tunnel IPsec dans 1801d |
| VRVDR-41283 |Mineure| Configd cesse de traiter les routes statiques durant l'amorçage si la configuration a désactivé les routes statiques |
| VRVDR-41266 | Majeure | La route statique qui fuite vers VRF ne fait pas transiter le trafic par le tunnel mGRE après le réamorçage |
| VRVDR-41255 | Majeure | Lorsqu'un esclave tombe en passe, il faut environ 60 secondes à l'état de la liaison principale pour refléter ce problème |
| VRVDR-41252 | Majeure | Avec une interface VTI non liée dans une stratégie de zone, une règle de suppression est ignorée en fonction de l'ordre de validation des règles de zone |
| VRVDR-41221 |Critique | Mise à niveau des routeurs vRouters depuis 1801b vers 1801c vers 1801d avec un taux d'échec de 10 % |
| VRVDR-40967 | Majeure | La désactivation du réacheminement IPv6 empêche le routage de paquets IPv4 dont la source est l'interface VTI |
| VRVDR-40858 | Majeure | L'interface VTI affiche la MTU 1428, ce qui génère des problèmes TCP PMTU |
| VRVDR-40857 |Critique | Vhost-bridge n'est pas présenté pour un VLAN balisé avec des noms d'interface d'une certaine longueur |
| VRVDR-40803 |Mineure| Les interfaces VIF ne sont pas présentes dans la sortie “show vrrp” après un réamorçage |
| VRVDR-40644 | Majeure | IKEv1: les retransmissions QUICK_MODE ne sont pas correctement gérées |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9.8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1 : mise à jour de sécurité perl |
| VRVDR- 41331 | 6.5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1 : mise à jour de sécurité openssl1.0
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1 : mise à jour de sécurité openssl |
| VRVDR- 41215 | 6.1 |CVE-2018-1059 | CVE-2018-1059 – Accès mémoire d'hôte hors limite pour DPDK vhost à partir d'invités de machine virtuelle|

##5.2R6S8
Publié le 16 avril 2018

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-41283 |Mineure| Configd cesse de traiter les routes statiques durant l'amorçage si la configuration a désactivé les routes statiques |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6.5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1 : mise à jour de sécurité openssl 

##1801e
Publié le 28 mars 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-39985 |Mineure| Les paquets TCP DF plus grands que les MTU de tunnel GRE sont supprimés sans nécessité de fragmentation ICMP renvoyée | 
| VRVDR-41088 |Critique | ASN (4 octets) étendus non représentés en interne en tant que type non signé |
| VRVDR-40988 |Critique | Vhost ne démarre pas lorsqu'il est utilisé avec un certain nombre d'interfaces |
| VRVDR-40927 |Critique | Message DNAT: SDP in SIP 200 OK non traduit lorsqu'il suit une réponse 183 |
| VRVDR-40920 | Majeure | Avec 127.0.0.1 comme adresse d'écoute, snmpd ne démarre pas |
| VRVDR-40920 |Critique | ARP ne fonctionne pas sur une interface SR-IOV liée |
| VRVDR-40294 | Majeure | Le plan de données ne restaure pas les précédentes files d'attente après le retrait d'un esclave du groupe de liaison |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/A | DSA-4140-1 | DSA 4140-1 : mise à jour de sécurité libvorbis |

##5.2R6S7
Publié le 15 mars 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-38801 | Majeure | Un paquet multi-segment reçu via l'interface VTI IPSec provoque une panne de l'interface de liaison 

##5.2R6S6
Publié le 12 mars 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40281 | Majeure | Après la mise à niveau depuis la version 5.2 vers une version plus récente, l'erreur “vbash: show: command not found” est générée en mode opérationnel |
| VRVDR-40135 | Majeure | Les paquets d'arbre maximal ne sont pas reçus sur un port de pont d'interface VIF |
| VRVDR-39991 | Majeure | Le pare-feu avec état déplace des paquets entre deux sous-réseaux sur la même interface | 
| VRVDR-36481 | Majeure | La mise à niveau/rétromigration depuis 5.2R4 vers 17.1.0/5.2R3 génère l'erreur /opt/vyatta/sbin/vyatta-install-image.functions: line 372: is_onie_boot: command not found |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8.8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1 : mise à jour de sécurité libxml2 |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch Target Injection / CVE-2017-5717 / Spectre, aka. Variante #2 |

##1801d
Publié le 8 mars 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40940 | Majeure | Panne du plan de données en raison de NAT/pare-feu |
| VRVDR-40886 | Majeure | La combinaison de “icmp name <value>” avec un nombre d'une autre configuration pour la règle entraîne l'échec du chargement du pare-feu |
| VRVDR-39879 | Majeure | Echec de la configuration de la liaison pour les trames jumbo |

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9.8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1 : mise à jour de sécurité curl |
| VRVDR- 39907 | 7.8 | CVE-2017-5717 | Branch Target Injection / CVE-2017-5715 / Spectre, aka variante #2 |

##1801c
Publié le 7 mars 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40281 | Majeure | Après la mise à niveau depuis la version 5.2 vers une version plus récente, l'erreur “-vbash: show: command not found” est générée en mode opérationnel |

##1801b
Publié le 21 février 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40622 | Majeure | Les images Cloud-init ne sont pas correctement détectées si l'adresse IP a été obtenue à partir du serveur DHCP |
| VRVDR-40613 |Critique | L'interface de liaison n'est pas présentée si l'une des liaisons physiques est arrêtée |
| VRVDR-40328 | Majeure | L'amorçage des images Cloud-init dure longtemps |

##1801a
Publié le 7 février 2018.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-40324 | Majeure | Charges moyennes supérieures à 1.0 sans charge sur routeur avec interface de liaison |

##5.2R6S5
Publié le 19 janvier 2018.

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5.6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1 : mise à jour de sécurité linux (Meltdown) |
| VRVDR- 38265 | 8.8 | DSA-3970-1 | CVE-2017-1 |

##5.2R6S4
Publié le 15 décembre 2017.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-39529 | Majeure | Le basculement de serveur DHCP ne synchronise pas les bases de données |
| VRVDR-39399 |Critique | Vyatta a déposé un état réseau FAULT lors de l'affichage d'erreur vrrp/bagotage de plusieurs interfaces/seg |
| VRVDR-39112 | Majeure | Le trafic DNAT qui correspond à ZBF supprime uniquement le premier paquet du flux |
| VRVDR-38075 |Mineure| Lorsqu'une commande “restart vpn” est émise à partir du répondeur, le demandeur ne rétablir pas la connexion |
| VRVDR-37934 |Critique | BGPd est tombé en panne lorsque aggregate-address summary-only était configuré/que des routes statiques étaient manquantes |
| VRVDR-37717 |Mineure| Renommer les zones “Description” et “License” hard-enf dans la sortie de la version |
| VRVDR-37689 | Majeure | Taux élevé d'interruptions NIC PF |
| VRVDR-37633 |Critique | Blocage de signal de présence |

## 5.2R6S3
Publié le 4 décembre 2017.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-39207 |Critique | Echec d'ARP sur interface VIF de liaison |


##5.2R6S2
Publié le 2 novembre 2017.

**Problèmes résolus**

| Numéro de problème | Priorité | Récapitulatif |
| --- | --- | --- |
| VRVDR-39177 | Majeure | Option domain-name du serveur OpenVPN non appliquée avec –push dhcp-option |
| VRVDR-39129 |Critique | Le paramètre push-route du serveur OpenVPN entraîne l'échec du démarrage de ce dernier |

##5.2R6S1
Publié le 12 octobre 2017.

**Vulnérabilités en matière de sécurité résolues**

| Numéro de problème |Score CVSS| Recommandation | Récapitulatif |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9.8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 : mise à jour de sécurité dnsmasq |

Les informations publiées ici ne constituent pas une offre, un engagement, une représentation ou une garantie d'AT&T et sont susceptibles d'être modifiées. Elles ne sont pas destinées à être utilisées ou divulguées hors des entreprises AT&T sauf dans le cadre d'accords écrites. 

© 2018 AT&T Intellectual Property. All rights reserved. AT&T et le logo Globe sont des marques d'AT&T Intellectual Property. D'autres sociétés sont propriétaires des autres marques
qui pourraient apparaître dans ce document. 
