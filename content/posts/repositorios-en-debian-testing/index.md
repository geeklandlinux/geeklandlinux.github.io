---
title: "Repositorios en Debian Testing"
date: 2012-12-29
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "debian"
  - "linux"
  - "repositorios"
coverImage: "recuperacion.jpg"
---

Me gustaría compartir con todos los repositorios que uso en debian testing. La primera vez que instale Debian la verdad es que me costo. Primero averiguar los que tenia que poner y a posteriori aunque los ponía correctamente me daban errores ya que recientemente se había cambiado el repositorio multimedia.<!--more-->

Para modificar o introducir los repositorios debemos entrar y editar el archivo _/etc/apt/sources.list ._ Para ello en la terminal escribimos:

> ```
> sudo nano /etc/apt/sources.list
> ```

Ahora ya podemos editar el archivo para incluir los siguientes repositorios:

###### Nota: Hay que ir con mucho cuidado modificando el sources.list. En case de utilizar repositorios inadecuados podemos llegar a dañar nuestro sistema.

### MIS REPOSITORIOS EN DEBIAN TESTING SON:

**Repositorios Básicos y oficiales:**

> ```
> ## Debian Testing
> deb http://ftp.de.debian.org/debian/ testing main contrib non-free
> deb-src http://ftp.de.debian.org/debian/ testing main contrib non-free
> ```

###### Nota: En el repositorio Debian Testing es donde se encuentran la totalidad de paquetes de la distribución Debian. Es totalmente indispensable tener este repositorio.

> ```
> ## Actualizaciones de seguridad
> deb http://security.debian.org/ testing-security main contrib non-free
> deb-src http://security.debian.org/ testing-security main contrib non-free
> ```

###### Nota: En el repositorio  actualizaciones de seguridad es donde Debian pone a disponibilidad del usuario las actualizaciones de seguridad y parches para solucionar las vulnerabilidades que se van descubriendo. No es estrictamente necesario tener este repositorio instalado pero si muy altamente recomendable.

Como podéis ver son los repositorios que cito hay las palabras **main**, **contrib** y **non-free**:

Al poner **main** lo que hacemos es incluir aproximadamente el 90% de paquetes de la distribución Debian. La totalidad de paquetes que contiene main cumplen con los 10 principios de la [Debian Free software Guidelines](http://www.debian.org/social_contract#guidelines).

Al poner la palabra **contrib** lo que hacemos es añadir paquetes que solo cumplen con algunos de los puntos de la [Debian Free Software Guidelines](http://www.debian.org/social_contract#guidelines).

Al poner **non-free** estaremos incluyendo paquetes que no son libres (privativos) y por lo tanto contradicen los puntos de la [Debian Free Software Guidelines](http://www.debian.org/social_contract#guidelines).

###### Nota: En función de lo que este buscando el usuario puede decidir incluir los campos non-free o contrib.

**Repositorios no oficiales de terceros:**

> ```
> ##Multimedia testing ## aptitude install debian-multimedia-keyring
> deb http://www.deb-multimedia.org/ testing main non-free
> deb-src http://www.deb-multimedia.org/ testing main non-free
> ```

###### Nota: Los repositorios multimedia incluyen códecs de audio de audio no oficiales, aplicaciones de vídeo, etc. El repositorio contiene paquetes no libres multimedia.

> ```
> ##Repositorios experimentales
> deb http://deb.debian.org/debian experimental main
> ```

###### Nota: También incluyo los repositorios experimentales de Debian para poder tener las últimas versiones de Firefox y Thunderbird y otros programas en el caso que alguien lo considere necesarios.

> ```
> ## Repositorio VirtualBox Debian Wheezy
> deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian stretch contrib
> ```

###### Nota: Repositorios en Debian para instalar VirtualBox en Debian Stretch.

> ```
> ##Repositorio de Spotify
> deb http://repository.spotify.com stable non-free
> ```

###### Nota: Repositorios en Debian para poder instalar Spotify de forma nativa en Linux

> ```
> ##Editor de texto Typora
> deb https://typora.io/linux ./
> 
> ```

###### Nota: Repositorio para instalar el editor de texto Markdown Typora.

Hay una serie de programas en que los repositorios se instalan de forma automática cuando se instala el paquete binario que que utilizamos para su instalación. Algunos ejemplos de lo que acabamos de citar son Google Chrome, Dropbox, etc.
