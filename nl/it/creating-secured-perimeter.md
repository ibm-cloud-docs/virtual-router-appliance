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

# Creazione di un perimetro protetto
Un aspetto fondamentale dell'isolamento di rete è l'istituzione di un perimetro protetto.  Un perimetro protetto controlla il traffico tra internet pubblico e gli asset cliente ospitati in IBM© Cloud.  Il perimetro protetto abiliterà anche una connettività diretta all'azienda del cliente mediante l'utilizzo di tunnel VPN (Virtual Private Network) e IBM Cloud Direct Link.

Un perimetro protetto utilizza un SPS (Secure Perimeter Segment) per separare le reti tra gli asset all'interno del perimetro protetto. Questa separazione offre diversi vantaggi, compresi il controllo dell'accesso e l'isolamento del traffico di servizio tra i segmenti. Un SPS (Secure Perimeter Segment) comprende due VLAN (Virtual Local Area Network), una VLAN front-end e una VLAN back-end e una VRA connessa alle VLAN per gestire il traffico in entrata e in uscita dall'SPS. Un perimetro protetto può includere più SPS (Secure Perimeter Segment) (ad esempio per scopi ad alta disponibilità).

## Configurazione del perimetro protetto

Sono qui di seguito indicati i passi per configurare un perimetro protetto.  Per una descrizione dettagliata di questi passi, vedi [Set up a Secure Perimeter in the IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter){:new_window}.

1. Configura le autorizzazioni in IBM Cloud e nell'infrastruttura necessarie per accedere ai servizi cloud e ai componenti dell'infrastruttura coinvolti nel perimetro protetto.
2. Crea un limite esterno in internet pubblico tramite una VLAN pubblica e un firewall. Questo limite esterno verrà utilizzato per isolare l'SPS.
3. Colloca l'SPS all'interno di quel limite
4. Configura un gateway per il bridging di VLAN pubblica e SPS
5. Crea e configura una VRA ({{site.data.keyword.vra_full}})
6. Crea e configura uno o più SPS (Secure Perimeter Segment)
7. Crea una VLAN front-end
8. Crea una VLAN back-end
9. Crea una coppia Vyatta
10. Configura Vyatta
11. Associa le VLAN a un gateway
12. Configura l'interfaccia gateway per la VLAN pubblica
13. Configura il gateway sul master Vyatta
14. Configura il gateway sul backup Vyatta
15. Configura l'interfaccia gateway per la VLAN privata
16. Configura il gateway sul master Vyatta
17. Configura il gateway sul backup Vyatta
18. Abilita l'ottimizzazione della rete
19. Configura le regole del firewall
20. Configura il cluster Kubernetes
21. Configura gli IP forniti dall'utente (facoltativo).
22. Distribuisci il cluster Kubernetes.

## Soluzioni dedicate in IBM Cloud
Un perimetro protetto, insieme all'isolamento del calcolo e alla crittografia dei dati, contribuisce a una soluzione dedicata completa in IBM Cloud pubblico.  Vedi [Implementing a Dedicated Solution Pattern ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/){:new_window} per una descrizione dei modelli di soluzione dedicata cloud.
