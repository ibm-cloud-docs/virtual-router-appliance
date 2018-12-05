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

# Correções do software AT&T Vyatta 5600 vRouter

**A partir de: 2 de novembro de 2018**

Este documento lista as correções para as versões atualmente suportadas do Vyatta Network OS 5600. Com as versões 5.2 e mais antigas, as correções são nomeadas usando um número S. Com as versões 17.1 e mais novas, as correções são nomeadas com uma letra minúscula, excluindo “i”, “o”, “l” e “x”.

Quando vários números de CVE são abordados em uma única atualização, a pontuação CVSS mais alta é listada.

## 1801t

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-44172 | Bloqueador | Erro “as interfaces [openvpn] não são válidas” relatado em testes mss-clamp |
| VRVDR-43969 | Secundário | GUI do Vyatta 18.x relatando uso incorreto de memória de verificação de status |
| VRVDR-43847  | Grave | Rendimento lento para conversas do TCP na interface de ligação |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-43842 | N/A | DSA-4305-1 | CVE-2018-16151, CVE-2018-16152: Debian DSA4305-1: strongswan – atualização de segurança |

## 1801s

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-44041 | Grave | SNMP ifDescr oid tempo de resposta lento |
 
**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-44074| 9,1 | DSA-4322-1 | CVE-2018-10933: Debian DSA-4322-1: libssh – atualização de segurança|
| VRVDR-44054 | 8,8 | DSA-4319-1 | CVE-2018-10873: Debian DSA-4319-1: spice – atualização de segurança |
| VRVDR-44038 | N/A | DSA-4315-1 | CVE-2018-16056, CVE-2018-16057, CVE-2018- 16058: Debian DSA-4315-1: wireshark – atualização de segurança |
| VRVDR-44033 | N/A | DSA-4314-1 | CVE-2018-18065: Debian DSA-4314-1: net-snmp – atualização de segurança | 
|VRVDR-43922 | 7,8 | DSA-4308-1 | CVE-2018-6554, CVE-2018-6555, CVE-2018-7755, CVE-2018-9363, CVE-2018-9516, CVE-2018-10902, CVE-2018-10938, CVE-2018-13099, CVE-2018- 14609, CVE-2018-14617, CVE-2018-14633, CVE- 2018-14678, CVE-2018-14734, CVE-2018-15572, CVE-2018-15594, CVE-2018-16276, CVE-2018- 16658, CVE-2018-17182: Debian DSA-4308-1: linux – atualização de segurança |
| VRVDR-43908 | 9,8 | DSA-4307-1 | CVE-2017-1000158, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4307-1: python3.5 - atualização de segurança |
| VRVDR-43884 | 7,5 | DSA-4306-1 | CVE-2018-1000802, CVE-2018-1060, CVE-2018- 1061, CVE-2018-14647: Debian DSA-4306-1: python2.7 - atualização de segurança |

## 1801r

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-43738 | Grave | Pacotes não acessíveis do ICMP retornados por meio da sessão SNAT não são entregues |
| VRVDR-43538 | Grave | Receber erros de tamanho excessivo na bondinginterface | 
| VRVDR-43519 | Grave | Vyatta-keepalived em execução sem nenhuma configuração presente | 
| VRVDR-43517 | Grave | O tráfego falha quando o terminal do IPsec baseado em VFP/Política reside no próprio vRouter | 
| VRVDR-43477 | Grave | A confirmação da configuração da VPN do IPsec retorna o aviso "Aviso: impossível [VPN toggle net.ipv4.conf.intf.disable_policy], código de erro recebido 65280" |
| VRVDR-43379 | Secundário | Estatísticas NAT mostradas incorretamente |
 
**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-43837 | 7,5 | DSA-4300-1 | CVE-2018-10860: Debian DSA-4300-1: libarchive-zip-perl – atualização de segurança |
| VRVDR-43693 | N/A | DSA-4291-1 | CVE-2018-16741: Debian DSA-4291-1: mgetty – atualização de segurança | 
| VRVDR-43578 | N/A | DSA-4286-1 | CVE-2018-14618: Debian DSA-4286-1: curl - atualização de segurança |
| VRVDR-43326 | N/A | DSA-4280-1 | CVE-2018-15473: Debian DSA-4280-1: openssh - atualização de segurança | 
| VRVDR-43198 | N/A | DSA-4272-1 | CVE-2018-5391: Debian DSA-4272-1: atualização de segurança do linux (FragmentSmack) |
| VRVDR-43110 | N/A | DSA-4265-1 | Debian DSA-4265-1 : xml-security-c - atualização de segurança | 
| VRVDR-43057 | N/A | DSA-4260-1 | CVE-2018-14679, CVE-2018-14680, CVE-2018-14681, CVE-2018-14682: Debian DSA-4260-1 : libmspack - atualização de segurança | 
| VRVDR-43026 | 9,8 | DSA-4259-1 | Debian DSA-4259-1 : ruby2.3 - atualização de segurança VRVDR-42994N/ADSA-4257-1CVE-2018-10906: Debian DSA-4257-1 :fuse - atualização de segurança |

## 1801q

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-43531 | Grave |A inicialização no 1801p resulta em pânico kernel no prazo de aproximadamente 40 segundos |
| VRVDR-43104 | Crítico | ARP gratuito falso na rede DHCP quando o IPsec é ativado |
| VRVDR-41531 | Grave | O IPsec continua tentando usar a interface VFP após sua desvinculação |
| VRVDR-43157 | Secundário | Quando o túnel é devolvido, o trap SNMP não é gerado corretamente. |
| VRVDR-43114 | Crítico | Na reinicialização, um roteador em um par de HA com uma prioridade mais alta do que seu peer não honra sua própria configuração de “preempção falsa” e se torna o principal imediatamente após a inicialização |
| VRVDR-42826 | Secundário | Com o ID remoto “0.0.0.0”, a negociação de peer falha devido à incompatibilidade de chave pré-compartilhada |
| VRVDR-42774 | Crítico| Driver X710 (i40e) enviando quadros de controle de fluxo em uma taxa muito alta |
| VRVDR-42635 | Secundário | A mudança de política do mapa de rota de redistribuição do BGP não entra em vigor |
| VRVDR-42620 | Secundário | Vyatta-ike-sa-daemon lança o erro “Falha do comando: estabelecendo peer de passagem CHILD_SA” enquanto o túnel parece estar ativo |
| VRVDR-42483 | Secundário | Falha na autenticação TACACS |
| VRVDR-42283 | Grave | O estado do VRRP muda para FAULT para todas as interfaces quando um IP da interface vif é excluído |
| VRVDR-42244 | Secundário | O monitoramento de fluxo exporta apenas 1.000 amostras para o coletor |
| VRVDR-42114 | Crítico | O serviço HTTPS NÃO DEVE expor o TLSv1 |
| VRVDR-41829 | Grave |Ocorre core dump no plano de dados até que o sistema se torne não responsivo com o teste de estabilidade SIP ALG |
| VRVR-41683 | Bloqueador | O endereço do servidor de nomes DNS informado por meio do VRF não é reconhecido consistentemente |
| VRVDR-41628 | Secundário | Rota/prefixo da propaganda do roteador ativo no kernel e no plano de dados, mas ignorado pelo RIB |
 
**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-43288 | 5,6 | DSA-4279-1 | CVE-2018-3620, CVE-2018-3646: Debian DSA-4279- 1 – Atualização de segurança do Linux |
| VRVDR-43111 | N/A | DSA-4266-1 | CVE-2018-5390, CVE-2018-13405: Debian DSA- 4266-1 – Atualização de segurança do Linux |

## 1801n

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-42588 | Secundário | Configuração do protocolo de roteamento sensível vazada acidentalmente no log do sistema |
| VRVDR-42566 | Crítico | Depois do upgrade de 17.2.0h para 1801m, um dia mais tarde ocorreram várias reinicializações nos membros de HA |
| VRVDR-42490 | Grave | VTI-IPSEC IKE SAs falham um minuto após a transição de VRRP |
| VRVDR-42335 | Grave | IPSEC: o comportamento do “nome do host” do ID remoto muda de 5400 para 5600 |
| VRVDR-42264 | Crítico | Sem conectividade sobre o túnel SIT - “kernel: sit: não ECT de 0.0.0.0 com TOS=0xd” |
| VRVDR-41957 | Secundário | Pacotes NAT bidirecionais muito grandes para GRE falham ao retornar o Código 4 Tipo 3 do ICMP |
| VRVDR-40283 | Grave | Mudanças na configuração geram muitas mensagens de log |
| VRVDR-39773 | Grave | O uso de um mapa de rota com o comando BGP vrrp-failover pode fazer com que todos os prefixos sejam retirados |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-42505 | N/A | DSA-4236-1 | CVE-2018-12891, CVE-2018-12892, CVE-2018-12893: Debian DSA-4236-1: xen - atualização de segurança |
| VRVDR-42427 | N/A | DSA-4232-1 | CVE-2018-3665: Debian DSA 4232-1: xen - atualização de segurança |
| VRVDR-42383 | N/A | DSA-4231-1 | CVE-2018-0495: Debian DSA-4231-1: libgcrypt20 - atualização de segurança |
| VRVDR-42088 | 5,5 | DSA-4210-1 | CVE-2018-3639: Debian DSA-4210-1: xen – atualização de segurança |
| VRVDR-41924 | 8,8 | DSA-4201-1 | CVE-2018-8897, CVE-2018-10471, CVE-2018-10472, CVE-2018-10981, CVE-2018-10982: Debian DSA-4201- 1: xen – atualização de segurança |

## 5.2R6S12

Liberado em 21 de junho de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-42084 | Bloqueador | Interface Vfp marcada como “interface não é de plano de dados” em “mostrar rota de plano de dados” quando a configuração de nat/ipsec é reaplicada |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-42317 | 5,4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – atualização de segurança |
| VRVDR-42284 | 7,5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – atualização de segurança |
| VRVDR-41797 | 8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: linux – atualização de segurança |
| VRVDR-41680 | 7,8 | DSA-4188-1 | Debian DSA-4188-1: linux – atualização de segurança (Spectre) |

## 1801m

Liberado em 15 de junho de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-42256 | Crítico | Sem tráfego de saída se o CHILD_SA estabelecido mais recente for excluído |
| VRVDR-42084 | Bloqueador | As sessões NAT vinculadas a interfaces VFP para túneis PB IPsec não estão sendo criadas para pacotes que chegam no roteador, embora o roteador esteja configurado para fazer isso |
| VRVDR-42018 | Secundário | Quando “restart vpn” é executado, um erro “IKE SA daemon: org.freedesktop.DBus.Error.Service.Unknown” é lançado |
| VRVDR-42017 | Secundário | Quando “show vpn ipsec sa” estiver em execução no backup do VRRP, o erro “ConnectionRefusedError” será lançado em relação à linha 563 de vyatta-op-vpn- ipsec-vici |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 42317 | 5,4 | DSA-4226-1 | CVE-2018-12015: Debian DSA-4226-1: perl – atualização de segurança |
| VRVDR- 42284 | 7,5 | DSA-4222-1 | CVE-2018-12020: Debian DSA-4222-1: gnupg2 – atualização de segurança |

## 5.2R6S11

Liberado em 11 de junho de 2018

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-42109 | Crítico | Apenas 1 pacote de resposta do ICMP recebido com SNAT+FW no 5.2R6S7 |
| VRVDR-42084 | Bloqueador | As sessões NAT vinculadas a interfaces VFP para túneis PB IPsec não estão sendo criadas para pacotes que chegam no roteador, embora o roteador esteja configurado para fazer isso |
| VRVDR-42027 | Grave | SFLOW usando entrada incorreta ifIndex |
| VRVDR-41558 | Grave | Os registros de data e hora relatados em rastreios de pacote não são consistentes com o horário real e com o relógio do sistema |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7,5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE-2018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – atualização de segurança |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639: execução especulativa, variante 4: bypass de armazenamento especulativo/Spectre v4/Spectre-NG |
| VRVDR- 42006 | 9,8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – atualização de segurança |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – atualização de segurança |
| VRVDR- 41795 | 6,5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – atualização de segurança |

## 1801k

Liberado em 8 de junho de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-42084 | Bloqueador | As sessões NAT vinculadas a interfaces VFP para túneis PB IPsec não estão sendo criadas para pacotes que chegam no roteador, embora o roteador esteja configurado para fazer isso |
| VRVDR-41944 | Grave | Depois de ocorrer failover do VRRP, alguns túneis VTI falham ao serem restabelecidos até que um “vpn restart” ou uma reconfiguração do peer seja emitido |
| VRVDR-41906 | Grave | A descoberta de PMTU falha porque as mensagens do tipo 3 código 4 do ICMP são enviadas de um IP de origem incorreto |
| VRVDR-41558 | Grave | Os registros de data e hora relatados em rastreios de pacote não são consistentes com o horário real e com o relógio do sistema |
| VRVDR-41469 | Grave | Um link de interface inativo - a ligação não está transportando tráfego |
| VRVDR-41420 | Grave | Estado/link da ligação LACP “u/D” com mudança de modo de backup ativo para LACP |
| VRVDR-41313 | Crítico | IPsec - instabilidade da interface VTI |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 42207 | 7,5 | DSA-4217-1 | CVE-2018-11358, CVE-2018-11360, CVE-2018-11362, CVE- 2018-7320, CVE-2018-7334, CVE-2018-7335, CVE02018- 7419, CVE-2018-9261, CVE-2018-9264, CVE-2018-9273: Debian DSA-4217-1: wireshark – atualização de segurança |
| VRVDR- 42013 | N/A | DSA-4210-1 | CVE-2018-3639: execução especulativa, variante 4: bypass de armazenamento especulativo/Spectre v4/Spectre-NG |
| VRVDR- 42006 | 9,8 | DSA-4208-1 | CVE-2018-1122, CVE-2018-1123, CVE-2018-1124, CVE-2018- 1125, CVE-2018-1126: Debian DSA-4208-1: procps – atualização de segurança |
| VRVDR- 41946 | N/A | DSA-4202-1 | CVE-2018-1000301: Debian DSA-4202-1: curl – atualização de segurança |
| VRVDR- 41795 | 6,5 | DSA-4195-1 | CVE-2018-0494: Debian DSA-4195-1: wget – atualização de segurança |

## 1801j

Liberado em 18 de maio de 2018

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41481 | Secundário | O VRRP na interface de ligação não envia propaganda de VRRP |
| VRVDR-39863 | Grave | Ocorre failover do VRRP quando o cliente remove a instância de roteamento com GRE associado e o endereço local do túnel faz parte do VRRP |
| VRVDR-27018 | Crítico | A execução do arquivo de configuração é legível globalmente |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR-41680 | 7,8 | DSA-4188-1 | Debian DSA-4188-1: linux – atualização de segurança |

## 5.2R6S10

Liberado em 17 de maio de 2018

**Problemas resolvidos**
| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41543 | Grave | "update config-sync" está relatando erros quando a barra invertida “\” é usada nas descrições de configuração
| VRVDR-27018 | Crítico | A execução do arquivo de configuração é legível globalmente |

## 1801h

Liberado em 11 de maio de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41664 | Crítico | O plano de dados descarta pacotes ESP do tamanho do MTU |
| VRVDR-41536 | Secundário | O limite de inicialização do serviço Dnsmasq é atingido ao incluir mais de 4 entradas de host estáticas caso o encaminhamento de DNS esteja ativado |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 41797 | 7,8 | DSA-4196-1 | CVE-2018-1087, CVE-2018-8897: Debian DSA-4196-1: atualização de segurança do linux |

## 5.2R6S

Liberado em 8 de maio de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40803 | Secundário | As interfaces do VIF não estão presentes na saída “show vrrp” após uma reinicialização |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9,8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – atualização de segurança |

## 1801g

Liberado em 4 de maio de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41620 | Grave | O tráfego da interface vTI para de enviar tráfego após a inclusão de nova vIF |
| VRVDR-40965 | Grave | A ligação não se recupera após o travamento de um plano de dados |

## 1801f

Liberado em 23 de abril de 2018

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41537 | Secundário | O ping não está funcionando sobre o túnel IPsec no 1801d |
| VRVDR-41283 | Secundário | Configd parará o processamento de rotas estáticas durante a inicialização se a configuração tiver rotas estáticas desativadas |
| VRVDR-41266 | Grave | A rota estática com vazamento para o VRF não transita tráfego pelo túnel mGRE após a reinicialização |
| VRVDR-41255 | Grave | Quando o escravo fica inativo, leva mais de 60 segundos para que o estado do link principal reflita isso |
| VRVDR-41252 | Grave | Com a VTI desvinculada na política de zona, a regra de descarte é ignorada, dependendo da ordem de confirmação das regras de zona |
| VRVDR-41221 | Crítico | Fazendo upgrade de vRouters de 1801b para 1801c para 1801d com taxa de falha de 10% |
| VRVDR-40967 | Grave | A desativação do encaminhamento de IPv6 evita o roteamento de pacotes IPv4 de origem da VTI |
| VRVDR-40858 | Grave | A interface VTI mostrando a MTU 1428 está causando problemas de PMTU do TCP |
| VRVDR-40857 | Crítico | A ponte Vhost não aparece para VLAN identificada com nomes de interface de um determinado comprimento |
| VRVDR-40803 | Secundário | As interfaces do VIF não estão presentes na saída “show vrrp” após uma reinicialização |
| VRVDR-40644 | Grave | IKEv1: as retransmissões de QUICK_MODE não são manipuladas corretamente |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 41512 | 9,8 | DSA-4172-1 | CVE-2018-6797, CVE-2018-6798, CVE-2018-6913: Debian DSA-4172-1: perl – atualização de segurança |
| VRVDR- 41331 | 6,5 | DSA-4158-1 |CVE-2018-0739: Debian DSA-4158-1: openssl1.0 – atualização de segurança
| VRVDR- 41330 | 6,5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – atualização de segurança |
| VRVDR- 41215 | 6,1 |CVE-2018-1059 | CVE-2018-1059 – DPDK vhost com acesso à memória de host fora do limite de guests da VM |

##5.2R6S8
Liberado em 16 de abril de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-41283 | Secundário |Configd parará o processamento de rotas estáticas durante a inicialização se a configuração tiver rotas estáticas desativadas |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 41330 | 6,5 | DSA-4157-1 | CVE-2017-3738, CVE-2018-0739: Debian DSA-4157-1: openssl – atualização de segurança

##1801e
Liberado em 28 de março de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-39985 | Secundário | Os pacotes TCP DF maiores que MTU do túnel GRE são descartados sem a fragmentação de ICMP necessária retornada | 
| VRVDR-41088 | Crítico | ASN estendido (4 bytes) não representado internamente como tipo não assinado |
| VRVDR-40988 | Crítico | Vhost não iniciando quando usado com um determinado número de interfaces |
| VRVDR-40927 | Crítico | DNAT: SDP em SIP 200 OK não convertido ao seguir uma resposta 183 |
| VRVDR-40920 | Grave | Com 127.0.0.1 como endereço de atendimento, o snmpd não é iniciado |
| VRVDR-40920 | Crítico | O ARP não funciona em uma interface SR-IOV ligada |
| VRVDR-40294 | Grave | O plano de dados não restaura filas anteriores depois que o escravo é removido do grupo de ligação |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 41172 | N/A | DSA-4140-1 | DSA 4140-1: atualização de segurança do libvorbis |

##5.2R6S7
Liberado em 15 de março de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-38801 | Grave | Pacote com diversos segmentos recebido via IPsec VTI faz com que a interface de ligação seja desativa

##5.2R6S6
Liberado em 12 de março de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40281 | Grave | Após upgrade da 5.2 para a versão mais recente, erro “vbash: show: comando não localizado” no modo de operação |
| VRVDR-40135 | Grave | Os pacotes de árvore de amplitude não são recebidos em uma porta de ponte da interface VIF |
| VRVDR-39991 | Grave | O firewall stateful descarta pacotes entre duas sub-redes na mesma interface | 
| VRVDR-36481 | Grave | O upgrade/downgrade da 5.2R4 para 17.1.0/5.2R3 mostra /opt/vyatta/sbin/vyatta-install-image.functions: linha 372: is_onie_boot: comando não localizado |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 40019 | 8,8 | DSA-4086-1 | CVE-2017-15412: Debian DSA-4086-1: libxml2 – atualização de segurança |
| VRVDR- 39907 | 7,8 | CVE-2017-5717 | Injeção de destino de ramificação/CVE-2017-5717/Spectre, também conhecido como Variante nº 2 |

##1801d
Liberado em 8 de março de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40940 | Grave | Travamento do plano de dados relacionado à NAT/ao firewall |
| VRVDR-40886 | Grave | Combinar “icmp name <value>” com várias outras configurações da regra fará com que o firewall não seja carregado |
| VRVDR-39879 | Grave | A configuração da ligação para quadros gigantes falha |

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 40327 | 9,8 | | DSA-4098-1
CVE-2018-1000005, CVE-20178-1000007: Debian DSA- 4098-1: curl – atualização de segurança |
| VRVDR- 39907 | 7,8 | CVE-2017-5717 | Injeção do destino de ramificação/CVE-2017-5715/Spectre, também conhecido como variante nº 2 |

##1801c
Liberado em 7 de março de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40281 | Grave | Após upgrade da 5.2 para a versão mais recente, erro “-vbash: show: comando não localizado” no modo de operação |

##1801b
Liberado em 21 de fevereiro de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40622 | Grave | As imagens de inicialização da nuvem não serão detectadas corretamente se o endereço IP tiver sido obtido do servidor DHCP |
| VRVDR-40613 | Crítico | A interface de ligação não aparecerá se um dos links físicos estiver inativo |
| VRVDR-40328 | Grave | As imagens de inicialização da nuvem demoram muito para serem inicializadas |

##1801a
Liberado em 7 de fevereiro de 2018.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-40324 | Grave | As médias de carregamento excedem 1,0 sem nenhuma carga no roteador com a interface de ligação |

##5.2R6S5
Liberado em 19 de janeiro de 2018.

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 39891 | 5,6 | DSA-4078-1 | CVE-2017-5754: Debian DSA-4078-1: linux – atualização de segurança (Meltdown) |
| VRVDR- 38265 | 8,8 | DSA-3970-1 | CVE-2017-1 |

##5.2R6S4
Liberado em 15 de dezembro de 2017.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-39529 | Grave | O failover do servidor DHCP não está sincronizando bancos de dados |
| VRVDR-39399 | Crítico | O Vyatta descartou o estado de rede FAULT em mostrar vrrp/múltiplas oscilações de interfaces/falha do seg |
| VRVDR-39112 | Grave | O tráfego do DNAT que corresponde ao ZBF descarta apenas o primeiro pacote no fluxo |
| VRVDR-38075 | Secundário | Quando “restart vpn” é emitido do respondente, o inicializador não restabelece a conexão |
| VRVDR-37934 | Crítico | O BGPd trava quando somente o resumo do endereço agregado está configurado/rotas estáticas ausentes |
| VRVDR-37717 | Secundário | Renomear campos “Descrição” e “Licença” de hard-enf na saída da versão |
| VRVDR-37689 | Grave | Alta taxa de interrupções de NIC PF |
| VRVDR-37633 | Crítico | Interrupção de keep-alive |

## 5.2R6S3
Liberado em 4 de dezembro de 2017.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-39207 | Crítico | ARP com falha na interface VIF de ligação |


##5.2R6S2
Liberado em 2 de novembro de 2017.

**Problemas resolvidos**

| Número do problema | Prioridade | Resumo |
| --- | --- | --- |
| VRVDR-39177 | Grave | A opção domain-name do servidor OpenVPN não está sendo aplicada com -push dhcp-option |
| VRVDR-39129 | Crítico | O parâmetro push-route do servidor OpenVPN faz com que o OpenVPN falhe ao iniciar |

##5.2R6S1
Liberado em 12 de outubro de 2017.

**Vulnerabilidades de segurança resolvidas**

| Número do problema | Pontuação CVSS | Recomendação | Resumo |
| --- | --- | --- | --- |
| VRVDR- 38819 | 9,8 | DSA-3989-1 | CVE-2017-14491, CVE-2017-14492, CVE-2017-14493, CVE- 2017-14494, CVE-2017-14495, CVE-2017-14496: DSA- 3989-1 dnsmasq -- atualização de segurança |

As informações aqui contidas não são uma oferta, um compromisso, uma representação ou uma garantia da AT&T e estão sujeitas a mudanças. Ele não se destina ao uso ou divulgação fora das empresas AT&T, exceto sob acordo por escrito.

© 2018 AT&T Intellectual Property. Todos os direitos reservados. AT&T e o logotipo Globe são marcas registradas da AT&T Intellectual Property. Todas as outras marcas são propriedade de seus respectivos proprietários.
