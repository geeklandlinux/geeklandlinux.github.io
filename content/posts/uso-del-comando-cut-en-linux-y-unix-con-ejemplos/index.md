---
title: "Uso del comando cut en Linux y UNIX con ejemplos"
date: 2021-12-18
categories: 
  - "linux-2"
tags: 
  - "bash"
  - "cut"
coverImage: "uso-del-comando-cut-en-Linux.png"
---

Del mismo modo que vimos como [usar sed]({{< relref "/posts/uso-del-comando-sed-en-linux-y-unix-con-ejemplos" >}}) y [awk]({{< relref "/posts/uso-del-comando-awk-en-linux-y-unix-con-ejemplos" >}}), en el siguiente artículo verán todo lo necesario para empezar a usar la utilidad cut. Al igual que sed y awk, cut será extremadamente útil para realizar scripts y automatizar tareas.<!--more-->

## ¿QUÉ HACE EXACTAMENTE LA UTILIDAD CUT?

La utilidad cut es bastante más simple de usar que sed y awk, pero a pesar de esto es tremendamente útil. Su principal utilidad es la de borrar secciones, campos o caracteres de la salida de un comando o de cada una de las líneas de un fichero de texto.

## EJEMPLOS DE USO DE LA UTILIDAD CUT

A continuación verán una serie de ejemplos que les serán útiles para comprender y sacar partido a la utilidad cut.

### Mostrar los caracteres que nos interesen en una línea de texto o en un conjunto de líneas

**Nota:** Una letra acentuada se consideran 2 caracteres.

Imaginemos que tenemos un fichero de texto con el nombre `geekland.txt` que tiene el siguiente contenido:

```shell
la utilidad cut
es fácil de usar y es útil
```

Si únicamente queremos mostrar el cuarto carácter de cada una de las líneas lo haremos con la opción `-c 4`.

```shell
joan@gk55:~$ cut -c 4 geekland.txt
u
f
```

Si queremos mostrar los caracteres 1 y 2 lo podemos hacer del siguiente modo:

```shell
joan@gk55:~$ cut -c 1,2 geekland.txt
la
es
```

Para mostrar los 2 últimos caracteres de cada línea lo podríamos realizar de la siguiente forma:

```shell
joan@gk55:~$ cat geekland.txt | rev | cut -c 1,2 | rev
ut
il
```

Si ahora queremos mostrar los caracteres del 2 al 6:

```shell
joan@gk55:~$ cut -c 2-6 geekland.txt
a uti
s fá
```

Si finalmente queremos mostrar los caracteres 1,2,3 y 5,6,7 y 8 de cada una de las líneas de un fichero de texto:

```shell
joan@gk55:~$ cut -c 1-3,5-8 geekland.txt
la tili
es áci
```

Cut también ofrece la posibilidad de seleccionar un carácter inicial y seleccionar el resto de caracteres hasta el final. Por ejemplo para seleccionar el texto a partir del carácter 5 hasta el final usaremos la opción `-c 5-` del siguiente modo:

```shell
joan@gk55:~$ cut -c 5- geekland.txt 
tilidad cut
ácil de usar y es útil
```

**Nota:** El comando que acabamos de ver es equivalente a realizar `cut -c 4-28 geekland.txt`

O también permite seleccionar un carácter final y seleccionar el resto de caracteres hasta el inicio de la frase o fichero. Por lo tanto para mostrar desde el carácter 4 hasta el inicio de la frase lo haremos del siguiente modo:

```shell
joan@gk55:~$ cut -c -4 geekland.txt
la u
es f
```

Si añadimos la opción `--complement` al comando que acabamos de ejecutar invertiremos el resultado de salida. Por lo tanto en vez de obtener desde el carácter 4 hasta el inició obtendremos desde el carácter 4 hasta el final.

```shell
joan@gk55:~$ cut --complement -c -4 geekland.txt
tilidad cut
ácil de usar y es útil
```

### Cambiar el modo en que cut delimita las nuevas líneas

Si tenemos el archivo de texto `ejemplocut.txt` con el siguiente contenido:

```shell
Explicamos el uso del comando
cut en mi blog
```

Lo normal y lo que pasa siempre es que cada salto de línea sea una línea nueva. Por lo tanto si queremos mostrar los 13 primeros caracteres de cada una de las líneas ejecutaré el siguiente comando:

```shell
joan@gk55:~$ cut -c 1-13 ejemplocut.txt
Explicamos el
cut en mi blo
```

Pero si queremos podemos modificar este comportamiento usando el comando cut mediante la opción `-z`. La opción `-z` hará que cut considere que el final de la línea es el final del fichero. Por lo tanto cuando ejecutemos el comando `cut -z -c 1-13 ejemplocut.txt` obtendremos el siguiente resultado:

```shell
joan@gk55:~$ cut -z -c 1-13 ejemplocut.txt
Explicamos el
```

**Nota:** Esta opción básicamente hace que todo el fichero de texto se considere una sola línea.

### Capturar texto a partir de un delimitador y fijando el campo que queremos mostrar

Cut también permite capturar texto tomando como referencia un delimitador que nosotros podemos fijar. Imaginemos que tenemos el siguiente texto:

```shell
estoy escribiendo una línea
```

Si únicamente queremos mostrar la tercera palabra:

1. Cada palabra esta separada por un espacio. Por lo tanto tendremos que fijar el espacio como delimitador. Para fijar el espacio como delimitador lo haré con la opción `-d ' '`.
2. A continuación hay que definir la palabra que queremos mostrar. si queremos mostrar la tercera palabra lo haremos con la opción `-f3`. La opción `-f3` hace que se muestre la palabra que hay entre el segundo y tercer delimitador.

**Nota**: Únicamente podemos usar un carácter como delimitador.

Un ejemplo de lo que acabamos de explicar es el siguiente:

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f3
una
```

Si en vez de la tercera palabra queremos mostrar la cuarta palabra:

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f4
línea
```

En el caso que en vez de mostrar un campo o palabra queremos mostrar varios campos o palabras lo podemos realizar del siguiente modo. Si queremos mostrar las 3 primeras palabras usaremos la opción `-f1-3`

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f1-3
estoy escribiendo una
```

Y si queremos mostrar todos los campos que no sean ni el 1, ni el 2 ni el 3 usaremos el comando que acabamos de ejecutar usando la opción `--complement`

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f1-3 --complement
línea
```

Ahora imaginemos que la salida del comando `cat /etc/passwd` es la siguiente:

```shell
joan@gk55:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
```

Si únicamente queremos mostrar los usuarios podemos fijar que el delimitador sea `:` y a posteriori definir que se muestre el primer campo. Para hacer lo que acabo de mencionar podemos usar el siguiente comando:

```shell
joan@gk55:~$ cut -d ':' -f1 /etc/passwd
root
daemon
bin
sys
...
```

**Nota:** Un comando alternativo al que acabamos de usar seria `cat /etc/passwd | cut -d ':' -f1`

### Cambiar el delimitador de salida con la utilidad cut

Cuando usamos cut fijando un delimitador y seleccionando los campos a imprimir vemos que el delimitador de salida es el mismo que el delimitador de entrada. Un ejemplo de lo que acabo de citar es el siguiente:

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f1-3
estoy escribiendo una
```

Pero si queremos podemos modificar el delimitador de salida por el que nosotros queramos. De este modo si queremos reemplazar el espacio por un guion lo haremos mediante la opción `--output-delimiter='-'`. A continuación tienen un ejemplo:

```shell
joan@gk55:~$ echo "estoy escribiendo una línea" | cut -d ' ' -f1-3 --output-delimiter='-'
estoy-escribiendo-una
```

### Despreciar las líneas que no tienen ningún delimitador

En los recientes ejemplos que acabamos de ver hemos visto como trabajar con delimitadores. Puede que existan casos en que sea interesante despreciar las líneas que no tienen ningún delimitador. Para ello podemos usar la opción `-s` del siguiente modo. Imaginemos que tenemos un archivo con el nombre `test.txt` con el siguiente contenido:

```shell
la utilidad cut
es fácil de usar y es útil
final
```

Si queremos fijar el espacio como delimitador y mostrar los campos 1, 2 y 3 lo haremos del siguiente modo:

```shell
joan@gk55:~$ cut -d ' ' -f1-3 test.txt 
la utilidad cut
es fácil de
final
```

El resultado obtenido es correcto, pero si además queremos que la salida excluya todas y cada una de las líneas que no tenga el delimitador espacio lo haremos con la opción `-s` del siguiente modo:

```shell
joan@gk55:~$ cut -s -d ' ' -f1-3 test.txt 
la utilidad cut
es fácil de
```

### Mostrar los Byte que nos interesen en una línea de texto o en un conjunto de líneas

**Nota:** Antes de leer el apartado es importante remarcar que un carácter ocupa un byte. Un espacio, una tabulación o un retroceso también representan un byte. Por lo tanto con la opción de separar por bytes también podemos extraer u obtener los caracteres que nos interesen.

Imaginemos que tenemos un fichero de texto con el nombre `geeklandblog.txt` que tiene el siguiente contenido:

```shell
El blog geekland explica
el uso de cut con ejemplos
```

Si únicamente queremos mostrar el segundo Byte de cada una de las líneas lo haremos con la opción `-b 2`.

```shell
joan@gk55:~$ cut -b 2 geeklandblog.txt
b
u
```

Si queremos mostrar los bytes 1 y 2 lo podemos hacer del siguiente modo:

```shell
joan@gk55:~$ cut -b 1,2 geeklandblog.txt
El
el
```

Si ahora queremos mostrar los Bytes del 1 al 4:

```shell
joan@gk55:~$ cut -b 1-4 geeklandblog.txt
El b
el u
```

si finalmente queremos mostrar los Byte 1,2,3 y el 5,6,7 y 8 de cada una de las líneas de un fichero de texto:

```shell
joan@gk55:~$ cut -b 1-3,5-8 geeklandblog.txt
El log
ek so d
```

Cut también ofrece la posibilidad de seleccionar un byte inicial y seleccionar el resto de bytes hasta el final. Por ejemplo para seleccionar el texto a partir del byte 4 hasta el final usaremos la opción `-b 4-` del siguiente modo:

```shell
joan@gk55:~$ cut -b 4- geeklandblog.txt 
blog geekland explica
uso de cut con ejemplos
```

**Nota:** El comando que acabamos de ver es equivalente a realizar `cut -b 4-26 geeklandblog.txt`

O también permite seleccionar un byte inicial y seleccionar el resto de bytes hasta el inicio de la frase o fichero. Por lo tanto para mostrar del 4 byte al inicio de la frase lo haremos del siguiente modo:

```shell
joan@gk55:~$ cut -b -4 geeklandblog.txt
El b
el u
```

#### Fuentes

[https://www.geeksforgeeks.org/command-linux-examples/](https://www.geeksforgeeks.org/cut-command-linux-examples/)
