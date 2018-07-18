---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Comment se connecter à un système Vyatta

Brocade NFV (Network Functions Virtualization) est accessible à l'aide d'une interface graphique Web ou directement avec une connexion SSH.

## S'authentifier auprès du réseau privé de SoftLayer

1. Entrez l'adresse IP privée à partir de l'écran **Détails** (menu **Réseau > Dispositifs de passerelle**) dans votre navigateur en utilisant le préfixe https:// (par exemple, https://10.54.207.131/). Sachez que vous risquez de recevoir une erreur de certificat car l'appareil n'utilise pas un certificat public. Acceptez l'erreur pour pouvoir accéder à la page Web. 

2. Entrez les données d'identification à partir de l'onglet **Mot de passe** situé sous le menu **Unités** dans le portail Web pour l'utilisateur **vyatta**. Vous accédez au tableau de bord Brocade. 
