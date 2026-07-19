---
title: "¿Cuál es el soporte que nos ofrece la distribución Debian?"
date: 2017-06-24
categories: 
  - "linux-2"
tags: 
  - "debian"
  - "lts"
  - "oldstable"
  - "repositorios"
coverImage: "soporte-que-ofrece-debian.jpg"
---

En el momento que sale una nueva versión estable de Debian, todo el mundo escribe los típicos tutoriales de como actualizar Debian a la última versión. No obstante en el momento que sale una nueva versión de Debian no es necesario realizar absolutamente nada porque la Debian que a priori es vieja aun tiene soporte para muchos años.

Por lo tanto en el momento que sale una versión nueva de Debian no es necesario realizar ningún tipo de actualización.<!--more-->

## SOPORTE QUE OFRECE LA DISTRIBUCIÓN DEBIAN

Las distribuciones Debian reciben un soporte de 5 años de la forma que se detalla a continuación.

### En la rama estable

En el momento que una versión de Debian se libera como estable pasará a tener un soporte con las siguientes características:

1. Tendrá una duración de aproximadamente 2 años.
2. Será realizado por parte del equipo de Debian.
3. Será completo y por lo tanto incluirá todos los paquetes y todas las arquitecturas de la distribución.

A modo de ejemplo, El 25 de Abril de 2015 se libero Debian Jessie como versión estable y el día 17 de Junio de 2017 finalizo su soporte como rama estable.

### En la rama oldstable

Una vez finalizado el soporte de la rama estable empezará el de la rama oldstable. Las características del soporte oldstable son las siguientes:

1. Su duración es de un año.
2. Es realizado por parte del equipo de Debian.
3. Es completo y al mismo nivel que en la rama estable. Por lo tanto también incluye todos los paquetes y todas las arquitecturas de la distribución.

Por lo tanto, a partir de 17 de Junio de 2017 Debian Jessie pasa a ser la versión oldstable y recibirá soporte por parte del equipo de Debian hasta el Mayo o Junio del año 2018.

### En la rama LTS

Al finalizar el soporte oldstable empezará el soporte [LTS](https://wiki.debian.org/LTS "Explicación del soporte LTS de Debian"). El soporte LTS tendrá las siguientes características:

1. Nos ofrecerá 2 años adicionales de actualizaciones críticas de seguridad.
2. No es proporcionado por el equipo de Debian. Es proporcionado por grupos de voluntarios y empresas interesadas en prolongar la vida de la rama LTS de Debian.
3. Es inferior en comparación con las ramas estable y oldstable. El nivel de soporte ofrecido dependerá de los voluntarios existentes y del soporte financiero que reciban.
4. No cubre todas las arquitecturas ni todos los paquetes. Únicamente se cubren paquetes críticos y las arquitecturas i386, amd64, armel y armhf.
5. El repositorio de backports no recibe actualizaciones por parte del equipo que proporciona el soporte extendido LTS. Las actualizaciones en los repositorios backports son realizadas por el personal encargado de mantener los paquetes en el repositorio backports.

A modo de ejemplo, en Mayo o Junio de 2018 finalizará el soporte oldstable de Debian Jessie. Justo entonces empezará el soporte LTS que durará hasta Abril del 2020. A partir del Abril del 2020 podremos considerar que Debian Jessie ha muerto.

### Resumen del soporte que ofrecen las distribuciones Debian

En la siguiente tabla pueden ver de forma esquemática el soporte que ofrece Debian en cualquiera de sus versiones:

   
|  |   **Duración (años)**   |   **¿Quién lo realiza?**   |   **Alcance**   |
| --- | --- | --- | --- |
|   Rama estable   |   2   |   Equipo de Debian   |   Completo   |
|   Rama oldstable   |   1   |   Equipo de Debian   |   Completo   |
|   Rama LTS   |   2   |   Grupo de Voluntarios   |   Solo actualizaciones críticas en algunas arquitecturas   |
|   Total soporte   |   5   |  |  |

Por lo tanto, podremos usar Debian Jessie, o cualquier otra versión de Debian, durante 5 años sin tener ningún tipo de problema. Durante los 3 primeros años el soporte de nuestra distribución será completo mientras que los últimos 2 años será parcial.

## ¿QUÉ HAY QUE HACER PARA USAR EL SOPORTE DE 5 AÑOS EN DEBIAN?

No hay que realizar absolutamente nada. No es necesario tocar ningún repositorio ni realizar nada en especial. Simplemente tenemos seguir usando nuestra distribución Debian de forma habitual.

## DISPONER DE PAQUETERIA ACTUALIZADA EN OLDSTABLE Y LTS

###### Nota: Este apartado solo aplica a usuarios que utilicen el repositorio backports.

En el momento que una versión de Debian pasa a oldstable dejará de actualizarse el [repositorio backports]({{< relref "/posts/repositorio-backports-debian-estable" >}}). Por lo tanto con el paso del tiempo, los paquetes y programas de nuestro ordenador pasarán a ser bastante antiguos. Esto es así para facilitar la vida a los usuarios que algún día pretendan actualizar de la versión oldstable o LTS a la versión estable.

En el caso que alguien considere que esto es un problema puede agregar el repositorio backports-sloppy. De este modo, aunque estemos en una distribución oldstable o LTS podremos usar algunos de los paquetes presentes en Debian Testing o Inestable. En futuros post detallaré de forma simple como podemos usar el repositorio backports-sloppy.

## ¿ES NECESARIO ACTUALIZAR DEBIAN CUANDO SALE UNA NUEVA DEBIAN ESTABLE?

En el momento que sale una nueva versión de Debian no muere la antigua y no es necesario actualizar a la nueva versión estable.

Existen usuarios y empresas que retrasan tanto como pueden la actualización a la nueva versión de Debian por los siguientes motivos:

1. La actualización a la nueva versión de un gran número de servidores y ordenadores exige tiempo. Por lo tanto, el soporte extendido será usado para muchos administradores para actualizar con calma y con seguridad sus equipos.
2. Existen administradores que por simple practicidad deciden no tocar lo que funciona. Por lo tanto simplemente realizan actualizaciones importantes cuando no hay más remedio.

Espero que este artículo les haya ayudado a conocer un poco más la distribución Debian.
