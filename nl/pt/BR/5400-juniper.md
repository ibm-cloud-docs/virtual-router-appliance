---

copyright:
  years: 2017
lastupdated: "2019-03-03"

keywords: 5400, vyatta, migrate, fsa, Fortigate

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

# Migrando um Vyatta 5400 para um Juniper vSRX ou para um Fortigate Security Appliance (FSA) de 10 Gbps
{: #migrating-a-vyatta-5400-to-a-juniper-vsrx-or-fortigate-security-appliance-fsa-10gbps}

Além de você ser capaz de fazer upgrade do dispositivo Vyatta 5400 para um IBM Virtual Router Appliance, também poderá migrar para algumas das opções que são mais econômicas, confiáveis e seguras no mercado: o Juniper vSRX e o Fortigate Security Appliance.
No entanto, não será possível reutilizar o dispositivo Vyatta 5400 existente ou manter seus endereços IP associados.

Os procedimentos a seguir fornecem instruções de upgrade de um Vyatta 5400 independente ou de dois dispositivos Vyatta 5400 operando em um par de Alta disponibilidade (HA) para um Juniper vSRX ou um FSA.

## Fazendo upgrade de um Vyatta 5400 independente
{: #upgrading-a-stand-alone-vyatta-5400}

Para fazer upgrade de um único Vyatta 5400 para um Juniper vSRX ou para um Dispositivo FSA - 10 G com a menor quantidade de tempo de inatividade, recomendamos executar o procedimento a seguir.

1. Assegure-se de que tenha feito backup de seu 5400 e armazenado os dados em dois locais diferentes. Isso inclui as chaves SSH, os certificados SSL, os scripts e quaisquer outros arquivos necessários para recuperar sua configuração do Vyatta 5400 atual, se necessário. Para fazer isso, siga [estas instruções](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Entre em contato com nwom@us.ibm.com para solicitar que sua configuração seja convertida

3. Solicite o novo dispositivo seguindo as instruções para o [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) ou para o [FSA - 10 G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

  Esta é uma boa oportunidade para considerar a solicitação de uma solução de Alta disponibilidade.
  {: note}

4. Carregue a configuração que você recebeu de nwom@us.ibm.com no seu dispositivo recém-solicitado.

5. Planeje o tempo para migrar suas VLANs para seu novo dispositivo.

  Esta etapa requer um breve tempo de inatividade.
  {: note}

6. Teste e confirme se o seu novo dispositivo está funcionando corretamente.

7. Abra um chamado de suporte para cancelar seu dispositivo Vyatta 5400.

## Fazendo upgrade de um par de Alta disponibilidade Vyatta 5400
{: #upgrading-a-vyatta-5400-high-availability-pair}

Para fazer upgrade de dois Vyatta 5400s em um par de HA, execute o procedimento a seguir:

1. Assegure-se de que tenha feito backup de seu 5400 e armazenado os dados em dois locais diferentes. Isso inclui as chaves SSH, os certificados SSL, os scripts e quaisquer outros arquivos necessários para recuperar sua configuração do Vyatta 5400 atual, se necessário. Para fazer isso, siga [estas instruções](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-backing-up-a-configuration).

2. Entre em contato com nwom@us.ibm.com e solicite a conversão da sua configuração.

3. Solicite o novo dispositivo seguindo as instruções para o [Juniper vSRX](/docs/infrastructure/vsrx?topic=vsrx-getting-started-with-ibm-cloud-juniper-vsrx-gateway#steps-for-ordering) ou para o [FSA - 10 G](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started-with-fortigate-security-appliance-10gbps#ordering-the-fsa-10gbps). 

4. Carregue a configuração que você recebeu de nwom@us.ibm.com no seu dispositivo recém-solicitado.

5. Planeje o tempo para migrar suas VLANs para seu novo dispositivo.

  Esta etapa requer um breve tempo de inatividade.
  {: note}

6. Teste e confirme se o seu novo dispositivo está funcionando corretamente.

7. Abra um chamado de suporte para cancelar seus dispositivos Vyatta 5400.
