---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuration d'IPSec sur Vyatta 5400
{: #configuring-ipsec-on-vyatta-5400}

L'unité Brocade 5400 vRouter (Vyatta) est appelée "local" en ce qui concerne le tunnel IPSec (Internet Security Protocol). Chacune des commandes suivantes exécutera des fonctions différentes pour configurer IPSec de site à site. Notez que cet exemple d'IPSec de site à site illustre le tunnel sur le réseau public de SoftLayer ; utilisez **bond0** pour les connexions de site à site IPSec privées.

1. Indiquez au tunnel l'objectif de **interface bond1** :

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Configurez la première phase du tunnel en deux phrases. Les commandes suivantes permettent d'effectuer ce qui suit :

  * Créer un nouveau groupe **ike** nommé **test** et utiliser **dh-group** comme type d'échange de clé.
  * Spécifier le type de chiffrement à utiliser, sinon, l'unité utilisera **aes128** par défaut.
  * Utiliser la fonction de hachage **sha-1**.<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Configurez la seconde phase du tunnel en deux phrases. Les commandes suivantes permettent d'effectuer ce qui suit :

  * Désactiver la confidentialité persistante parfaite (PFS) car les unités ne peuvent pas toutes l'utiliser. (La mention ESP dans la commande correspond à la seconde partie du chiffrement.)
  * Spécifier le type de chiffrement à utiliser, sinon, l'unité utilisera **aes128** par défaut.
  * Utiliser la fonction de `hachage` **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Configurez les paramètres de chiffrement de site à site IPSec. Les commandes suivantes permettent d'effectuer ce qui suit :

  * Spécifier l'adresse IP distante et le fait qu'IPSec utilisera une valeur confidentielle prépartagée.
  * Utiliser l'adresse IP distante et le secret TestPSK
  * Affecter la valeur TestESP au groupe **esp** par défaut pour le tunnel.
  * Indiquer à IPSec qu'il doit utiliser ike-group TestIKE, défini précédemment.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Créez le mappage pour le tunnel IPSec. Les commandes suivantes permettent d'effectuer ce qui suit, en fonction de l'exemple utilisé dans le document :

  * Indiquer au tunnel qu'il doit procéder à un mappage entre l'adresse IP distante 169.54.254.117 et l'adresse IP locale de bond1, 50.97.240.219.
  * Router uniquement des adresses IP avec le sous-réseau 10.54.9.152/29 qui se trouvent sur l'interface de serveur local avec le serveur distant 169.54.254.117.
  * Router le trafic distant 169.54.254.117 du tunnel 1 au sous-réseau distant 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

L'étape suivante consiste à configurer l'unité côté distant, qui est une unité Brocade 5400 vRouter 6.6.5 R.

  * Utilisez l'unité que vous venez de configurer (configurée en mode opérationnel) pour entrer la commande show configuration commands. Une liste de commandes utilisées pour configurer l'unité s'affichera.
  * Copiez les commandes dans un éditeur de texte. Les commandes utilisées pour configurer l'unité locale serviront pour configurer le serveur distant et des modifications seront apportées à l'adresse IP afin qu'elle pointe vers l'unité Brocade 5400 vRouter 6.6.5R sur SoftLayer.

La configuration côté distant utilisée précédemment est présentée ci-dessous. Les modifications nécessaires pour la configuration côté local sont présentées en gras.

Configuration côté distant :

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (L'adresse IP homologue de site à site a été permutée.)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (L'adresse IP locale a été permutée.)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (Le préfixe local et le préfixe distant ont été permutés.)

* Copiez et collez les nouvelles commandes dans le serveur distant (assurez-vous que vous êtes bien en mode configuration), tapez commit, puis sauvegardez.
* Tapez `run show vpn ike sa` pour voir si le tunnel est maintenant établi.

Voici un récapitulatif de ce qui a été fait : router uniquement les adresses IP avec le sous-réseau '10.54.9.152/29' qui résident sur l'interface locale (bond1, 50.97.240.219) aux sous-réseaux 192.168.1.2/32 sur le service distant figurant dans l'interface avec l'adresse IP 169.54.254.117.
