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

# Sobre
O IBM Virtual Router Appliance (VRA) permite rotear seletivamente o tráfego de rede privada e pública por meio de um roteador corporativo completo com firewall, formato de tráfego, roteamento baseado em política, VPN e uma série de outros recursos. O Virtual Router Appliance fornece desempenho, facilidade de configuração e as vantagens de manutenção da execução em um servidor de hardware normal. O hardware é dimensionado para manipular a carga de roteamento para várias VLANs e pode ser solicitado com links de rede redundantes e matrizes RAID redundantes. Todos os recursos do VRA são gerenciados pelo cliente. 

O IBM Virtual Router Appliance fornece o sistema operacional Vyatta 5600 mais recente para servidores bare metal x86. Ele é oferecido como uma oferta de Alta Disponibilidade (HA) ou independente.

**NOTA:** o FortiGate Security Appliance (FSA) 10Gbps é um firewall de hardware de único locatário (dedicado) e alto rendimento (10Gbps) com recursos de última geração, como Antivírus (AV), Prevenção de Intrusão (IPS) e filtragem da web. Ele pode ser uma alternativa ao VRA para alcançar objetivos semelhantes. Para obter mais informações, consulte a [Documentação do FSA](https://console.bluemix.net/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Firewall
Para proteger seu ambiente contra ameaças externas, o Virtual Router Appliance pode ser alavancado como um firewall. É possível incluir regras de firewall para permitir ou negar o tráfego de rede de entrada ou de saída para as portas em que seu aplicativo é executado e é possível filtrar o tráfego dentro de suas próprias redes. O Virtual Router Appliance também pode ser configurado para executar filtragem de IPv4 e IPv6 stateful para proteger seus dados críticos.

## Gateway de Rede Privada Virtual (VPN)
Conecte seu data center ou escritório no site ao IBM Cloud usando tunelamento de VPN fornecendo seu Virtual Router Appliance como um dispositivo de gateway de rede. É possível usar um túnel VPN IPsec de site para site para comunicação segura do data center ou do escritório de sua empresa com a rede do IBM Cloud. Outras opções de VPN são: VPN IPsec de acesso remoto (cliente para site), OpenVPN, GRE, L2TP e DMVPN.

Consulte os Guias de configurações de VPN do Brocade em nossa [seção de documentação suplementar do VRA](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)

## Conversão de Endereço de Rede (NAT)
Com o Virtual Router Appliance, é possível provisionar servidores de aplicativo e banco de dados sem interfaces de rede pública enquanto ainda permite que seus servidores acessem a Internet usando NAT de Origem. Também é possível ocultar seus servidores por trás do dispositivo de gateway com NAT de Destino para segurança aprimorada.

## Roteamento de Nível Corporativo

Para aplicativos com multicamadas em diferentes redes isoladas, o Virtual Router Appliance permite construir a conectividade entre essas redes com maior flexibilidade. Será possível configurar o roteamento dinâmico usando BGP, que permitirá anunciar seu próprio espaço de IP público nos roteadores do IBM Cloud. O BGP também oferecerá mais flexibilidade para configurações de rede privada customizada ao usar vários túneis e soluções de link direto.

Consulte os Guias de configurações de BGP do Brocade em nossa [seção de documentação suplementar do VRA](https://console.bluemix.net/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)
