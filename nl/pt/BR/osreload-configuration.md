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

# Recarregar o OS
O processo de recarregamento do SO, quando iniciado no Portal do Cliente, limpará todas as configurações presentes no dispositivo e o restaurará para sua configuração original. Quaisquer mudanças feitas desde o último carregamento do sistema serão apagadas. Como resultado, se você tiver feito mudanças, por exemplo, no grupo do VRRP em outra máquina, a máquina recarregada provavelmente terá um conflito quando ficar on-line novamente.

Para recarregar seu SO, execute o procedimento a seguir:

1. Efetue login no [Portal do Cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){: new_window} usando suas credenciais exclusivas.
2. Selecione **Lista de Dispositivos** na lista suspensa Dispositivos.
3. Clique no servidor que você deseja recarregar.
4. Selecione **Recarregamento do SO** no menu suspenso **Ações** na parte superior esquerda da página.
5. Na tela Recarregamento do SO, escolha quaisquer recursos e opções que gostaria de aplicar como parte do recarregamento. 
6. Clique no botão **Recarregar em Cima da Configuração** para continuar no pop-up **Revisão**. Clique em **Cancelar** para cancelar as mudanças no dispositivo e sair da tela.
7. Verifique se todos os detalhes na seção Nova configuração estão corretos. Clique em **Avançar** para avançar para o pop-up Confirmar.
8. Clique no botão **Confirmar Recarregamento do SO** para confirmar e iniciar o Recarregamento do SO. Clique em **Cancelar** para cancelar a ação.

Depois de iniciar o processo de Recarregamento do SO, o dispositivo será colocado off-line e o processo de Recarregamento do SO iniciará. O tempo que leva para um recarregamento do SO concluir varia com base na configuração atual e nova do dispositivo. Durante o processo de configuração, o tempo mínimo para o Recarregamento do SO é exibido em cada tela. O intervalo de tempo é uma estimativa feita pelo sistema. Se o recarregamento levar mais de 24 horas, entre em contato com nossa equipe de Suporte. Quando o dispositivo ficar on-line novamente, ele funcionará conforme especificado na Nova Configuração para o Recarregamento do SO. 

**NOTA:** todos os dados salvos anteriormente no dispositivo serão perdidos, mas poderão ser restaurados se foi feito um backup do dispositivo antes de seu recarregamento. Caso não tenha sido feito backup dos dados, eles não poderão ser recuperados.
