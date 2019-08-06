---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, 5600, migrate, upgrade, migration, ip, standalone, ha

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Fazendo upgrade do Vyatta 5400 e reutilizando seus endereços IP
{: #upgrading-the-vyatta-5400-and-reusing-its-ip-addresses}

Essa opção permite reutilizar o dispositivo Vyatta 5400 existente como um {{site.data.keyword.vra_full}} (VRA) equivalente e manter seus endereços IP associados. Os procedimentos a seguir fornecem instruções para fazer upgrade de um Vyatta 5400 independente ou dois dispositivos Vyatta 5400 operando em um par de Alta disponibilidade (HA).

## Fazendo upgrade de um Vyatta 5400 independente
{: #upgrading-a-stand-alone-vyatta-5400}

Para fazer upgrade de um único Vyatta 5400 para um {{site.data.keyword.vra_full}}, execute o procedimento a seguir:

1. Assegure-se de que tenha feito backup de seu 5400 e armazenado os dados em dois locais diferentes. Isso inclui as chaves SSH, os certificados SSL, os scripts e quaisquer outros arquivos necessários para recuperar sua configuração do Vyatta 5400 atual, se necessário.
2. Recarregue seu Vyatta 5400 para um {{site.data.keyword.vra_full}} padrão usando as instruções [neste tópico](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-reloading-the-os).

  Um recarregamento de S.O. gerará uma nova senha para os IDs de usuário raiz e Vyatta.
  {: note}

4. Anote a nova senha do {{site.data.keyword.vra_full}}.
5. Configure o VRA recém-recarregado com suas configurações desejadas usando [estas instruções](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).

## Fazendo upgrade de um par de Alta disponibilidade Vyatta 5400
{: #upgrading-a-vyatta-5400-high-availability-pair}

Para fazer upgrade de dois Vyatta 5400s em um par de HA, execute o procedimento a seguir:

1. Identifique o dispositivo Backup Vyatta 5400 e recarregue-o primeiro como um {{site.data.keyword.vra_full}}, usando o procedimento acima.

  Idealmente, uma configuração de HA funcional mostra todas as interfaces como BACKUP ou MASTER em um nó específico. Quando em dúvida, use o comando `show vrrp` para confirmar.
  {: note}
2. Configure o VRA recém-recarregado com suas configurações desejadas usando [estas instruções](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
3. O Master Vyatta 5400 e o Backup {{site.data.keyword.vra_full}} estarão em um par de VRRP. Mude o Backup {{site.data.keyword.vra_full}} atual para que se torne o Principal usando o comando VRRP `reset VRRP master`.

  Essa ação causará uma indisponibilidade em qualquer sessão existente e o estado de sessões será listado como Perdido. Qualquer sessão NAT existente também será reconfigurada.
  {: note}

5. Verifique se o novo Master {{site.data.keyword.vra_full}} está funcionando conforme desejado.
6. Execute o procedimento de recarregamento acima no Backup Vyatta 5400 atual para fazer upgrade dele para um VRA.
7. Após o segundo recarregamento, configure o Backup {{site.data.keyword.vra_full}} conforme desejado, usando [estas instruções](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-accessing-and-configuring-the-ibm-virtual-router-appliance).
