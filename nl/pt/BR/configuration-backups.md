---

copyright:
  years: 2017
lastupdated: "2017-10-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Fazer backup de uma configuração
Os comandos de configuração precisam ser submetidos a backup quando há uma mudança no sistema. Isso pode ser feito executando o comando do modo operacional `mostrar comandos de configuração` e, em seguida, salvando a saída (por exemplo, copiando e colando na sessão SSH). Isso seria considerado um backup mínimo para a configuração.

Um backup mais completo envolve gerar um archive de suporte técnico para o sistema: 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

O archive gerado pode, então, ser copiado do Virtual Router Appliance para o dispositivo de armazenamento de sua escolha. O archive contém os backups das informações de configuração, diretórios iniciais e informações de criação de log. Ele é um backup muito mais completo de um sistema. 

Como um exemplo:

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Considere fazer backup de quaisquer notas que você crie ao configurar o dispositivo em um local central acessível a toda sua equipe de administração.
