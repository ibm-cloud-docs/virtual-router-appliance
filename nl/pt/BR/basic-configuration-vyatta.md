---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuração básica do Vyatta 5400

Execute o procedimento a seguir para configurar seu Vyatta 5400.

A virtual LAN (VLAN) pública 1224, que agora é associada e roteada, precisa ser configurada antes de poder transmitir tráfego. São necessárias duas informações para concluir a configuração:

  * A ligação do lado público do dispositivo Brocade 5400 vRouter
  * O gateway padrão de 1224

Observe que é a VLAN do lado público na qual a opção de cálculo está localizada - não a VLAN pública do dispositivo Brocade 5400 vRouter.

## No Portal do Softlayer

1\. No Portal da web do SoftLayer, em **Dispositivos**, localize a ligação na guia **Configuração** do dispositivo Brocade 5400 vRouter. Suponha que <span style="text-decoration: underline">eth1=bond1</span> em seu dispositivo.

2\. Selecione **Rede > Gerenciamento de IP > VLANs** no Portal da web para localizar o gateway padrão para a VLAN 1224.

3\. Clique em sua VLAN na lista e clique na **Sub-rede** na qual você vê seu Gateway. Anote os detalhes de Classless Inter-Domain Routing (CIDR) da máscara localizados após a barra. 

## Na GUI do Vyatta

4\. No painel, clique na guia **Configuração**.

5\. Expanda **Interfaces > Ligação > bond1** no lado esquerdo da tela; destaque **vif**.

6\. Insira os "nomes" das VLANs no campo **vif** (1224 para nossos propósitos) e clique no botão **Criar**. O processo levará alguns segundos para concluir.

7\. Selecione o **vif 1224** recém-criado na seta **vif**.

8\. Verifique a caixa em **dhcp** e digite o endereço IP do gateway padrão e a notação CIDR da máscara da VLAN que você deseja passar pelo Brocade 5400 vRouter (por exemplo, VLAN 1224).

9\. Clique no botão **Configurar** e depois em **Confirmar**.

10\. Clique em **Salvar** na barra de menus do meio; caso contrário, a configuração retrocederá para o seu padrão na próxima vez em que o sistema for reinicializado.<sup>1</sup>

11\. Clique na guia Estatísticas e abra a nova interface para verificar e monitorar o tráfego.

## Configurar a VLAN privada usando a CLI

Há dois modos de comando na CLI (Interface da linha de comandos): operacional e de configuração. 

O modo operacional fornece acesso a comandos operacionais para:

  * Exibir e limpar informações
  * Ativar e desativar a depuração
  * Configurar definições de terminal
  * Carregar e salvar a configuração
  * Reiniciar o sistema

A configuração fornece acesso a comandos para:

  * Criar
  * Modificar
  * Excluir
  * Confirmar
  * Mostrar informações de configuração
  * Navegar por meio da hierarquia de configuração

Quando você efetuar logon no sistema, o sistema estará no modo operacional; será necessário alternar para a configuração para os comandos.<sup>2</sup>

Use as etapas a seguir para configurar a VLAN privada usando a CLI. Lembre-se que os valores necessários para configurar a VLAN são:

  * O nome da VLAN a ser roteada por meio do dispositivo Brocade 5400 vRouter (2254)
  * Gateway e máscara (formato CIDR) da VLAN a ser roteada (10.52.69.201/29)
  * Nome da ligação privada do Dispositivo Brocade 5400 vRouter (bond0)

1\. Faça SSH para o seu Brocade 5400 vRouter (endereço IP público ou privado) usando **vyatta**<sup>3</sup> como o **Nome do usuário**; forneça a senha quando solicitado.

2\. Configure o vif:

  * Digite *configure* no prompt de comandos para entrar no modo de configuração.
  * Digite *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* no prompt de comandos para configurar o vif.
  * Digite *commit* no prompt de comandos para confirmar as configurações.
  * Digite *save* para salvar as configurações.
  * Digite *exit* para sair novamente do modo de operação.

3\. Digite *show interfaces* para verificar as configurações recém-confirmadas.

4\. Roteie as VLANs restantes por meio do dispositivo Brocade 5400 vRouter.

**Notas:**

<sup>1</sup> O retrocesso de configuração poderá ser um recurso muito útil se você quebrar a sua configuração enquanto testar mudanças. Enquanto as mudanças não são salvas é possível reinicializar o servidor do Portal da web e restaurar a configuração anterior.

<sup>2</sup> É possível informar em qual modo você está, operacional ou de configuração, com base no prompt. Você estará no modo operacional se o prompt for # e no modo de configuração se o prompt for $.

<sup>3</sup> É necessário criar um novo usuário dentro do Brocade 5400 vRouter e desativar o usuário inicial padrão `vyatta`.
