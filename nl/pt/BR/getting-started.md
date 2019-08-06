---

copyright:
  years: 2017
lastupdated: "2019-05-03"

keywords: vra, virtual router, order

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


# Introdução ao {{site.data.keyword.vra_full}}
{: #getting-started}

O {{site.data.keyword.vra_full}} (VRA) fornece o sistema operacional Vyatta 5600 mais recente para servidores bare metal x86. Ele é oferecido como uma configuração de Alta disponibilidade (HA) ou independente e permite que você roteie o tráfego de rede privada e pública seletivamente, por meio de um roteador corporativo completo que tenha firewall, formato de tráfego, roteamento baseado em política, VPN e outros recursos.

Os requisitos mínimos do servidor de VRA requerem 8 GB de RAM e um núcleo de CPU para cada 10 Gbps de capacidade de rede. Por exemplo, um sistema com uplinks duais públicos e privados de 10 Gbps requer pelo menos quatro núcleos. Além disso, se a sua intenção é configurar serviços de VPN com criptografia, talvez você queira incluir núcleos adicionais. A inclusão de núcleos adicionais para serviços de VPN assegurará que o VRA não será atolado por cargas pesadas ao rotear e criptografar/decriptografar dados simultaneamente.

## Solicitando um {{site.data.keyword.vra_full}}
{: #order-vra}

Para solicitar um VRA, execute o procedimento a seguir:

1. Em seu navegador, abra a página Dispositivos de gateway no [Console da IU do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: new_window} e efetue login em sua conta. 

  Também é possível chegar a essa página selecionando o menu de navegação na parte superior esquerda do [Catálogo do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com) e selecionando **Infraestrutura clássica > Rede > Dispositivo de gateway**.
  {: tip}

2. Na seção **Fornecedor de gateway**, selecione a opção **AT&T** (quando ela é selecionada, um visto azul aparece no botão). Na lista suspensa no mesmo botão, escolha sua largura da banda (de 20 Gbps ou 2 Gbps).

  	<img src="images/ordering_vra.png" alt="desenho" style="width: 500px;"/>

3. Na seção **Dispositivo de gateway**, insira suas informações de **Nome do host** e de nome do **Domínio**. Esses campos já estarão preenchidos com informações padrão, portanto, assegure-se de que os valores estejam corretos. Marque a opção **Alta disponibilidade** se desejar e, em seguida, selecione o **Local** desejado do seu data center e o **Pod** específico que deseja no menu suspenso.

  Somente os pods que já possuem uma VLAN associada serão exibidos aqui. Se você desejar provisionar seu Dispositivo de gateway em um pod que você não vê listado, primeiro crie uma VLAN lá.
  {: note}

4. Na seção **Configuração**, escolha seu processador selecionando sua RAM e as chaves SSH (se necessário).

  <img src="images/ordering_vra_2.png" alt="desenho" style="width: 600px;"/>
  
  O processador apropriado é escolhido para você com base na versão de licença selecionada na etapa dois. No entanto, é possível escolher configurações de RAM diferentes.
  {: note}

5. Na seção **Discos de armazenamento**, escolha as opções que atendem aos requisitos de armazenamento. 

  As opções RAID0 e RAID1 estão disponíveis para proteção incluída contra perda de dados, assim como os hot spares (componentes de backup que podem ser colocados em serviço imediatamente quando um componente primário falha).
  {: note}

  Você pode ter até quatro discos por VRA. "Tamanho do disco" com uma configuração de RAID é o tamanho do disco utilizável, pois as configurações do RAID são espelhadas.
  {: note}

  Reserve mais do que a configuração de disco padrão se planeja executar diagnósticos de rede que gerem logs detalhados.
  {: tip}

6. Na seção **Interface de rede**, selecione suas **Velocidades de porta de uplink**. A seleção padrão é uma interface única, mas também há opções redundantes e apenas privadas. Escolha a que melhor se ajustar às suas necessidades.

  A seção **Complementos** da Interface de rede permite selecionar um endereço IPv6, se necessário, e mostrar quaisquer opções padrão incluídas adicionais. 
  
8. Revise suas seleções, verifique se você leu os Contratos de prestação de serviços de terceiros e, em seguida, clique em **Criar**. O pedido é verificado automaticamente.

Após a aprovação de seu pedido, o fornecimento de seu {{site.data.keyword.vra_full}} é iniciado automaticamente. Quando o processo de fornecimento for concluído, o novo VRA aparecerá na página de lista Dispositivos de gateway. Clique no nome do gateway para abrir a página Detalhes do gateway. Você localizará os endereços IP, o nome do usuário de login e a senha para o dispositivo.  

  <img src="images/gateway_details.png" alt="desenho" style="width: 500px;"/>

Lembre-se de que, depois que você solicitar e configurar o VRA por meio do Catálogo do IBM Cloud, o próprio dispositivo também deverá ser configurado com as mesmas configurações.
{: tip}

## VLANs e a função do Dispositivo de Gateway
{: #vlans-and-the-gateway-appliance-s-role}

Uma VLAN (LAN virtual) é um mecanismo que divide uma rede física em muitos segmentos virtuais. Por conveniência, o tráfego de várias VLANs selecionadas pode ser entregue por meio de um único cabo de rede, um processo comumente chamado de "entroncamento".

O {{site.data.keyword.vra_full}} é entregue em duas partes: o servidor ou os servidores do VRA e o utensílio do Dispositivo de gateway. O Dispositivo de gateway fornece uma interface (GUI e API) para selecionar as VLANs que você deseja associar ao VRA. Associar uma VLAN a um Dispositivo de Gateway roteia novamente (ou "entronca") essa VLAN e todas as suas sub-redes para seu VRA, fornecendo-lhe controle sobre a filtragem, o encaminhamento e a proteção. Para cada VLAN associada ao Dispositivo de gateway, essa VLAN é permitida nas portas de comutador às quais o VRA está conectado, e qualquer sub-rede na VLAN é roteada estaticamente para o IP de VRRP público do VRA (caso a sub-rede seja uma sub-rede pública) ou é roteada estaticamente para o IP do VRRP privado do VRA (caso a sub-rede seja uma sub-rede privada). Esse roteamento é feito no roteador do qual o VRA está por trás, que será o FCR (Frontend Customer Router) ou o BCR (Backend Customer Router) para tráfego público e privado, respectivamente. 

Esteja ciente de que o VRRP fica desativado por padrão e ele deve ser ativado para que o tráfego de VLAN funcione, mesmo em vyattas independentes. Isso é uma consequência das sub-redes na VLAN associada que estão sendo roteadas para o IP do VRRP ou o endereço virtual designado ao VRA. Para obter mais informações, consulte [Endereços IP virtuais (VIP) do VRRP](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#vrrp-virtual-ip-vip-addresses).
{: important}

Os servidores em uma VLAN associada podem ser acessados somente por meio de outras VLANs passando por seu {{site.data.keyword.vra_full}}; não é possível contornar o VRA, a menos que você efetue bypass ou desassocie a VLAN.

Por padrão, um novo Dispositivo de Gateway está associado a duas VLANs de "trânsito" não removíveis, uma para público e outra para privado. Elas normalmente são usadas para administração e podem ser protegidas separadamente por comandos do VRA.

As VLANs de trânsito são para dispositivos de rede, como firewalls ou balanceadores de carga, para que possam rotear o tráfego enquanto mantêm outros dispositivos, como servidores ou contêineres, isolados da Internet.

Em comparação, as VLANs de "gateway" estão em locais em que os dispositivos, como servidores e contêineres, são hospedados.

O VRA pode gerenciar apenas VLANs que estão associadas a ele por meio do Dispositivo de Gateway.

Para obter informações sobre como gerenciar VLANs na tela Detalhes de Dispositivo de Gateways, consulte [Gerenciando VLANs](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-managing-your-vlans).
