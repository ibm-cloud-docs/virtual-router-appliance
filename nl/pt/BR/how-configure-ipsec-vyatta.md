---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configurar o IPSec no Vyatta 5400

O dispositivo Brocade 5400 vRouter (Vyatta) será referido como "local" em relação ao túnel Internet Security Protocol (IPSec). Cada um dos comandos a seguir executará funções diferentes para configurar o site para site do IPSec. Observe que este exemplo de site para site do IPSec demonstra o túnel na rede pública do SoftLayer; use **bond0** para conexões privadas de site para site do IPSec.

1. "Informe" ao túnel o propósito de **interface bond1:**

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Configure a primeira fase do túnel de duas fases. Os comandos a seguir irão:

  * Criar um novo grupo **ike** chamado **teste** e usar **dh-group** como o tipo de troca de chave.
  * Especificar o tipo de criptografia a ser usado; se isso não for configurado, o dispositivo usará **aes128** como um padrão
  * Usar a função hash **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Configure a segunda fase do túnel de duas fases. Os comandos a seguir irão:

  * Desativar o Perfect Forward Secrecy (PFS) porque nem todos os dispositivos podem usá-lo. (O esp no comando é a segunda parte da criptografia.)
  * Especificar o tipo de criptografia a ser usado; se isso não for configurado, o dispositivo usará **aes128** como um padrão
  * Usar a função hash **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Configure os parâmetros de criptografia de site para site do IPSec. Os comandos a seguir irão:

  * Especificar o IP do lado remoto e que o IPSec estará usando o segredo pré-compartilhado
  * Usar o IP remoto e a chave secreta TestPSK
  * Configurar o grupo **esp** padrão para o túnel como TestESP
  * "Informar" ao IPSec para usar o grupo ike TestIKE, definido anteriormente<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Crie o mapeamento para o túnel IPSec. Os comandos a seguir irão, com base no exemplo no material:

  * "Informar" ao túnel para mapear para o IP remoto de 169.54.254.117 para o endereço IP local de bond1, 50.97.240.219
  * Rotear somente endereços IP com a sub-rede de 10.54.9.152/29 que está na interface do servidor local para o servidor remoto 169.54.254.117
  * Rotear o tráfego remoto 169.54.254.117 do túnel 1 para a sub-rede remota de 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

A próxima etapa será configurar o dispositivo do lado remoto, que é um Brocade 5400 vRouter 6.6.5 R

  * Use o dispositivo recém-configurado (que foi configurado no modo de operação) para inserir o comando show configuration commands. Uma lista de comandos usados para configurar o dispositivo será apresentada.
  * Copie os comandos para um editor de texto. Os comandos usados para configurar o dispositivo local serão usados para configurar o servidor remoto com modificações no IP para apontar o dispositivo Brocade 5400 vRouter 6.6.5R no SoftLayer.

A configuração do lado remoto usada anteriormente está abaixo. As mudanças necessárias para a configuração do lado local estão em negrito.

Configuração do lado remoto:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (O IP peer de site para site foi trocado)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (O endereço IP local foi trocado)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (O prefixo local e o prefixo remoto não foram trocados)

* Copie e cole os novos comandos no servidor remoto (certifique-se de estar no modo de configuração), digite commit e, em seguida, salve.
* Digite run show vpn ike sa para ver se o túnel está agora estabelecido.

Aqui está uma recapitulação do que foi feito: apenas roteie endereços IP com a sub-rede de '10.54.9.152/29' que residem na interface local (bond1, 50.97.240.219) para apenas sub-redes 192.168.1.2/32 no serviço remoto que residem na interface com o endereço IP de 169.54.254.117.
