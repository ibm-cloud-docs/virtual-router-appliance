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

# Resolução de problemas da interface VFP
Este tópico contém informações de resolução de problemas para interfaces VFP.

* Uma interface VFP não é uma interface "real", tal como `dp0bond0` (ou até mesmo um VIF ou um TUN). É um item temporário para que o firewall e os processos NAT interrompam uma interface para que o tráfego possa ser processado corretamente. Ainda é possível rotear o tráfego sobre um VFP como uma interface regular, mas `thsark` e outros comandos de monitor não exibirão nenhum tráfego.
* Com a NAT, deve-se usar um intervalo de sub-rede mais específico para que o tráfego seja roteado para o VFP, em vez da rota do kernel criada pelo IPsec. Se uma rota estática não estiver configurada, a rota do kernel será seguida. É possível testar isso usando `show ip route x.x.x.x`.
* O DNAT deve ser processado corretamente saindo do VFP, mas o tráfego de retorno ainda precisa de uma rota estática configurada. Procure o tráfego não NAT que sai da interface IPsec, `dp0bond1` ou `dp0bond0` (ou qualquer interface que use o tráfego IPsec).
* O uso de protocolos de roteamento sobre um VFP não foi testado. 
* O uso de um túnel GRE sobre um VFP também não foi testado.
