---
title: "¿Qué son y cómo podemos usar los repositorios redirector en Debian?"
date: 2017-01-16
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "debian"
  - "redirector"
  - "repositorios"
coverImage: "Usar-los-repositorios-redirector-en-Debian.png"
---

En el siguiente artículo veremos como seleccionar el mejor repositorio de forma automática mediante los repositorios redirector de Debian. En el pasado ya escribí un post en el que detallaba como [encontrar los repositorios más rápidos]({{< relref "/posts/elegir-el-mejor-repositorio-en-debian" >}}) en Debian. El método que mostrábamos era y es perfectamente válido pero presenta los siguientes inconvenientes:<!--more-->

1. El que hoy es el mejor repositorio puede no serlo mañana.
2. Si viajamos periódicamente resulta un engorro tener que estar mirando constantemente cual es el repositorio más adecuado.

Para solucionar estos inconvenientes y usar en todo momento el mejor repositorio disponible de forma automática podemos usar los repositorios redirector de Debian.

## ¿QUÉ SON LOS REPOSITORIOS REDIRECTOR DE DEBIAN?

Los repositorios redirector es un proyecto de Debian que nos permite solucionar el problema de tener que seleccionar el repositorio más rápido.

¿Por qué?

Porque los repositorios redirector de Debian harán que usemos en todo momento el mejor repositorio disponible de forma automática.

## VARIABLES USADAS PARA SELECCIONAR EL MEJOR REPOSITORIO

Los factores que se usan para seleccionar el mejor repositorio de forma automática son los siguientes:

1. Nuestra ubicación geográfica.
2. La ubicación geográfica de los repositorios.
3. La arquitectura de nuestro sistema operativo.
4. La carga y la respuesta de los servidores de los repositorios.

## VENTAJAS PROPORCIONADAS FRENTE A LOS REPOSITORIOS TRADICIONALES

Algunas de las ventajas ofrecidas por los repositorios Redirector frente a los repositorios tradicionales son las siguientes:

1. Siempre estaremos usando el repositorio más adecuado para actualizar e instalar nuestros programas.
2. Los repositorios que usaremos siempre estarán actualizados.
3. No nos encontraremos nunca con un servidor caído. En el caso que un servidor esté caído se seleccionará otro sin que nosotros nos demos cuenta.
4. Si estamos constantemente en movilidad y viajamos nos ayudarán a obtener siempre el mejor repositorio de forma automática.
5. El proceso es completamente automático e imperceptible para el usuario.
6. Etc.

## ¿ÁMBITO DE USO DE LOS REPOSITORIOS REDIRECTOR?

Los repositorios redirector se pueden usar en todas y cada una de las ramas de Debian. Por lo tanto los podemos usar en las siguientes ramas:

1. Oldstable
2. Stable
3. Testing
4. Sid
5. Experimental

Podemos usar los repositorios redirector en todos los repositorios oficiales de la distro Debian a excepción de:

1. El repositorio que proporciona las actualizaciones de seguridad.
2. El repositorio multimedia porque simplemente no forma parte de los repositorios oficiales de Debian.

Teniendo en cuenta las indicaciones de este apartado ya podemos pasar a modificar el fichero /etc/apt/sources.list para poder usar los repositorios redirector.

## COMO CONFIGURAR LOS REPOSITORIOS REDIRECTOR

Editaremos el fichero /etc/apt/sources.list ejecutando el siguiente comando en la terminal:

> ```
> sudo nano /etc/apt/sources.list
> ```

Una vez se abra el editor de textos veremos los repositorios que estamos usando que en mi caso son los siguientes:

> ```
> # Repositorios principales de Debian
> deb http://ftp.fr.debian.org/debian/ jessie main
> deb-src http://ftp.fr.debian.org/debian/ jessie main
> 
> # Repositorio actualizaciones de seguridad
> deb http://security.debian.org/ jessie/updates main 
> deb-src http://security.debian.org/ jessie/updates main
> 
> # jessie-updates, previously known as 'volatile'
> deb http://ftp.fr.debian.org/debian/ jessie-updates main 
> deb-src http://ftp.fr.debian.org/debian/ jessie-updates main
> 
> # Repositorio Backports
> deb http://ftp.debian.org/debian/ jessie-backports main
> 
> ##Repositorio Multimedia
> deb http://www.deb-multimedia.org/ jessie main non-free
> deb-src http://www.deb-multimedia.org/ jessie main non-free
> ```

Seguidamente identificamos los repositorios que tenemos que modificar siguiendo los criterios del apartado anterior. En mi caso los repositorios a modificar son los siguientes:

> ```
> # Repositorios principales de Debian
> deb http://ftp.fr.debian.org/debian/ jessie main
> deb-src http://ftp.fr.debian.org/debian/ jessie main
> 
> # jessie-updates, previously known as 'volatile'
> deb http://ftp.fr.debian.org/debian/ jessie-updates main 
> deb-src http://ftp.fr.debian.org/debian/ jessie-updates main
> 
> # Repositorio Backports
> deb http://ftp.debian.org/debian/ jessie-backports main
> ```

###### Nota: Las partes de color rojo de los repositorios son las que tendremos que sustituir para usar los repositorios redirector.

Finalmente reemplazamos el texto de color rojo por el siguiente:

> ```
> http://deb.debian.org/debian
> ```

Una vez realizadas las modificaciones nuestros repositorios quedarán de la siguiente forma:

> ```
> # Repositorios principales de Debian
> deb http://deb.debian.org/debian/ jessie main
> deb-src http://deb.debian.org/debian/ jessie main
> 
> # Repositorio actualizaciones de seguridad
> deb http://security.debian.org/ jessie/updates main 
> deb-src http://security.debian.org/ jessie/updates main
> 
> # jessie-updates, previously known as 'volatile'
> deb http://deb.debian.org/debian/ jessie-updates main 
> deb-src http://deb.debian.org/debian/ jessie-updates main
> 
> # Repositorio Backports
> deb http://deb.debian.org/debian/ jessie-backports main
> 
> ##Repositorio Multimedia
> deb http://www.deb-multimedia.org/ jessie main non-free
> deb-src http://www.deb-multimedia.org/ jessie main non-free
> ```

###### Nota: El texto verde corresponde a las partes modificadas de nuestros repositorios.

Una vez realizadas las modificaciones guardamos los cambios y cerramos el fichero.

Ahora ya podemos instalar programas y actualizar nuestro sistema operativo teniendo la seguridad que en todo momento estamos usando el mejor repositorio disponible.

## COMO SABER LOS REPOSITORIOS QUE ESTAMOS USANDO

Si alguien tiene curiosidad para saber los repositorios que está usando puede observar detalladamente los resultados que nos da la terminal mientras se actualizan los repositorios con el comando **sudo apt-get update**
