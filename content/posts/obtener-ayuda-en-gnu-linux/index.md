---
title: "Fuentes para obtener ayuda y solucionar problemas en Linux"
date: 2017-08-28
categories: 
  - "linux-2"
tags: 
  - "comandos"
  - "manpages"
  - "terminal"
cover:
  image: "images/formas-para-obtener-ayuda-en-gnu-Linux.jpg"
  relative: true
---

Todo usuario que empieza en Linux, e incluso usuarios expertos, necesitaran obtener ayuda o información sobre determinados aspectos del sistema operativo. Las fuentes disponibles para intentar obtener ayuda son las que se muestran a continuación.<!--more-->

## OBTENER AYUDA A TRAVÉS DEL SISTEMA OPERATIVO

Nuestro propio sistema operativo nos puede brindar ayuda para solucionar gran de los problemas y dudas que podamos tener. Las distintas opciones que nos ofrece el sistema operativo son las siguientes:

### Las páginas del manual (manpages)

Si tenemos dudas sobre el funcionamiento de algún comando de la terminal podemos utilizar las páginas del manual.

Para ello tan solo tenemos que teclear el comando man seguido del comando que queremos buscar información. De este modo, si queremos buscar las instrucciones de uso del comando apt tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> man apt
> ```

Después de ejecutar el comando obtendrán una explicación extensa y completa sobre los siguientes aspectos:

1. Utilidad de la aplicación apt.
2. Explicación de como se construyen los comandos con apt.
3. Descripción de cada una de las opciones que se pueden usar con el comando apt.
4. Ejemplos de uso del comando apt.
5. Etc.

De forma predeterminada las páginas man de están en Inglés. No obstante es muy fácil poner las [páginas man en Español]({{< relref "/posts/poner-paginas-man-en-espanol-en-linux" >}}) o en Francés.

###### Nota: Para usar el comando man hay que tener instalado el paquete manpages en su sistema operativo.

### Ayuda a través del comando info

La información/ayuda proporcionada por el comando info es muy similar a la que proporcionan las páginas del manual. En algunos casos incluso es la misma.

No obstante, la información proporcionada por info acostumbra a ser más extensa que la de las páginas man. Por lo tanto, en el caso que la información proporcionada por man no sea suficiente pueden usar el comando info.

A modo de ejemplo, si queremos buscar información sobre el comando sed deberemos ejecutar el siguiente comando en la terminal:

> ```
> info sed
> ```

Si ejecutan el comando verán que info es capaz de proporcionar información mediante una interfaz navegable. Además podrán ver que las explicaciones son más extensas que las de las paginas man.

###### Nota: Para usar el comando info hay que tener instalado el paquete info en su sistema operativo.

### Obtener ayuda mediante el comando --help

Otro de los comandos que podemos utilizar para obtener ayuda es el \--help. Este comando nos ofrecerá una ayuda breve, concisa y en nuestro idioma de como ejecutar un comando en concreto.

A modo de ejemplo, para ver el uso del comando sed ejecutaríamos el siguiente comando en la terminal:

> ```
> sed --help
> ```

De este modo podremos comprender de forma simple y concisa como utilizar el comando sed.

### Usar la ayuda que traen incorporada los programas

Muchos programas disponen de ayuda incorporada. Tan solo tenemos que consultar el apartado Ayuda de su menú y encontraremos información sobre el uso básico del programa en cuestión. Un ejemplo de lo que estoy comentando es Libreoffice.

[![Obtener ayuda en Libreoffice](images/obtener-ayuda-en-Libreoffice.png "Obtener ayuda en Libreoffice")](images/obtener-ayuda-en-Libreoffice.png)

## OBTENER AYUDA MEDIANTE LOS BLOGS

En el caso que la ayuda proporcionada por las páginas de ayuda no sea suficiente pueden buscar en blogs que hablen sobre Linux.

Existen muchísimos blogs que se dedican a realizar tutoriales sobre como realizar determinadas tareas. Por lo tanto, en el caso que tengan que realizar un proyecto o solucionar un problema, les recomiendo que usen su buscador web preferido para encontrar algún blog que contenga la información que necesitan.

En el caso que después de seguir las instrucciones recomendadas en el blog tengan problemas tienen la posibilidad de detallar lo que sucede en los comentarios del blog.

En este apartado no voy a recomendar ningún blog. Lo mejor en este caso es que se guíen por su experiencia y por los [resultados de búsqueda]({{< relref "/posts/tecnicas-consejos-buscar-en-google" >}}) de vuestro buscador web.

## OBTENER AYUDA EN YOUTUBE

En YouTube también encontrarán canales que hablan de como solucionar problemas, como usar un determinado programa y realizar todo tipo de proyectos. Por lo tanto les recomiendo que entren en el buscador de YouTube y realicen una búsqueda para encontrar información sobre el tema que están buscando.

Algunos de los canales que están en activo y considero interesantes que visiten son los siguientes:

Canales de habla hispana:

- [La Guía Linux](https://www.youtube.com/channel/UCYRyGbiEUZyoNjLjv3MYA7w "Link del canal la Guía Linux")
- [Kalerolinex](https://www.youtube.com/channel/UC-ApRMpARJwZ3ypkwWry-eQ "Link del canal Kalerolinux")
- [La cueva del último dragón](https://www.youtube.com/channel/UCllrFdkbcxcuAdzRHj-KSMA "Link")
- [Reality Cracking](https://www.youtube.com/channel/UChHaITAvQD-Py8sR26inAmw "Link del canal Reality Cracking")

Canales de habla inglesa:

- [The Urban Penguin](https://www.youtube.com/channel/UCFFLP0dKesrKWccYscdAr9A "Link del canal The Urban Penguin")
- [Gotbletu](https://www.youtube.com/channel/UCkf4VIqu3Acnfzuk3kRIFwA "Link del canal Gotbletu")
- [Hak5](https://www.youtube.com/channel/UC3s0BtrBJpwNDaflRSoiieQ "Link del canal Hak5")
- [Matthew Moore](https://www.youtube.com/channel/UCVTlmFfclZsEj_Wo5P_3YRQ "Link del canal Matthew Moore")

###### Nota: Esta lista está basada en mis gustos personales. Obviamente existen otros canales que están bien y hablan sobre Linux y el software libre.

## OBTENCIÓN DE AYUDA MEDIANTE LOS FOROS DE INTERNET

Cada día existen menos foros que hablen sobre Linux y Software Libre. No obstante aun siguen existiendo buenos foros en los que podremos obtener ayuda.

Algunos de los foros que recomiendo visitar son los siguientes:

Foros de habla Hispana:

- [Ubuntu es](http://www.ubuntu-es.org/)
- [Exdebian](https://exdebian.org/)
- [Manjaro Linux](http://manjaro-es.org/)
- [Espacio Linux](http://www.espaciolinux.com/)
- [Stack Overflow](https://es.stackoverflow.com/)

Foros de habla Inglesa:

- [Debian Users](http://forums.debian.net/)
- [Ubuntuforums](https://ubuntuforums.org/)
- [Linux Questions](https://www.linuxquestions.org/)
- [Stack Exchange](https://unix.stackexchange.com/)
- [Stack Overflow](https://stackoverflow.com/)

Estos son los foros que acostumbro a visitar. Algunos de ellos únicamente están enfocados para dar soporte a la gente que usa distribuciones con paquetería .deb. Si alguien quiere recomendar un foro importante que no esté en el listado, lo puede realizar a través de los comentarios de blog.

## OBTENER AYUDA EN LAS REDES SOCIALES

Existen redes sociales en la que es muy efectivo obtener ayuda. Algunas de ellas son las siguientes:

### Reddit

Bajo mi punto de vista, Reddit es la mejor red social para obtener ayuda en Linux. El único inconvenientes es que la red social es en Inglés.

Los 3 subreddits que en mi caso acostumbro a usar para resolver mis dudas son los siguientes:

- [linuxquestions](https://www.reddit.com/r/linuxquestions/ "Subreditt para pedir ayuda en Linux")
- [linux4noobs](https://www.reddit.com/r/linux4noobs/ "Subreditt para pedir ayuda en Linux")
- [xfce](https://www.reddit.com/r/xfce/ "Subreditt para pedir ayuda relacionada con XFCE")

En estos subreddits pueden hacer cualquier tipo de pregunta y casi con total seguridad será respondida de forma clara y concisa. Es recomendable hacer una búsqueda previa antes de preguntar en Reddit, en caso contrario, es probable que no se os responsa con educación.

### Google Plus

Otra fuente importante para obtener ayuda en Linux son las comunidades de Google Plus. Algunas comunidades destacadas en las que podemos presentar nuestras dudas son las siguientes:

- [Linux en Español](https://plus.google.com/communities/116960921595845362114)
- [Usemos Linux](https://plus.google.com/communities/110075815123635300569)
- [Software Libre](https://plus.google.com/communities/105479089621784007901)
- [Debian en Español](https://plus.google.com/communities/100422384136157860261?hl=es)

Obviamente existen más comunidades, pero las que cito son las más grandes y activas que existen en el mundo de habla hispana.

### Otras redes sociales para obtener ayuda en Linux

Existen otras redes a las que podemos acudir. Sin ir más lejos podemos citar Telegram o Facebook. No obstante no profundizo sobre ellas porque no son medios que acostumbre a usar para buscar ayuda.

## OBTENER AYUDA MEDIANTE LIBROS Y LAS WIKI

Finalmente pueden encontrar libros interesantes para poderse iniciar en el uso de un sistema operativo Linux. Un ejemplo interesante es el [manual del administrador de Debian](https://debian-handbook.info/download/es-ES/stable/debian-handbook.pdf)

Tampoco debemos olvidar las Wiki. Existen muchas wiki, pero las que en mi caso considero detacables son las siguientes:

1. [Wiki de Archlinux](https://wiki.archlinux.org/) (Disponible tanto en Inglés como en Español)
2. [Wiki de Gentoo](https://www.gentoo.org/support/documentation/)
3. [La Wiki de Ubuntu](https://wiki.ubuntu.com/)
4. Etc.

Para finalizar solo quiero animar a los lectores para que dejen las fuentes de ayuda que usan en los comentarios del blog.
