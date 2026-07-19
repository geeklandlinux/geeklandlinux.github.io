---
title: "Usar y ejecutar Docker en un disco duro externo y mejorar el rendimiento"
date: 2020-03-21
categories: 
  - "docker"
tags: 
  - "disco-duro"
cover:
  image: "images/Hacer-Docker-funcione-en-un-disco-duro-externo.jpg"
  relative: true
---

A continuación les mostraré un método sencillo para usar y ejecutar Docker en un disco duro externo. Los motivos por los que puede ser una buena idea evitar que los contenedores e imágenes de Docker se ejecuten y almacenen en la partición raíz de nuestro sistema operativo son los siguientes.<!--more-->

## VENTAJAS DE USAR DOCKER EN UN DISCO DURO EXTERNO

1. Podremos descargar imágenes y ejecutar contenedores **sin miedo a quedarnos sin espacio**. Si corréis Docker en la tarjeta MiniSD de una Raspberry Pi veréis que es relativamente fácil quedarse sin espacio.
2. **Evitaremos dañar el dispositivo de almacenamiento que tiene instalado el sistema operativo**. A modo de ejemplo si usamos Docker en una tarjeta miniSD de una Raspberry Pi acabaremos teniendo una muerte prematura por el elevado número de lecturas y escrituras que tendrá que realizar la tarjeta miniSD.
3. **Los contenedores se ejecutarán más rápido**, siempre y cuando el nuevo dispositivo de almacenamiento sea más rápido que el dispositivo de almacenamiento del sistema operativo.

Por todos los motivos citados es una buena idea que todos las imágenes y contenedores se descarguen y ejecuten en un disco duro externo ajeno al sistema operativo Para conseguir lo que acabo de decir deberán seguir los siguientes pasos.

**Nota:** El procedimiento que verán a continuación ha sido realizado en una Raspberry Pi con Raspbian y Docker recién instalados. El procedimiento también funciona en Ubuntu. Desconozco si funciona en otros sistemas operativos.

## USAR Y EJECUTAR DOCKER EN UN DISCO DURO EXTERNO

Los pasos a seguir se resumen a continuación.

### AUTOMONTAR EL DISPOSITIVO DE ALMACENAMIENTO EXTERNO AL ARRANCAR NUESTRO EQUIPO

En nuestro ordenador o Raspberry Pi tenemos que conseguir que el dispositivo de almacenamiento se automonte al arrancar el sistema operativo. Para ello tan solo tienen que seguir las [instrucciones]({{< relref "/posts/montar-particion-ntfs-fat-o-ext4-en-el-arranque-del-sistema" >}}).

https://geekland.eu/montar-particion-ntfs-fat-o-ext4-en-el-arranque-del-sistema/

En mi caso cada vez que arranque el sistema operativo, mi unidad de almacenamiento se automontará en la siguiente ubicación:

> ```
> /media/nextcloud/
> ```

### DEFINIR LA UBICACIÓN EN LA QUE DOCKER ALMACENARÁ LAS IMÁGENES Y CONTENEDORES

En mi caso quiero que Docker almacene todo el contenido en /media/nextcloud/docker. Para ello crearé el directorio docker ejecutando el siguiente comando en la terminal:

> ```
> mkdir /media/nextcloud/docker
> ```

### DETENER EL SERVICIO DOCKER

A continuación detendremos el servicio de Docker. Para ello ejecutaremos el siguiente comando:

> ```
> sudo service docker stop
> ```

### DEFINIR EL DIRECTORIO RAÍZ EN QUE SE EJECUTARÁ DOCKER

El siguiente paso consistirá en crear un fichero con el nombre daemon.json en el directorio /etc/docker. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> sudo nano /etc/docker/daemon.json
> ```

Como queremos almacenar todas las imágenes y carpetas dentro de media/nextcloud/docker, cuando se abra el editor de textos nano pegaremos el siguiente código:

> ```
> {
>    "data-root": "/media/nextcloud/docker"
> }
> ```

**Nota:** Deberéis reemplazar /media/nextcloud/docker por la ubicación que vosotros queráis.

Una vez pegado el texto guardan los cambios y cierran el fichero. A partir de estos momento el demonio de Docker ya sabrá donde queremos guardar todas las imágenes y contenedores.

### TRASLADAR LA INFORMACIÓN DE /VAR/LIB/DOCKER A LA NUEVA UBICACIÓN

Copiaremos absolutamente todos los ficheros y directorios de /var/lib/docker a la nueva ubicación que en mi caso es /media/nextcloud/docker. Para ello ejecutaré el siguiente comando en la terminal.

> ```
> sudo rsync -aP /var/lib/docker/ /media/nextcloud/docker
> ```

**Nota:** Para realizar la copia tenéis que tener instalado rsync. La copia realizada copiará todo el contenido de una ubicación a otra preservando todas las propiedades y permisos.

### RENOMBRAR EL DIRECTORIO ORIGINAL DE DOCKER

Una vez copiada toda la información a la nueva ubicación ya la podríamos borrar. Pero en mi caso prefiero renombrarla por si en un futuro me fuera necesaria. Por lo tanto renombraremos el directorio /var/lib/docker a /var/lib/docker.old ejecutando el siguiente comando en la terminal:

> ```
> sudo mv /var/lib/docker /var/lib/docker.old
> ```

### INICIAR DOCKER

Una vez finalizado todo el proceso ya podemos iniciar de nuevo Docker. Para ello ejecutaremos el siguiente comando:

> ```
> sudo service docker start
> ```

### COMPROBAR EL FUNCIONAMIENTO DE DOCKER EN UN DURO EXTERNO

Una vez iniciado docker descarguen imágenes y corran contenedores para asegurar que todo funciona de forma adecuada.

Si todo funciona de forma adecuada ejecuten el siguiente comando para comprobar que la partición raíz de Docker es la que nosotros hemos definido:

> ```
> docker info | grep Docker
> ```

En mi caso el resultado obtenido es el siguiente.

> ```
> Docker Root Dir: /media/nextcloud/docker
> ```

Como contiene la ruta que definimos anteriormente puedo estar seguro que todas mis imágenes y contenedores de Docker están corriendo en al nueva ubicación.

**Fuentes** [https://www.guguweb.com/2019/02/07/how-to-move-docker-data-directory-to-another-location-on-ubuntu/](https://www.guguweb.com/2019/02/07/how-to-move-docker-data-directory-to-another-location-on-ubuntu/)
