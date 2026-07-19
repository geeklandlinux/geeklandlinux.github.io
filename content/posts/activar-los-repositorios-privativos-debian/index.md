---
title: "Activar los repositorios privativos non-free en Debian"
date: 2016-11-13
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "debian"
  - "repositorios"
coverImage: "Activar-los-repositorios-privativos-non-free-en-Debian.jpg"
---

Se que habrá gente a la que no gustará este artículo porque estarán en contra de activar los repositorios privativos en Debian. No obstante en ocasiones se hace necesario activar los repositorios privativos en Debian.<!--more-->

## MOTIVOS PARA ACTIVAR LOS REPOSITORIOS PRIVATIVOS EN DEBIAN

Algunas de los motivos que pueden forzar la activación de los repositorios privativos en Debian son los siguientes:

1. [Hacer que nuestra tarjeta de red inalámbrica funcione]({{< relref "/posts/instalar-drivers-tarjeta-de-red-wifi-debian" >}}).
2. Sacar el máximo rendimiento a nuestro tarjeta gráfica o a nuestro hardware en general.
3. Para reproducir algunos formatos de archivo específicos.
4. Para instalar y usar Flash en el caso que aún lo usemos.
5. Etc.

## ESTRUCTURA DE LOS REPOSITORIOS DE DEBIAN

Los repositorios de Debian se dividen en 3 ramas conocidas como main, contrib y non-free. Cada una de estas ramas contiene paquetes clasificados en función de su licencia.

**La rama main** contiene el 90% de los paquetes de la distribución. La totalidad de estos paquetes son libres y cumplen con los 10 principios de la [Debian Free software Guidelines](https://www.debian.org/social_contract#guidelines "link para consultar las directrices de software libre de Debian ").

**La rama contrib** contiene software que también cumple con los principios de la Debian Free software Guidelines, pero que precisa de software o paquetes no libres para su compilación y/o ejecución.

Finalmente la **rama non-free** incluye paquetes privativos que obviamente no cumplen con las directrices de la Debian Free software Guidelines.

## ACTIVACIÓN DE LOS REPOSITORIOS PRIVATIVOS EN DEBIAN

Debian es una distribución que después de ser instalada únicamente contiene paquetes y software 100% libre. La razón es porque la única rama de repositorios activa es la **main**.

Para poder activar los repositorios privativos tenemos que activar las ramas de los repositorios **contrib** y **non-free**.

Para hacerlo accedemos al archivo de configuración de los repositorios ejecutando el siguiente comando en la terminal:

> ```
> sudo nano /etc/apt/sources.list
> ```

Justo al abrirse el fichero veremos los repositorios de Debian que estamos usando. En mi caso uso los siguientes repositorios:

> ```
> deb http://ftp.de.debian.org/debian/ testing main
> deb-src http://ftp.de.debian.org/debian/ testing main
> 
> deb http://security.debian.org/ testing/updates main
> deb-src http://security.debian.org/ testing/updates main
> ```

Si leemos el contenido del fichero vemos que de las 3 ramas de repositorios únicamente disponemos de la **main**. Para añadir las otras 2 ramas tan solo tenemos que añadir las palabras **contrib non-free** justo después de main. Por lo tanto en mi caso los repositorios pasarán a quedar de la siguiente forma:

> ```
> deb http://ftp.de.debian.org/debian/ testing main contrib non-free
> deb-src http://ftp.de.debian.org/debian/ testing main contrib non-free
> 
> deb http://security.debian.org/ testing/updates main contrib non-free
> deb-src http://security.debian.org/ testing/updates main contrib non-free
> ```

Una vez realizadas las modificaciones guardamos los cambios y cerramos el fichero. En estos momentos podemos afirmar que los repositorios privativos están activados.

###### Nota: Tenéis que ir con cuidado a la hora de manipular los repositorios. En ningún caso reemplacéis vuestros repositorios por los mios. Lo único que os tenéis que limitar a realizar es añadir y o quitar las palabras contrib y non-free.

## ACTUALIZAR EL ÍNDICE DE LOS REPOSITORIOS

Para poder tener disponibles los paquetes de los repositorios contrib y non-free actualizamos el índice de nuestros repositorios ejecutando el siguiente comando en la terminal:

> ```
> sudo apt-get update
> ```

Una vez actualizado el índice podremos instalar software privativo en el caso que lo consideremos oportuno.

## CONSULTAR LOS NUEVOS PAQUETES QUE PODEMOS INSTALAR

En estos momentos los repositorios privativos de Debian están activados y por lo tanto ya podemos instalar paquetes privativos sin ningún tipo de problema.

Para consultar los paquetes disponibles en la sección **contrib** podemos usar Synaptic o ejecutar el siguiente comando en la terminal:

> ```
> aptitude search '?section(contrib) ?origin(Debian)'
> ```

Para consultar los paquetes disponibles en la sección **non-free** podemos usar Synaptic o ejecutar el siguiente comando en la terminal:

> ```
> aptitude search '?section(non-free) ?origin(Debian)'
> ```

Si en un momento determinado pretenden averiguar la cantidad de software privativo instalado en su equipo pueden seguir las instrucciones del siguiente enlace:

[https://geekland.eu/conocer-software-privativo-instalado/]({{< relref "/posts/conocer-software-privativo-instalado" >}})

De este modo siempre podremos controlar de forma fácil los paquetes privativos que tenemos instalados en nuestra distribución.
