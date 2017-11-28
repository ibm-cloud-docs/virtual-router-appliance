---

copyright:
  anos: 2017
última atualização: "12-10-2017"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gerenciar Firewalls
O Virtual Router Appliance (VRA) tem a capacidade de processar regras de firewall para proteger as VLANs roteadas por meio do dispositivo. Os firewalls no VRA podem ser divididos em duas etapas:

1. Definindo um ou mais conjuntos de regras
2. Aplicando um conjunto de regras em uma interface ou uma zona. Uma zona consiste em uma ou mais interfaces de rede.

É importante testar regras de firewall após a criação para assegurar que as regras funcionem conforme pretendido e que as novas regras não restrinjam o acesso administrativo ao dispositivo.

Ao manipular as regras na interface `dp0bond1`, é aconselhável se conectar ao dispositivo usando o `dp0bond0`. Conectar-se ao console usando o Intelligent Platform Management Interface (IPMI) também é uma opção.

## Stateless versus Stateful
Por padrão, o firewall é stateless, mas pode ser configurado como stateful se necessário. Um firewall stateless precisará de regras para o tráfego em ambas as direções, enquanto firewalls stateful são para conexões nas quais apenas regras de tráfego de entrada são necessárias. Para configurar um firewall stateful, deve-se especificar conexões relacionadas ou estabelecidas para o tráfego de saída.

Para tornar todos os firewalls 'stateful', execute os comandos a seguir:

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Para tornar individuais firewalls 'stateful':

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```

## Conjuntos de Regras de Firewall
As regras de firewall são agrupadas em conjuntos nomeados para tornar a aplicação de regras para várias interfaces mais fácil. Cada conjunto de regras tem uma ação padrão associada a ele. Considere o exemplo a seguir: 
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1 set security firewall name ALLOW_LEGACY rule 2 action drop set security firewall name ALLOW_LEGACY rule 2 destination port 23 set security firewall name ALLOW_LEGACY rule 2 log set security firewall name ALLOW_LEGACY rule 2 protocol tcp set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

No conjunto de regras, `ALLOW_LEGACY`, há duas regras definidas. A primeira regra elimina qualquer tráfego originado de um grupo de endereços denominado `network-group1`. A segunda regra descarta e registra qualquer tráfego destinado para a porta telnet (`tcp/23`) do grupo de endereços denominado `network-group2`. A ação padrão indica que qualquer outra coisa é aceita.

## Permitindo o Acesso ao Data Center
A IBM oferece várias sub-redes IP para fornecer serviços e suporte para sistemas em execução dentro do data center. Por exemplo, os serviços do resolvedor de DNS estão em execução em `10.0.80.11` e `10.0.80.12`. Outras sub-redes são usadas durante o fornecimento e o suporte. É possível localizar os intervalos de IP usados nos data centers neste [artigo](http://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall).

^^ Precisamos mudar esse link... O KnowledgeLayer será encerrado ^^
^^ Somente quando as mesmas informações king estão no doc do bluemix ou em outro local. Podemos corrigir isso mais tarde ^^

É possível permitir acesso ao data center colocando as regras `SERVICE-ALLOW` adequadas no início dos conjuntos de regras de firewall com uma ação igual a `aceitar`. Onde o conjunto de regras deve ser aplicado depende do roteamento e do design de firewall que estão sendo implementados.

É recomendado que você coloque as regras de firewall no local que causa menos duplicação de trabalho. Por exemplo, permitir a entrada de sub-redes de backend em `dp0bond0` seria menos trabalhoso do que permitir a saída de sub-redes de backend em direção a cada interface virtual de VLAN.

### Regras de Firewall por Interface
Um método para configurar o firewall em um VRA é aplicar conjuntos de regras de firewall em cada interface. Nesse caso, uma interface pode ser uma interface de plano de dados (`dp0s0`) ou uma interface virtual (`dp0bond0.303`). Cada interface possui três designações de firewall possíveis:

`in` O firewall é verificado com relação aos pacotes que entram por meio da interface. Esses pacotes podem atravessar ou ser destinados ao VRA.

`out` O firewall é verificado com relação aos pacotes que saem por meio da interface. Esses pacotes podem atravessar ou se originar no VRA.

`local` O firewall é verificado com relação aos pacotes que são destinados diretamente para o VRA.

Uma interface pode ter vários conjuntos de regras aplicados em cada direção. Eles são aplicados na ordem de configuração. Observe que não é possível para o tráfego de firewall se originar no dispositivo VRA usando firewalls por interface.

Como um exemplo, para designar o conjunto de regras `ALLOW_LEGACY` para a opção `in` para a interface `bp0s1`, você usaria o comando de configuração: 

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Policiamento de Plano de Controle (CPP)
O policiamento de plano de controle (CPP) fornece proteção contra ataques no Virtual Router Appliance, permitindo que você configure as políticas de firewall que são designadas às interfaces desejadas e aplicando essas políticas em pacotes que entram e saem do VRA.

O CPP é implementado quando a palavra-chave `local` é usada em políticas de firewall que são designadas a qualquer tipo de interface de VRA, tais como interfaces de plano de dados ou loopback. Diferentemente das regras de firewall aplicadas para pacotes que atravessam o VRA, a ação padrão de regras de firewall para o tráfego que entra ou sai do plano de controle é `Permitir`. Os usuários deverão incluir regras de descarte explícitas se o comportamento padrão não será desejado.

O VRA fornece um conjunto de regras de CPP básicas como modelo. É possível mesclá-lo com sua configuração executando: 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

Após esse conjunto de regras ser mesclado, um novo conjunto de regras de firewall denominado `CPP` é incluído e aplicado à interface lookback. É recomendado que você modifique esse conjunto de regras para adequar seu ambiente.

## Definindo o Firewall da Zona
Outro conceito de firewall dentro do Virtual Router Appliance são firewalls baseados em zona. Na operação de firewall baseado em zona, uma interface é designada a uma zona (apenas uma zona por interface) e conjuntos de regras de firewall são designados para os limites entre zonas com a ideia de que todas as interfaces dentro de uma zona possuem o mesmo nível de segurança e têm permissão para rotear livremente. O tráfego é analisado apenas quando ele está passando de uma zona para outra. As zonas descartam qualquer tráfego que entre nelas que não seja explicitamente permitido.

Uma interface pode pertencer a uma zona ou ter uma configuração de firewall por interface; uma interface não pode fazer os dois.

Imagine o seguinte cenário de escritório com três departamentos, cada departamento com sua própria VLAN: 

* Departamento A - VLANs 10 e 20 (interface dp0bond1.10 e dp0bond1.20)
* Departamento B - VLANs 30 e 40 (interface dp0bond1.30 e dp0bond1.40)
* Departamento C - VLAN 50 (interface dp0bond1.50)

Uma zona pode ser criada para cada departamento e as interfaces para esse departamento podem ser incluídas na zona. O exemplo a seguir ilustra isso: 
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20  set security zone-policy zone DEPARTMENTB interface dp0bond1.30  set security zone-policy zone DEPARTMENTB interface dp0bond1.40  set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

O comando `commit` preenche cada zona como uma interface e as regras de descarte padrão descartam qualquer tráfego que tente entrar na zona a partir de fora. No exemplo, VLAN 10 e 20 podem transmitir o tráfego, pois estão na mesma zona (`DEPARTMENTA`), mas VLAN 10 e VLAN 30 não podem transmitir o tráfego porque a VLAN 30 está em uma zona diferente (`DEPARTMENTB`).

As interfaces dentro de cada zona podem transmitir o tráfego livremente e as regras podem ser definidas para interação entre as zonas. Um conjunto de regras é configurado do ponto de vista de sair de uma zona para outra zona. O comando a seguir mostra um exemplo de como configurar uma regra:

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING `

Esse comando associa a transição de DEPARTMENTC para DEPARTMENTB com o conjunto de regras denominado `ALLOW_PING`. O tráfego que entra na zona DEPARTMENTB da zona DEPARTMENTC será verificado com relação a este conjunto de regras.

É importante entender que essa designação da zona DEPARTMENTC indo para a zona DEPARTMENTB não faz nenhuma declaração sobre o inverso. Se não houver regras que permitam o tráfego da zona DEPARTMENTB para a zona DEPARTMENTC, o tráfego (respostas do ICMP) não voltará para os hosts em DEPARTMENTC.

## Criação de Log de Sessão e Pacote
O VRA suporta dois tipos de criação de log:
1. Criação de log de sessão. Use o comando ``security firewall session-log`` para configurar a criação de log de sessão do firewall.
  
  Para UDP, ICMP e todos os fluxos não TCP, uma sessão é transicionada para quatro estados durante o tempo de vida do fluxo. Para cada transição, é possível configurar o VRA para registrar uma mensagem. O TCP tem um número maior de transições de estado, cada uma das quais pode ser configurada para log.  

*	Criação de log por pacote. Inclua a palavra-chave ``log`` na regra de firewall ou NAT para registrar cada pacote de rede que corresponde à regra.

  A criação de log por pacote ocorre nos caminhos de encaminhamento do pacote e gera grandes quantidades de saída. Ela pode reduzir significativamente o rendimento do VRA e aumentar dramaticamente o espaço em disco usado para os arquivos de log. Recomendamos usar a criação de log por pacote SOMENTE para propósitos de depuração. Para todos os propósitos operacionais, a criação de log de sessão stateful deve ser usada.
