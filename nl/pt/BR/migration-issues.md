---

copyright:
  years: 2017
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Problemas de migração comuns do Vyatta 5400
A tabela a seguir ilustra problemas comuns ou mudanças de comportamento que você pode encontrar após migrar de um dispositivo do Vyatta 5400 para um IBM Virtual Router Appliance. Em alguns casos, ela inclui soluções alternativas para direcionar os problemas.

## Política de estado global baseada na interface para o firewall stateful

### Problemas
O comportamento ao configurar "Política de estado de estado" para firewalls stateful da liberação 5.1 foi mudado. Em versões anteriores à liberação 5.1, se você configurar `state - global -state -policy` de um firewall stateful, o vRouter incluirá automaticamente uma regra implícita `Allow` para a comunicação de retorno da sessão Automaticamente.

Na liberação 5.1 e mais recente, deve-se incluir uma configuração de regra `Allow` no Virtual Router Appliance. A configuração stateful funciona por interfaces em dispositivos do Vyatta 5400 e por protocolo em dispositivos do VRA.

### Soluções alternativas
Se a regra `firewall-in` for aplicada em uma interface Ingress/Inside, em seguida, a regra `Firewall-out` deverá ser aplicada na interface Egress/Outside. Caso contrário, o tráfego de retorno será descartado na interface Egress/Outside.        

## Ativação de estado em Regras de firewall

### Problemas
Se `global-state-policy` não estiver configurado, essa mudança de comportamento não será afetada.

Além disso, se você estiver usando a opção `state enable` em cada regra em vez de `global-state-policy`, a mudança de comportamento não será afetada.

## Política baseada em zona: manipulação de zona local

### Problemas
Não há pseudo-interface "zona local" para designar para a política de zona. 

### Solução alternativa
Esse comportamento pode ser simulado aplicando um firewall baseado em zona para interfaces físicas e um firewall de interface para a interface de loopback. O firewall na interface de loopback filtra tudo que ingresse e egresse do roteador. 

Por exemplo:

set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## Order of operation for firewalls, NAT, routing and DNS

### Issues
Em um cenário em que o Masquerade Source NAT é implementado em um IBM Virtual Router Appliance não será possível usar o firewall para determinar o acesso para hosts para a Internet. Isso é porque o endereço do NAT posterior será o mesmo.

Para dispositivos do Vyatta 5400, essa operação era possível porque o firewall era feito antes do NAT, permitindo a restrição de hosts para acessar a Internet.

### Workarounds
Um novo esquema de roteamento é necessário para o VRA:
![routing dns](./images/routing-dns.png)

## Policy Based Routing Table

### Issues
A palavra "Tabela" nas configurações é opcional no Roteamento baseado em política do v5400, mas para o VRA, se a ação for `aceitar`, então, o campo **Tabela** será obrigatório. Se a ação for `descartar` na configuração do VRA, então, o campo de Tabela será opcional.

### Workarounds
"Tabela principal" é uma opção disponível no Roteamento baseado em política do Vyatta 5400. O equivalente no VRA é "padrão de instância de roteamento".

## Policy Based Routing on Tunnel Interface

### Issues
No Virtual Router Appliance PBR (Roteamento baseado em política) as políticas podem ser aplicadas em interfaces de plano de dados para tráfego de entrada, mas não para loopback, túnel, ponte, OpenVPN, VTI e interfaces sem número de IP.

### Workarounds
Atualmente, não há soluções alternativas para esse problema.

## TCP-MSS

### Issues
O IBM Virtual Router Appliance usa nftables e não suporta o TCP-MSS.

### Workarounds
Atualmente, não há soluções alternativas para esse problema.

## OpenVPN

### Issues
O OpenVPN não começa a funcionar ao usar o parâmetro `push-route` no Virtual Router Appliance.

### Workarounds
Use o parâmetro `openvpn-option` em vez de `push-route`.

## GRE/VTI over IPSEC + OSPF

### Issues
* Quando o VIF tiver múltiplas sub-redes configuradas, o tráfego não poderá ir por essas sub-redes no VRA.
* O Roteamento do InterVlan não está funcionando no VRA.

### Workarounds
Use Regras de permissão implícitas para aceitar o tráfego por essas interfaces do VIF.

## IPSec

### Issues
O IPSec (Baseado em prefixo) não funciona com IN Filter.

### Workarounds
Use o IPSec (BASEADO no VTI).

## IPSEC 'match-none"

### Issues
Com dispositivos do Vyatta 5400, a regra de firewall a seguir é permitida:

set firewall name allow rule 10 ipsec 

No entanto, com o dispositivo do IBM Virtual Router, não há IPSec.

### Workarounds
Regras alternativas possíveis para dispositivos do VRA:

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### Issues
Com dispositivos do Vyatta 5400, as regras de firewall a seguir são permitidas:

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

No entanto, com o dispositivo do IBM Virtual Router, não há IPSec.

### Workarounds
Inclua os protocolos `ESP` e `AH` (protocolos IP 50 e 51 respectivamente).

A regra `ação` pode ser `aceitar` ou `descartar`, conforme mostrado abaixo:

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## Site-to-Site IPSEC with DNAT

### Issues
O IPSec (Baseado em prefixo) não funciona com o DNAT.

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1) 
===S-S-IPsec=== (12.0.0.1) 
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

O snippet acima é um exemplo de configuração pequeno para tradução de DNAT após um pacote do IPSec ter sido decriptografado em um Vyatta 5400. No exemplo, há dois vyattas, `vyatta1 (11.0.0.1)` e `vyatta2 (12.0.0.1)`. O peering do IPsec é estabelecido entre `11.0.0.1` e `12.0.0.1`. Nesse caso, o cliente está direcionando `172.16.1.245` originado de `10.103.0.1` de ponta a ponta. O comportamento esperado desse cenário é que o endereço de destino `172.16.1.245` será traduzido para `10.71.68.245` no cabeçalho do pacote.

Inicialmente, o dispositivo do Vyatta 5400 estava executando o DNAT no IPSec de entrada, finalizando a interface e retornando o tráfego normalmente ao túnel do IPsec usando a tabela de rastreamento de conexão.

Em um Virtual Router Appliance, a configuração não funciona da mesma forma. A sessão é criada; no entanto, o tráfego de retorno ignora o túnel do IPsec depois que a tabela conntrack reverte a mudança do DNAT. O VRA, em seguida, envia o pacote na ligação sem a criptografia do IPsec. O dispositivo de envio de dados não está esperando esse tráfego e provavelmente o descartará. Enquanto a conectividade de ponta a ponta estiver quebrada, esse será o comportamento desejado.   

### Workarounds
Para acomodar o cenário de rede acima, a IBM criou um RFE. 

Embora a RFE esteja sendo avaliada atualmente, recomendamos a seguinte solução alternativa:

**Comandos de configuração de interface**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**Comandos de configuração de VPN**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**Comandos de configuração do NAT**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**Comandos de configuração de protocolos**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**Comandos de configuração do PBR**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### Issues
O PPTP não é mais suportado no Virtual Router Appliance.

### Workarounds
Use o protocolo L2TP como alternativa.

## Script for IPSec Restart

### Issues
Sempre que um endereço virtual do VRRP for incluído em um IBM Virtual Router Appliance em uma VPN de Alta disponibilidade, deve-se reinicializar o daemon do IPsec. Isso porque o serviço do IPsec atenderá apenas para conexões com os endereços que estiverem presentes no VRA quando o daemon de serviço do IKE for inicializado.

Para um par de VRAs com VRRP, o roteador de espera poderá não ter o endereço virtual do VRRP presente no dispositivo durante a inicialização, se o roteador principal não tiver esse endereço presente. Portanto, para reinicializar o daemon do IPsec quando ocorrer uma transição de estado do VRRP, execute o comando a seguir nos roteadores principal e de backup:

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Recent count and Recent time

### Issues

A intenção da regra a seguir é limitar as conexões SSH a 3 a cada 30 segundos para SSH usando qualquer endereço:

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

No IBM Virtual Router Appliance, essa regra tem os problemas a seguir:

* A opção para a contagem recente e o horário recente foram descontinuados.

* Devido ao problema anterior, a regra não pode funcionar conforme esperado e bloqueará todas as conexões SSH com a interface aplicada.

### Workarounds
Use o CPP como alternativa.

## Set system conntrack issues

### Issues
```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## Set system conntrack timeout

### Issues
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## Time based firewall

### Issues
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### Issues
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## Specific Application or port broken in S-S Ipsec VPN

### Issues

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote 
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Significant change in logging behavior 

### Issues
Há uma mudança significativa no comportamento de criação de log entre o dispositivo do Vyatta 5400 e o IBM Virtual Router Appliance, da criação de log por sessão até a por pacote.

* Criação de log de sessão: registra transições de estado de sessão stateful

* Criação de log de pacotes: registra todos os pacotes que correspondem à regra. Como a criação de log de pacotes é registrada no arquivo de log em "unidades de pacote", há uma diminuição marcada no rendimento e na pressão da capacidade do disco.

* O recurso de criação de log do vRouter pode ser usado para capturar a atividade de firewall. Como qualquer função de criação de log, será necessário ativá-la apenas se estiver solucionando um problema específico e desativar a criação de log assim que puder.

O firewall stateful que gerencia sessões de Firewall/NAT, grava em "unidades de sessão". É recomendado usar a criação de log de sessão. Cada exemplo de configuração é descrito a seguir:

**Sessão/criação de log**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Firewall de criação de log de pacote**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>` 
