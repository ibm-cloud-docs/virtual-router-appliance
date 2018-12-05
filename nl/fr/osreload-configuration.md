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

# Rechargement du système d'exploitation
Le processus de rechargement du système d'exploitation, lorsqu'il est lancé sur le portail client, efface complètement toutes les configurations présentes sur l'unité et restaure la configuration d'origine sur l'unité. Tout changement effectué depuis le dernier chargement du système sera effacé. Par conséquent, si vous avez effectué des modifications (par exemple, pour le groupe VRRP sur une autre machine), il y aura sans doute un conflit sur la machine rechargée lorsqu'elle sera à nouveau en ligne.

Pour recharger votre système d'exploitation, procédez comme suit :

1. Connectez-vous au [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){: new_window} à l'aide de vos identifiants uniques.
2. Sélectionnez **Liste des unités** dans la liste déroulante Unités.
3. Cliquez sur le serveur que vous souhaitez recharger.
4. Sélectionnez **Rechargement du système d'exploitation** dans le menu déroulant **Actions** situé en haut à gauche de la page.
5. Sur l'écran Rechargement du système d'exploitation, cliquez sur **Editer** pour la catégorie qui nécessite une mise à jour. Sélectionnez **AT&T** comme fournisseur et la version de système d'exploitation que vous souhaitez recharger. 
6. Cliquez sur le bouton **Recharger configuration ci-dessus** pour accéder à la fenêtre contextuelle **Vérifier**. Cliquez sur **Annuler** pour annuler les modifications de l'unité et quitter l'écran.
7. Vérifiez que les informations s'affichant dans la section Nouvelle configuration sont correctes. Cliquez sur **Suivant** pour passer à la fenêtre contextuelle de confirmation.
8. Cliquez sur le bouton **Confirmer le rechargement du système d'exploitation** pour confirmer le rechargement et effectuer cette opération. Cliquez sur **Annuler** pour annuler l'action.

Après avoir lancé le processus de rechargement du système d'exploitation, l'unité est déconnectée et le processus de rechargement du système d'exploitation commence. La durée de rechargement d'un système d'exploitation varie en fonction de la configuration actuelle et de la nouvelle configuration de l'unité. Tout au long du processus de configuration, la durée minimale de rechargement du système d'exploitation s'affiche sur chaque écran. Le délai est une estimation indiquée par le système. Si le rechargement dure plus de 24 heures, contactez notre équipe de support. Lorsque l'unité est à nouveau en ligne, elle fonctionnera selon la nouvelle configuration indiquée pour le rechargement du système d'exploitation. 

**REMARQUE :** Toutes les données qui avaient été préalablement sauvegardées sur l'unité seront perdues, mais pourront être restaurées si une sauvegarde de l'unité a été effectuée avant son rechargement. Si les données n'ont pas été sauvegardées, elles ne seront plus récupérables.
