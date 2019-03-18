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

# Traitement des incidents liés à votre interface VFP
{: #troubleshooting-your-vfp-interface}

Cette rubrique contient des informations de traitement des incidents pour les interfaces VFP.

* Une interface VFP n'est pas une interface "réelle", telle que `dp0bond0` (ni même une interface VIF ou un tunnel). Il s'agit d'une marque de réservation pour le pare-feu et les processus NAT afin de bloquer une interface pour qu'ils puissent prendre en charge le trafic correctement. Vous pouvez toujours acheminer le trafic via une interface VFP telle qu'une interface classique, mais `tshark` et d'autres commandes de surveillance n'afficheront aucun trafic.
* Avec la conversion NAT, vous devez utiliser une plage de sous-réseaux plus spécifique pour que le trafic soit acheminé vers l'interface VFP, au lieu d'utiliser la route du noyau qui est créée par IPsec. Si une route statique n'est pas définie, la route du noyau sera suivie. Vous pouvez tester cela en utilisant `show ip route x.x.x.x`.
* La conversion DNAT doit être traitée correctement pour le trafic sortant de l'interface VFP, mais une route statique doit tout de même être définie pour le trafic renvoyé. Recherchez l'en-tête de trafic non NAT hors de l'interface IPsec, `dp0bond1` ou `dp0bond0` (ou n'importe quelle interface utilisant le trafic IPsec).
* L'utilisation de protocoles de routage sur une interface VFP n'est pas testée. 
* L'utilisation d'un tunnel GRE sur une interface VFP n'est pas testée non plus.
