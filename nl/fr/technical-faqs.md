---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: faqs, vlan, traffic, firewall, SSH,

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# Foire aux questions techniques concernant IBM Virtual Router Appliance
{: #technical-faqs-for-ibm-virtual-router-appliance}

La foire aux questions suivante aborde la configuration de l'IBM© Virtual Router Appliance (VRA) et la migration vers le dispositif VRA à partir de Vyatta 5400.

## Comment puis-je autoriser le trafic lié par Internet à partir de systèmes hôte résidant sur un réseau local virtuel privé ?
{: faq}

Ce trafic doit obtenir une adresse IP source publique, par conséquent, une conversion d'adresses réseau source doit usurper l'adresse IP privée avec l'adresse publique du dispositif VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

La configuration ci-dessus effectue uniquement une conversion d'adresses réseau sécurisée à partir du trafic provenant des serveurs du réseau `10.0.0.0/8` privé.

Cela permet de garantir qu'elle n'interférera pas avec les paquets qui possèdent déjà une adresse source routable par Internet.

## Comment puis-je filtrer le trafic lié par Internet et autoriser uniquement des protocoles/destinations spécifiques ?
{: faq}

Il s'agit d'une question courante lorsque la conversion d'adresses réseau source et un pare-feu doivent être combinés.

Gardez à l'esprit l'ordre des opérations dans le dispositif VRA lorsque vous concevez vos jeux de règles.

En résumé, les règles de pare-feu sont appliquées *après* la conversion d'adresses réseau sécurisée.

Pour bloquer l'ensemble du trafic sortant dans un pare-feu, mais autoriser des flux SNAT spécifiques, vous devez déplacer la logique de filtrage vers votre conversion d'adresses réseau sécurisée.

Par exemple, afin d'autoriser uniquement le trafic lié par Internet HTTPS pour un hôte, la règle SNAT doit être la suivante :

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` serait une adresse publique pour le dispositif VRA.

Il est fortement recommandé d'utiliser l'adresse publique VRRP du dispositif VRA, par conséquent, vous pouvez faire la différence entre le trafic public de l'hôte et du dispositif VRA.

Supposons que `150.1.2.3` corresponde à l'adresse VRA VRRP et que `150.1.2.5` soit l'adresse dp0bond1 réelle.

La pare-feu avec état appliqué sur `dp0bond1 out` serait le suivant :

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

Notez que la combinaison de la conversion d'adresses réseau source et du pare-feu permet d'atteindre l'objectif de conception demandé.

Assurez-vous que les règles sont adaptées à votre conception et qu'aucune autre règle ne peut autoriser le trafic qui doit être bloqué.

## Comment puis-je protéger le dispositif VRA proprement dit avec un pare-feu basé sur zone ?
{: faq}

Le dispositif VRA ne possède pas de `zone locale`.

A la place, vous pouvez utiliser la fonctionnalité CPP (Control Plane Policing) car elle est appliquée en tant que pare-feu `local` sur un bouclage.

Notez qu'il s'agit d'un pare-feu sans état et que vous devrez autoriser explicitement le trafic en retour de sessions sortantes provenant du dispositif VRA proprement dit.

## Comment puis-je restreindre SSH et bloquer les connexions provenant d'Internet ?
{: faq}

La meilleure pratique consiste à ne pas autoriser les connexions SSH provenant d'Internet et à utiliser d'autres moyens pour accéder à l'adresse privée, par exemple, SSL VPN.

Par défaut, le dispositif VRA accepte SSH sur toutes les interfaces.
Afin de mettre en place une écoute des connexions SSH uniquement sur l'interface privée, la configuration suivante doit être définie :

```
set service ssh listen-address '10.1.2.3'
```

Gardez à l'esprit que l'adresse IP doit être remplacée par l'adresse appartenant au dispositif VRA.
