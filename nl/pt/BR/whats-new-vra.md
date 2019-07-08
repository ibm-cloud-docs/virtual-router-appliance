---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: updates, changes, additions

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


# Atualizações recentes para o IBM Virtual Router Appliance
{: #recent-updates-for-ibm-virtual-router-appliance}

Descubra recursos novos e atualizados no IBM© Virtual Router Appliance (VRA).

## Agosto de 2018
{: #august-2018}

### Sistema operacional Brocade Versão 18.x
{: #brocade-operating-system-version-18-x}

A versão 18.x do S.O. Brocade agora está disponível para o Virtual Router Appliance. Entre outros novos recursos, essa versão fornece correção para a violação de segurança do Spectre.

Novos recursos do VRA 18.x são discutidos nos tópicos a seguir:

* [Configurando um túnel de IPsec que funciona com firewalls de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls)
* [Configurando uma interface VFP com IPsec e firewalls de zona](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)
* [Usando a NAT com IPsec baseado em prefixo](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-using-nat-with-prefix-based-ipsec)
* [Resolução de problemas da interface VFP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-troubleshooting-your-vfp-interface)

Se você está migrando do Vyatta 5400, a melhor maneira de fazer upgrade para a 18.x é por meio do [procedimento normal](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) de um recarregamento completo do S.O.

Como não há um mapeamento simples de um para um de funcionalidade entre o Vyatta 5400 e o Virtual Router Appliance, a criação de uma configuração de linha de base para o VRA é útil. Um Parceiro IBM, WanClouds, pode ajudá-lo com esse processo e fornecer orientação sobre a criação de funcionalidade semelhante ao Vyatta 5400 em seu VRA.

Para obter mais informações sobre os problemas comuns encontrados durante esse processo de upgrade, consulte a nossa [documentação adicional](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
