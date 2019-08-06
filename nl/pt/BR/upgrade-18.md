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

# Considerações sobre upgrade para o S.O. Versão 18.01

Este arquivo contém uma lista de assuntos que você deve saber quando está fazendo upgrade do VRA (Vyatta 5600) a partir do sistema operacional Brocade 5.X para o sistema operacional 18.01, que inclui a correção para a violação de segurança do Spectre. Há vários problemas a serem observados ao executar esse upgrade.

## Quem precisa ler este tópico

* Qualquer um com um VRA atual que não esteja executando o sistema operacional 1801. (A versão atual é a 18.01g)

* Qualquer pessoa procurando instalar uma nova versão 18.01f para um {{site.data.keyword.vra_full}} (por exemplo, qualquer um que fizer upgrade da 17.2X).

* Se você tiver uma Vyatta 5400, essas informações não se aplicarão a você.

## Por que você precisa dessas informações

A versão 1801f do VRA contém uma correção de segurança para a vulnerabilidade do Spectre, no entanto, uma mudança no próprio instalador deve ser feita antes que a correção possa ser instalada. É necessária uma etapa intermediária para instalar a versão 1801C.

## Procedimento normal de upgrade
Para fazer upgrade para a versão 18.01f, deve-se primeiro fazer download e instalar a correção 18.01C para atualizar o instalador do VRA:

1. Faça download da correção 1801c por meio deste local e, em seguida, siga o [procedimento normal](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) para instalá-la.

2. Depois de reinicializar, faça download da correção 18.01f nesse local e instale-a usando o [procedimento normal](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os).

Agora é necessário que a versão 18.01f esteja sendo executada, com a limitação de segurança do Spectre corrigida.

## Procedimento de recarregamento completo
Como alternativa, também é possível fazer um recarregamento completo do VRA no nível 18.01f:

*AVISO:* este procedimento permite ignorar a etapa intermediária de download e da instalação de duas correções, mas você perderá todos os dados durante um recarregamento completo usando a ISO.

1. Obtenha o ISO 18.01f nesse local.
2. Siga o [procedimento normal](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) para instalá-lo e reinicializar.

Agora é necessário estar executando a versão 1801f, com a limitação de segurança do Spectre corrigida.

## Orientação adicional

Como não há um mapeamento simples de um para um de funcionalidade entre o Vyatta 5400 e o {{site.data.keyword.vra_full}}, a criação de uma configuração de linha de base para o VRA é útil. Um Parceiro IBM©, o WanClouds, pode ajudá-lo com esse processo e fornecer orientação sobre a criação de uma funcionalidade semelhante à do Vyatta 5400 em seu VRA.

Para obter mais informações sobre problemas comuns encontrados nesse processo de upgrade, consulte nossa [documentação adicional](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues).
