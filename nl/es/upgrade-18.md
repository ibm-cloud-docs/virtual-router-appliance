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

# Consideraciones sobre la actualización para SO Versión 18.01

Este archivo contiene una lista de cosas que debe saber cuando vaya a actualizar el VRA (Vyatta 5600) del sistema operativo Brocade 5.X al sistema operativo 1801, que incluye la solución para la brecha de seguridad de Spectre. Hay algunos problemas que debe tener en cuenta cuando realice esta actualización.

## A quién va dirigido este tema

* Cualquiera que tenga un VRA que no ejecute el sistema operativo 1801. (La versión actual es la 1801g)

* Cualquiera que desee instalar una nueva versión 1801f para un dispositivo de direccionador virtual (por ejemplo, cualquiera que vaya a actualizar desde 17.2X).

* Si tiene un Vyatta 5400, esta información no se aplica a su caso.

## Por qué necesita esta información

La versión 1801f de VRA contiene un arreglo de seguridad para la vulnerabilidad de Spectre; sin embargo, debe realizarse un cambio en el propio instalador para que se pueda instalar el parche. Se necesita un paso intermedio para instalar la versión 1801C.

## Procedimiento normal de actualización
Para actualizar a la versión 1801f, primero debe descargar e instalar el parche 18.01C para actualizar el instalador de VRA:

1. Descargue el parche 1801c desde esta ubicación y luego siga el [procedimiento normal](upgrade-os.html) para instalarlo.

2. Cuando reinicie, descargue el parche 1801f desde esta ubicación e instálelo siguiendo el [procedimiento normal](upgrade-os.html).

Ahora debería ejecutar la versión 1801f, con la limitación de seguridad de Spectre solucionada.

## Procedimiento de recarga completa
Como alternativa, también puede realizar una recarga completa de VRA en el nivel 1801f:

*AVISO:* este procedimiento le permite omitir el paso intermedio de la descarga e instalación de dos parches, pero perderá todos los datos durante una recarga completa mediante el ISO.

1. Obtenga el ISO 1801f de esta ubicación.
2. Siga el [procedimiento normal](upgrade-os.html) para instalarlo y reinicie.

Ahora debería ejecutar la versión 1801f, con la limitación de seguridad de Spectre solucionada.

## Orientación adicional

Puesto que no hay una correlación sencilla de uno a uno en la funcionalidad entre Vyatta 5400 y el dispositivo direccionador virtual, resulta útil crear una configuración de línea base para VRA. Un IBM Partner, WanClouds, puede ayudarle con este proceso y proporcionarle orientación sobre la creación de una funcionalidad similar a la Vyatta 5400 en su VRA.

Para obtener más información sobre los problemas comunes que se han encontrado en esta actualización, consulte nuestra [documentación adicional](/docs/infrastructure/virtual-router-appliance/migration-issues.html#vyatta-5400-common-migration-issues).
