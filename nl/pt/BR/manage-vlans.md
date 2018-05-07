---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gerenciar VLANs
É possível executar uma variedade de ações da [tela Detalhes do Dispositivo de Gateway](access-gateway-details.html).

## Associar uma VLAN a um Dispositivo de Gateway

Uma VLAN precisa ser associada a um Dispositivo de Gateway antes de poder ser roteada. A associação de VLAN é a vinculação de uma VLAN elegível a um Gateway de Rede para que ele possa ser roteado para um Dispositivo de Gateway no futuro. O processo de associação não roteia automaticamente uma VLAN para um Dispositivo de Gateway; a VLAN continua usando os roteadores do cliente de front-end e backend até que seja roteada para o Gateway. 

VLANs podem ser associadas a apenas um Gateway por vez e não devem ter um firewall. Execute o procedimento a seguir para associar uma VLAN a um Gateway de Rede.

1. [Acesse a tela Detalhes do Dispositivo de Gateway](access-gateway-details.html) no Portal do Cliente. 
2. Selecione a VLAN desejada na lista suspensa **Associar uma VLAN**.
3. Clique no botão **Associar** para associar a VLAN.

Depois de associar uma VLAN ao Dispositivo de Gateway, ela aparece na seção VLANs Associadas da tela Detalhes do Dispositivo de Gateway. Nesta seção, a VLAN pode ser roteada para o Gateway ou pode ser desassociada do Gateway. VLANs adicionais elegíveis podem ser associadas a um Dispositivo de Gateway a qualquer momento repetindo as etapas acima.

## Rotear uma VLAN Associada

VLANs associadas são vinculadas a um Dispositivo de Gateway, mas o tráfego dentro e fora da VLAN não atinge o Gateway até que a VLAN tenha sido roteada. Após uma VLAN associada ter sido roteada, todo o tráfego de front-end e backend será roteado por meio do Dispositivo de Gateway ao invés de para roteadores do cliente. 

Execute o procedimento a seguir para rotear uma VLAN associada:

1. [Acesse a tela Detalhes do Dispositivo de Gateway](access-gateway-details.html) no Portal do Cliente. 
2. Localize a VLAN desejada na seção VLANs Associadas.
3. Selecione **Rotear VLAN** no menu suspenso Ações.
4. Clique em **Sim** para rotear a VLAN. 

Depois de rotear uma VLAN, todo o tráfego de front-end e backend é movido de roteadores do cliente para o Network Gateway. Os controles adicionais relacionados ao trânsito e ao próprio Dispositivo de Gateway podem ser assumidos acessando a ferramenta de gerenciamento do Gateway. O roteamento por meio do Gateway de Rede pode ser descontinuado em qualquer momento [efetuando bypass no Dispositivo de Gateway](#bypass-gateway-appliance-routing-for-a-vlan).

## Efetuar Bypass do Roteamento do Dispositivo de Gateway para uma VLAN

Após uma VLAN ser roteada, todo o tráfego de front-end e backend atravessa o Gateway de Rede. A qualquer momento, o Dispositivo de Gateway pode ter bypass efetuado para que o tráfego retorne aos roteadores do cliente de front-end e backend (FCR e BCR). 

O bypass de uma VLAN permite que a VLAN permaneça associada ao Gateway de Rede. Se a VLAN não precisar mais ser associada ao Dispositivo de Gateway, consulte [Desassociar uma VLAN de um Dispositivo de Gateway](#disassociate-a-vlan-from-a-gateway-appliance). 

Execute o procedimento a seguir para efetuar bypass do roteamento de Gateway para uma VLAN:

1. [Acesse a tela Detalhes do Dispositivo de Gateway](access-gateway-details.html) no Portal do Cliente. 
2. Localize a VLAN desejada na seção VLANs Associadas.
3. Selecione **Efetuar Bypass na VLAN** no menu suspenso Ações.
4. Clique em **Sim** para efetuar bypass no Gateway. 

Depois de efetuar bypass no Network Gateway, todo o tráfego de front-end e backend é roteado por meio do FCR e BCR associados à VLAN. A VLAN permanecerá associada ao Dispositivo de Gateway e poderá ser roteada de volta para o Dispositivo de Gateway a qualquer momento.

## Desassociar uma VLAN de um Dispositivo de Gateway

VLANs podem ser vinculadas a um Dispositivo de Gateway em um momento por meio de [associação](#associate-a-vlan-to-a-gateway-appliance). A associação permite que a VLAN seja roteada para o Dispositivo de Gateway a qualquer momento. Se uma VLAN precisar ser associada a outro Dispositivo de Gateway ou se a VLAN não precisar mais ser associada ao seu Gateway, é necessária a desassociação. A desassociação remove o "link" da VLAN com o Dispositivo de Gateway, permitindo que ela seja associada a outro Gateway, se necessário. 

Execute o procedimento a seguir para desassociar uma VLAN de um Dispositivo de Gateway:

1. [Acesse a tela Detalhes do Dispositivo de Gateway](access-gateway-details.html) no Portal do Cliente. 
2. Localize a VLAN desejada na seção VLANs Associadas.
3. Selecione **Desassociar** no menu suspenso **Ações**. 
4. Clique em **Sim** para desassociar a VLAN. 

Depois de desassociar uma VLAN de um Dispositivo de Gateway, a VLAN pode ser associada a outro Gateway. A VLAN também pode ser associada de volta ao Dispositivo de Gateway a qualquer momento. Após desassociar uma VLAN de um Dispositivo de Gateway, o tráfego da VLAN não pode ser roteado por meio do Gateway. As VLANs devem estar associadas a um Dispositivo de Gateway antes de poderem ser roteadas.

## Rotear múltiplas VLANs na mesma interface de rede
O Virtual Router Appliance é capaz de rotear múltiplas VLANs na mesma interface de rede (por exemplo, `dp0bond0` ou `dp0bond1`). Isso é feito configurando a porta do comutador no modo de tronco e configurando interfaces virtuais (VIFs) no dispositivo.

Por exemplo: 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

Os comandos acima criam duas interfaces virtuais na interface `dp0bond0`. A interface `dp0bond0.1432` processa o tráfego para a VLAN 1432 enquanto a interface `dp0bond0.1693` processa o tráfego para a VLAN 1693.
