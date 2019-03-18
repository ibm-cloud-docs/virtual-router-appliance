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


# Introdução ao IBM Virtual Router Appliance
{: #getting-started-with-ibm-virtual-router-appliance}

Para começar a usar o IBM© Virtual Router Appliance (VRA), navegue para a página de pedido no Portal do Cliente:

1. Em seu navegador, abra o [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){: new_window} e efetue login em sua conta.
2. Na navegação do Portal do Cliente, selecione **Rede > Dispositivo de Gateways**.
3. Na página de lista **Dispositivo de Gateways**, clique em **Pedir Gateway**.
4. Na página **Pedido**, selecione seu data center desejado no menu suspenso, em seguida, escolha o tipo desejado de hardware do servidor.

    **NOTA:** os requisitos mínimos de servidor do VRA demandam 8 GB de RAM e um núcleo de CPU para cada 10 Gbps de capacidade de rede. Por exemplo, um sistema com uplinks duais públicos e privados de 10 Gbps requer pelo menos quatro núcleos. Reserve mais do que a configuração de disco padrão se planeja executar diagnósticos de rede que gerem logs detalhados. Finalmente, se a sua intenção é configurar serviços VPN com criptografia, talvez você queira incluir núcleos adicionais. A inclusão de núcleos adicionais para Serviços VPN assegurará que o VRA não seja paralisado devido à carga pesada ao rotear e simultaneamente criptografar/decriptografar dados.

5. Na página Pedido, selecione a opção **Par de alta disponibilidade**, se desejar, selecione Tamanho da memória, selecione a versão apropriada do Sistema operacional do VRA e, em seguida, selecione a velocidade de uplink da rede.

6. Revise suas seleções, em seguida, clique em **Incluir no Pedido** e o pedido será verificado automaticamente.
7. Na página **CHECK-OUT**, se você já possui VLANs no data center selecionado, selecione as VLANs de backend que precisam ser protegidas. Forneça um nome do host e o nome de domínio para o VRA. Marque todas as caixas para os termos de serviço do IBM Cloud e o Contrato de Prestação de Serviços de Terceiro. Em seguida, clique em **Enviar Pedido**.

Depois que seu pedido é aprovado, o fornecimento de seu VRA inicia automaticamente. Quando o processo de fornecimento estiver concluído, o novo VRA aparecerá na página de lista **Dispositivos de gateway**. Clique no nome do gateway para abrir a página **Detalhes do Gateway**, em seguida, clique em cada membro do gateway para abrir a página **Detalhes do Dispositivo**. Você localizará os endereços IP, o nome do usuário de login e a senha para o dispositivo.  

**NOTA:** é importante lembrar-se de que, uma vez que você pede e configura o VRA por meio do Portal do cliente do IBM Cloud, deve-se também configurar o próprio dispositivo com as mesmas configurações.

## VLANs e a função do Dispositivo de Gateway
Uma VLAN (LAN virtual) é um mecanismo que divide uma rede física em muitos segmentos virtuais. Por conveniência, o tráfego de várias VLANs selecionadas pode ser entregue por meio de um único cabo de rede, um processo comumente chamado de "entroncamento".

O VRA é entregue em duas partes: o servidor do VRA e o acessório Dispositivo de Gateway. O Dispositivo de gateway fornece uma interface (GUI e API) para selecionar as VLANs que você deseja associar ao VRA. Associar uma VLAN a um Dispositivo de Gateway roteia novamente (ou "entronca") essa VLAN e todas as suas sub-redes para seu VRA, fornecendo-lhe controle sobre a filtragem, o encaminhamento e a proteção. Os servidores em uma VLAN associada só podem ser acessados por meio de outras VLANs passando por seu VRA; não é possível contornar o VRA, a menos que você efetue bypass ou desassocie a VLAN.

Por padrão, um novo Dispositivo de Gateway está associado a duas VLANs de "trânsito" não removíveis, uma para público e outra para privado. Elas normalmente são usadas para administração e podem ser protegidas separadamente por comandos do VRA.

As VLANs de trânsito são para dispositivos de rede, como firewalls ou balanceadores de carga, para que possam rotear o tráfego enquanto mantêm outros dispositivos, como servidores ou contêineres, isolados da Internet.

Em comparação, as VLANs de "gateway" estão em locais em que os dispositivos, como servidores e contêineres, são hospedados.

O VRA pode gerenciar apenas VLANs que estão associadas a ele por meio do Dispositivo de Gateway.

Para obter informações sobre como gerenciar VLANs na tela Detalhes de Dispositivo de Gateways, consulte [Gerenciando VLANs](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
