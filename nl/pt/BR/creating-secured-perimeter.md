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

# Criando um perímetro seguro
Um aspecto fundamental do isolamento da rede é o estabelecimento de um perímetro seguro.  Um perímetro seguro controla o tráfego entre a internet pública e os ativos do cliente hospedados no IBM© Cloud.  O perímetro seguro também permitirá a conectividade direta com a empresa do cliente pelo uso de túneis da Rede privada virtual (VPN) e do IBM Cloud Direct Link.

Um perímetro seguro usa um Secure Perimeter Segment (SPS) para segregar redes entre ativos dentro do perímetro seguro. Essa segregação tem vários benefícios, incluindo controle de acesso e isolamento do tráfego de serviço entre os segmentos. Um Secure Perimeter Segment (SPS) abrange duas Redes locais virtuais (VLANs), uma VLAN de front-end e uma VLAN de backend, além de um VRA conectado às VLANs para gerenciar o tráfego para dentro e para fora do SPS. Um perímetro seguro pode incluir vários Secure Perimeter Segments (por exemplo, para propósitos de alta disponibilidade).

## Configurando o perímetro seguro

A seguir estão as etapas para configurar um perímetro seguro.  Para obter uma descrição detalhada dessas etapas, consulte [Configurar um perímetro seguro no IBM Cloud
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter){:new_window}.

1. Configurar permissões no IBM Cloud e na infraestrutura necessária para acessar os serviços de nuvem e os componentes de infraestrutura envolvidos no perímetro seguro.
2. Criar um limite exterior na Internet pública por meio de uma VLAN pública e de um firewall. Esse limite exterior será usado para isolar o SPS.
3. Colocar o SPS dentro desse limite
4. Configurar um gateway para servir de ponte entre a VLAN pública e o SPS
5. Criar uma configuração do {{site.data.keyword.vra_full}} (VRA)
6. Criar e configurar um ou mais Secured Perimeter Segments (SPS)
7. Criar a VLAN de front-end
8. Criar a VLAN de backend
9. Criar o par de Vyatta
10. Configurar o Vyatta
11. Associar VLANS a um gateway
12. Configurar a interface de gateway para a VLAN pública
13. Configurar gateway no Vyatta principal
14. Configurar gateway no backup do Vyatta
15. Configurar a interface de gateway para a VLAN privada
16. Configurar gateway no Vyatta principal
17. Configurar gateway no backup do Vyatta
18. Ativar o ajuste de rede
19. Configurar regras de firewall
20. Configurar o cluster do Kubernetes
21. Configure os IPs fornecidos pelo usuário (opcional).
22. Implementar o cluster do Kubernetes.

## Soluções dedicadas no IBM Cloud
Um perímetro seguro, junto ao isolamento de cálculo e à criptografia de dados, contribui para uma solução dedicada completa no IBM Cloud público.  Consulte [Implementando um padrão de solução dedicada ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/){:new_window} para obter uma descrição dos padrões de solução dedicada de nuvem.
