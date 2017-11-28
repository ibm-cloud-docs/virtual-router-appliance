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

# Informações básicas do VRA
O VRA pode ser configurado por meio de uma sessão do console remoto por meio de SSH ou efetuando login na GUI da web. Por padrão, a GUI da web não está disponível da Internet pública. Para ativar a GUI da web, efetue login por meio de SSH primeiro.

**NOTA:** configurar o VRA fora de seu shell e interface pode produzir resultados inesperados e, portanto, não é recomendado.

## Acessando o dispositivo por meio de SSH
A maioria dos sistemas operacionais baseados em Unix, como Linux, BSD e Mac OSX, tem clientes OpenSSH incluídos com suas instalações padrão. Os usuários do Windows podem fazer download de um cliente SSH, tal como PuTTy.

É recomendado que o SSH para o IP público seja desativado e apenas o SSH para o IP privado seja permitido. Conexões com IPs privados requerem que você esteja na mesma VPN.

Use a conta do Vyatta na página **Detalhes do Dispositivo** para efetuar login por meio de SSH. A senha raiz também é fornecida, mas o login raiz está desativado por padrão por motivos de segurança.

`ssh vyatta@[IP address] `

**NOTA:** é recomendável manter logins raízes desativados. Efetue login usando a conta do Vyatta e eleve para raiz somente quando necessário.

Chaves SSH também podem ser fornecidas durante a implementação para evitar a necessidade da conta Vyatta efetuar login. Depois de verificar sua capacidade de acessar seu VRA usando sua chave SSH, é possível desativar logins de usuário/senha padrão executando os comandos a seguir:

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## Acessando o dispositivo por meio da GUI da web

Efetue login no VRA usando as instruções de SSH acima, em seguida, execute os comandos a seguir para ativar o serviço HTTPS:

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

Após esses comandos serem concluídos, insira https://<ip.address> na barra de endereço de seu navegador, substituindo o endereço IP pelo de seu VRA. Pode ser solicitado que você aceite o certificado autoemitido do VRA. Faça isso, em seguida, efetue login na interface da web com suas credenciais 'vyatta' quando solicitado.

## Modos
**Modo de configuração:** chamado com o uso do comando `configure`, esse modo é onde a configuração do sistema VRA é executada.

**Modo operacional:** o modo inicial para efetuar login em um sistema do VRA. Nesse modo, comandos `show` podem ser executados para consultar informações sobre o estado do sistema. O sistema também pode ser reiniciado desse modo.

O shell do VRA é uma interface modal, com vários modos de operação. O modo primário/padrão é **Operacional** e esse será o modo apresentado após o login. No modo **Operacional**, é possível visualizar comandos de informações e emissão que impactam a execução atual do sistema, tal como configurar a data/hora ou reinicializar o dispositivo.

O comando `configure` coloca o usuário no modo **Configuração**, no qual edições na configuração podem ser executadas. Observe que as mudanças na configuração não ocorrem imediatamente; elas devem ser confirmadas e salvas. À medida que os comandos são inseridos, eles vão para um buffer de configuração. Quando todos os comandos necessários forem inseridos, execute o comando `commit` para que as mudanças entrem em vigor.

Para salvar comandos permanentemente, execute o comando `save` após o comando `commit`.

Comandos do modo operacional podem ser executados do modo de Configuração iniciando o comando com `run`. Por exemplo:


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

A marca de hash (`#`) indica o modo de Configuração. Iniciar um comando com `run` indica ao shell do VRA que um comando operacional está sendo apresentado. O exemplo anterior também ilustra a capacidade de "grep" na saída de comandos.

## Exploração de comando

O shell de comando do VRA inclui recursos de conclusão da guia. Se você estiver curioso sobre quais comandos estão disponíveis, pressione a tecla Tab para obter uma lista e uma breve explicação. Isso funciona no prompt de shell e ao digitar um comando. Por exemplo:  

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Configuração de Amostra
As configurações são organizadas em um padrão hierárquico de nós. Considere este bloco de rota estática:

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

Isso seria gerado pelos comandos:

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

Este exemplo ilustra que é possível para um nó (estático) ter múltiplos nós-filhos. Para remover a rota para `192.168.1.0/24`, o comando `delete protocols static route 192.168.1.0/24` seria usado. Se `192.168.1.0/24` fosse deixado fora do comando, ambos os nós de rota seriam marcados para exclusão.

Lembre-se de que a configuração não é realmente mudada até que o comando `commit` seja emitido. Para comparar a configuração de execução atual com relação a quaisquer mudanças que estejam presentes no buffer de configuração, use o comando `compare`. Para limpar o buffer de configuração, use `discard`.

## Usuários e Controle de Acesso Baseado na Função (RBAC)
As contas do usuário podem ser configuradas com três níveis de acesso:

* Administrador
* Operador
* Superusuário.

Os usuários no nível de operador podem executar comandos `show` para visualizar o status da execução do sistema e emitir comandos `reset` para reiniciar os serviços no dispositivo. As permissões no nível de operador não implicam em acesso somente leitura.

Os usuários no nível de administrador têm acesso total a todas as configurações e operações para o dispositivo. Os usuários administrativos podem visualizar configurações em execução, mudar as definições de configuração para o dispositivo, reinicializar o dispositivo e encerrar o dispositivo.

Os usuários no nível de superusuário são capazes de executar comandos com privilégios de administrador por meio do comando `sudo`, além de ter privilégios no nível de administrador.

Os usuários podem ser configurados para estilos de autenticação por senha ou de chave pública ou ambos. A autenticação de chave pública é usada com SSH e permite que os usuários efetuem login usando um arquivo de chave em seus sistemas. Para criar um usuário operador com uma senha:


```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**NOTA:** quando nenhum nível for especificado, um usuário será considerado no nível de administrador. As senhas do usuário neste caso são exibidas como criptografadas no arquivo de configuração.

Controle de Acesso Baseado na Função (RBAC) é um método para restringir o acesso à parte da configuração para usuários autorizados. O RBAC permite que os administradores definam regras para um grupo de usuários que restrinjam os comandos que eles podem executar.

O RBAC é executado criando um grupo designado ao conjunto de regras de Gerenciamento de Controle de Acesso (ACM), incluindo um usuário no grupo, criando um conjunto de regras para corresponder o grupo aos caminhos no sistema, em seguida, configurando o sistema para permitir ou negar esses caminhos que foram aplicados ao grupo.

Por padrão, não há nenhum conjunto de regras de ACM definido no Virtual Router Appliance e o ACM está desativado. Se você desejar usar o RBAC para fornecer controle de acesso granular, deverá ativar o ACM e incluir as regras padrão do ACM a seguir, além de suas próprias regras definidas:

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

Consulte a [documentação](vra-docs.html) suplementar antes de tentar ativar as regras do ACM. Configurações de regras imprecisas do ACM podem causar negações de acesso ao dispositivo ou erros de inoperância do sistema.
