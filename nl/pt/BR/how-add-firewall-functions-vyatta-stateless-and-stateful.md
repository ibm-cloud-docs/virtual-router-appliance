---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Incluir funções de firewall no Vyatta 5400 (stateless e stateful)

A aplicação de conjuntos de regras de firewall em cada interface é um método de aplicação de firewall ao usar os dispositivos Brocade 5400 vRouter (Vyatta). Cada interface possui três possíveis instâncias de firewall - Dentro, Fora e Local - e cada instância tem regras que podem ser aplicadas a ela. A ação padrão é Descartar, com regras que permitem que o tráfego específico seja aplicado de acordo com a regra 1 a N. Assim que uma correspondência for feita, o firewall aplicará a ação específica da regra de correspondência.

Para qualquer uma das três instâncias de firewall abaixo, **somente uma** pode ser aplicada.

* **IN:** o firewall filtra pacotes que entram na interface e atravessam o sistema Brocade. Será necessário permitir que certos intervalos de IP do SoftLayer tenham acesso ao front-end e backend para gerenciamento (ping, monitoramento e assim por diante).
* **OUT:** o firewall filtra pacotes que saem da interface. Esses pacotes podem estar atravessando o sistema Brocade ou sendo originados no sistema.
* **LOCAL:** o firewall filtra pacotes destinados para o próprio sistema Brocade vRouter por meio da interface do sistema. É necessário estabelecer restrições em portas de acesso que chegam no dispositivo Brocade vRouter por meio de endereços IP externos não restritos.

Use as etapas a seguir para configurar uma regra de firewall de exemplo para desativar o Internet Control Message Protocol (ICMP) *(ping - IPv4 echo reply message)* para suas interfaces do Brocade 5400 vRouter (essa é uma ação stateless; uma ação stateful será revisada posteriormente):

1\. Digite o comando *show configuration* no prompt de comandos para ver quais configurações estão definidas. Você verá uma lista de todos os comandos configurados em seu dispositivo (que poderá ser útil se você decidir migrar e desejar ver todas as suas configurações). Observe o comando *set firewall all-ping 'enable'*, que informa que o ICMP ainda está ativado para o seu Dispositivo.

2\. Digite *configure*.

3\. Digite *set firwall all-ping 'disable'*.

4\. Digite *commit*.

Se você tentar executar ping do dispositivo Brocade 5400 vRouter agora, não receberá mais uma resposta.

Para designar regras de firewall ao tráfego roteado, as regras deverão ser aplicadas às **interfaces** ou **criação de zonas** do Brocade 5400 vRouter e aplicadas às zonas.

Para este exemplo, as zonas serão criadas para as VLANs que foram usadas até aqui.

**VLAN = Zone**

bond1 = dmz

bond1.2007 = prod (para produção)

bond0.2254 = private (para desenvolvimento)

bond1.1280 = reservado para uso futuro

bond1.1894 = reservado para uso futuro

**Criar regras de firewall**

Antes que as zonas sejam realmente criadas, é uma boa ideia criar as regras de firewall que devem ser aplicadas às zonas. Criar as regras antes das zonas permite aplicá-las imediatamente, versus criar a zona, criar as regras e ter que voltar para a zona para aplicação da regra.

**NOTA:** zonas e conjuntos de regras têm uma instrução de ação padrão. Ao usar Políticas de zona, a ação padrão é configurada pela instrução zone-policy e é representada pela regra 10.000. Também é importante lembrar-se de que a ação padrão de um conjunto de regras de firewall é **descartar** todo o tráfego.

Os comandos a seguir irão:

* Criar uma regra de firewall denominada **dmz2private** com a ação padrão para descartar qualquer pacote.
* Ativar a criação de log de tráfego aceito e negado para a regra denominada **dmz2private**.


1\. Digite *configure* no prompt de comandos.

2\. Insira os comandos a seguir no prompt:

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

O próximo conjunto de comandos é ativar **iptables** para permitir que o tráfego para uma sessão estabelecida retorne. Por padrão, **iptables** não permite isso, que é o motivo pelo qual uma regra explícita é necessária.

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

O terceiro conjunto de comandos permite que TCP/UDP passe pela porta 22, o padrão para SSH.

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

O conjunto final de comandos permite que TCP/UDP passe pela porta 80, o padrão para HTTP.

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. Digite *commit* para assegurar-se de que todas as regras sejam executadas quando concluídas.

4\. Visualize sua configuração digitando *show firewall name dmz2private* no prompt de comandos.

A próxima regra de firewall que criarmos será aplicada à nossa zona **dmz**. A regra será denominada **pública**. Insira os comandos a seguir no prompt.

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**NOTA:** as regras de firewall precisam do fluxo de saída de **prod** para **dmz**. Use o comando a seguir para ajudar a solucionar problemas de fluxo de rede: *sudo tcpdump -i any host 10.52.69.202*.

**Criar zonas**

As zonas são a representação lógica de uma interface. Os comandos a seguir irão:

* Criar uma zona e uma política denominada **dmz** com uma ação padrão para descartar pacotes destinados para essa zona.
* Configurar a política **dmz** para usar a interface **bond1**.
* Configurar a política **prod** para usar a interface **bond1.2007**.
* Criar uma política de zona denominada **private** com uma ação padrão para descartar pacotes destinados para essa zona.
* Configurar a política denominada **private** para usar a interface **bond0.2254**.

1\. Insira os comandos a seguir no prompt:

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. Use os comandos a seguir para configurar a política de firewall para as zonas:

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

Observe que será possível aplicar uma regra de firewall a uma interface específica se você não desejar aplicá-la a uma política de zona. Use os comandos abaixo para aplicar uma regra a uma interface.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## Firewalls stateful

Um firewall *stateful* mantém uma tabela de fluxos vistos anteriormente e os pacotes podem ser aceitos ou descartados de acordo com sua relação com pacotes anteriores. Como regra geral, firewalls stateful serão geralmente preferenciais onde o tráfego de aplicativo for predominante. 

<span style="text-decoration: underline">*O Brocade 5400 vRouter não rastreia o estado das conexões com configuração padrão. O firewall será stateless até que uma das condições a seguir tenha sido atendida:*</span>

* A configuração de um firewall e de qualquer regra tem um parâmetro state configurado
* A configuração de um firewall state-policy
* A configuração de NAT
* A ativação do serviço de proxy da web transparente
* A ativação de uma configuração de balanceamento de carga da WAN

## Firewalls stateless

Um firewall *stateless* considera cada pacote isoladamente. Os pacotes podem ser aceitos ou descartados de acordo somente com os critérios básicos da lista de controle de acesso (ACL), como os campos de origem e de destino nos cabeçalhos de IP ou do Transmission Control Protocols/User Datagram Protocol (TCP/UDP). Um Brocade 5400 vRouter stateless não armazena informações de conexão e não tem nenhum requisito para consultar a relação de cada pacote com fluxos anteriores, ambos os quais consomem pequenas quantias de memória e tempo de CPU. O desempenho do encaminhamento bruto é portanto melhor em um sistema stateless. O Brocade recomenda manter o roteador stateless para melhor desempenho se você não requerer os recursos específicos para a condição stateful.
