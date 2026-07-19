---
title: "Instalar Docker y Docker-Compose en Raspbian, Ubuntu, Debian, Fedora"
date: 2020-04-05
categories: 
  - "docker"
tags: 
  - "docker"
  - "docker-compose"
cover:
  image: "images/instalar-docker-y-docker-compose.jpg"
  relative: true
---

A continuación veremos el procedimiento más sencillo y efectivo para poder instalar Docker y Docker-compose en prácticamente cualquier distribución GNU-Linux. Por ejemplo podremos usar el siguiente método para las siguientes distribuciones:<!--more-->

1. Raspbian.
2. Ubuntu y distribuciones derivadas.
3. Debian y distribuciones derivadas.
4. Fedora y distribuciones derivadas.
5. CentOS y Red Hat Enterprice Linux.

Pero antes de iniciar la explicación veremos de forma muy breve que es Docker y los beneficios que nos aporta.

## ¿QUÉ ES Y QUE BENEFICIOS NOS APORTA DOCKER?

Docker es una herramienta de virtualización ligera que permite correr aplicaciones y servicios en contenedores. Algunas de las aplicaciones y servicios que nos permite usar Docker son:

1. Tiny Tiny RSS
2. Nextcloud
3. Bitwarden
4. Nextcloud
5. Wordpress
6. Jekyll
7. Jellyfin
8. Etc

**Nota:** Docker tiene miles de servicios y aplicaciones para instalar y usar de forma sencilla.

La principales ventajas de correr aplicaciones y servicios con Docker son las siguientes:

1. **Poner en marcha un servicios autoalojados** en nuestro servidor **es extremadamente simple**. A modo de ejemplo, con solo un comando podemos instalar el gestor de contraseñas bitwarden (modo servidor) en nuestro equipo.
2. **Los servicios que instalamos están preconfigurados** y son plenamente funcionales. Una vez lanzado el contenedor ya podemos usar el servicio sin problema alguno y funcionará de forma adecuada.
3. **Una imagen de Docker permitirá usar un servicio en multitud de equipos** sin tener problemas de dependencias. Todo contenedor en funcionamiento dispone de la totalidad librerías y dependencias para funcionar en cualquier equipo que tenga Docker correctamente instalado.
4. **Actualizar los servicios** a la última versión **es sumamente sencillo** e incluso podemos automatizar el proceso. Tan solo tenemos que descargar la nueva imagen y montar el contenedor con los mismos parámetros que lo montamos en su día. De esta forma en segundos podremos actualizar a la nueva versión de un servicio manteniendo la configuración anterior.
5. **Migrar un servicio de un equipo a otro es sumamente sencillo**. Tan solo tenemos que crear una imagen del servicio que tenemos corriendo en un contenedor. Está imagen la podremos instalar en multitud de equipos y funcionará.
6. De una misma imagen podemos crear varios contenedores que estén corriendo de forma paralela.
7. Por todo lo comentado hasta el momento podemos concluir que es una muy **buena opción para probar servicios** que queramos autoalojar en nuestro servidor. También es una **buena opción para correr servicios en producción**.

## PROCEDIMIENTO PARA INSTALAR DOCKER DE FORMA SENCILLA

La forma más segura y sencilla para instalar Docker es la que veremos a continuación.

### Paso 1: Descargar el script de instalación de Docker

Instalaremos Docker mediante un script de instalación que instalará la versión Community estable de Docker. Como hemos dicho al inicio del artículo este script se puede utilizar en multitud de distribuciones GNU-Linux.

Para descargar el script de instalación tan solo tenemos que ejecutar el siguiente comando:

> ```
> curl -fsSL https://get.docker.com -o get-docker.sh
> ```

### Paso 2: Instalar Docker

A continuación instalaremos Docker ejecutando el script que acabamos de descargar. Para ello tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> sudo sh get-docker.sh
> ```

A partir de este momento tan solo tenemos que esperar a que Docker se instale de forma totalmente automática.

### Paso 3: Añadir nuestro usuario al grupo Docker

Una vez finalizada la instalación, Docker solamente podrá ser usado por el usuario root. Para que el usuario con el que nos logueamos de forma habitual pueda usar Docker tendremos que añadirlo al grupo docker. En mi caso siempre uso el usuario pi, por lo tanto añadiré el usuario pi al grupo docker ejecutando el siguiente comando en la terminal:

> ```
> sudo usermod -aG docker pi
> ```

Una vez ejecutado el comando reinicien su equipo. Una vez reiniciado el equipo todo debería funcionar a la perfección.

### Paso 4: Ver información sobre la versión de Docker que acabamos de instalar

Para ver la versión de docker que estamos usando ejecutaremos el siguiente comando:

> ```
> docker -v
> ```

Para ver la versión de docker que estamos usando, así como otros parámetros como por ejemplo:

1. Detalles sobre las imágenes y contenedores presentes en nuestro OS.
2. El directorio raíz de Docker.
3. La versión de kernel y arquitectura del equipo en que está instalado Docker.
4. El sistema operativo que estamos usando.
5. Especificaciones técnicas del equipo en que hemos instalado Docker.

Podemos ejecutar el siguiente comando:

> ```
> docker info
> ```

### Paso 5: Correr nuestro primer contenedor para asegurar que docker funciona

Finalmente tan solo falta comprobar que Docker funcione de forma adecuada. Para ello ejecutaremos el primer contenedor ejecutando este simple comando:

> ```
> docker run hello-world
> ```

En mi caso el resultado obtenido ha sido el siguiente:

> Unable to find image 'hello-world:latest' locally latest: Pulling from library/hello-world 4ee5c797bcd7: Pull complete Digest: sha256:f9dfddf63636d84ef479d645ab5885156ae030f611a56f3a7ac7f2fdd86d7e4e Status: Downloaded newer image for hello-world:latest
> 
> Hello from Docker! This message shows that your installation appears to be working correctly.

Si se fijan, en la salida del comando obtenemos el resultado Hello from Docker! que es justamente la finalidad del contenedor. Justo por debajo también vemos un mensaje que nos informa que la instalación de Docker es correcta. Por lo tanto tenemos Docker instalado y funcionando de forma correcta.

## INSTALAR DOCKER COMPOSE

Una vez instalado Docker también aconsejo instalar docker-compose. Docker-compose les será de utilidad para asociar distintos contenedores entre si o para simplemente ejecutar contenedores.

En mi caso instalo docker-compose mediante el gestor de paquetes pip. Para asegurar que tenemos instalado el gestor de paquetes pip ejecutaremos los siguientes comandos en la terminal:

> ```
> sudo apt install -y libffi-dev libssl-dev
> 
> sudo apt install -y python3-pip
> ```

**Nota:** Los 2 últimos comandos son válidos para distros que usan el gestor de paquetes apt. Si usan otro gestor de paquetes deberán realizar las modificaciones pertinentes.

Finalmente ejecutaremos el siguiente comando en la terminal para instalar docker-compose.

> ```
> sudo pip3 install docker-compose
> ```

## ACTUALIZAR DOCKER Y DOCKER COMPOSE

En todo momento podremos disponer de las versiones más actualizadas de Docker y Docker-Compose. Para ello tan solo tendremos que ejecutar los siguientes comandos:

> ```
> sudo apt update && sudo apt upgrade
> 
> pip3 install --upgrade docker-compose
> ```

**Nota**: El primero de los comandos únicamente es válido para distros que usan el gestor de paquetes apt. Si usan otro gestor de paquetes deberán realizar las modificaciones pertinentes.

## DESINSTALAR DOCKER Y DOCKER COMPOSE

Para desinstalar docker Docker en aplicaciones que usen el **gestor de paquetes apt** tan solo tienen que ejecutar el siguiente comando:

> ```
> sudo apt remove --purge docker-ce
> ```

En distribuciones que usen el **gestor de paquetes DNF** deberán ejecutar el siguiente comando:

> ```
> sudo dnf remove docker-ce
> ```

En el caso que uséis una distro que use el **gestor de paquetes yum** deberéis ejecutar el siguiente comando:

> ```
> sudo yum remove docker-ce
> ```

Para borrar la totalidad de imágenes y contenedores que hayan instalado y usado tendrán que ejecutar el siguiente comando:

> ```
> sudo rm -rf /var/lib/docker
> ```

Finalmente tendrán que borrar uno por uno la totalidad de volúmenes que hayan creado al montar sus contenedores.

Para desinstalar Docker-compose tan solo tendremos que ejecutar el siguiente comando en la terminal:

> ```
> sudo pip3 uninstall docker-compose
> ```

**Fuente**

[https://docs.docker.com/install/](https://docs.docker.com/install/)
