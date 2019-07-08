---

copyright:
  years: 2017
lastupdated: "2019-03-14"

keywords: 5400, migrate, migration, support, faqs, eos

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

# Perguntas mais frequentes do fim de suporte ao Vyatta 5400
{: #vyatta-5400-end-of-support-faqs}

A seguir, estão as perguntas mais frequentes ao migrar do Vyatta 5400.

## Por que o "Fim de Suporte" (EoS) do Vyatta 5400 é em 31 de março de 2019?
{: faq}

Setembro de 2017, o Vyatta 5400 anterior anunciou seu EoS para 20 de fevereiro de 2018, com base na política de ciclo de vida da IBM para suporte: seis meses após a data de julho da Disponibilidade geral (GA) para a próxima versão, o IBM Virtual Router Appliance (VRA).

Para honrar as linhas de tempo de migração do cliente, a data de EoS do Vyatta 5400 foi estendida para 31 de março de 2019. Como o software Debian 7 agora não é mais suportado pela comunidade do Debian Open Source, não há planos futuros para estender o suporte ao fornecedor da AT&T.

[Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) para visualizar o Comunicado formal de fim de suporte.
{: important}

## O que “Fim de suporte” significa para mim como um cliente?
{: faq}

Após a data de Fim de suporte, a AT&T não fornecerá mais nenhuma correção de código nem aceitará escaladas de suporte da IBM.

Da mesma forma, o Suporte do IBM Cloud não solucionará mais problemas de configuração ou de rede em implementações do Vyatta 5400. O suporte será limitado a solicitações de nível de hardware (unidade de disco rígido, RAM e assim por diante), conectividade de energia e de fora da banda (IPMI).

Nós altamente recomendamos que os clientes tomem ação imediata para migrar para uma solução alternativa, como o Virtual Router Appliance (VRA; com base no Vyatta 5600) ou Juniper vSRX. [Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para iniciar.

## O que acontecerá se eu ainda estiver executando minhas cargas de trabalho do IBM Cloud usando um Vyatta 5400 depois de 31 de março?
{: faq}

Seu Vyatta 5400 funcionará depois de 31 de março. No entanto, seus ambientes de negócios e aplicativos podem ser expostos a possíveis ameaças de segurança e a outras violações devido a vulnerabilidades latentes no software Vyatta 5400.

Se você encontrar um problema de rede que derrube seu ambiente de negócios e de aplicativo e rastrear a causa raiz para o Vyatta 5400, escale esse problema para o nosso 5400 Offering Manager, pois o suporte não estará mais disponível na IBM ou na AT&T. É possível atingir a equipe de Gerenciamento de oferta por meio de e-mail em `nwom@us.ibm.com`.

## E o meu hardware Bare Metal Server subjacente, ele ainda é suportado?
{: faq}

As substituições de hardware são suportadas, mas, se a resolução de problemas indicar que seu problema está relacionado ao S.O. Vyatta, você será direcionado para migrar para uma oferta de hardware suportada imediatamente. [Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para iniciar.

## Como um cliente que possui um Vyatta 5400, o que eu preciso fazer em 31 de março de 2019?
{: faq}

Os clientes que têm um Vyatta 5400 devem migrar para o VRA (Vyatta 5600), o Juniper vSRX ou o Fortigate Security Appliance (FSA) de 10 G. O VRA (Vyatta 5600) ainda é totalmente suportado. Não há nenhuma data de fim de suporte atual ou projetada para o VRA do IBM Cloud ou da AT&T. [Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para iniciar.

  VRAs e vSRXs são dispositivos gerenciados pelo cliente.
  {: note}

## O suporte está disponível da IBM para migrar do Vyatta 5400 para o VRA, vSRX ou FSA 10 G?
{: faq}

O Serviço de conversão de configuração do Vyatta 5400 para o VRA (5600) ainda está disponível:

* Para clientes existentes, o IBM Cloud está fornecendo uma oferta sem custo para ajudar a refatorar sua configuração do Vyatta 5400 existente em formatos de VRA (Virtual Router Appliance), Juniper vSRX ou Fortigate Security Appliance (FSA) 10 G. Para enviar uma solicitação para o serviço de Conversão de configuração, envie um e-mail para nwom@us.ibm.com com o assunto: `Request for Configuration Conversion to aaaaaaaa: IBM Cloud Account ID xxxxxx. `.

  Certifique-se de inserir a sua opção de aplicativo no lugar de `aaaaaaaa` (Virtual Router Appliance, Juniper vSRX ou Fortigate Security Appliance (FSA) 10 G) e seu número de conta específico no lugar de `xxxxxx` em sua linha de assunto.
  {: note}

* Wanclouds, nosso parceiro neste processo de configuração de conversão, concluiu várias centenas de engajamentos de migração bem-sucedidos. Eles transformarão seu Vyatta 5400 existente para criar uma funcionalidade semelhante na Plataforma Vyatta 5600. Eles fornecem seus serviços em duas camadas, descritas abaixo:

  <img src="images/tiers.png" alt="desenho" style="width: 700px;"/>

[Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview) para iniciar.

## Há algum serviço de migração pago adicional disponível por Parceiros de Negócios IBM para migração do Vyatta 5400?
{: faq}

Temos vários parceiros de negócios que fornecem suporte pago para migrações Vyatta 5400. [Clique aqui](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-end-of-support-announcement) para obter mais informações.

## Existe um contato de suporte ao Gerenciamento de ofertas do Vyatta 5400 na IBM, no qual eu posso fazer perguntas relacionadas à migração da Vyatta 5400?
{: faq}

Entre em contato com o IBM Vyatta 5400 e o VRA Network Offering Management com perguntas em `nwom@us.ibm.com`. Também é possível entrar em contato com eles usando a folga com a área de trabalho do IBM Watson Cloud Platform: `#vyatta-migration`

## Quais recursos adicionais estão disponíveis para me ajudar com essa migração?
{: faq}

Revise os recursos de documentação do Virtual Router Appliance a seguir para obter mais informações:

  * [Introdução ao IBM Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started)
  * [Sobre o VRA](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra)
  * [Visão geral da migração do Vyatta 5400](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-migration-overview)
