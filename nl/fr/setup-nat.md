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

# Configuration de règles NAT sur Vyatta 5400
Cette rubrique contient des exemples de règles NAT utilisées sur un système Vyatta.

## Règle NAT un à plusieurs (usurpation)

Entrez les commandes suivantes à l'invite :

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

Les demandes de connexion à partir de machines du réseau `10.xxx.xxx.xxx` sont mappées à l'adresse IP sur bond1 et reçoivent un port temporaire associé lorsqu'elles sortent. L'objectif est d'affecter des numéros de règle d'usurpation un à plusieurs plus élevés de sorte qu'ils n'entrent pas en conflit avec les règles NAT inférieures que vous possédez éventuellement.

**REMARQUE :** vous devez configurer le serveur pour qu'il transmette son trafic Internet via le dispositif VRA de sorte que sa passerelle par défaut corresponde à l'adresse IP privée du réseau local virtuel géré. Par exemple, pour `bond0.2254`, la passerelle est `10.52.69.201`. Il doit s'agir de l'adresse de passerelle du serveur qui transmet le trafic Internet.

**REMARQUE :** utilisez la commande suivante pour vous aider à traiter les incidents liés à NAT : 

'''
run show nat source translations detail 
'''

## Règle NAT un à un

Les commandes ci-dessous montrent comment configurer une règle NAT un à un. Notez que les numéros de règle sont configurés de manière à être inférieurs à la règle d'usurpation. Il s'agit de faire en sorte que les règles un à un soient prioritaires par rapport aux règles un à plusieurs.

**REMARQUE :** les adresses IP qui sont mappées un à un ne peuvent pas faire l'objet d'une usurpation. Si vous convertissez une adresse IP entrante, vous devez convertir cette adresse IP sortante de sorte que le trafic puisse être acheminé dans les deux sens.

Les commandes ci-après s'appliquent à une règle source et à une règle de destination. Tapez `show nat` en mode configuration pour voir le type de règle NAT.

**REMARQUE :** utilisez la commande suivante pour vous aider à traiter les incidents liés à NAT : `run show nat source translations detail`. 

Entrez les commandes suivantes à l'invite après avoir vérifié que vous êtes bien en mode configuration :

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

Si le trafic entre sur l'adresse IP `50.97.203.227` sur bond1, cette adresse IP sera mappée à l'adresse IP `10.52.69.202` (sur n'importe quelle interface définie). Si le trafic sort avec l'adresse IP `10.52.69.202` (sur n'importe quelle interface définie), il sera converti en adresse IP `50.97.203.227` et sortira sur l'interface bond1.

**REMARQUE :** les adresses IP qui sont mappées un à un ne peuvent pas faire l'objet d'une usurpation. Si vous convertissez une adresse IP entrante, vous devez convertir cette même adresse IP sortante de sorte que son trafic puisse être acheminé dans les deux sens.


## Ajout de plages d'adresses IP via votre dispositif VRA

Selon votre configuration VRA, vous souhaiterez peut-être accepter des adresses IP IBM Cloud spécifiques. 

De nouveaux déploiements vRouter sont fournis avec les adresses IP de réseau des services IBM Cloud définies dans une règle de pare-feu appelée `SERVICE-ALLOW`.

Voici un exemple de `SERVICE-ALLOW`. Il ne s'agit pas d'un jeu de règles d'adresse IP privée complet.

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

Maintenant que vous avez défini les règles de pare-feu, vous pouvez les affecter comme bon vous semble. Deux exemples sont présentés ci-dessous. 

Application à une zone :

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Application à une interface de liaison :

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
