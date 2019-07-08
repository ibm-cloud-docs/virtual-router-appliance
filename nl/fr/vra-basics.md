---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: basics, vra, access, configure, gui

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Accès et configuration du dispositif IBM Virtual Router Appliance
{: #accessing-and-configuring-the-ibm-virtual-router-appliance}

Le dispositif VRA peut être configuré à l'aide d'une session de console distante via SSH ou en se connectant à l'interface graphique Web. Par défaut, l'interface graphique Web n'est pas disponible depuis l'Internet public. Pour activer cette interface, connectez-vous d'abord via SSH.

La configuration de VRA hors de son interpréteur de commandes et de son interface peut engendrer des résultats imprévisibles et n'est donc pas recommandée.
{: note}

## Accès à l'unité via SSH
{: #accessing-the-device-using-ssh}

La plupart des systèmes fonctionnant sous UNIX, tels que Linux, BSD et Mac OSX, ont des clients OpenSSH inclus dans leurs installations par défaut. Les utilisateurs Windows peuvent télécharger un client SSH, par exemple PuTTy.

Il est recommandé que la connexion SSH à l'adresse IP publique soit désactivée et que seule la connexion SSH à l'adresse IP privée soit autorisée. Les connexions à des adresses IP privées nécessitent que votre client soit connecté à un réseau privé. Vous pouvez vous connecter à l'aide de l'une des options VPN par défaut (PPTP, SSL-VPN et IPsec) offertes dans le portail client ou à l'aide d'une solution VPN personnalisée configurée sur le dispositif VRA.

Utilisez le compte Vyatta indiqué à la page **Détails de l'unité** pour vous connecter via SSH. Le mot de passe root est également fourni, mais la connexion root est désactivée par défaut pour des raisons de sécurité.

`ssh vyatta@[IP address] `

Il est recommandé de maintenir les connexions root désactivées. Connectez-vous à l'aide du compte Vyatta et n'utilisez root qu'en cas de nécessité.
{: tip}

Des clés SSH peuvent également être fournies lors du déploiement, pour éviter de passer par un compte Vyatta pour se connecter. Après avoir vérifié que vous pouvez accéder à votre système VRA avec votre clé SSH, vous pouvez désactiver les connexions à l'aide d'un utilisateur et d'un mot de passe standard en exécutant les commandes suivantes :

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Accès à l'unité à l'aide de l'interface graphique Web
{: #accessing-the-device-using-the-web-gui}

Connectez-vous à VRA à l'aide des instructions SSH ci-dessus, puis exécutez les commandes suivantes pour activer le service HTTPS :

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

Une fois ces commandes terminées, entrez `https://<ip.address>` dans la barre d'adresse de votre navigateur, en remplaçant l'adresse IP par celle de votre VRA. Vous serez peut-être invité à accepter le certificat auto-émis par VRA. Pour cela, à l'invite, connectez-vous à l'interface Web avec vos données d'identification 'vyatta'.

## Modes
{: #modes-1}

**Mode configuration :** Appelé avec la commande `configure`, ce mode est utilisé pour effectuer la configuration du système VRA.

**Mode opérationnel :** Mode initial lors de la connexion à un système VRA. Dans ce mode, les commandes `show` peuvent être exécutées pour demander des informations sur l'état du système. Le système peut également être redémarré dans ce mode.

L'interpréteur de commandes (shell) de VRA est une interface modale, avec plusieurs modes de fonctionnement. Le mode principal/par défaut est **Opérationnel**, et il s'agit du mode présenté lors de la connexion. En mode **Opérationnel**, vous pouvez afficher des informations et exécutez des commandes ayant un impact sur l'exécution en cours du système, par exemple le réglage de la date et de l'heure ou le redémarrage de l'unité.

La commande `configure` fait passer l'utilisateur en mode **Configuration**, dans lequel il est possible d'effectuer des modifications de configuration. Notez que les changements de configuration ne sont pas appliqués immédiatement, ils doivent être d'abord validés et sauvegardés. Lorsque les commandes sont émises, elles passent par une mémoire tampon de configuration. Une fois toutes les commandes nécessaires saisies, exécutez la commande `commit` pour que les changements prennent effet.

Pour sauvegarder les commandes de manière permanente, exécutez la commande `save` après la commande `commit`.

Les commandes du mode opérationnel peuvent être exécutées en mode Configuration en démarrant la commande par `run`. Par exemple :


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

La marque de hachage (`#`) indique le mode Configuration. Commencer la commande par `run` indique à l'interpréteur de commandes VRA qu'il s'agit d'une commande opérationnelle. L'exemple précédent illustre également la possibilité de faire des recherches avec "grep" sur la sortie des commandes.

## Exploration des commandes
{: #command-exploration}

L'interpréteur de commandes VRA comprend des fonctions de saisie semi-automatique via la touche TAB. Si vous voulez connaître les commandes disponibles, appuyez sur la touche TAB pour obtenir la liste, ainsi qu'une brève explication. Vous pouvez le faire à l'invite de l'interpréteur de commandes ou en tapant une commande. Par exemple :  

```
$show log dns [Appuyez sur Tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Exemple de configuration
{: #sample-configuration}

Les configurations sont agencées dans un modèle de noeuds hiérarchique. Prenez par exemple ce bloc de commandes static route :

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

Ce résultat peut être généré par les commandes :

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

Cet exemple montre qu'il est possible qu'un noeud (static) ait plusieurs noeuds enfant. Pour supprimer la route correspondant à `192.168.1.0/24`, la commande `delete protocols static route 192.168.1.0/24` sera utilisée. Si `192.168.1.0/24` ne figure pas dans la commande, les deux noeuds de route sont marqués pour suppression.

N'oubliez pas que la configuration n'est réellement modifiée qu'une fois la commande `commit` émise. Pour comparer la configuration en cours aux changements présents dans la mémoire tampon de configuration, utilisez la commande `compare`. Pour vider la mémoire tampon de configuration, utilisez la commande `discard`.

## Contrôle d'accès RBAC (Users and Role-based Access Control)
{: #users-and-role-based-access-control-rbac-}

Les comptes utilisateur peuvent être configurés avec trois niveaux d'accès :

* Administrateur
* Opérateur
* Superutilisateur

Les utilisateurs avec le niveau Opérateur peuvent exécuter des commandes `show` pour afficher le statut du système et émettre des commandes `reset` permettant de redémarrer les services sur l'unité. Les droits de niveau Opérateur n'impliquent pas l'accès en lecture seule.

Les utilisateurs avec le niveau Administrateur ont l'accès complet à toutes les configurations et opérations de l'unité. Ils peuvent afficher les configurations en cours, modifier les paramètres de configuration de l'unité, ainsi que redémarrer et arrêter l'unité.

Les utilisateurs avec le niveau Superutilisateur sont en mesure d'exécuter des commandes avec des privilèges de superutilisateur (root) via la commande `sudo` et en plus des privilèges de niveau admin.

Les utilisateurs peuvent être configurés pour les styles d'authentification par mot de passe et/ou clé publique. L'authentification par clé publique est utilisée avec SSH et permet aux utilisateurs de se connecter à l'aide d'un fichier de clés sur leur système. Pour créer un utilisateur de niveau Opérateur avec un mot de passe :

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

Lorsqu'aucun niveau n'est spécifié, l'utilisateur est considéré être au niveau administrateur. Dans ce cas, les mots de passe de l'utilisateur apparaissent chiffrés dans le fichier de configuration.
{: note}

Le contrôle d'accès RBAC est une méthode utilisée pour limiter l'accès à une partie de la configuration à des utilisateurs autorisés. RBAC permet aux administrateurs de définir des règles pour un groupe d'utilisateurs qui limitent les commandes qu'ils peuvent exécuter.

RBAC s'exécute en créant un groupe affecté au jeu de règles Access Control Management (ACM), en ajoutant un utilisateur au groupe, en créant un jeu de règles mettant en correspondance ce groupe avec les chemins dans le système, puis en configurant le système pour autoriser ou refuser l'accès aux chemins appliqués à ce groupe.

Par défaut, il n'y a aucun jeu de règles ACM défini dans Virtual Router Appliance et ACM est désactivé. Si vous voulez utiliser RBAC pour fournir un contrôle d'accès granulaire, vous devez activer ACM et ajouter les règles ACM par défaut suivantes en plus des règles que vous avez définies :

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

Reportez-vous à la [documentation supplémentaire](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation) avant toute tentative d'activation des règles ACM. Des paramètres de règle ACM inexacts peuvent entraîner des accès refusés à l'unité ou des erreurs rendant le système inexploitable.
