---
title: "Ejecutar comandos y scripts con ssh en equipos remotos"
date: 2021-03-27
categories: 
  - "linux-2"
tags: 
  - "nohup"
  - "screen"
  - "ssh"
cover:
  image: "images/Ejecutar-comandos-y-scripts-con-ssh-en-equipos-remotos.png"
  relative: true
---

En ocasiones es necesario o práctico ejecutar un comando o un script en un equipo remoto sin tenerse que loguear a la shell del sistema operativo remoto. Para conseguir lo que acabo de realizar haremos uso de SSH para ejecutar comandos de forma remota. De esta forma podremos ejecutar comandos y scripts desde nuestro teléfono móvil con tan solo presionar un botón. También podremos automatizar un script para realizar una serie de tareas comunes en la totalidad de nuestros equipos de una sola tacada, etc.<!--more-->

**Nota**: La totalidad de comandos que verán a continuación se han ejecutado desde mi ordenador a una Raspberry Pi. Esta Raspberry Pi tiene un usuario llamado `pi` y su IP es la `192.168.1.254`. Es importante conocer estos 2 parámetros antes de intentar seguir las instrucciones de este tutorial.

## EJECUTAR COMANDOS SIMPLES DE FORMA REMOTA A TRAVÉS DE SSH

Pongamos el simple ejemplo que queremos consultar la temperatura de la GPU de nuestra Raspberry Pi. Para ello sabemos que podemos usar el siguiente comando:

> ```shell
> vcgencmd measure_temp
> ```

Para ejecutar el comando de forma remota y de esta forma consultar la temperatura de mi Raspberry Pi desde mi ordenador ejecutaré un comando del siguiente tipo en la terminal:

> ```shell
> ssh usuario@ip_equipo_remoto "comando_a_ejecutar"
> ```

Como hemos comentado anteriormente:

1. el usuario que uso en mi Raspberry Pi es `pi`.
2. Mi equipo remoto, que es la Raspberry Pi, tiene la ip `192.168.1.254`
3. El comando a ejecutar parar mostrar la temperatura de la GPU de la Raspberry Pu es `vcgencmd measure_temp`.

De este modo si ejecuto el comando `ssh pi@192.168.1.254 "vcgencmd measure_temp"` obtendré el siguiente resultado:

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 "vcgencmd measure_temp"
> Host key fingerprint is SHA256:U5Sz0l9/068szX7QWyNTtfo9TT0shTFphTeCkrSZyP8
> +---[ECDSA 256]---+
> |        ,.a + o*.|
> |      . .+++ +=Ox|
> |       o +++ o..+|
> |        6. . ...B|
> |        ·o    .=*|
> |         . f .oo+|
> |            .*+.o|
> |             oB*o|
> |             o*oo|
> +----[SHA256]-----+
> pi@192.168.1.254's password: 
> temp=38.9'C
> ```

Por lo tanto veo claramente que la temperatura de mi GPU es 38.9 ºC.

Si por ejemplo quisiera reiniciar un servidor web lighttpd de forma remota ejecutaría el siguiente comando:

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 "service lighttpd restart"
> ```

## EJECUTAR MÚLTIPLES COMANDOS EN UN EQUIPO REMOTO MEDIANTE SSH

Si queremos ejecutar más de un comando de forma simultanea tenemos varias opciones. La primera de ellas es usar un comando del siguiente tipo:

> ```shell
> ssh usuario@ip_equipo_remoto 'comando1; comando2; comando3'
> ```

Otro alternativa es la siguiente:

> ```shell
> ssh usuario@ip_equipo_remoto "comando1 && comando2 && comando3"
> ```

Por lo tanto si queremos ver la carga de CPU y el espacio de almacenamiento disponible en mi Raspberry ejecutaré el siguiente comando en la terminal de mi ordenador.

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 'uptime; df -h'
> Host key fingerprint is SHA256:U5Sz0l9/068szX7QWyNTtfo9TT0shTFphTeCkrSZyP8
> +---[ECDSA 256]---+
> |        ,.a + o*.|
> |      . .+++ +=Ox|
> |       o +++ o..+|
> |        6. . ...B|
> |        ·o    .=*|
> |         . f .oo+|
> |            .*+.o|
> |             oB*o|
> |             o*oo|
> +----[SHA256]-----+
> pi@192.168.1.254's password: 
>  11:58:07 up 1 day, 14:43,  1 user,  load average: 0,81, 0,71, 0,73
> S.ficheros     Tamaño Usados  Disp Uso% Montado en
> /dev/root         29G   4,0G   24G  15% /
> devtmpfs         430M      0  430M   0% /dev
> tmpfs            463M      0  463M   0% /dev/shm
> tmpfs            463M   6,4M  456M   2% /run
> tmpfs            5,0M   4,0K  5,0M   1% /run/lock
> tmpfs            463M      0  463M   0% /sys/fs/cgroup
> /dev/sda1        114G    15G   93G  14% /media/hd
> /dev/mmcblk0p1   253M    48M  205M  19% /boot
> tmpfs             93M      0   93M   0% /run/user/1000
> ```

Alternativamente también podríamos ejecutado el siguiente comando:

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 "uptime && df -h"
> ```

## EJECUTAR COMANDOS DE FORMA REMOTA A TRAVÉS DE SSH E INTERACTUAR CON LA SALIDA DEL COMANDO EJECUTADO

En el caso que queramos ejecutar un comando que implique interactuar con él necesitamos que SSH asigne una pseudo-terminal (ptty) al iniciarse la sesión. Lo que acabo de mencionar lo haremos añadiendo la opción `-t`. De este modo si quiero consultar el monitor de recursos htop de mi Rasperry Pi tendré que ejecutar el siguiente comando:

> ```shell
> joan@gk55:~$ ssh -t pi@192.168.1.254 "htop"
> ```

**Nota**: Si no asignamos al opción `-t` no podremos visualizar la información mostrada por htop. Para que htop muestre los resultados precisa de una terminal o pseudo-terminal.

Si ahora queremos editar un fichero de forma remota mediante Vim también deberemos usar la opción `-t`. Para editar el script `backup.sh` almacenado en mi Raspberry Pi tendré que ejecutar el siguiente comando:

> **`joan@gk55:~$ ssh -t pi@192.168.1.254 "vim /home/pi/backup.sh"`**

**Nota**: Para que Vim funcione también necesitamos asignarle una terminal. Si no asignamos ninguna terminal no podrá ejecutarse.

## EJECUTAR COMANDOS DE FORMA REMOTA CON UN USUARIO DETERMINADO

La totalidad de comandos vistos en este artículo han sido ejecutados por el usuario `pi` que es quien inicia la sesión SSH. Si ahora queremos iniciar la sesión SSH con el usuario `pi` y ejecutar uno o varios comandos como si fuéramos otro usuario ejecutaremos un comando del siguiente tipo:

> ```shell
> ssh usuario@ip_equipo_remoto "sudo su usuario2 -c 'comando'"
> ```

Por lo tanto si quiero editar un script que tengo almacenado en mi Raspberry Pi como si fuera el usuario `restic` ejecutaré el siguiente comando en la terminal:

> ```shell
> joan@gk55:~$ ssh -t pi@192.168.1.254 "sudo su restic -c 'nano /home/restic/restic/run_backup.sh'"
> ```

**Nota**: Fíjense que por lo comentado en el apartado anterior he añadido la opción `-t` en el comando.

## EJECUTAR UN SCRIPT DE UN EQUIPO LOCAL EN UN SERVIDOR REMOTO

Para ejecutar un script que tenemos en nuestro ordenador en un equipo remoto tenemos que ejecutar un comando del siguiente tipo:

> ```shell
> ssh usuario@ip_equipo_remoto 'bash -s' < nombre_script.sh
> ```

Por lo tanto para ejecutar un simple script con nombre `test.sh` ejecutaré el siguiente comando en mi caso:

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 'bash -s' < test.sh
> Host key fingerprint is SHA256:U5Sz0l9/068szX7QWyNTtfo9TT0shTFphTeCkrSZyP8
> +---[ECDSA 256]---+
> |        ,.a + o*.|
> |      . .+++ +=Ox|
> |       o +++ o..+|
> |        6. . ...B|
> |        ·o    .=*|
> |         . f .oo+|
> |            .*+.o|
> |             oB*o|
> |             o*oo|
> +----[SHA256]-----+
> pi@192.168.1.254's password: 
> raspberrypi
>  14:31:22 up 1 day, 17:16,  2 users,  load average: 0,31, 0,59, 0,77
> ```

Si observamos el resultado obtenido vemos que el script nos proporciona el hostname y la carga de CPU del equipo remoto.

## EJECUTAR UN SCRIPT O UN COMANDO A MULTITUD DE SERVIDORES DE FORMA SIMULTANEA

Si ahora queremos ejecutar un script local que tenemos en nuestro equipo a multitud de equipos remotos de forma simultanea procederemos del siguiente modo:

Primero crearemos un fichero de texto con el nombre `remotes.txt` que contendrá la totalidad de usuarios e IP's de los servidores en que queremos ejecutar el script. Para ello ejecutaremos el siguiente comando

> ```shell
> joan@gk55:~$ nano remotes.txt
> ```

Una vez se abra el editor de textos nano introduciremos los usuarios e IP's para establecer la conexión con los equipos remotos. En mi caso los datos son los siguientes:

> **`pi@192.168.1.253 pi@192.168.1.254 vps`**

A continuación generaré un script simple que quiero ejecutar en todos mis equipos. Para ello ejecutaré el siguiente comando:

> ```shell
> joan@gk55:~$ nano script.sh
> ```

Cuando se abra el editor de textos escribiremos un script para por ejemplo mostrar el hostname y la carga de CPU de los equipos remotos que definimos en el fichero remotes.txt:

> **`#!/bin/bash  hostname uptime`**

Acto seguido guardaremos los cambios y daremos permisos de ejecución al script mediante el siguiente comando:

> **`joan@gk55:~$ chmod +x script.sh`** 

Finalmente crearemos el script `remotexec.sh` que será el encargado de ejecutar el script `script.sh` en los equipos, `pi@192.168.1.253`, `pi@192.168.1.254` y `vps`. Para ello ejecutaremos el siguiente comando en la terminal:

> **`joan@gk55:~$ nano remotexec.sh`**

Cuando se abra el editor de textos nano escribiremos el siguiente siguiente código:

> ```shell
> #!/bin/bash
> 
> servidores=$(cat remotes.txt)
> 
> for host in $servidores; do
>    ssh $host 'bash -s' < script.sh
> done
> ```

Acto seguido guardaremos los cambios y daremos permisos los correspondientes permisos de ejecución.

> ```shell
> joan@gk55:~$ chmod +x remotexec.sh
> ```

Finalmente ejecutamos el script y veremos que los resultados obtenidos son los siguientes:

> ```shell
> joan@gk55:~$ bash remotexec.sh
> Host key fingerprint is SHA256:2KijF4FHWzXONP474Lq9BERhc8H8IO5ndriuUrXnV3Q
> +---[ECDSA 256]---+
> |      ==B.       |
> |    .ooB+o       |
> |   o +..+o       |
> |  . +..+...   . E|
> |   . o+.So.  . . |
> |    ..oo*.o.  .  |
> |    oo +o=o  .   |
> |   .o. +. ...    |
> |  .. .+++. .     |
> +----[SHA256]-----+
> pi@192.168.1.253's password: 
> raspberrypi
>  20:52:18 up 1 day, 23:37,  1 user,  load average: 1,23, 1,03, 0,88
> Host key fingerprint is SHA256:U3Sz0l9/065szL7QWytyewyhgfdsrtwshTFphTeCkrSZyM8
> +---[ECDSA 256]---+
> |        ..o + o*.|
> |      . .+++ +=oo|
> |       o ++ o..++|
> |        o. . ...B|
> |        &%    .=*|
> |         .   .ff+|
> |            .*+.o|
> |             oB*o|
> |             o*oo|
> +----[SHA256]-----+
> pi@192.168.1.254's password: 
> raspberrypi
>  20:52:21 up 1 day, 23:37,  0 users,  load average: 0,08, 0,19, 0,37
> Host key fingerprint is SHA256:Hy066f+p0m4auytrkiyuWPst0UsDUxp4SinQfYfs5lb8ZQ
> +---[ECDSA 256]---+
> |  o  o+o         |
> | o o o++   .     |
> |  o *.= = E      |
> |   B . O =       |
> |  . + B S T +    |
> |     + B = B     |
> |      + B *      |
> |     . . C       |
> |      . .        |
> +----[SHA256]-----+
> ubuntu-20-04
>  20:52:24 up 1 day, 23:28,  0 users,  load average: 0,08, 0,03, 0,01
> ```

**Nota**: Para hacer este tipo de tarea es conveniente [tener configurado correctamente el acceso sin contraseña]({{< relref "/posts/conectarse-servidor-ssh-sin-contrasena" >}}) a los distintos equipos remotos. También es útil [tener configurado adecuadamente el cliente SSH]({{< relref "/posts/configurar-cliente-ssh-para-facilitar-la-conexion-remota-a-nuestros-equipos" >}}).

## EJECUTAR SCRIPTS DE LARGA DURACIÓN A TRAVÉS DE SSH

Si ejecutamos un script de larga duración de forma remota a través de SSH es posible que se detenga antes de finalizar su ejecución. Para evitar este problema tendremos que usar screen o nohup.

Para poder usar screen y/o nohup deberán instalar los siguientes paquetes en el equipo remoto:

> ```shell
> sudo apt install screen coreutils
> ```

Para por ejemplo ejecutar un script con nombre `backup.sh`, ubicado en mi Raspberry Pi y que realiza una copia haremos lo siguiente:

Si lo hacemos mediante la utilidad screen ejecutaremos el siguiente comando en la terminal:

> ```shell
> joan@gk55:~$ ssh pi@192.168.1.254 "screen -d -m /home/pi/backup.sh"
> ```

Si por lo contrario queremos usar `nohup` deberemos ejecutar el siguiente comando:

> ```shell
> joan@gk55:~$ ssh -t pi@192.168.1.254 "nohup /home/pi/backup.sh &"
> ```

Con cualquiera de las opciones podremos iniciar la ejecución del script y cerrar la sesión SSH sin problema alguno. Aunque lo hagamos la copia de seguridad se realizará sin ningún tipo de problema.

#### Fuentes

[https://www.atareao.es/como/ssh-a-fondo/](https://www.atareao.es/como/ssh-a-fondo/)
