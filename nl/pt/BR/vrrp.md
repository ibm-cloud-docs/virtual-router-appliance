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

# Trabalhando com alta disponibilidade e VRRP
{: #working-with-high-availability-and-vrrp}

O Virtual Router Appliance (VRA) suporta Virtual Router Redundancy Protocol (VRRP) como um protocolo de alta disponibilidade. A implementação de dispositivos é feita de uma maneira ativa/passiva, na qual uma máquina é a principal e a outra é o backup. Todas as interfaces em ambas as máquinas serão um membro do mesmo "sync-group", portanto, se uma interface tiver uma falha, as outras interfaces no mesmo grupo também falharão e o dispositivo deixará de ser principal. O backup atual detectará que o principal não está mais transmitindo mensagens keepalive/pulsação e assumirá o controle dos IPs virtuais VRRP e se tornará principal.

O VRRP é a parte mais importante da configuração ao provisionar Gateways. A funcionalidade de alta disponibilidade depende das mensagens de pulsação, portanto, é crítico certificar-se de que elas não estejam bloqueadas.

## Endereços de IP virtual (VIP) VRRP

O IP virtual VRRP, ou VIP, é o endereço IP flutuante mudado do dispositivo principal para o de backup quando o failover ocorre. Quando um VRA for implementado, ele terá uma conexão de rede ligada pública e privada e IPs reais designados em cada interface. Um VIP é designado em ambas as interfaces também, independentemente de o dispositivo ser independente ou de estar em um par de HA. O tráfego que tem um IP de destino em sub-redes em VLANs associadas ao VRA será enviado diretamente para os VIPs do VRRP.

Os endereços IP virtuais VRRP para qualquer grupo de gateway não devem ser mudados, nem a interface VRRP deve ser desativada. Esses endereços IP são o método pelo qual o tráfego é roteado para o gateway quando uma VLAN está associada. Se o endereço IP não estiver presente, o tráfego não poderá ser encaminhado do BCR/FCR da Softlayer para o próprio gateway.

## Grupo do VRRP

Um grupo do VRRP consiste em um cluster de interfaces ou interfaces virtuais que fornecem redundância para uma interface primária, ou “principal“, no grupo. Cada interface no grupo geralmente está em um roteador separado. A redundância é gerenciada pelo processo VRRP em cada sistema. O grupo do VRRP tem um identificador numérico exclusivo e pode ter até 20 endereços IP virtuais designados.  

O ID do grupo do VRRP é designado pela SoftLayer e não deve ser mudado. Quando um novo grupo de gateway for provisionado por trás de um Front Customer Router (FCR)/Backend Customer Router (BCR) pela primeira vez, ele receberá um grupo do VRRP igual a 1. As provisões subsequentes do grupo de gateway incrementarão esse valor para evitar conflitos, o próximo grupo terá o grupo 2, depois o grupo 3 e assim por diante. Isso é, então, calculado e designado pelo processo de fornecimento do SoftLayer. Alterar esse valor causa o risco de colisão com outros grupos ativos e, então, a contenção principal/principal, que provavelmente causará uma interrupção em ambos os grupos de gateway.

Se você migrar de uma configuração anterior, recomenda-se que verifique sua configuração do código para garantir que o ID do grupo do VRRP não seja designado estaticamente.

O ID do grupo do VRRP é persistido no banco de dados SoftLayer, então o mesmo valor de ID do grupo será usado durante um recarregamento ou upgrade do SO. Qualquer ID do grupo do VRRP modificado pelo usuário será sobrescrito com o valor designado pelo sistema durante um recarregamento do SO.  

## Prioridade do VRRP

A primeira máquina em um grupo de gateway terá uma prioridade de 254 e esse valor é reduzido para o próximo dispositivo provisionado. Você nunca deve configurar uma prioridade como 255, pois isto define o "proprietário do endereço" VIP e pode resultar em comportamento indesejado quando a máquina é colocada on-line com uma configuração que difere da principal ativa em execução.

## Preempção do VRRP

A preempção deve sempre ser desativada em todas as interfaces do VRRP, para que um novo dispositivo ou um que está passando pelo recarregamento de seu SO não tente assumir o cluster.

## Intervalo de aviso do VRRP

Para sinalizar que ela ainda está em serviço, a interface principal ou VIF envia pacotes de “pulsação“ chamados “propagandas“ para o segmento de LAN, usando os endereços multicast designados por IANA para o VRRP (`224.0.0.18` para IPv4 e `FF02:0:0:0:0:0:0:12` para IPv6).

Por padrão, o intervalo é configurado como 1. Esse valor pode ser aumentado, mas não é recomendado que você configure um valor acima de 5. Em uma rede ocupada, pode levar muito mais tempo do que um segundo para que todos os avisos do VRRP cheguem no dispositivo de backup do principal, portanto, um valor igual a 2 deve ser utilizado para redes de alto tráfego.

## Sincronização do VRRP (sync-group)

As interfaces em um grupo de sincronização do VRRP (“grupo de sincronização”) serão sincronizadas de modo que, se uma das interfaces no grupo efetuar failover para o backup, todas as interfaces no grupo efetuem failover para o backup. Por exemplo, em muitos casos, se uma interface em um roteador principal falhar, o roteador inteiro efetuará failover para um roteador de backup.

Esse valor é diferente do grupo do VRRP, pois ele define quais interfaces em um dispositivo efetuarão failover quando uma interface nesse grupo registrar uma falha. É recomendado que todas as interfaces pertençam ao mesmo grupo de sincronização, caso contrário, algumas interfaces serão principal e terão IPs de gateway, e outras serão de backup e o tráfego não será mais encaminhado corretamente. Configurações ativo/ativo não são suportadas.

## rfc-compatibility do VRRP

A compatibilidade RFC ativa ou desativa endereços MAC RFC 3768 para VRRP em uma interface. Isso deve ser ativado nas interfaces nativas, mas não ativado em quaisquer VIFs configurados. Alguns comutadores virtuais (principalmente vmware) têm problemas com isso sendo ativado e farão com que o tráfego seja eliminado e não enviado para o IP de gateway da máquina host do hypervisor. Deixe essa configuração sozinha e não defina a configuração para nenhum VIF.

## Autenticação do VRRP
Se uma senha for configurada para autenticação do VRRP, o tipo de autenticação também deverá ser definido. Se a senha for configurada e o tipo de autenticação não for definido, o sistema gerará um erro quando você tentar confirmar a configuração.

Da mesma forma, não é possível excluir a senha do VRRP sem também excluir o tipo de autenticação do VRRP. Se você o fizer, o sistema gerará um erro quando você tentar confirmar a configuração. Se você excluir a senha de autenticação e o tipo de autenticação do VRRP, a autenticação do VRRP será desativada.

O IETF decidiu que a autenticação não deve ser usada para a versão 3 do VRRP. Para obter mais informações, consulte RFC 5798 VRRP.

## Suporte da Versão 2/3
O VRA suporta ambos os protocolos do VRRP, versão 2 (padrão) e versão 3. Na versão 2, não é possível ter IPv4 e IPv6 juntos. Mas, na Versão 3, é possível ter IPv4 e IPv6 ao mesmo tempo.

## VPN de Alta Disponibilidade com VRRP
O VRA fornece a capacidade de manter a conectividade por meio de um túnel IPsec usando um par de Virtual Router Appliances com VRRP. Quando um roteador falhar ou for encerrado para manutenção, o novo roteador principal VRRP restaurará a conectividade IPsec entre as redes locais e remotas.

Ao configurar VPN de Alta Disponibilidade com VRRP, sempre que um endereço virtual VRRP é incluído em uma interface de VRA, deve-se reinicializar o daemon IPsec porque o serviço IPsec atende apenas conexões com endereços que estão presentes no VRA quando o daemon de serviço Internet Key Exchange (IKE) é inicializado.

Execute o comando a seguir nos roteadores principal e de backup de forma que, quando o failover acontecer, os daemons do IPsec sejam reiniciados em um novo dispositivo principal após a transição do VIP, conforme mostrado no exemplo a seguir:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
`

## Firewalls de Alta Disponibilidade com o VRRP

Quando dois dispositivos estão em um par de HA, deve-se tomar cuidado em seu dispositivo principal para que você não bloqueie o acesso do outro dispositivo. A porta 443 deve ser permitida entre ambos os dispositivos para que a sincronização de configuração funcione e o VRRP deve ter permissão para ser enviado e recebido, incluindo o intervalo multicast de 224.0.0.0/24.

## Sub-redes de VLAN associadas ao VRRP

Para qualquer configuração de VIF com VRRP, será necessário um endereço IP virtual (o primeiro IP utilizável na sub-rede, o IP do gateway para o qual tudo nessa sub-rede será roteado) e também um endereço IP da interface real para o VIF em ambos os dispositivos. Para conservar IPs utilizáveis, é recomendado que os IPs de interface reais usem um intervalo fora da banda, tal como 192.168.0.0/30, no qual um dispositivo tem .1 e o outro dispositivo tem o endereço .2. Você pode ter vários IPs virtuais de VRRP, mas precisará de apenas um endereço real em cada vif.

Aqui está um exemplo de uma configuração do VRRP com duas VLANs privadas e três sub-redes:

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* Um grupo de sincronização VRRP é diferente de um grupo VRRP. Quando uma interface que pertence a um grupo de sincronização mudar de estado, todos os outros membros do mesmo grupo de sincronização serão transicionados para o mesmo estado.
* O número do grupo VRRP das interfaces VLAN (VIFs) não precisa ser o mesmo que uma das interfaces nativas ou das outras VLANs. No entanto, é altamente sugerido manter todos os endereços virtuais da mesma VLAN em um grupo VRRP, conforme visto na VLAN 10.
* Os endereços da interface real nas VLANs nativas (por exemplo, dp0bond1: 169.110.20.28/29) nem sempre estão na mesma sub-rede que seus VIPs (169.110.21.26/29).

## Failover manual do VRRP
Se for necessário forçar um failover do VRRP, ele poderá ser obtido executando o seguinte no dispositivo que atualmente é o VRRP principal:

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

O ID do grupo é o ID do grupo de vrrp das interfaces nativas e, conforme mencionado acima, pode ser diferente em seu par.

## Sincronização de conexão
Quando dois dispositivos VRA estão em um par de HA, pode ser útil rastrear conexões stateful entre os dois dispositivos para que, se um failover ocorrer, o estado atual de todas as conexões que estão no dispositivo com falha seja replicado para o dispositivo de backup, para que não seja necessário que quaisquer sessões ativas atuais (como conexões SSL) sejam reconstruídas a partir do zero, o que pode resultar em uma experiência do usuário comprometida.

Isso é chamado de sincronização de rastreamento de conexão.

Para configurá-lo, deve-se declarar qual é o método de failover, qual interface você utilizará para enviar as informações de rastreamento de conexão e o IP do peer remoto:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

O outro VRA terá a mesma configuração, mas um `remote-peer` diferente.

Observe que isso poderá saturar um link se houver um número alto de conexões entrando em outras interfaces e competirá com outro tráfego no link declarado.

## Recurso de atraso de início do VRRP
O S.O. Vyatta versão 1801p e superior inclui um novo comando `vrrp`.

`vrrp` especifica um protocolo de eleição que designa dinamicamente a responsabilidade de um roteador virtual a um dos roteadores VRRP em uma LAN. O roteador VRRP que controla os endereços IPv4 ou IPv6 associados a um roteador virtual é chamado Principal e encaminha pacotes enviados para esses endereços IPv4 ou IPv6. O processo de eleição fornece failover dinâmico na responsabilidade de encaminhamento, caso o Principal se torne indisponível. Todo o sistema de mensagens de protocolo é executado usando datagramas multicast IPv4 ou IPv6; como resultado, o protocolo pode operar em uma variedade de tecnologias de LAN multiacesso que suportam multicast IPv4/IPv6.

Para minimizar o tráfego de rede, apenas o Principal de cada roteador virtual envia mensagens periódicas de Propaganda de VRRP. Um roteador de Backup não tentará priorizar o Principal, a menos que ele tenha prioridade mais alta. Isso elimina a interrupção do serviço, a menos que um caminho mais preferencial se torne disponível. Também é possível proibir, administrativamente, todas as tentativas de preempção. Se o Principal se tornar indisponível, o Backup com prioridade mais alta será transicionado para Principal após um breve atraso, fornecendo uma transição controlada da responsabilidade do roteador virtual com interrupção mínima do serviço.

**NOTA:** em implementações provisionadas do IBM© Cloud, o valor de atraso de início é configurado para o valor padrão. Talvez você queira alterar isso a seu critério ao testar seus métodos de failover e de alta disponibilidade.


### Preempção vs. sem preempção

O protocolo `vrrp` define a lógica que decide qual peer do VRRP em uma rede tem a prioridade mais alta e, como tal, o melhor peer para executar a função como Principal. Com uma configuração padrão, o VRRP será ativado para executar preempção, o que significa que um novo peer de prioridade mais alta na rede forçará o failover da Função principal.

Quando a preempção for desativada, um peer de prioridade mais alta só executará failover da função Principal se o peer de prioridade inferior existente não estiver mais disponível na rede. A desativação da preempção é, às vezes, útil em cenários do mundo real, já que é melhor lidar com situações em que o peer de prioridade mais alta pode ter começado a oscilar periodicamente devido a problemas de confiabilidade com o próprio peer ou uma de suas conexões de rede. Também é útil evitar o failover prematuro em um novo peer de prioridade mais alta que não tenha concluído a convergência de rede.

### Suposições e limitações da preempção

Se os peers do VRRP tiverem sido configurados para desativar a preempção, haverá alguns casos em que a preempção poderá “parecer” ocorrer, mas, na realidade, o cenário é apenas um failover padrão do VRRP. Conforme descrito acima, o VRRP usa datagramas IP multicast como um meio de confirmar a disponibilidade do roteador Principal atualmente selecionado. Como é um protocolo da camada 3 que está detectando falha de um peer VRRP, é importante que a detecção de failover no VRRP seja atrasada até que o VRRP e a camada 1 a 2 da pilha de rede estejam prontas e convergidas. Em alguns casos, a interface de rede que está executando o VRRP pode confirmar para o protocolo que a interface está ativa, mas outros serviços subjacentes, como a Árvore de amplitude ou a Ligação, podem não ter sido concluídos. Como resultado, a conectividade IP entre os peers não pode ser estabelecida. Se isso ocorrer, o VRRP em um novo peer mais alto se tornará Principal, já que não será possível detectar mensagens VRRP do peer principal atual na rede. Após a convergência, o breve período de tempo em que há dois peers VRRP Principais resultará na lógica dual do Principal de VRRP sendo executada, o peer de prioridade mais alta permanecerá o Principal e o de prioridade mais baixa se tornará o de Backup. Esse cenário pode “aparecer” para demonstrar uma falha na funcionalidade “sem preempção”.

### Recurso de atraso de início

Para acomodar os problemas associados com atraso na convergência dos níveis mais baixos da pilha de rede durante um evento de interface, bem como outros fatores contributivos, um novo recurso chamado “Atraso de Inicialização” é introduzido na correção 1801p. O recurso faz com que o estado do VRRP em uma máquina que foi “recarregada” permaneça no estado INIT até depois de um atraso predefinido, que pode ser configurado pelo operador de rede. A flexibilidade nesse valor de atraso permite que o operador de rede customize as características de sua rede e dispositivos em condições do mundo real.

### Detalhes do comando

```
vrrp {
start-delay <0-600>
}
```

`start-delay` pode ter um valor entre 0 (padrão) e 600 segundos.

### Configuração de exemplo

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```
