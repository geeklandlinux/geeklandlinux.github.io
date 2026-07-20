---
title: "Crear un archivo de swap como espacio de intercambio"
date: 2017-01-02
categories: 
  - "linux-2"
tags: 
  - "archivo-de-swap"
  - "particion-de-swap"
  - "swap"
cover:
  image: "images/crear-archivo-swap.webp"
  relative: true
---

En el caso que vuestra partición de Swap sea demasiado pequeña disponemos de varias soluciones. Una de las posibles soluciones es crear un archivo de Swap.

Para crear un archivo de Swap tenemos que seguir las siguientes instrucciones.<!--more-->

## CREAR UN ARCHIVO DE SWAP

Para empezar nos logueamos como usuario root.

A continuación ejecutamos el siguiente comando para comprobar las particiones o archivos de Swap que tenemos disponibles:

> ```
> root@jessie:/home/joan# swapon -s
> Filename   Type      Size    Used Priority
> /dev/sda5  partition 731132  0    -1
> ```

En mi caso dispongo de la partición swap /dev/sda5 Esta partición tiene un tamaño aproximado de 714 MB y a continuación veremos como la podemos ampliar.

### Crear el archivo de Swap

Mi objetivo es ampliar 1 GB nuestra Swap. Por lo tanto el siguiente paso será crear un archivo de Swap de 1GB ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# dd if=/dev/zero of=/var/swap bs=4096k count=256
>  256+0 registros leídos
>  256+0 registros escritos
>  1073741824 bytes (1,1 GB) copiados, 35,0223 s, 130,7 MB/s
> ```

###### Nota: Recomiendo que el archivo de swap se cree en una partición que no sea la root. La partición /var o incluso la partición home pueden ser buenas opciones.

El significado de cada uno de los parámetros usado para crear el archivo de Swap es el siguiente:

**dd:** Parte del comando para usar la aplicación duplicate date con el fin de crear un archivo de swap.

**if=/dev/zero:** Indicamos que como fichero de origen (input file) se debe usar el dispositivo /dev/zero. El dispositivo /dev/zero genera una cadena de ceros con las características que definiremos con los comandos bs y count.

**of=/var/swap:** indicamos que se genere un archivo con nombre swap en /var. Este archivo será generado por el dispositivo /dev/zero y estará formado por una cadena de ceros con las características que definiremos con los comando bs y count.

**bs=4096k:** Definimos que la información que se escribe en el fichero /var/swap sea en bloques de 4096k.

**count=256:** Establecemos que en el archivo /var/swap se escriban 256 bloques del tamaño definido en el parámetro bs (4096k)

Por lo tanto ejecutando el comando creamos el archivo de swap /var/swap que ocupará 1 GB y estará formado por 256 bloques de 4096k. (256 x 4096 = 1GB). Por lo tanto habéis visto que crear un archivo de swap es de lo más sencillo.

### Modificar los permisos del archivo de Swap

A continuación comprobamos los permisos del archivo que acabamos de crear ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# ls -lth /var/swap
>  -rw-r--r-- 1 root root 1,0G dic 25 23:10 /var/swap
> ```

Al ejecutar el comando vemos lo siguiente:

1. El usuario propietario, que en este caso es el root, dispone de permisos de lectura y escritura.
2. Los usuarios del grupo del usuario propietario tienen permiso de lectura.
3. El resto de usuarios disponen de permisos de lectura.

Para minimizar los riesgos de seguridad modificaremos los permisos del archivo de swap ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# chmod 600 /var/swap
> ```

Seguidamente comprobaremos los nuevos permisos del archivo de swap ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# ls -lth /var/swap
>  -rw------- 1 root root 1,0G dic 25 23:10 /var/swap
> ```

En estos momentos el usuario propietario dispone de permisos de lectura y escritura. El resto de usuarios no disponen de ningún permiso.

### Establecer y activar la zona de intercambio

Vamos a configurar el archivo de swap que hemos creado para que se pueda usar como área de intercambio ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# mkswap /var/swap
>  Configurando espacio de intercambio versión 1, tamaño = 1048572 kiB
>  sin etiqueta, UUID=217e9a31-4673-49e0-a446-f339b7a96970
> ```

Seguidamente activamos el archivo de swap ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# swapon /var/swap
> ```

En estos momentos el archivo de swap está activo y lo podemos usar sin mayores problemas. Para comprobar lo que acabo de comentar podemos ejecutar el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# swapon -s
>  Filename   Type      Size    Used Priority
>  /dev/sda5  partition 731132  0    -1
>  /var/swap  file      1048572 0    -2
> ```

Como podemos observar el archivo está disponible para su uso. Además en estos momentos disponemos 1714 MB como espacio de intercambio.

### Hacer que el archivo de Swap se monte al iniciar el sistema operativo

Para que el de memoria Swap se monte automáticamente cada vez que arrancamos el sistema tenemos que acceder al fichero /etc/fstab ejecutando el siguiente comando en la terminal

> ```
> root@jessie:/home/joan# nano /etc/fstab
> ```

Una vez se abra el editor de textos nano tenemos que añadir la siguiente línea al final de archivo:

> ```
> /var/swap  none        swap    sw     0     0
> ```

Guardamos los cambios y reiniciamos nuestro ordenador. La próxima vez que se inicie el ordenador el archivo de swap estará completamente activado.

Para comprobarlo abrimos una terminal y ejecutamos el siguiente comando:

> ```
> root@jessie:/home/joan# swapon -s
>  Filename   Type      Size    Used Priority
>  /dev/sda5  partition 731132  0    -1
>  /var/swap  file      1048572 0    -2
> ```

Hasta estos momentos hemos visto como crear un archivo de swap, modificar sus permisos, activarlo y hacer que se monte automáticamente al arrancar el sistema. A continuación veremos como podemos priorizar su uso en el caso que tengamos más de uno.

## PRIORIZAR EL USO DE LAS PARTICIONES Y ARCHIVOS SWAP

En estos momentos disponemos de más de una partición o archivo de Swap y nos puede interesar priorizar el uso de uno respecto del otro.

Para ello primero abriremos una terminal y ejecutaremos el siguiente comando:

> ```
> root@jessie:/home/joan# swapon -s
>  Filename   Type      Size    Used Priority
>  /dev/sda5  partition 731132  0    -1
>  /var/swap  file      1048572 0    -2
> ```

Vemos que la partición de Swap /dev/sda5 tiene una prioridad más alta que el archivo /var/swap (-1 Versus -2). Por lo tanto la primera Swap que se usará será la /dev/sda5 y cuando esté completamente llena empezará a usarse el archivo de swap /var/swap.

Este comportamiento es correcto y es el que deseo. Los motivos de esta elección son los siguientes:

1. El archivo de Swap se ha creado en un disco duro bastante lleno. Por lo tanto existe la posibilidad que este archivo de Swap presente fragmentación.
2. Prefiero [usar particiones swap antes que archivos de Swap]({{< relref "/posts/archivo-de-swap-vs-particion-de-swap" >}}).

### Modificar la prioridad de uso de los archivos de intercambio

En el caso que quisiera modificar la prioridad de uso de los espacios de intercambio ejecutaría el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# nano /etc/fstab
> ```

Una vez dentro del fichero fstab localizamos la totalidad de líneas encargadas de montar la memoria swap. En mi caso son las siguientes:

> ```
> UUID=7dcf6a90-907a-421c-9605-dc5b4c1438e1 none  swap sw    0  0
> ```
> 
> ```
> /var/swap                                 none  swap sw    0  0
> ```

Para priorizar el uso de una partición respecto la otra, justo después del comando **sw** tenemos que añadir el siguiente término:

> ```
> ,pri=(número de prioridad)
> ```

Por lo tanto en mi caso las 2 líneas de montaje del espacio de intercambio quedarían de la siguiente forma:

> ```
> UUID=7dcf6a90-907a-421c-9605-dc5b4c1438e1 none  swap sw,pri=0    0  0
> ```
> 
> ```
> /var/swap                                 none  swap sw,pri=1    0  0
> ```

En la primera de líneas, correspondiente a la partición /dev/sda5, le asigno prioridad 0.

En la segunda línea, correspondiente al archivo de Swap, le asigno prioridad 1.

Una vez terminada la edición guardamos los cambios y cerramos el fichero.

A continuación reiniciamos el ordenador y ejecutamos el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# swapon -s
>  Filename   Type      Size    Used Priority
>  /dev/sda5  partition 731132  0    0
>  /var/swap  file      1048572 0    1
> ```

En estos momentos la prioridad de /var/swap es más elevada que la de la partición de swap /dev/sda5. Por lo tanto ahora empezará a llenarse el archivo de swap y cuando esté completamente lleno empezará a llenarse la partición de swap.

## DESACTIVAR Y ELIMINAR EL ARCHIVO DE SWAP

Si que queremos deshacer la totalidad de cambios que hemos realizado podemos seguir los siguientes pasos.

Primero desactivamos el archivo swap ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# swapoff /var/swap
> ```

A continuación borramos el archivo de swap ejecutando el siguiente comando en la terminal:

> ```
> root@jessie:/home/joan# rm -f /var/swap
> ```

Finalmente borramos la línea del archivo /etc/fstab que hace que se monte el archivo de swap. Por lo tanto ejecutamos el siguiente comando.

> ```
> root@jessie:/home/joan# nano /etc/fstab
> ```

Una vez se abra el fichero /etc/fstab localizamos la siguiente línea y la borramos.

> ```
> /var/swap        none       swap  sw     0     0
> ```

Guardamos los cambios y cerramos el fichero.

En esto momentos ya no estamos usando ningún archivo de Swap. Además la próxima vez que iniciemos el ordenador no se montará ningún archivo de swap.

###### Nota: Las sistemas de archivos BTRFS no admiten archivos de Swap.
