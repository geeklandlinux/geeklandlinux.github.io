---
title: "Apt-pinning en Debian, Mezclar paquetes de distintas ramas"
date: 2013-01-12
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "debian"
cover:
  image: "images/target-packages-300x225.jpg"
  relative: true
---

Hoy hablamos de como realizar apt-pinning en Debian. Muchos usuarios usan Debian Stable o Debian Testing y se desesperan por no poder disponer de las últimas versiones de paquetes y tener que usar versiones de software que van 3 o 4 versiones por detrás de la última versión.<!--more-->

Mediante el apt-pinning en Debian podemos solucionar este problema ya que sin necesidad de cambiar de rama podremos instalar paquetes de cualquiera de las ramas existentes de Debian. Por lo tanto en el caso que usemos Debian Testing podremos instalar paquetes y software de la rama Unstable o experimental.

El primer paso antes de empezar es asegurar que nuestro sistema esta completamente actualizado. Así una vez realizados los pasos para aplicar apt-pinning en Debian podremos intuir si nuestro apt.conf y nuestro fichero de preferences están bien configurados. Para asegurar que nuestro sistema esta bien actualizado tecleamos los siguientes comandos en la terminal:

> ```
> sudo apt-get update
> ```
> 
> ```
> sudo apt-get upgrade
> ```
> 
> ```
> sudo apt-get dist-upgrade
> ```

## APT PINNING EN DEBIAN TESTING

El objetivo a conseguir es un sistema cuya rama principal sea testing y además que podamos disponer de los paquetes de la ramas Unstable y Experimental. Para ello seguimos los siguientes pasos:

### 1- Añadir los repositorios de las distintas ramas en nuestro sources.list

En este caso añadimos los repositorios de Unstable y Experimental. Tecleamos:

> ```
> sudo gedit /etc/apt/sources.list
> ```

Nuestro sources.list tiene que quedar de la siguiente forma:

> ```
> ## Paqueteria del sistema Debian Testing
>   deb http://ftp.de.debian.org/debian/ testing main contrib non-free
>   deb-src http://ftp.de.debian.org/debian/ testing main contrib non-free
> ```
> 
> ```
> ## Actualizaciones de seguridad Debian Testing
>   deb http://security.debian.org/ testing/updates main contrib non-free
>   deb-src http://security.debian.org/ testing/updates main contrib non-free
> ```
> 
> ```
> ## Repositorio para incluir los paquetes de Debian Unstable
>   deb http://ftp.us.debian.org/debian unstable main non-free contrib
> ```
> 
> ```
> ## Repositorio para incluir los paquetes de Debian experimental
>   deb http://ftp.fr.debian.org/debian experimental main non-free contrib
> ```

###### Nota: Ir con cuidado en los repositorios. No usar nombres de versiones Wheezy, Lenny o cualquier otro. Hay que vigilar ya que tanto en la configuración del preferences como la del apt.conf apuntaran a las ramas y no a las distribuciones. 

### 2- Configurar el fichero apt.conf

Dentro de la ubicación /etc/apt es altamente recomendable crear el fichero apt.conf. Este fichero nos servirá para configurar el conjunto de herramientas apt. Para crear el fichero entramos en la terminal y tecleamos:

> ```
> sudo gedit /etc/apt/apt.conf
> ```

Introducimos el siguiente código dentro del archivo:

> ```
> APT::Default-Release "testing";
> APT::Cache-Limit 55000000;
> Apt::Get::Purge;
> APT::Clean-Installed;
> APT::Get::Fix-Broken;
> APT::Get::Fix-Missing;
> APT::Get::Show-Upgraded "true";
> ```

Con este código conseguimos::

\- Definir a Debian Testing como nuestra rama principal.

\- Limitar el cache que vamos a utilizar en el proceso de actualización de paquetes.

\- Borrar los archivos de configuración de todos los paquetes desinstalados.

\- Arreglar el sistema en caso de detectar dependencias rotas.

### 3- Creación del fichero preferences

Dentro de la ubicación /etc/apt tenemos que crear el fichero preferences. El contenido del fichero  preferences es el que indicará los paquetes que son seleccionados para la instalación. En otras palabras en este fichero indicaremos que paquetes de cada una de las ramas se instalaran y bajo que circunstancias.

Abrimos la terminal y tecleamos:

> ```
> sudo gedit /etc/apt/preferences
> ```

Añadimos el siguiente contenido en el archivo:

> ```
> Package: *
> Pin: release a=testing
> Pin-Priority: 900
> ```
> 
> ```
> Package: *
> Pin: release a=unstable
> Pin-Priority: 600
> ```
> 
> ```
> Package: *
> Pin: release a=experimental
> Pin-Priority: 50
> ```

El significado de cada una de las prioridades que indicamos dentro de las gamas testing, unstable y experimental es:

**Prioridad: P >1000:** El paquete si instala aunque sea más antiguo que el paquete que tenemos instalado. (De este modo teóricamente es posible cambiar incluso de rama en Debian)

**Prioridad:  <990 P <=1000:** El paquete se instala aunque no provenga de la rama principal. Si existe una versión más reciente del paquete previamente instalado el paquete no se instalará.

**Prioridad : <500 P <=990:** El paquete se instala siempre y cuando no exista una versión en la rama principal o tengamos una versión instalada de este paquete que sea más actual.

**Prioridad : <100 P <=500:** El paquete se instala siempre y cuando no exista el mismo paquete en cualquiera de las otras ramas. Tampoco se instala en caso de tener una versión del paquete instalada que sea más reciente..

**Prioridad : <0 P <=100:** Solo se instala el paquete si no hay otra versión disponible en ninguna de las ramas existentes en nuestro sources.list.

**Prioridad negativa:** Nunca se instalará este paquete. (Teóricamente es equivalente a borrar un repositorio del sources.list)

###### **Nota: La rama principal es la Testing. Se ha definido en el punto 2.**

### 4- Comprobación del funcionamiento

Ya hemos terminado al apt-pinning en Debian. Ahora entramos en la terminal y actualizamos los repositorios mediante:

> ```
> sudo apt-get update
> ```

Seguidamente intentamos aplicar las actualizaciones:

> ```
> sudo apt-get dist-upgrade
> ```

Si todo funciona correctamente en teoría no tenemos que tener ninguna actualización. Si nos entran multitud de actualizaciones significa que algún parámetro esta fallando en nuestra configuración. En caso de encontrar multitud de actualizaciones abortar el proceso de actualización y averiguar donde está el error.

### 5- Instalar programas que no forman parte de nuestra rama principal

El apt-pinning en Debian ya está configurado y estamos en disposición de instalar paquetes de otras ramas.

**Instalación sin forzar la actualización de dependencias**

Para entender como hacerlo ponemos un ejemplo práctico. Si estamos en testing y queremos instalar gimp de la rama unstable tecleamos en la terminal:

> ```
> apt-get install gimp/unstable
> ```

Con este comando instalamos el paquete gimp de la rama unstable. También hacemos que las dependencias del paquete gimp las intente buscar en la rama predefinida como predeterminada. En nuestro caso Testing.

**Instalación forzando la actualización de dependencias**

Para entender como hacerlo ponemos un ejemplo práctico. Si estamos en testing y queremos instalar gimp de la rama unstable tecleamos en la terminal:

> ```
> apt-get -t unstable install gimp
> ```

Con este comando instalamos el paquete gimp de la rama unstable. También hacemos que las dependencias del paquete gimp las intente buscar en la rama unstable.

###### Nota: Cada vez que instalemos paquetes de distintas ramas hay que ir con cuidado y leer atentamente los mensajes.

### 6- Actualización del sistema

Cada vez que actualicemos el sistema hay que ir con mucho cuidado con los mensajes y advertencias y elegir sabiamente. Una elección errónea puede crearnos problemas de dependencias y tener que invertir tiempo para solucionarnos. Es aconsejable no abusar del recurso del apt-pinning porqué al final nuestro sistema puede ser un caos.

### 7- Visualización de que paquetes tenemos instalados para cada una de las ramas

Una vez tenemos instalados multitud de paquetes y de dependencias nos puede interesar tener un listado de los paquetes instalados de cada una de las ramas de Debian. Para ello les dejo el siguiente enlace:

[https://geeklandlinux.github.io/posts/paquetes-de-debian-clasificados-por-rama/]({{< relref "/posts/paquetes-de-debian-clasificados-por-rama" >}})

## APT-PINNING EN DEBIAN ESTABLE

En el caso de estar usando la rama estable el apt-pinning no es la solución más recomendable a la hora de disponer de paqueteria actualizada. En este caso la mejor opción es añadir los backports en nuestro el sources.list. De esta forma los problemas de dependencias serán muchos menores y tendremos un sistema mucho más limpio.

Para finalizar el post de apt-pinning en Debian dar las gracias al foro de [daboweb](http://www.daboweb.com/ "daboweb.com") por la transmisión de sus experiencias sobre este tema y a la wiki de [esdebian.org](http://www.esdebian.org/ "esdebian.org").
