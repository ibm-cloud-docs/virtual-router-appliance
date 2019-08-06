---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-10"

keywords: vra, about, firewall, VPN, NAT, Routing

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
{:important: .important}

# Sobre o VRA
{: #about-the-vra}

O {{site.data.keyword.vra_full}} (VRA) fornece o sistema operacional Vyatta 5600 mais recente para servidores bare metal **x86**. Ele é oferecido como uma configuração de Alta disponibilidade (HA) ou independente.

O {{site.data.keyword.vra_full}} permite rotear o tráfego de rede privada e pública seletivamente, por meio de um roteador corporativo repleto de recursos que tem firewall, modelagem de tráfego, roteamento baseado em política, VPN e outros recursos. O VRA oferece desempenho com facilidade de configuração. Ele tem as vantagens de manutenção de execução em um servidor de hardware normal. O dispositivo de hardware do VRA é dimensionado para manipular o carregamento de roteamento para várias VLANs e pode ser pedido com links de rede redundantes e matrizes RAID redundantes. Todos os recursos do VRA são gerenciados por você, o cliente.

**Alternativas:** o FortiGate Security Appliance (FSA) de 10 Gbps é um firewall de hardware de único locatário (dedicado), de alto rendimento (10 Gbps) com recursos de última geração, como Antivírus (AV), Prevenção de intrusão (IPS) e filtragem da web. Ele pode ser uma alternativa ao VRA para alcançar objetivos semelhantes. Para obter mais informações, consulte a [Documentação do FSA](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-getting-started).

## Firewall
{: #firewall}

Para proteger seu ambiente contra ameaças externas, o {{site.data.keyword.vra_full}} pode ser alavancado como um firewall. É possível incluir regras de firewall para permitir ou negar tráfego de rede de entrada ou de saída para as portas nas quais seu aplicativo é executado e é possível filtrar o tráfego dentro de suas próprias redes. O {{site.data.keyword.vra_full}} também pode ser configurado para executar filtragem stateful de IPv4 e IPv6 para proteger seus dados críticos.

## Gateway de Rede Privada Virtual (VPN)
{: #virtual-private-network-vpn-gateway}

Conecte seu data center ou escritório no site ao IBM Cloud usando tunelamento de VPN fornecendo seu {{site.data.keyword.vra_full}} como um dispositivo de gateway de rede. É possível usar um túnel VPN IPsec de site para site para comunicação segura do data center ou do escritório de sua empresa com a rede do IBM Cloud. Outras opções de VPN são: VPN IPsec de acesso remoto (cliente para site), OpenVPN, GRE, L2TP e DMVPN.

Dê uma olhada nos guias de configuração da VPN do Brocade em nossa [seção Documentação do VRA suplementar](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).

## Conversão de Endereço de Rede (NAT)
{: #network-address-translation-nat-}

Com o {{site.data.keyword.vra_full}}, é possível provisionar servidores de aplicativo e banco de dados sem interfaces de rede pública enquanto ainda permite que seus servidores acessem a Internet usando NAT de Origem. Também é possível ocultar seus servidores por trás do dispositivo de gateway com NAT de Destino para segurança aprimorada.

## Roteamento de Nível Corporativo
{: #enterprise-grade-routing}

Para aplicativos com multicamadas em redes isoladas diferentes, o {{site.data.keyword.vra_full}} fornece maneiras flexíveis de construir a conectividade entre essas redes. É possível configurar o roteamento dinâmico usando o BGP, que permite anunciar seu próprio espaço de IP público nos roteadores do IBM Cloud. O BGP também oferece mais flexibilidade para configurações de rede privada customizadas ao usar vários túneis e soluções de link direto.

Dê uma olhada nos guias de configuração do BGP do Brocade em nossa [seção Documentação do VRA suplementar](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation).
