---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: faq, faqs, questions, gateway, vrouter, vyatta, 5400, 5600, password, traffic, firewall, provision, login, ha, high availability

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

# Foire aux questions concernant IBM Virtual Router Appliance
{: #faqs-for-ibm-virtual-router-appliance}

Cette section présente les questions courantes sur l'utilisation d'IBM© Virtual Router Appliance (VRA).

## Qu'est-ce qu'un dispositif VRA ?
{: faq}

Virtual Router Appliance (VRA) permet à un client d'IBM Cloud de router de manière sélective le trafic réseau privé et public via un routeur d'entreprise doté de fonctions complètes avec pare-feu, régulation de flux, routage basé sur des règles, VPN et un hôte avec d'autres fonctions. Toutes les fonctions VRA sont gérées par les clients. VRA offre à un client d'IBM Cloud un degré de contrôle habituellement réservé aux réseaux sur site.

## Qu'est-ce qu'un dispositif de passerelle ?
{: faq}

Un dispositif de passerelle vous permet d'utiliser le portail Web ou l'API permettant de choisir des segments de réseau (VLAN) à router via VRA. Vous pouvez modifier les sélections de réseaux locaux virtuels (VLAN) à tout moment. Le dispositif de passerelle traite également la haute disponibilité (HA) de VRA, en configurant un deuxième dispositif VRA qui prendra le relais en cas de défaillance de la première.

## Je vois parfois des références à des termes, tels que "Vyatta" et "vRouter." Quel est leur lien avec VRA ?
{: faq}

Vyatta est un logiciel de routage open source pour PC qui a été racheté et qui est devenu une source fermée. Aujourd'hui, "Vyatta" et "Vyatta OS" se réfèrent à des adaptations logicielles commerciales dérivées de ce projet à source fermée. IBM VRA incorpore des éléments de Vyatta OS, ainsi que des améliorations substantielles de fonctions et de services disponibles exclusivement via IBM Cloud.

"vRouter" est une autre appellation de Vyatta créée par son ancien propriétaire mais qui n'a pas duré. Lorsque vous l'apercevez dans la documentation, elle peut être considérée comme synonyme de Vyatta.

## Vyatta 5400 est-il toujours pris en charge ?
{: faq}

IBM ne prendra plus en charge Vyatta 5400 à compter du 31 mars 2019.

## Quelles sont les améliorations de Virtual Router Appliance (Vyatta 5600) par rapport à Vyatta 5400 ?
{: #what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-}

Vyatta 5600 offre les améliorations suivantes par rapport à Vyatta 5400 :

- Un débit plus rapide jusqu'à 10 Gbit/s par coeur d'UC
- Un débit au moins trois fois plus important par session VPN IPsec (normes AES (Advanced Encryption Standards))
- Une capacité accrue pour les routeurs, les flux et les connexions simultanées
- Un support de norme mis à jour comprenant Layer 2 Tunneling Protocol, Version 3 (L2TPv3), Internet Key Exchange, Version 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2), et l'encapsulation 802.1Q tunneling (Q-in-Q)

## Qu'est-il advenu de l'offre AT&T vRouter 5600 ?
{: faq}

AT&T (anciennement Brocade) a annoncé la fin de vie et la fin du support de l'offre Brocade vRouter 5600. Bien que Brocade vRouter 5600 fournit la capacité technologique sous-jacente d'IBM Virtual Router Appliance, cette annonce ne s'applique pas aux clients IBM. Les clients IBM continueront à recevoir du support avec cette nouvelle offre.

## Comment est fourni VRA ?
{: faq}

Vous obtenez un dispositif VRA en commandant une passerelle réseau. Ce processus rationalisé vous permet de choisir un centre de données et un serveur VRA adapté, et de déterminer si vous souhaitez déployer une paire HA d'unités VRA. Les serveurs, les systèmes d'exploitation et l'installation du dispositif de passerelle sont fournis automatiquement. Lorsqu'ils sont mis à disposition, vous pouvez utiliser l'interface du dispositif de passerelle pour router les réseaux locaux virtuels (VLAN) via le dispositif VRA. Vous pouvez configurer votre serveur VRA directement à l'aide de SSH (Secure Shell) avec les mots de passe fournis dans la section des détails du matériel sur le portail client.

## Mon mot de passe est-il sûr ?
{: faq}

Oui. Les mots de passe aléatoires affectés aux unités VRA ne sont visibles que par le titulaire du compte. Les mots de passe sont facilement modifiés, tout comme les clés publiques SSH et les restrictions d'accès IP admin.

## Puis-je obtenir un dispositif VRA sans dispositif de passerelle ?
{: faq}

Oui, mais elle ne pourra gérer que le trafic entre les interfaces publiques et privées du dispositif VRA. Les réseaux locaux virtuels (VLAN) et la haute disponibilité (HA) nécessitent l'installation d'un dispositif de passerelle.

## Est-ce que tout le trafic réseau transite via le dispositif VRA ?
{: faq}

Non. Le dispositif de passerelle vous permet de sélectionner les segments de réseau public et privé (VLAN) que vous souhaitez router via le dispositif VRA. Vous pouvez modifier et ignorer les sélections de VLAN à tout moment. VRA vous permet aussi de définir des règles d'adresse IP qui s'appliquent à des sous-réseaux ou des plages d'adresses IP. Les règles de ce type fonctionnent uniquement si les VLAN contenant ces sous-réseaux sont routés via le dispositif VRA.

## Un dispositif VRA ou un pare-feu dédié peuvent-ils empêcher la mise à disposition de nouveaux serveurs ?
{: faq}

Oui. Dans la mesure du possible, vous ne devez pas verrouiller votre réseau tant que les serveurs que vous envisagez d'utiliser ne sont pas mis à disposition.

Il est interdit au support IBM d'examiner ou de modifier une configuration de VRA ou de pare-feu dédié sans l'implication explicite du client, par conséquent, dans la plupart des cas, le support ne peut pas savoir qu'un dispositif VRA est responsable de l'échec ou du blocage lié à la mise à disposition des serveurs.

Il incombe au client de s'assurer que le dispositif VRA ou le pare-feu est configuré pour autoriser la mise à disposition automatique des serveurs avant la commande des serveurs. Il incombe au client de résoudre les problèmes de mises à disposition bloquées par un dispositif VRA ou un pare-feu géré par un client. De tels retards de mise à disposition n'entrent pas dans les contrats SLA ou ne peuvent pas faire l'objet de crédits. Les systèmes commandés peuvent être retournés (une fois les données du client détruites) si le client tarde à répondre.

De la même façon, si un dispositif VRA ou un pare-feu est ignoré une fois la commande effectuée, la commande risque de ne pas aboutir. Il peut y avoir un cours laps de temps au cours duquel le processus d'automatisation effectue une nouvelle tentative. Il est préférable que le processus complet de mise à disposition se poursuive sans perturbation du réseau.

## Quels produits de pare-feu sont offerts par IBM ?
{: faq}

Vous trouverez une comparaison détaillée de tous les produits de pare-feu offerts dans IBM Cloud en lisant [cette rubrique](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-exploring-firewalls).

## Un dispositif VRA peut-elle entraver le support client ?
{: faq}

Oui, pour les raisons décrites ci-dessus. VRA est une "boîte noire" : les VLAN entrent et sortent et IBM n'a aucune idée de ce que font les clients avec les paquets intermédiaires.

Le support fait de son mieux, mais avec VRA et un pare-feu dédié : a) la confidentialité du client l'emporte sur la connectivité et b) notre personnel de support standard n'est pas équipé pour analyser des configurations de VRA/pare-feu ultra complexes ou incorrectes.

En guise de première étape de diagnostic, nous pouvons vous demander de mettre vos réseaux locaux virtuels (VLAN) VRA ou de pare-feu en mode contournement. Si, dans cet état, les mises à disposition qui ont échoué aboutissent, il est à supposer que le problème provient de la configuration de votre VRA/pare-feu.

## Quel effet aura VRA sur les performances réseau ?
{: faq}

N'oubliez pas que même si ce n'est pas visible, un cloud public partage des réseaux avec d'autres clients. Dans le meilleur des cas, le débit VRA est déterminé par la capacité du réseau disponible à moment donné, plus la distance que doivent parcourir les données.

Hormis ces variables, VRA est capable de transférer 80 Gbit/s de trafic non modifié sur plusieurs interfaces, à l'aide d'une formule approximative selon laquelle tous les 10 Gbit/s de débit nécessitent un coeur de processeur complet (hors Hyperthread). Etant donné que les serveurs actuels ont une capacité maximale de 40 Gbit/s (2 x 10 Gbit/s (public) + 2 x 10 Go (privé)), un serveur avec 8 coeurs ou plus devrait avoir suffisamment de marge de calcul pour traiter plusieurs fonctions VRA courantes avec des performances réseau quasi optimales

## Que faire en cas de perte de mon mot de passe VRA ?
{: faq}

Si vous avez accès au système, définissez un nouveau mot de passe en exécutant la commande suivante :

```
set system login user [account] authentication plaintext-password [password]  
```

Si vous n'avez pas accès au système, vous pouvez redémarrer l'unité et utiliser l'option de récupération de mot de passe dans le menu GRUB pour réinitialiser le mot de passe de l'utilisateur root.

## Que faire si le pare-feu a verrouillé mon accès ?
{: faq}

La construction `reboot at [time]` peut être utile lors du test de règles de pare-feu potentiellement dangereuses.

Si la règle fonctionne, utilisez la commande `reboot cancel` pour annuler le redémarrage. Si la règle bloque votre accès, attendez le redémarrage programmé.

S'il n'y a aucun accès possible au système, un redémarrage peut être utilisé pour rétablir l'accès. Lors du redémarrage, le système lit le fichier de configuration qui n'a pas changé depuis la suppression des entrées précédentes.

Si l'accès est possible à l'aide de l'interface IPMI, vous pouvez effectuer les actions suivantes pour rétablir l'accès :

1. Désactivez la règle incriminée en exécutant :

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Détachez le jeu de règles nommé complet de l'interface nécessaire en exécutant :

	```
	delete interfaces dataplane [interface] firewall [type][firewall name]
	commit
	```

**REMARQUE :** Une utilisation incorrecte de ces commandes peut effacer complètement la configuration de votre interface.

## Pourquoi voudrais-je exécuter deux périphériques Vyatta dans une Paire à haute disponibilité ?
{: faq}

La plupart des clients de cloud sollicitent des services à haute disponibilité (HA). Ainsi, votre charge de travail est hébergée sur deux machines (matérielles) (ou plus) complètement séparées, voire mieux, dans deux zones de disponibilité distinctes (pensez aux centres de données), de sorte que si l'une tombe en panne, l'autre puisse continuer le service. Si une machine tombe en panne, il se produit un basculement vers l'autre machine, ce qui signifie que le service peut continuer à fonctionner. Ce type de fonctionnement est appelé service à haute disponibilité (HA), à savoir le service est presque toujours disponible. 

## Comment activer les connexions root sur le dispositif VRA ?
{: faq}

Pour activer l'accès des utilisateurs root via SSH, exécutez la commande suivante :

`set service ssh allow-root`

Remarque : le fait d'accorder des droits d'accès root via SSH est considéré comme peu sûr. L'autre solution pour accéder à un shell root consiste à se connecter avec un autre ID utilisateur et à s'élever au niveau root localement avec `su -` ou à permettre aux superutilisateurs d'exécuter des commandes sudo.

Par exemple, pour configurer le dispositif vyatta en tant que superutilisateur :

`set system login vyatta level superuser`
