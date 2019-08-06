---

copyright:
  years: 2017
lastupdated: "2019-03-07"

keywords: 5400, 5600, migration, overview, upgrade, brocade

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:note: .note}

# Visão geral da migração
{: #migration-overview}

Os clientes Vyatta 5400 anteriores devem migrar para o {{site.data.keyword.vra_full}} (VRA) assim que possível, já que o suporte para o Vyatta 5400 será removido a partir de 31 de março de 2019. Para ler o comunicado de fim de suporte completo e para obter mais informações, clique [aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement).
{: important}

## Como o upgrade pode beneficiar você
{: #how-upgrading-can-benefit-you}

Além de uma variedade de novos recursos e capacidades que o {{site.data.keyword.vra_full}} apresenta, há também várias melhorias que ele fornece também. Consulte nossas [Perguntas mais frequentes](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-faqs-for-ibm-virtual-router-appliance#what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-) para obter mais detalhes.

O upgrade para um novo dispositivo terá uma configuração diferente do Vyatta 5400. Como resultado, uma configuração do Vyatta 5400 (arquivo) não será transferida para seu novo dispositivo.
{: note}

## Documentação de migração
{: #migration-documentation}

Para ajudá-lo a migrar da sua Vyatta 5400, nós preparamos a documentação e o suporte a seguir:

| Documentação de migração | Descrição |
| ------------- | ------------- |
| [Fazendo upgrade do Vyatta 5400 e reutilizando seu endereço IP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-vyatta-5400-and-reusing-its-ip-addresses) | Instruções sobre como fazer upgrade de seu Vyatta 5400 para um {{site.data.keyword.vra_full}} equivalente, enquanto reutiliza seus endereços IP do Vyatta 5400. |
| [Migrando um Vyatta 5400 para um Juniper vSRX ou para um Fortigate Security Appliance (FSA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps) | Instruções sobre como migrar para um Juniper vSRX ou para o Fortigate Security Appliance. Essa opção não permitirá que você reutilize seu dispositivo Vyatta 5400 existente nem mantenha seus endereços IP associados. |
| [Problemas comuns de migração](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)  | Informações sobre os problemas encontrados mais frequentemente ou mudanças de comportamento podem ser encontradas após a migração de um dispositivo Vyatta 5400 para um {{site.data.keyword.vra_full}}. Em muitos casos, isso inclui soluções alternativas para tratar desses problemas. |

Se você precisar simplesmente pedir um novo {{site.data.keyword.vra_full}}, [clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started) para obter mais detalhes.
{: note}
