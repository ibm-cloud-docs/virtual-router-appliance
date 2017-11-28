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

# Configuración de copia de seguridad
Se debe realizar una copia de seguridad de los mandatos de configuración cuando se realiza un cambio en el sistema. Esto se puede lograr ejecutando el mandato de modo operativo `show configuration commands` y guardando la salida (por ejemplo, copiando y pegando desde la sesión SSH).Esto se considera una copia de seguridad mínima de la configuración. 

Una copia de seguridad más complica implica la generación de un archivo de soporte técnico para el sistema: 

```
$ generate tech-support archive
Saving the archivals...
Saved tech-support archival at /opt/vyatta/etc/configsupport/mpatr-vyatta-one.tech-support-archive
2013-08-27-155554.tgz
```

El archivo de archivado generado se puede copiar de Virtual Router Appliance al dispositivo de almacenamiento de su elección. Este archivo contiene copias de seguridad de la información de configuración, los directorios de inicio y la información de registro. Es una copia más completa del sistema. Por ejemplo:

```
-rw-r--r--  1 michael  michael    7863 Aug 22 12:46 config.tgz
-rw-r--r--  1 michael  michael     112 Aug 22 12:46 core-dump.tgz
-rw-r--r--  1 michael  michael  716033 Aug 22 12:46 etc.tgz
-rw-r--r--  1 michael  michael    3698 Aug 22 12:46 home.tgz
-rw-r--r--  1 michael  michael    1092 Aug 22 12:46 root.tgz
-rw-r--r--  1 michael  michael    4204 Aug 22 12:46 tmp.tgz
-rw-r--r--  1 michael  michael   82976 Aug 22 12:46 var-log.tgz
```

Tenga una copia de las notas que crea mientras configura el dispositivo en una ubicación central accesible para todo el personal de administración.
