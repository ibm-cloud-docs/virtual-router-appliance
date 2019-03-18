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

# Configurando regras de NAT no Vyatta 5400
{: #setting-up-nat-rules-on-vyatta-5400}

Este tópico contém exemplos das regras de Conversão de endereço de rede (NAT) usadas em um Vyatta.

## Regra NAT uma para muitas (mascarada)

Insira os comandos a seguir no prompt:

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

A solicitação de conexão de máquinas na rede `10.xxx.xxx.xxx` é mapeada para o IP em bond1 e recebe uma porta efêmera associada ao sair. A intenção é designar números maiores de regras mascaradas uma para muitas para que não entrem em conflito com regras NAT menores que você possa ter.

**NOTA:** deve-se configurar o servidor para transmitir seu tráfego de Internet por meio do VRA para que seu gateway padrão seja o endereço IP privado da virtual LAN (VLAN) gerenciada. Por exemplo, para `bond0.2254`, o gateway é `10.52.69.201`. Esse deve ser o endereço do gateway para o servidor que transmite o tráfego da Internet.

**NOTA:** use o comando a seguir para ajudar a solucionar problemas de NAT: 

'''
run show nat source translations detail 
'''

## Regra NAT uma para uma

Os comandos abaixo mostram como configurar uma regra NAT uma para uma. Observe que os números das regras são configurados para serem menores que a regra mascarada. Isso é para que as regras uma para uma tenham precedência sobre as regras uma para muitas.

**NOTA:** os endereços IP mapeados um para um não podem ser mascarados. Se você converte uma entrada de IP, deve-se converter essa saída de IP para que o tráfego siga em ambas as direções.

Os comandos a seguir são para uma regra de origem e de destino. Digite `show nat` no modo de configuração para ver o tipo de regra NAT.

**NOTA:** use o comando a seguir para ajudar a resolver o NAT: `run show nat source translations detail`. 

Insira os comandos a seguir no prompt depois de assegurar-se de que está no modo de configuração:

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

Se o tráfego vier no IP `50.97.203.227` em bond1, esse IP será mapeado para o IP `10.52.69.202` (em qualquer interface definida). Se o tráfego sair com o IP de `10.52.69.202` (em qualquer interface definida), ele será convertido no IP `50.97.203.227` e continuará de saída na interface bond1.

**NOTA:** os endereços IP mapeados um para um não podem ser mascarados. Se você converte uma entrada de IP, deve-se converter essa mesma saída de IP para que seu tráfego siga em ambas as direções.


## Incluindo intervalos de IP por meio de seu VRA

Dependendo de sua configuração do VRA, talvez você queira aceitar endereços IP específicos do IBM© Cloud. 

Novas implementações do vRouter vêm com endereços IP de rede de serviços do IBM Cloud definidos em uma regra de firewall chamada `SERVICE-ALLOW`.

A seguir está um exemplo de `SERVICE-ALLOW`. Isso não é um conjunto de regras IP privado completo.

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

Depois de definir as regras de firewall, será possível designá-las como você achar melhor. Dois exemplos são listados abaixo. 

Aplicando a uma zona:

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Aplicando a uma interface de ligação:

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`
