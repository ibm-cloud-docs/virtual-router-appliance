---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Como visualizar o Vyatta no portal

Um único gateway de 1 Gbps foi implementado no data center de San Jose; ele tem acesso redundante a ambas as infraestruturas, pública e privada. Use a guia **dispositivos** sob o menu **Rede > Dispositivos de gateway** para ver a configuração real do hardware implementado.

Observe o link **VLAN pública 2546**, **VLAN privada 2576** e **Detalhes de exibição de hardware de rede**. As redes são dedicadas e contêm apenas dispositivos de gateway. Elas estão sob o controle da plataforma de fornecimento (não é possível provisionar servidores normais por meio dessa guia).

Para visualizar as associações de rede, clique no menu **Rede, Dispositivos de gateway** no Portal da Web.

As portas de comutação de rede e os adaptadores Ethernet no Brocade 5400 vRouter (Vyatta) são todos configurados pela SoftLayer como troncos. Nossas associações simplesmente atualizam a configuração de porta para permitir as LANs virtuais adicionais (VLANs) selecionadas.<sup>1</sup>

O menu suspenso **Associar uma VLAN** permite selecionar uma VLAN específica a ser conhecida ou "associada", para o seu dispositivo do Brocade 5400 vRouter de dentro do data center. **Não** é possível associar uma VLAN que esteja protegida por um firewall de hardware ou dispositivo do FortiGate, já que isso quebraria o controle da VLAN. Se nenhuma VLAN adicional estiver listada no menu suspenso, então, será possível solicitar mais enviando um chamado para a Softlayer.

É possível ver as VLANs associadas ao dispositivo do Brocade 5400 vRouter em VLANs Associadas na Figura 4. O Status padrão é Bypassed (a SoftLayer tem atrás das cenas identificadas as portas de comutação de tronco para incluir 1894, 2254 da privada e 2007 e 1280 da pública).<sup>2</sup>

Na tela **Detalhes** (**Rede, Dispositivos de gateway**) sob o menu suspenso **VLAN Associada, Ações**, há uma opção para **Rotear VLAN**. Se você selecionar essa opção, terá solicitado efetivamente ao SoftLayer FCR e BCR para remover o gateway padrão e, em vez disso, incluir uma rota para as sub-redes usando as VLANs de trânsito para o dispositivo do Brocade 5400 vRouter.

O dispositivo do Brocade vRouter é agora a única rota válida dentro e fora da VLAN. A chave para lembrar é que isso não é apenas para o seu tráfego de aplicativo, mas também para todos os serviços adicionais da SoftLayer que agora precisem passar por seu dispositivo.<sup>4</sup>

A configuração ainda precisa ser feita no lado do Brocade 5400 vRouter, mas qualquer coisa na VLAN com as sub-redes não está mais acessível. Isso também significa que os novos servidores provisionados como o Portal da Web também não podem alcançar a VLAN.

**Notas:**

<sup>1</sup>A associação da Vlan é o processo de vinculação de uma VLAN elegível a um gateway de rede para que ele possa ser roteado para o gateway no futuro. O processo de associação não roteará automaticamente uma VLAN para um gateway; a VLAN continua a usar roteadores de cliente de front-end e backend até que ele seja roteado manualmente para o gateway. As VLANs podem ser associadas a um gateway de cada vez e não devem ter um pedido de firewall para serem consideradas elegíveis para associação.

<sup>2</sup>O gateway pode ser ignorado a qualquer momento, de modo que o tráfego retornará para os roteadores de cliente de front-end e backend (FCR e BCR). O bypass de uma VLAN permite que a VLAN permaneça associada ao gateway de rede.

<sup>3</sup>As sub-redes móveis podem ser compradas e incluídas nos gateways primários.

<sup>4</sup>Os dispositivos do Brocade 5400 vRouter são normalmente provisionados com o endereço IP de Serviço da SoftLayer predefinido.
