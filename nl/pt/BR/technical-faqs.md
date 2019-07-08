---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: faqs, vlan, traffic, firewall, SSH,

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

# Perguntas frequentes técnicas sobre o IBM Virtual Router Appliance
{: #technical-faqs-for-ibm-virtual-router-appliance}

As perguntas mais frequentes a seguir tratam da configuração do IBM© Virtual Router Appliance (VRA) e da migração para o VRA a partir do Vyatta 5400.

## Como posso permitir o tráfego ligado à Internet de hosts que estão em uma VLAN privada?
{: faq}

Esse tráfego precisa obter um IP de origem pública, portanto, um NAT de origem precisa mascarar o IP privado com aquele público do VRA.

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

A configuração acima só executa SNAT de tráfego originado de servidores na rede `10.0.0.0/8` privada.

Isso assegura que não haverá interferência com pacotes que já possuem um endereço de origem roteável pela Internet.

## Como posso filtrar o tráfego ligado à Internet e somente permitir protocolos/destinos específicos?
{: faq}

Essa é uma pergunta comum quando o NAT de origem e um firewall precisam ser combinados.

Lembre-se da ordem das operações no VRA quando projetar seus conjuntos de regras.

Em resumo, as regras de firewall são aplicadas *após* o SNAT.

Para bloquear todo o tráfego de saída em um firewall, mas permitir fluxos SNAT específicos, você precisaria mover a lógica de filtragem para o SNAT.

Por exemplo, para permitir apenas tráfego HTTPS ligado à Internet para um host, a regra SNAT seria:

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` seria um endereço público para o VRA.

É altamente recomendável usar o endereço público do VRRP do VRA, portanto, é possível diferenciar entre o host e o tráfego público do VRA.

Suponha que `150.1.2.3` seja o endereço do VRA do VRRP e `150.1.2.5` seja o endereço real dp0bond1.

O firewall stateful aplicado em `dp0bond1 out` seria:

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

Observe que a combinação do NAT de origem e do firewall atinge o objetivo de design necessário.

Assegure-se de que as regras sejam apropriadas para seu design e que nenhuma outra regra permita tráfego que deve ser bloqueado.

## Como proteger o próprio VRA com um firewall baseado em zona?
{: faq}

O VRA não tem uma `local zone`.

É possível usar a funcionalidade do Control Plane Policing (CPP), em vez de ser aplicada como um firewall `local` no loopback.

Observe que este é um firewall stateless e será necessário permitir explicitamente o tráfego de retorno de sessões de saída originadas no próprio VRA.

## Como restringir SSH e bloquear conexões provenientes da Internet?
{: faq}

Considera-se uma melhor prática não permitir conexões SSH da Internet e usar outro meio de acesso ao endereço privado, como SSL VPN.

Por padrão, o VRA aceita SSH em todas as interfaces.
Para atender apenas às conexões SSH na interface privada, a configuração a seguir precisa ser feita:

```
set service ssh listen-address '10.1.2.3'
```

Tenha em mente que o endereço IP precisa ser substituído pelo endereço pertencente ao VRA.
