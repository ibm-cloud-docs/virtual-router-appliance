---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sobre o VRA

O IBM Virtual Router Appliance (VRA) fornece o sistema operacional Vyatta 5600 mais recente para servidores bare metal **x86**. Ele é oferecido como uma configuração de Alta disponibilidade (HA) ou independente.

O Virtual Router Appliance permite rotear o tráfego de rede privada e pública seletivamente, por meio de um roteador corporativo repleto de recursos que tem firewall, modelagem de tráfego, roteamento baseado em política, VPN e outros recursos. O VRA oferece desempenho com facilidade de configuração. Ele tem as vantagens de manutenção de execução em um servidor de hardware normal. O dispositivo de hardware do VRA é dimensionado para manipular o carregamento de roteamento para várias VLANs e pode ser pedido com links de rede redundantes e matrizes RAID redundantes. Todos os recursos do VRA são gerenciados por você, o cliente. 

**Alternativas:** o FortiGate Security Appliance (FSA) de 10 Gbps é um firewall de hardware de único locatário (dedicado), de alto rendimento (10 Gbps) com recursos de última geração, como Antivírus (AV), Prevenção de intrusão (IPS) e filtragem da web. Ele pode ser uma alternativa ao VRA para alcançar objetivos semelhantes. Para obter mais informações, consulte a [Documentação do FSA](/docs/infrastructure/fortigate-10g/getting-started.html#getting-started).

## Firewall
Para proteger seu ambiente contra ameaças externas, o Virtual Router Appliance pode ser alavancado como um firewall. É possível incluir regras de firewall para permitir ou negar tráfego de rede de entrada ou de saída para as portas nas quais seu aplicativo é executado e é possível filtrar o tráfego dentro de suas próprias redes. O Virtual Router Appliance também pode ser configurado para executar filtragem stateful de IPv4 e IPv6 para proteger seus dados críticos.

## Gateway de Rede Privada Virtual (VPN)
Conecte seu data center ou escritório no site ao IBM Cloud usando tunelamento de VPN fornecendo seu Virtual Router Appliance como um dispositivo de gateway de rede. É possível usar um túnel VPN IPsec de site para site para comunicação segura do data center ou do escritório de sua empresa com a rede do IBM Cloud. Outras opções de VPN são: VPN IPsec de acesso remoto (cliente para site), OpenVPN, GRE, L2TP e DMVPN.

Consulte os Guias de configurações de VPN do Brocade em nossa [seção de documentação suplementar do VRA](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)

## Conversão de Endereço de Rede (NAT)
Com o Virtual Router Appliance, é possível provisionar servidores de aplicativo e banco de dados sem interfaces de rede pública enquanto ainda permite que seus servidores acessem a Internet usando NAT de Origem. Também é possível ocultar seus servidores por trás do dispositivo de gateway com NAT de Destino para segurança aprimorada.

## Roteamento de Nível Corporativo

Para aplicativos com multicamadas em redes isoladas diferentes, o Virtual Router Appliance fornece maneiras flexíveis de construir a conectividade entre essas redes. É possível configurar o roteamento dinâmico usando o BGP, que permite anunciar seu próprio espaço de IP público nos roteadores do IBM Cloud. O BGP também oferece mais flexibilidade para configurações de rede privada customizadas ao usar vários túneis e soluções de link direto.

Consulte os Guias de configurações de BGP do Brocade em nossa [seção de documentação suplementar do VRA](/docs/infrastructure/virtual-router-appliance/vra-docs.html#supplemental-vra-documentation)
