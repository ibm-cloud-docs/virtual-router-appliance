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
{:faq: data-hd-content-type='faq'}

# FAQ
A seguir estão as perguntas mais frequentes ao trabalhar com o IBM Virtual Router Appliance (VRA).

## O que é um VRA? 
{:faq}

Um Virtual Router Appliance (VRA) permite que um cliente IBM Cloud roteie seletivamente o tráfego de rede privada e pública por meio de um roteador corporativo completo com firewall, formato de tráfego, roteamento baseado em política, VPN e uma série de outros recursos. Todos os recursos do VRA são gerenciados pelo cliente. O VRA fornece a um cliente IBM Cloud um grau de controle normalmente reservado para redes locais.

## O que é um Dispositivo de Gateway? 
{:faq}

Um acessório Dispositivo de Gateway permite usar o portal da web ou a API para escolher segmentos de rede (VLANs) para rotear por meio de um VRA. É possível mudar as seleções de VLAN a qualquer momento. O Dispositivo de Gateway também manipula a alta disponibilidade (HA) do VRA, configurando um segundo VRA para assumir o controle se o primeiro falhar.

## Às vezes vejo referências a termos como "Vyatta" e "vRouter". Como eles se relacionam com o VRA?
{:faq}

Vyatta era um software livre, software roteador baseado em PC que foi adquirido integralmente e transicionado para origem fechada. Hoje, "Vyatta" e "Vyatta OS" descrevem as adaptações de software comercial derivadas do projeto de origem fechada. O IBM VRA incorpora elementos do Vyatta OS, juntamente com importantes aprimoramentos de recursos e serviços disponíveis exclusivamente por meio do IBM Cloud.

"vRouter" foi uma reestruturação da marca de curta duração de Vyatta por seu então proprietário. Quando visto na documentação, ele pode ser considerado sinônimo de Vyatta.

## O Vyatta 5400 ainda é suportado?
{:faq}

A IBM não suportará mais o Vyatta 5400 a partir de 31 de março de 2019.

## Quais melhorias o Virtual Router Appliance (Vyatta 5600) têm com relação ao Vyatta 5400?
{:faq}

O Vyatta 5600 oferece os aprimoramentos a seguir com relação ao Vyatta 5400:

- Rendimento mais rápido, em até 10 Gbps por núcleo de CPU
- Rendimento aumentado em mais de 3x por sessão de VPN IPsec (Padrões de Criptografia Avançados)
- Aumento de capacidade para roteadores, fluxos e conexões simultâneas
- Suporte de padrões atualizado, incluindo Layer 2 Tunneling Protocol, Versão 3 (L2TPv3), Internet Key Exchange, Versão 2 (IKEv2), Secure Hash Algorithm 2 (SHA-2) e encapsulamento de tunelamento 802.1Q (Q-in-Q)

## E a oferta AT&T vRouter 5600?
{:faq}

A AT&T (anteriormente Brocade) anunciou o Término de Vida e o Fim do Suporte de sua oferta Brocade vRouter 5600. Embora o Brocade vRouter 5600 forneça a capacidade de tecnologia subjacente para o IBM Virtual Router Appliance, este anúncio não se aplica aos clientes IBM. Os clientes IBM continuarão tendo suporte no uso desta nova oferta.

## Como o VRA é entregue? 
{:faq}

Você obtém um VRA solicitando um Gateway de Rede. Esse processo simplificado permite escolher um data center e um servidor VRA adequado, bem como se você deseja implementar um par de HA de VRAs. Servidores, sistemas operacionais e o acessório Dispositivo de Gateway são todos fornecidos automaticamente. Quando o fornecimento é concluído, é possível usar a interface do Dispositivo de Gateway para rotear VLANs por meio do VRA. É possível configurar seu servidor VRA diretamente usando SSH (shell seguro) com as senhas fornecidas na seção Detalhes do Hardware do Portal do Cliente.

## A minha senha é segura? 
{:faq}

Sim. Todos os VRAs têm senhas aleatórias visíveis designadas apenas ao titular da conta. As senhas são facilmente mudadas, assim como as chaves públicas SSH e as restrições de acesso de IP do administrador.

## Posso obter o VRA sem um Dispositivo de Gateway? 
{:faq}

Sim, mas ele só poderá gerenciar o tráfego entre as interfaces públicas e privadas do VRA. VLANs e HA requerem o acessório Dispositivo de Gateway.

## Tudo o tráfego de rede é enviado por meio do VRA? 
{:faq}

Não. O Dispositivo de Gateway permite selecionar os segmentos de rede privada e pública (VLANs) que você deseja rotear por meio do VRA. Você pode mudar e efetuar bypass nas seleções de VLAN a qualquer momento. O VRA também permite definir regras baseadas em IP que se aplicam a sub-redes ou intervalos de IP. Essas regras funcionam apenas se as VLANs que contêm essas sub-redes são roteadas por meio do VRA.

## Um VRA ou firewall dedicado pode impedir novas provisões do servidor? 
{:faq}

Sim. Sempre que possível, você não deve bloquear sua rede até que a tenha preenchido com os servidores que planeja usar.

O suporte IBM está proibido pela política de examinar ou alterar a configuração do VRA ou do firewall dedicado sem o envolvimento explícito do cliente, portanto, na maioria dos casos, o suporte não pode saber que um VRA é responsável pelas provisões do servidor paralisado ou com falha.

É responsabilidade do cliente garantir que o VRA ou o firewall esteja configurado para permitir provisões do servidor automatizado antes de o pedido do servidor ser efetuado. A resolução de provisões que são bloqueadas por um VRA ou firewall gerenciado pelo cliente é de responsabilidade do cliente. Esses atrasos de fornecimento não estão sujeitos a SLAs ou créditos. Os sistemas solicitados poderão ser retornados ao inventário (após os dados do cliente serem apagados) se o cliente não responder rapidamente.

Da mesma forma, se um VRA/firewall tiver bypass efetuado após um pedido ser efetuado, ainda é provável que o pedido falhe. Poderá haver uma janela de limitação durante a qual as novas tentativas de automação serão feitas. É melhor que o processo de provisão inteiro continue sem interferência de rede.

## Quais produtos de firewall a IBM oferece?
{:faq}

É possível localizar uma comparação detalhada de todos os produtos de firewall oferecidos no IBM Cloud revisando este [tópico ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/infrastructure/fortigate-10g/explore-firewalls.html#explore-firewalls){: new_window}. 

## Um VRA pode confundir os esforços de suporte ao cliente? 
{:faq}

Sim, pelos motivos descritos acima. O VRA é uma "caixa preta": VLANs entram, VLANs saem e a IBM não tem ideia do que os clientes estão fazendo com pacotes nesse ínterim.

O suporte sempre faz o seu melhor, mas com o VRA e o firewall dedicado: a) a privacidade do cliente supera a conectividade e b) nossa equipe de suporte padrão não está equipada para analisar configurações de VRA/firewall formadas inadequadamente ou altamente complexas.

Como uma primeira etapa de diagnóstico, podemos exigir que você coloque suas VLANs de VRA ou Firewall em bypass. Se, neste estado, as provisões que falharam começarem a passar, devemos assumir que o problema está em sua configuração de VRA/firewall.

## Qual efeito o VRA terá em meu desempenho de rede? 
{:faq}

Tenha em mente que, embora eles não possam ver você, uma nuvem pública compartilha redes com outros clientes. O melhor rendimento de VRA é determinado pela capacidade de rede disponível em um momento, além da distância que os dados devem viajar.

Consideradas essas variáveis, o VRA é capaz de encaminhar 80 Gbps de tráfego não modificado em várias interfaces, usando a fórmula bruta de que cada 10 Gbps de rendimento requer um núcleo de processador completo (não incluindo hyperthread). Dado que o máximo dos servidores atuais é 40 Gbps (2 x 10 Gbps público + 2 x 10 Gb privado), um servidor com 8 ou mais núcleos deve ter espaço suficiente para manipular múltiplos recursos comuns do VRA próximo do melhor desempenho de rede

## O que eu faço se perder minha senha do VRA?
{:faq}

Se houver acesso ao sistema, configure uma nova senha executando o comando a seguir:

```
set system login user [account] authentication plaintext-password [password]  
```

Se não houver acesso ao sistema, será possível reinicializar o dispositivo e usar a opção de recuperação de senha no menu GRUB para reconfigurar a senha do usuário raiz.

## O que eu faço se tiver sido bloqueado fora do firewall?
{:faq}

A construção `reinicializar em [tempo]` pode ser útil ao testar regras de firewall potencialmente perigosas.

Se a regra funcionar, use o comando `reboot cancel` para cancelar a reinicialização. Se a regra bloquear seu acesso, basta aguardar que a reinicialização planejada ocorra.

Se não houver acesso ao sistema, uma reinicialização poderá ser usada para recuperar o acesso. Na reinicialização, o sistema lerá o arquivo de configuração que não foi mudado por entradas anteriores que foram descartadas.

Se houver acesso utilizando IPMI, será possível executar as ações a seguir para recuperar o acesso:

1. Desativar a regra afetada executando:

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. Desconectar o conjunto de regras nomeado inteiro da interface necessária executando:

	```
	delete interfaces dataplane [interface] firewall [type][firewall name]
	commit
	```

**NOTA:** o uso incorreto desses comandos pode apagar os dados de sua interface de configuração.

## Como posso permitir logins de raiz no VRA?
{:faq}

Para ativar o acesso raiz por meio de SSH, execute o comando a seguir:

`set service ssh allow-root`

Observe que permitir acesso raiz usando SSH é considerado inseguro. Uma alternativa para acessar um shell raiz é efetuar login como outro usuário e elevar a raiz localmente com `su -` ou permitindo comandos sudo para 'superusuários'. 

Por exemplo, para configurar o vyatta como um superusuário:

`set system login vyatta level superuser`
