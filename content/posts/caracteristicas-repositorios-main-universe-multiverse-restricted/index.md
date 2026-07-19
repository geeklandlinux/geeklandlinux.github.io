---
title: "Características de los repositorios Main, Universe, Multiverse, restricted"
date: 2017-09-22
categories: 
  - "linux-2"
tags: 
  - "repositorios"
  - "ubuntu"
coverImage: "repositorios-en-ubuntu.jpg"
---

A continuación veremos una breve explicación de las características y el tipo de paquetes y software que encontraremos en cada uno de los repositorios principales de Ubuntu y distribuciones derivadas de Ubuntu. Además daré mi opinión sobre los repositorios que bajo mi experiencia podemos activar.<!--more-->

## CARACTERÍSTICAS DE LOS REPOSITORIOS PRINCIPALES DE UBUNTU Y LINUX MINT

Los repositorios existentes en Ubuntu se dividen en 4 bloques. Cada uno de los bloques contendrá paquetes y programas en función de los siguientes factores:

1. El encargado de realizar el mantenimiento de los repositorios.
2. La naturaleza los paquetes presentes en el repositorio.

La explicación de cada uno de los 4 grandes bloques es la siguiente:

### Características del repositorio Main

Es el repositorio principal de Ubuntu. En este repositorio hallamos los paquetes principales de la distribución. La totalidad de paquetes que están en este repositorio disponen de las siguientes características:

1. Son libres u Open Source.
2. El mantenimiento del repositorio es realizado por Canonical. Recuerden que Canonical es la empresa encargada de desarrollar Ubuntu.
3. El software incluido en este repositorio dispondrá de actualizaciones de seguridad y soporte técnico por parte del equipo de Canonical.
4. Incluyen los paquetes y las aplicaciones que la comunidad y los desarrolladores de Ubuntu consideran más importantes.

Algunos de los paquetes conocidos que acostumbramos de este repositorios son los siguientes:

1. Firefox
2. Evince
3. Nano
4. Etc.

### Características del repositorio Universe

Este repositorio incluye paquetes de software libre y open Source, pero a diferencia del repositorio Main, estos paquetes están mantenidos por la comunidad. Por lo tanto las características de los paquetes de este repositorio son las siguientes:

1. Incluye la totalidad de paquetes libres y Open Source que no han sido incluidos en el repositorio Main.
2. Los paquetes de este repositorio son mantenidos por la comunidad. Canonical no garantiza actualizaciones de seguridad ni la corrección de bugs de los paquetes incluidos en este repositorio. Las actualizaciones de seguridad y corrección de errores dependerá de la comunidad.
3. Usar los paquetes de este repositorio entraña un riesgo mínimo para el usuario.
4. En el caso que exista un software muy popular, o que esté bien soportado, se trasladará al repositorio main.

Algunos ejemplos de programas que encontraremos en este repositorio son Abiword, Vlc, Audacious, Pidgin, etc.

### Características del repositorio restricted

El repositorio restricted básicamente incluye drivers privativos. Mediante el uso de estos drivers privativos, en algunos casos podremos hacer funcionar el siguiente hardware:

1. La tarjeta de red wifi.
2. La tarjeta gráfica del ordenador.
3. El Bluetooth.
4. etc.

Las características principales del software de este repositorio son las siguientes:

1. La totalidad de software es privativo.
2. El mantenimiento de este repositorio es realizado por Canonical.
3. El soporte que puede proporcionar Canonical es limitado. Al tratarse de software privativo, Canonical únicamente podrá proporcionar soluciones o actualizaciones cuando los creadores del software privativo reaccionen.

### Características del repositorio multiverse

Repositorio que contiene software no libre y software open source con restricciones legales. Las características de los paquetes contenidos en este repositorio son las siguientes:

1. Los paquetes de este repositorio no cumplen con las directivas del software libre.
2. El soporte de este repositorio es realizado por la comunidad. Al tratarse de paquetes privativos o con restricciones legales, hace que las actualizaciones de seguridad y la resolución de problemas sea más lenta.
3. Antes de usar este software de este repositorio hay que asegurarse falta completar

Algunos de los paquetes que por ejemplo se encuentran en este repositorio son:

1. Adobe Flash Plugin.
2. Códecs para reproducir audio y vídeo.
3. libdvdcss para reproducir DVD.
4. Unrar.
5. Ubuntu-restricted-extras.
6. nautilus-dropbox.
7. Etc.

### Tabla resumen de los repositorios de Ubuntu

Una tabla resumen de la totalidad del contenido que hemos visto hasta el momento es la siguiente

   
|  |   **Software Libre / Open Source**   |   **Software privativo**   |   **Mantenido por Ubuntu**   |
| --- | --- | --- | --- |
|   **Main**   |   Sí   |   No   |   Sí   |
|   **Universe**   |   Sí   |   No   |   No   |
|   **Restricted**   |   No   |   Sí   |   Sí   |
|   **Multiverse**   |   Sí   |   Sí   |   No   |

## OTROS REPOSITORIOS EXISTENTES EN UBUNTU

Aparte de los 4 grandes repositorios que acabamos de mencionar existen otros repositorios. Algunos de ellos son los siguientes.

### Socios canonical

Todas las distribuciones Ubuntu, o basadas en Ubuntu, disponen de un repositorio llamado socios canonical.

Este repositorio contiene programas privativos que canonical empaqueta para que sus usuarios los puedan instalar y usar de forma fácil.

Canonical no tendrá acceso al código fuente de estos programas. Por lo tanto la totalidad de bugs y problemas de seguridad únicamente podrán ser solucionados por los desarrolladores

Algunos de los programas que figuran en este repositorios son por ejemplo:

1. Skype
2. adobe-flashplugin
3. Adobe Air
4. Adobe reader
5. etc.

### Repositorios ppa

En Ubuntu y en distribuciones derivadas de Ubuntu existen repositorios creados por terceros y que podemos añadir a nuestra distribución. Estos repositorios son los PPA (Personal Package Archive).

La utilidad principal de estos repositorios son:

1. Instalar un software que no se halla en los repositorios de nuestra distribución.
2. Actualizar un software que ya está presente en nuestro equipo a la última versión.

Estos repositorios son completamente ajenos a Ubuntu. Por lo tanto el mantenimiento y actualización de los paquetes de este repositorio dependen única y exclusivamente del personal que gestiona el repositorio ppa.

Estos repositorios son útiles y facilitan la vida de los usuarios. No obstante hay que ir con cuidado porque estos repositorios pueden llegar a crear problemas de dependencias en nuestro equipo.

Les recomiendo visitar el siguiente enlace para ver algunos de los [repositorios ppa](https://www.ubuntuupdates.org/ppas "Lista de repositorios ppa que podemos añadir") existentes en Ubuntu.

## ¿QUÉ REPOSITORIOS DEBO ACTIVAR EN UBUNTU Y LINUX MINT?

En mi caso siempre he usado Ubuntu o Linux Mint activando la totalidad de repositorios. Las conclusiones basadas en mi experiencia son las siguientes:

### Repositorios principales de Ubuntu y Linux mint

El hecho de activar los repositorios Universe y Multiverse nunca me ha generado problemas de estabilidad. Además en el hipotético caso que existiera un problema grave, la comunidad se volcaría a solucionarlo rápidamente.

Por lo tanto mi punto de vista es que el hecho de activar unos repositorios u otros se limita a una cuestión ética.

En el caso que no quieran tener programas privativos o con problemas de licencia en su equipo activen únicamente los repositorios Main y Universe.

Si por lo contrario no os importa tener la posibilidad de instalar programas privativos o con problemas de licencia activen la totalidad de repositorios.

### Repositorios de terceros ppa

En el caso de los repositorios ppa hay que tener cierta precaución. A pesar que para crear un repositorio ppa se tiene que aceptar un [código de conducta](https://launchpad.net/codeofconduct/1.1 "Código de conducta de los repositorios ppa"), solo recomiendo usar repositorios ppa que sean de confianza. Los factores a tener en cuenta para saber si un repositorio es fiable son los siguientes:

#### ¿Quién ha realizado y mantiene el repositorio?

A modo de ejemplo, si queremos instalar Libreoffice mediante un repositorio ppa y vemos que el repositorio ppa que queremos usar fue creado y es gestionado por Libreoffice podemos estar seguros que se trata de un repositorio confiable.

En Ubuntu también existen repositorios ppa creados y mantenidos por gente o entidades conocidas y confiables como por ejemplo el atereao, OMGUbuntu, webupd8, etc. Estos repositorios en principio también son confiables y se pueden usar sin ningún tipo de problema porque sus desarrolladores son gente conocida y de confianza.

#### ¿Cuantos usuarios están usando el repositorio ppa?

Cuantos más usuarios tenga un ppa mayor será la confianza que me ofrece. Si un ppa no es confiable lo usará muy poca gente.

#### Frecuencia de actualizaciones del repositorio ppa

El hecho que un repositorio reciba actualizaciones frecuentes también es una señal clara que se trata de un repositorio confiable. Si instalamos un programa de un repositorio que hace por ejemplo 2 años que no se actualiza, los más probable es que el programa instalado no funcione.

#### Naturaleza del ppa

Hay que ser consciente del ppa que añadimos en todo momento. Existen ppa de programas que por ejemplo están en una versión testing o inestable. Por lo tanto el uso de repositorios con software en estado inestable puede ocasionar algún tipo de inconveniente.
