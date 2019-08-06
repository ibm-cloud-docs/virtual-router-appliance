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

# Incluir funções de firewall no {{site.data.keyword.vra_full}} (stateless e stateful)
Aplicar conjuntos de regras de firewall a cada interface é um método de aplicação de firewall ao usar o {{site.data.keyword.vra_full}} (VRA). Cada interface possui três possíveis instâncias de firewall - Dentro, Fora e Local - e cada instância tem regras que podem ser aplicadas a ela. 

A ação padrão é Descartar, com regras que permitem que o tráfego específico seja aplicado de acordo com a regra 1 a N. Assim que uma correspondência for feita, o firewall aplicará a ação específica da regra de correspondência.

Para qualquer uma das três instâncias de firewall abaixo, somente uma pode ser aplicada.

**IN:** o firewall filtra pacotes que entram na interface e atravessam o sistema. Será necessário permitir que certos intervalos de IP tenham acesso ao front-end e backend para gerenciamento (ping, monitoramento e assim por diante).

**OUT:** o firewall filtra pacotes que saem da interface. Esses pacotes podem atravessar o sistema VRA ou serem originados no sistema.

**LOCAL:** o firewall filtra pacotes destinados para o próprio sistema VRA usando a interface do sistema. É necessário estabelecer restrições em portas de acesso que chegam no dispositivo por meio de endereços IP externos não restritos.

Use as etapas a seguir para configurar uma regra de firewall de exemplo para desativar o Internet Control Message Protocol (ICMP) (ping - IPv4 echo reply message) para suas interfaces do {{site.data.keyword.vra_full}} (essa é uma ação stateless; uma ação stateful será revisada posteriormente):

1. Digite `show configuration commands` no prompt de comandos para ver as configurações definidas. Você verá uma lista de todos os comandos configurados em seu dispositivo (que poderá ser útil se você decidir migrar e desejar ver todas as suas configurações). Observe o comando `set firewall all-ping enable`, que indica que o ICMP ainda está ativado para o seu Dispositivo.

2. Digite `configure`.

3. Digite `set firwall all-ping disable`.

4. Digite `commit`.

Se você agora tentar executar ping do dispositivo VRA, não receberá mais uma resposta.

Para designar regras de firewall ao tráfego roteado, as regras deverão ser aplicadas às interfaces ou à criação de zonas do VRA e, em seguida, aplicadas às zonas.

Para este exemplo, as zonas serão criadas para as VLANs que foram usadas até aqui.

 VLAN | Zona 
 ---- | ---- 
bond1 | dmz
bond1.2007 | prod (para produção)
bond0.2254 | private (para desenvolvimento)
bond1.1280 | reservado para uso futuro
bond1.1894 | reservado para uso futuro

## Criar regras de firewall
Antes que as zonas sejam realmente criadas, é uma boa ideia criar as regras de firewall que devem ser aplicadas às zonas. Criar as regras antes das zonas permite aplicá-las imediatamente, versus criar a zona, depois as regras e voltar para a zona para aplicação da regra.

Os comandos a seguir irão:

* Criar uma regra de firewall denominada `dmz2private` com a ação padrão para descartar qualquer pacote.
* Ativar a criação de log de tráfego aceito e negado para a regra denominada `dmz2private`.

1. Digite `configure` no prompt de comandos.

2. Insira os comandos a seguir:

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. Insira o próximo conjunto de comandos para que iptables permita que o tráfego para uma sessão estabelecida retorne. Por padrão, iptables não permite isso, que é o motivo pelo qual uma regra explícita é necessária.

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. Insira os comandos a seguir para permitir que TCP/UDP passe pela porta 22, o padrão para SSH.
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. Insira os próximos comandos para permitir que TCP/UDP passe pela porta 80, o padrão para HTTP.

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. Digite `commit` para assegurar-se de que todas as regras sejam executadas quando concluídas.

7. Visualize a nova configuração digitando `show firewall name dmz2private` no prompt de comandos.

8. Crie uma regra de firewall para ser aplicada à zona dmz inserindo os comandos a seguir no prompt. A regra será denominada pública. 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## Criar zonas

Zonas são representações lógicas de uma interface. Nesta seção, você irá:

* Criar uma zona e uma política denominada dmz com uma ação padrão para descartar pacotes destinados para essa zona.
* Configurar a política dmz para usar a interface `bond1`
* Configurar a política prod para usar a interface `bond1.2007`
* Criar uma política de zona denominada `private` com uma ação padrão para descartar pacotes destinados para essa zona.
* Configurar a política denominada `private` para usar a interface `bond0.2254`.

1. Insira os comandos a seguir no prompt:

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. Use os comandos a seguir para configurar a política de firewall para as zonas:

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
Será possível aplicar uma regra de firewall a uma interface específica se você não desejar aplicá-la a uma política de zona. Use os comandos abaixo para aplicar uma regra a uma interface.

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~
