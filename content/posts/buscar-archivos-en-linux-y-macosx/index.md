---
title: "Buscar archivos en Linux y en Mac OS X"
date: 2014-12-28
categories: 
  - "linux-2"
  - "mac-os-x"
tags: 
  - "buscar-archivos"
  - "find"
cover:
  image: "images/buscar-archivos-en-Linux.jpg"
  relative: true
---

Hace pocos días vimos como usar el comando find para buscar archivos que estaban llenando nuestro disco duro. Como bien dije en el pasado post el comando find es extremadamente potente y ofrece muchísimas más posibilidades de búsqueda que no otros software para buscar archivos como por ejemplo Catfish, recoll o el comando locate.<!--more-->

## VIRTUDES Y FUNCIONAMIENTO DEL BUSCADOR FIND

El comando find funciona de forma sensiblemente diferente al comando locate o a recoll. Las principales diferencias de find con el resto de buscadores son las siguientes:

1. **El comando find proporciona muchísimas** más **opciones de búsqueda** que el comando locate u otros software como por ejemplo recoll.
2. El comando **find realiza la búsqueda de archivos y carpetas en tiempo real** y no depende ni requiere de una indexación previa ni de ninguna base datos. En contraposición locate se tiene que alimentar de un índice o base de datos que se almacena en la ubicación /var/lib/mlocat/mlocate.db, y recoll requiere de un proceso previo de indexación de contenido.
3. Como find busca archivos y carpetas en tiempo real la búsqueda no será instantánea, pero será sorprendentemente rápida. Además **antes de realizar la búsqueda de los archivos que queremos buscar no será necesario actualizar ninguna base de datos ni indexar los últimos cambios**.
4. Al no tener que indexar archivos ni actualizar bases de datos, estaremos ahorrando recursos de nuestro ordenador y evitaremos la molestia de tener que actualizar bases de datos e indexar contenido antes de realizar las búsquedas.

## FUNCIONAMIENTO DEL COMANDO FIND

La sintaxis básica, o la forma de describir el funcionamiento, del comando find es la siguiente:

> ```
> find [ruta donde buscar] [indicar lo que hay buscar] [que hacer con los archivos buscados]
> ```

Por lo tanto cuando usemos find es interesante segmentar o dividir el comando en las siguientes partes:

1. Escribir el comando find.
2. Introducir la ruta o ubicaciones en las que queremos que se busquen los archivos.
3. especificar los archivos que queremos buscar.
4. De forma opcional y si es necesario definir que acciones realizar con los archivos que find encontrará.

Para familiarizarnos con el uso del comando find, en el próximo apartado veremos una serie de ejemplos prácticos para que todo el mundo pueda entender y usar find sin ningún tipo de problema. Después de ver los ejemplos prácticos todo el mundo espero que entienda como se debe usar el comando find y las diferentes opciones que le podemos dar.

## BUSCAR ARCHIVOS CON EL COMANDO FIND EN LINUX Y EN MAC OS X

Los ejemplos expuestos para que todo el mundo pueda llegar a comprender el funcionamiento de find son los siguientes:

#### _Buscar archivos y carpetas de un determinado tamaño_

En el caso que precisemos buscar ficheros y carpetas a partir del tamaño que ocupan lo podemos realizar de la siguiente forma:

###### Buscar todos los archivos y carpetas de nuestro disco duro que superan el tamaño de 500 MB

> ```
> sudo find / -size +500M
> ```

###### Buscar todos los archivos y carpetas de nuestro disco duro que tengan un tamaño exactamente igual a 1GB

> ```
> sudo find / -size 1G
> ```

###### Buscar ficheros de nuestra partición home que ocupen menos de 5000 KB

> ```
> find /home -size -5000k
> ```

###### Buscar ficheros de nuestra partición home que ocupen menos de 5000 Bytes

> ```
> find /home -size -5000c
> ```

###### Buscar la totalidad de ficheros de nuestro disco duro que tienen un tamaño comprendido entre 10 MB y 12MB

> ```
> sudo find / -size +10M -size -12M
> ```

###### Buscar los archivos de nuestro disco duro que superan el tamaño de 500 MB

> ```
> sudo find / -type f -size +500M
> ```

###### Nota: En este comando aparece -type f. Esto indica que solo se busquen archivos. Si quisiéramos buscar solo carpetas tendríamos que poner -type d. Si no ponemos el argumento -type find busca tanto archivos como carpetas.

######  Buscar todos los archivos de nuestra partición /home con tamaño inferior 100 Bytes

> ```
> find /home -type f -size -100c -exec ls -l {} \;
> ```

###### Nota: En este comando aparece una parte nueva, que se puede añadir a todos los comandos de este post, y es para poder leer mejor los resultados de salida del comando find. Añadiendo -exec ls -l {} \\; ,aparte del nombre y ubicación de los archivos encontrados, también conoceremos otra información como por ejemplo el nombre del propietario del fichero, los permisos, el tamaño en bytes de los archivos encontrados, etc.

###### Buscar todos los archivos de nuestra partición /home que son inferiores a 1 MB

> ```
> find /home -type f -size -1M -exec ls -l {} \;
> ```

###### Buscar todas las carpetas de nuestro disco duro que superan el tamaño de 100KB

> ```
> sudo find / -type d -size +100k
> ```

###### Buscar los 10 archivos o carpetas más grandes que tenemos en nuestro disco duro

> ```
> sudo find / -printf '%s %p\n'| sort -n -r | head -10
> ```

Este comando, por su complejidad, merece una explicación de lo que hace cada una de sus partes:

**\-printf '%s %p\\n' :** Imprime el resultado mostrando la ruta, el nombre y el tamaño en Bytes de cada uno de los archivos/carpetas encontrados. Cada archivo encontrado por el comando find se imprimirá en una linea diferente.

**sort -n -r :** Ordena los resultados encontrados por find de mayor a menor. Si quisiéramos ordenar los resultados obtenidos de menor a mayor, como podréis ver en el siguiente ejemplo, tendríamos que omitir -r.

**head -10:** devuelve únicamente el resultado de las 10 primeras lineas de la salida del comando find.

###### Buscar los 5 archivos o carpetas más pequeños en la ubicación /home/joan/Descargas

> ```
> sudo find /home/Descargas -printf '%s %p\n'| sort -n | head -5
> ```

###### Buscar las 10 carpetas más grandes que tenemos almacenadas en nuestro disco duro

> ```
> sudo find / -type d -printf '%s %p\n'| sort -n -r | head -10
> ```

###### Buscar los 10 archivos más grandes que tenemos almacenados en nuestro disco duro

> ```
> sudo find / -type f -printf '%s %p\n'| sort -n -r | head -10
> ```

###### Buscar los 10 archivos más grandes del tipo mp3 que tenemos almacenado en nuestro disco duro

> ```
> sudo find / -type f -iname "*.mp3" -printf '%s %p\n'| sort -n -r | head -10
> ```

#### _Buscar archivos o carpetas que contengan un texto determinado_

Si precisamos buscar un archivo o carpeta por el nombre que tiene podemos usar los siguientes comandos:

###### Buscar archivos y carpetas en nuestro disco duro que se llamen exactamente chuleta

> ```
> find / -name chuleta
> ```

###### Buscar archivos y carpetas en nuestro disco duro que contengan la palabra chuleta

> ```
> sudo find / -name '*chuleta*'
> ```

###### Buscar archivos y carpetas en nuestro disco duro que contengan la palabra chuleta dando igual que chuleta esté escrito en mayúsculas o minúsculas

> ```
> sudo find / -iname '*Chuleta*'
> ```

###### Buscar archivos y carpetas dentro de nuestra partición home que contengan la palabra chuleta y que el propietario del archivo sea joan

> ```
> find /home -name '*chuleta*' -user joan
> ```

#### _Buscar archivos o carpetas por propietario_

Si precisamos buscar archivos o carpetas que pertenecen a un cierto usuario lo podemos realizar de la siguientes forma:

###### Buscar todos los archivos y carpetas de nuestra partición home que su propietario sea joan

> ```
> find /home -user joan
> ```

###### Buscar todos los archivos y carpetas de nuestro disco que no pertenecen a ningún usuario

> ```
> find /home -nouser
> ```

#### _Buscar archivos y carpetas por grupo de usuario_

Si precisamos buscar archivos o carpetas que pertenecen a un determinado grupo de usuarios lo podemos realizar de la siguiente forma:

###### Buscar todas las carpetas de nuestro disco duro que pertenecen a un determinado grupo, que en mi caso es joan

> ```
> sudo find / -type d -group joan
> ```

###### Buscar todos los archivos de nuestro disco duro que pertenecen a un determinado grupo, que en mi caso joan

> ```
> sudo find / -type f -group joan
> ```

###### Buscar todos los archivos y carpetas de nuestro disco duro que pertenecen al grupo joan

> ```
> sudo find / -group joan
> ```

###### Buscar todos los archivos y carpetas de nuestro disco duro que no tengan usuario o en tengan grupo

> ```
> sudo find / -nouser -o -nogroup
> ```

#### _Buscar archivos que sean de un tipo determinado_

Si pretendemos buscar archivos por su tipo de formato lo podemos hacer de la siguiente manera:

###### Buscar todos los archivos del tipo odt que tenemos en nuestra partición home

> ```
> find /home -name '*.odt'
> ```

###### Buscar todos los archivos de nuestra partición home que contengan la palabra chuleta y que además sean del tipo pdf

> ```
> find /home -name '*chuleta*' -name '*.pdf'
> ```

###### Buscar todos los archivos del tipo odt que tenemos en la ubicación /home/joan/Descargas

> ```
> find /home/joan/Descargas -name '*.odt'
> ```

###### Buscar todos los archivos y carpetas ocultas de nuestra partición home

> ```
> find /home -name '.*'
> ```

#### _Buscar archivo y carpetas por su fecha de modificación_

En el caso de que queramos buscar archivos o carpetas por su fecha de modificación lo podemos realizar de la siguiente forma:

###### Buscar todos los archivos y carpetas de nuestra partición home que se han modificado hace menos de 5 minutos

> ```
> find /home -mmin -5
> ```

###### Nota: Comando útil para averiguar la ubicación de un archivo que nos hemos guardado hace poco y que no hay forma de acordarnos donde lo guardamos.

###### Buscar todos los archivos y carpetas de nuestro disco duro que se han modificado hace exactamente 5 minutos

> ```
> sudo find / -mmin 5
> ```

###### Buscar todos los ficheros de la ubicación /home/joan/Descargas que se han modificado hace más de 5 minutos

> ```
> find /home/joan/Descargas -mmin +5
> ```

###### Buscar la totalidad de archivos y carpetas de nuestro disco duro que han sido modificados durante los últimos 3 días y que tengan un tamaño comprendido entre 1 y 5 MB

> ```
> sudo find / -size +1M -size -5M -mtime -3
> ```

#### _Buscar archivos y carpetas por su fecha de creación_

En el caso de que queramos buscar archivos y carpetas por su fecha de creación lo podemos realizar de la siguiente forma:

###### Buscar la totalidad de archivos y carpetas de nuestra partición home que han sido creados durante los últimos 5 días

> ```
> find /home -ctime -5
> ```

###### Buscar la totalidad de archivos y carpetas de nuestra partición home que se crearon hace más de 5 días

> ```
> find /home -ctime +5
> ```

###### Buscar la totalidad de archivos y carpetas de nuestra partición home que tienen una antigüedad de creación de entre 5 y 10 minutos

> ```
> sudo find /home -cmin +5 -cmin -10
> ```

###### Buscar la totalidad de archivos y carpetas que se crearon hace exactamente 5 días en nuestra partición home

> ```
> sudo find /home -ctime 5
> ```

#### _Buscar archivos y carpetas por su fecha del última acceso_

Si queremos buscar archivos y carpetas por su fecha de acceso podemos usar los siguientes comandos:

###### Buscar la totalidad de archivos y carpetas de nuestra partición home a los que accedimos exactamente hace 5 días

> ```
> find /home -atime 5
> ```

###### Buscar la totalidad de carpetas de nuestra partición home a las que accedimos hace menos de 5 días

> ```
> find /home -type d -atime -5
> ```

###### Buscar la totalidad de ficheros de la partición home, en que el usuario root ha accedido durante los últimos 2 días

> ```
> find /home -atime -2 -user root
> ```

###### Buscar la totalidad de archivos y carpetas que el usuario joan ha accedido en la partición home durante los últimos 10 minutos

> ```
> find /home -amin -10 -user joan
> ```

###### Buscar la totalidad de archivos de nuestro disco duro que ocupan mas de 1 GB y no se hayan usado o accedido durante los últimos 30 días

> ```
> sudo find / -type f -size +1G -atime -30
> 
> ```

###### Buscar los 3 últimos archivos en que hemos accedido de la carpeta de la carpeta /home/joan/Descargas

> ```
> sudo find /home/joan/Descargas -type f -printf '%s %p\n'| ls -ltu | head -4
> ```

#### _Buscar archivos y carpetas que no tienen contenido_

Para buscar los archivos y carpetas que no disponen de ningún contenido les recomiendo usar el siguiente comando:

###### Buscar la totalidad de archivos y carpetas de nuestra partición home que no tienen contenido dentro

> ```
> find /home -empty
> ```

#### _Buscar archivos ejecutables_

Para buscar archivos ejecutables, como por ejemplo archivos pdf, fotografías, archivos de nuestro procesador de texto, etc. Podemos usar el siguiente comando:

###### Buscar todos los archivos ejecutables dentro de nuestra partición /home

> ```
> find /home -ls -executable
> ```

###### Buscar todos los archivos ejecutables en la totalidad de nuestro disco duro, que ocupen un tamaño entre 5 y 10 megas, y que su propietario sea joan

> ```
> sudo find / -user joan -executable -ls
> ```

#### _Buscar archivos y carpetas en función de sus permisos_

Para buscar archivos y carpetas en función de sus permisos lo podemos hacer de la siguiente manera:

###### Buscar todos los archivos y carpetas de nuestra partición que tienen permisos de lectura

> ```
> find /home -ls -readable
> ```

###### Buscar todos los archivos y carpetas de nuestra partición home que tienen permisos de escritura, y que tienen un tamaño entre 5 y 10 Megas

> ```
> find /home -size +5M -size -10M -ls -writable
> ```

###### Buscar todos los archivos y carpetas de nuestro disco duro que tengan permisos 644. (rw-r--r--)

> ```
> sudo find / -perm 644
> ```

###### Buscar todos los archivos de nuestro disco duro que tengan permisos 644 (rw-r--r--)

> ```
> sudo find / -type f -perm 644 -exec ls -lh {} \;
> ```

#### _Buscar archivos de texto en función de su contenido_

Para buscar archivos de texto que contengan un determinado texto lo podemos realizar de la siguientes forma:

###### Buscar todos los archivos de texto de nuestro disco duro que contengan la palabra domain

> ```
> sudo find / -name “*.txt” -exec grep -i “domain” {} \;
> ```

#### _Almacenar los resultados de la búsqueda de find dentro de un archivo de texto_

Todos los comandos vistos hasta el momento nos proporcionan la información mediante la terminal. Si queremos almacenar el resultado de la búsqueda en un archivo de texto tan solo tendríamos que añadir lo siguiente en todos los comandos que hemos visto hasta ahora.

> ```
> >> /home/joan/listado
> ```

Seguidamente les muestro un ejemplo de lo que acabo de explicar:

###### Buscar todos los archivos y carpetas de nuestro disco duro que contengan la palabra chuleta, y el resultado de salida del comando se almacenará en un archivo llamado listado en la ubicación /home/joan/

> ```
> sudo find / -name '*chuleta*' >> /home/joan/listado
> ```

#### _Buscar archivos y carpetas en función de criterios y aplicar acciones_

###### Nota: No aconsejo usar los comandos de este apartado a usuarios noveles. Aplicando los comandos que veréis a continuación podéis llegar a romper o borrar archivos importantes de vuestro sistema operativo.

###### Buscar todos archivos mp3 de nuestra partición home y borrarlos

> ```
> find /home -type f -name "*.mp3" -exec rm -f {} \;
> ```

###### Buscar todos los archivos con nombre archivoaborrar.txt de nuestra partición home y borrarlos

> ```
> find /home -type f -name archivoaaborrar.txt -exec rm -f {} \;
> ```

###### Buscar todos archivos de nuestra partición home con permisos 777 y cambiarles el permiso a 644

> ```
> sudo find /home -type f -perm 777 -print -exec chmod 644 {} \;
> ```

###### Buscar todas las carpetas de nuestra partición home con permisos 777 y cambiarles el permiso a 755

> ```
> find /home -type d -perm 777 -print -exec chmod 755 {} \;
> ```

###### Buscar todos archivos de nuestra partición home con extensión .log mayores de 10MB y borrarlos

> ```
> find /home/joan -type f -name *.log -size +10M -exec rm -f {} \;
> ```

###### Nota: Seguro que después de ver los ejemplos de este post os habréis dado cuenta de las increíbles posibilidades que ofrece find. A pesar de la multitud de ejemplos mostrados existen muchos más ejemplos que podríamos poner.

###### Nota: Con el tipo de post realizado queda muy patente la limitación que en algunos casos pueden generar los entornos gráficos (GUI)

###### Nota: Para quien quiera ampliar el conocimiento sobre el comando find puede hacerlo a través de las manpages de find.

## FUENTES DE INFORMACIÓN

[Vídeo explicativo del comando find en Inglés](https://www.youtube.com/watch?v=5DpDftqE6Zs "Explicación y ejemplos de uso del comando find")
