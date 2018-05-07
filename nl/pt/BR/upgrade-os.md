---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Faça upgrade do SO
O upgrade do sistema operacional VRA pode ser executado com o comando ``add system image`` em um arquivo ISO local transferido por upload por nossa equipe de Suporte. Uma lista de versões de upgrade de Vyatta disponíveis pode ser obtida usando o sistema IBM Support Ticket.

Para iniciar o processo de upgrade, abra um chamado com o sistema IBM Support Ticket solicitando um upgrade e que uma nova imagem ISO seja transferida por upload para seu sistema. Você receberá um e-mail do Suporte IBM indicando onde o arquivo ISO foi transferido por upload. No exemplo abaixo, ele está no diretório ``tmp``.

**NOTA:** o processo de upgrade ilustrado abaixo é para um único VRA. Se você estiver usando VRA no modo de alta disponibilidade, deverá executar o mesmo comando de upgrade em ambos os sistemas. Além disso, é recomendado que você faça upgrade da máquina `BACKUP` primeiro e verifique se ela está funcionando corretamente. Em seguida, acesse a máquina `MASTER` e efetue failover nela usando o comando `reset vrrp`. Finalmente, faça upgrade de `MASTER` original quando `BACKUP` tiver assumido o controle.

Para fazer upgrade do VRA, execute o procedimento a seguir.

1. Execute o comando ``add system image <Local ISO File>``.
2. Pressione **Enter** para aceitar o nome padrão da imagem ISO ou insira o seu próprio.
3. Escolha se deseja salvar o diretório de configuração e o arquivo de configuração atuais.
4. Escolha se deseja salvar as chaves do host SSH de sua configuração atual.
5. Pressione **Enter** para aceitar o console do sistema padrão ou insira o seu próprio.
6. Pressione **Enter** para aceitar a velocidade do console padrão ou insira a sua própria.
7. Insira o comando `reboot` e, em seguida, insira `Yes` para reinicializar a máquina.
8. Se você estiver usando o VRA em um modo de alta disponibilidade, execute as etapas 1 a 7 novamente na segunda máquina.

O exemplo abaixo ilustra o processo de upgrade.

```
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Welcome to the Brocade vRouter image installer.
Checking MD5 checksums of files on the ISO image...OK.
What would you like to name this image? [5.2R5S4.08091424]: 5.2R5S4
This image will be named: 5.2R5S4
Would you like to save the current configuration
directory and config file? (Yes/No) [Yes]:
Saving current configuration...
Would you like to save the SSH host keys from your
current configuration? (Yes/No) [Yes]:
Saving SSH keys...
Copying squashfs image...
Copying kernel and initrd images...
Copying saved configuration to config partition.
Copying saved SSH host keys.
Enter the desired system console [tty0]:
Enter the console speed [38400]:
Running post-install script...
Done.

vyatta@vra01:~$ show system image
The system currently has the following image(s) installed:

   1: 5.2R5S3.06301309 (running image)
   2: 5.2R5S4 (default boot)

vyatta@vra01:~$ reboot
Proceed with reboot? (Yes/No) [No] y
The system is going down for reboot NOW!

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```
