---
title: "Uso del comando AWK en Linux y UNIX con ejemplos"
date: 2021-10-10
categories: 
  - "linux-2"
tags: 
  - "awk"
  - "bash"
coverImage: "Uso-del-comando-Awk.png"
---

Hace unas semanas vimos con bastante profundidad y con ejemplos [el uso que podíamos dar a la utilidad Sed]({{< relref "/posts/uso-del-comando-sed-en-linux-y-unix-con-ejemplos" >}}). Ahora ha llegado el momento de hacer lo mismo pero esta vez con el comando Awk. Como ya dije en su día, si queréis empezar a programar scripts es esencial tener un conocimiento básico de herramientas como sed, awk, grep, cut, etc.<!--more-->

## ¿QUÉ NOS PERMITE REALIZAR EL COMANDO AWK?

Los usos básicos que podemos dar al comando Awk son los siguientes:

1. Buscar palabras y patrones de palabras y reemplazarlos por otras palabras y/o patrones.
2. Hacer operaciones matemáticas.
3. Procesar texto y mostrar las líneas y columnas que cumplen con determinadas condiciones.
4. Etc.

**Nota:** En términos generales el comando awk permite procesar y modificar el texto según nuestras necesidades.

Si leen el artículo verán una serie de explicaciones y ejemplos que les ayudarán a defenderse cuando tengan que utilizar el comando awk.

## MOSTRAR EN PANTALLA LAS COLUMNAS QUE QUERAMOS

A continuación veremos una serie de ejemplos que nos ayudarán a comprender como podemos manipular las columnas con el comando `awk`

### Extraer las columnas que queramos de un determinado texto

El comando Awk nos permite seleccionar una columna determinada y mostrarla en pantalla. En mi caso ejecutando el comando `ps` obtengo la siguiente salida:

```shell
joan@gk55:~$ ps
    PID TTY          TIME CMD
 636856 pts/1    00:00:00 bash
 636889 pts/1    00:00:00 ps
```

Si únicamente queremos mostrar la columna PID ejecutaremos un comando del siguiente tipo:

> ```shell
> ps | awk '{print $num_columna}'
> ```

Como el número de columna es la 1 entonces ejecutaremos el siguiente comando obteniendo el siguiente resultado.

```shell
joan@gk55:~$ ps | awk '{print $1}'
PID
636856
637797
637798
```

**Nota:** Colocamos `'{}'` y dentro de las llaves que introducimos/escribimos la acción que queremos realizar que en este caso es imprimir la columna 1.

**Nota:** El delimitador por defecto de awk es el espacio. Por lo tanto cada espacio que haya en la salida de texto será considerado como un cambio de columna. Más adelante verán que se puede cambiar el delimitador por defecto.

Si en vez de la primera columna quisiéramos imprimir la segunda ejecutaríamos el siguiente comando:

```shell
joan@gk55:~$ ps | awk '{print $2}'
TTY
pts/3
pts/3
pts/3
```

Si quisiéramos mostrar la primera columna sin mostrar la primera fila entonces ejecutaríamos el siguiente comando:

```shell
joan@gk55:~$ ps | awk 'NR>1{print $2}'
pts/3
pts/3
pts/3
```

**Nota:** Más adelante encontrarán más ejemplos y explicaciones para entender el uso del parámetro `NR`.

### Cambiar el delimitador por defecto en Awk y extraer la primera columna

En el primer apartado hemos visto que el delimitador por defecto de `awk` es el espacio, pero en caso de necesidad podemos definir el delimitador que más nos convenga. Imaginemos que ejecutando el comando `cat /etc/passwd` obtenemos la siguiente salida:

```shell
joan@gk55:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
...
```

Si únicamente queremos mostrar los usuarios del sistema operativo borrando el resto de información tendremos que:

1. Fijar `:` como delimitador.
2. Imprimir la primera columna.

Para hacer lo que acabo de citar ejecutaremos un comando del siguiente tipo:

> **`cat /etc/passwd | awk -F "delimitador" '{print $num_columna}'`**

Como el delimitador y el número de columna son `:` y `1` ejecutaré el siguiente comando obteniendo el siguiente resultado:

```shell
joan@gk55:~$ cat /etc/passwd | awk -F ":" '{print $1}'
root
daemon
bin
sys
sync
games
...
```

### Extraer varias columnas de texto de forma simultánea con el comando awk

Si queremos extraer varias columnas de forma simultánea lo haremos del siguiente modo:

```shell
joan@gk55:~$ cat /etc/passwd | awk -F ":" '{print $1 $3 $4}'
root00
daemon11
bin22
sys33
sync465534
games560
...
```

La salida es difícil de leer e interpretar porque se imprimen las columnas sin dejar espacios entre ellas. Si queremos añadir espacios entre columnas lo haremos añadiendo los caracteres `" "` entre las columnas que seleccionamos para imprimir. Por lo tanto para hacer más legible el resultado anterior ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ cat /etc/passwd | awk -F ":" '{print $1" "$3" "$4}'
root 0 0
daemon 1 1
bin 2 2
sys 3 3
sync 4 65534
games 5 60
...
```

Si en vez de un espacio quisiéramos añadir una tabulación reemplazaríamos `" "` por `"\t"` obteniendo el siguiente resultado:

```shell
joan@gk55:~$ cat /etc/passwd | awk -F ":" '{print $1"\t"$3"\t"$4}'
root	0	0
daemon	1	1
bin		2	2
sys		3	3
sync	4	65534
games	5	60
...
```

**Nota:** Otra opción para hacer que el formato sea más legible sería usar `printf` en vez de `print`. Más adelante veréis ejemplos de como se puede usar `printf`.

### Fijar un delimitador con la opción FS e imprimir varias columnas

Mediante la opción `FS` "Field Separator" también podemos fijar un delimitador. Para ello deberemos usar un comando del siguiente tipo:

> ```shell
> awk 'BEGIN{FS=":";} {print $1"\t"$3"\t"$4}'
> ```

Por lo tanto si volvemos al caso del apartado anterior y queremos imprimir las columnas 1, 3 y 4 lo haremos del siguiente modo:

```shell
joan@gk55:~$ cat /etc/passwd | awk 'BEGIN{FS=":";} {print $1"\t"$3"\t"$4}'
root	0	0
daemon	1	1
bin	2	2
sys	3	3
sync	4	65534
games	5	60
...
```

**Nota:** El comando ejecutado es equivalente a `awk 'BEGIN{FS=":";} {print $1"\t"$3"\t"$4}' /etc/passwd` o al `cat /etc/passwd | awk -F ":" '{print $1"\t"$3"\t"$4}'` **Nota:** Toda acción definida dentro de las llaves de `BEGIN` se ejecuta antes de procesar el fichero de texto `/etc(passwd)`

### Combinar el uso de FS con OFS en awk para tener un delimitador en la salida mostrada en pantalla

Si queremos combinar el uso de `FS` "field separator" con `OFS` "Output field separator" tendremos que proceder del siguiente modo.

La opción `FS` sirve para fijar el separador y la opción `OFS` define el separador de salida que usaremos para mostrar las columnas seleccionadas en pantalla.

Por ejemplo si queremos mostrar las columnas 1, 3 y 4 del comando `cat etc/passwd` y que las columnas de salida estén separadas por `-` entonces usaremos el siguiente comando:

```shell
joan@gk55:~$ cat /etc/passwd | awk 'BEGIN{FS=":";OFS=" - "} {print $1,$3,$4}'
root - 0 - 0
daemon - 1 - 1
bin - 2 - 2
sys - 3 - 3
sync - 4 - 65534
games - 5 - 60
...
```

### Imprimir en pantalla únicamente el último delimitador de una salida con awk mediante $NF

Imaginemos que la salida del comando `cat /etc/shells` es la siguiente:

```shell
joan@gk55:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
```

Si os fijáis, el nombre de la shell a veces está en la segunda columna y a veces en la tercera columna. Pero lo que es común en todas las salidas es que el nombre de la shell esté en el último delimitador. Para Imprimir el contenido que está después del último delimitador ejecutaremos un comando del siguiente tipo:

> **`awk -F "/" '/^\// {print $NF}'`**

Cada uno de los parámetros del comando que acabáis de leer hace lo siguiente:

`-F "/"`: Establecemos que el delimitador usado por awk será `/`

`/^\//`: Entre `//` introducimos `^\/` que es una expresión regular que significa toda línea que empiece por `/`

`{print $NF}`: Hace referencia a imprimir el último delimitador en todas las líneas que se cumpla la expresión regular `^\/`

Si ahora llevamos la teoría a la práctica ejecutaremos el siguiente comando obteniendo el siguiente resultado:

```shell
joan@gk55:~$ awk -F "/" '/^\// {print $NF}' /etc/shells
sh
bash
bash
rbash
rbash
dash
dash
```

El comando anterior veréis que muestra shells duplicadas. Si queremos eliminar las shell duplicadas usaremos el comando `uniq` del siguiente modo:

```shell
joan@gk55:~$ awk -F "/" '/^\// {print $NF}' /etc/shells | uniq
sh
bash
rbash
dash
```

### Imprimir el contenido del penúltimo o antepenúltimo delimitador mediante $NF

En el apartado anterior imprimimos el contenido del último delimitador. Si queremos imprimir el penúltimo tan solo tenemos que reemplazar `$NF` por `%(NF-1)`. Si lo hacemos el resultado obtenido será:

```shell
joan@gk55:~$ awk -F "/" '/^\// {print $(NF-1)}' /etc/shells
bin
bin
bin
bin
bin
bin
bin
```

Si quisiéramos imprimir el antepenúltimo delimitador deberíamos usar `$(NF-2)`

```shell
joan@asus:~$ awk -F "/" '/^\// {print $(NF-2)}' /etc/shells

usr

usr

usr
```

## OPERAR CON LÍNEAS DE TEXTO MEDIANTE EL COMANDO AWK

Hasta el momento hemos visto varios métodos para imprimir las columnas de texto que nos interesan. Del mismo modo que imprimimos las columnas de texto que nos interesan también podemos imprimir las filas o líneas de texto que nos interesen. Para ello deberéis proceder del siguiente modo.

### Imprimir únicamente las líneas que sigan un determinado patrón

En mi caso la salida del comando `df` es la siguiente:

```shell
joan@gk55:~$ df
S.ficheros          bloques de 1K   Usados Disponibles Uso% Montado en
udev                      3949508        0     3949508   0% /dev
tmpfs                      797996     1304      796692   1% /run
/dev/sda5                28767268 13356764    13926168  49% /
tmpfs                     3989968    31068     3958900   1% /dev/shm
tmpfs                        5120        4        5116   1% /run/lock
tmpfs                     3989968       16     3989952   1% /tmp
/dev/sda4                66559996 26678488    39881508  41% /media/DATOS
/dev/sda6                40054040 16034856    21954800  43% /home
/dev/sda8                  507904    50252      457652  10% /boot/efi
tmpfs                      797992       56      797936   1% /run/user/1000
```

Si únicamente queremos mostrar en pantalla las líneas que empiezan por `/` usaremos un comando del siguiente tipo:

> ```shell
> df | awk '/expresión_regular/ {print}'
> ```

En el apartado anterior hemos visto que la expresión regular para definir todas las líneas que empiezan por `/` es `^\/`. Por lo tanto el comando para que únicamente se muestren las líneas que empiecen por `/` será el siguiente:

```shell
joan@gk55:~$ df | awk '/^\// {print}'
/dev/sda5                28767268 13356764    13926168  49% /
/dev/sda4                66559996 26678488    39881508  41% /media/DATOS
/dev/sda6                40054040 16033804    21955852  43% /home
/dev/sda8                  507904    50252      457652  10% /boot/efi
```

Y si únicamente queremos mostrar la línea que empieza por `/dev/sda5` ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ df | awk '/^\/dev\/sda5/ {print}'
/dev/sda5                28767268 13356764    13926168  49% /
```

### Mostrar únicamente las líneas que empiezan o finalizan por un determinado patrón

Anteriormente ya hemos visto ejemplos para imprimir únicamente las líneas que empiezan por una letra determinada o por un patrón determinado. Por lo tanto para imprimir las líneas de la salida del comando `df` que empiezan por la palabra `tmpfs` ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ df | awk '/^tmpfs/ {print}'
tmpfs                 298144       916      297228   1% /run
tmpfs                1490708    149436     1341272  11% /dev/shm
tmpfs                   5120         4        5116   1% /run/lock
tmpfs                 298140        56      298084   1% /run/user/1000
```

Si ahora queremos mostrar únicamente las líneas que terminan por `/shm` deberemos cambiar la expresión regular `^tmpfs` por `\/shm$`. De este modo podéis ver que conseguimos nuestro objetivo:

```shell
joan@gk55:~$ df | awk '/\/shm$/ {print}'
tmpfs                1490708    155580     1335128  11% /dev/shm
```

Y si únicamente quisiéramos mostrar las líneas que empiezan por `tmpfs` y a la vez terminan por `/shm` lo haríamos del siguiente modo:

```shell
joan@gk55:~$ df | awk '/\/shm$/ && /^tmpf/ {print}'
tmpfs                1490708    134808     1355900  10% /dev/shm
```

### Mostrar las columnas que queramos de únicamente las líneas que cumplen una determinada condición

En el ejemplo del apartado anterior se mostraban la totalidad de columnas de las líneas que empiezan por `/`. Si en vez de mostrar todas las columnas tan solo quiero mostrar la 1, la 2 y la 3 procederemos del siguiente modo:

```shell
joan@gk55:~$ df -h | awk '/^\// {print $1"\t"$2"\t"$3}'
/dev/sda5	28G		13G
/dev/sda4	64G		26G
/dev/sda6	39G		16G
/dev/sda8	496M	        50M
```

Si ahora queremos sumar las columnas 2 y 3 para obtener el espacio total de cada partición lo podemos hacer del siguiente modo:

```shell
joan@gk55:~$ df -h | awk '/^\// {print $1"\t"$2 + $3}'
/dev/sda5	41
/dev/sda4	90
/dev/sda6	55
/dev/sda8	546
```

Y si queremos que se siga mostrando la unidad de capacidad de almacenamiento:

```shell
joan@gk55:~$ df -h | awk '/^\// {print $1"\t"$2 + $3"G"}'
/dev/sda5	41G
/dev/sda4	90G
/dev/sda6	55G
/dev/sda8	546G
```

### Mostrar únicamente las líneas que tengan una longitud de caracteres determinada mediante lenght

Si queremos imprimir las líneas del fichero `/etc/shells` que tienen una longitud superior a 9 caracteres ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ awk ' length($0) > 9 ' /etc/shells
# /etc/shells: valid login shells
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/dash
```

**Nota:** Del mismo modo que hemos usado `length($0) > 9` podríamos haber usado `length($0) < 9`, `length($0) == 9` o `length($0) != 9`.

### Mostrar filas que deben cumplir con varias condiciones de forma simultanea con el operador &&

Mediante el operador `&&` podemos imprimir o mostrar las líneas que cumplan con 2 o más condiciones. Por ejemplo si queremos mostrar las líneas de salida del comando `df -h` que empiezan por `t` y cuya columna `6` tenga más de `8` caracteres ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ sudo df -h | awk ' /^t/ && length($6) > 8 {print $0} '
tmpfs            5,0M   4,0K  5,0M   1% /run/lock
tmpfs            292M    52K  292M   1% /run/user/1000
```

### Ver la longitud de cada una de las líneas con el comando awk mediante '{print length}'

Para saber el número de caracteres de cada una de las líneas del fichero `/etc/shells` lo haremos mediante el siguiente comando:

```shell
joan@gk55:~$ awk '{print length}' /etc/shells
33
7
9
13
10
14
9
13
```

Si además de escribir el número de caracteres también queremos escribir el contenido de la línea:

```shell
joan@gk55:~$ awk '{print length"\t"$0}' /etc/shells
33	# /etc/shells: valid login shells
7	/bin/sh
9	/bin/bash
13	/usr/bin/bash
10	/bin/rbash
14	/usr/bin/rbash
9	/bin/dash
13	/usr/bin/dash
```

En este apartado hemos visto como imprimir el número de caracteres de un línea entera. Si únicamente queremos imprimir el número de caracteres de la columna `1` de la salida del comando `df -h` lo haremos del siguiente modo:

```shell
joan@gk55:~$ sudo df -h | awk '{print length($1)"\t"$1}'
10	S.ficheros
4	udev
5	tmpfs
9	/dev/sda1
5	tmpfs
5	tmpfs
10	compartida
5	tmpfs
```

Y si queremos eliminar la primera de la salida del comando `df` añadiríamos el parámetro `NR>1` del siguiente modo:

```shell
joan@gk55:~$ sudo df -h | awk 'NR>1{print length($1)"\t"$1}'
4	udev
5	tmpfs
9	/dev/sda1
5	tmpfs
5	tmpfs
10	compartida
5	tmpfs
```

### Imprimir la longitud de únicamente las líneas que tengan un determinado número de caracteres

Si ahora queremos imprimir la longitud de únicamente las líneas que tienen una longitud superior a 9 caracteres tan solo tenemos que combinar los comandos de los últimos apartados del siguiente modo:

```shell
joan@gk55:~$ awk ' length($0) > 9 {print length} ' /etc/shells
33
13
10
14
13
```

### Imprimir todas las líneas cuyo el último delimitador sea igual a una determinada palabra mediante el condicional if

En mi caso la salida del comando `ps -ef` es la siguiente:

```shell
joan@gk55:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 17:11 ?        00:00:01 /sbin/init
root           2       0  0 17:11 ?        00:00:00 [kthreadd]
root           3       2  0 17:11 ?        00:00:00 [rcu_gp]
root           4       2  0 17:11 ?        00:00:00 [rcu_par_gp]
...
```

Si queremos obtener un listado de procesos cuyo último delimitador contenga la palabra `firefox` podemos usar lo aprendido hasta ahora conjuntamente con el comando `if`. Para conseguir nuestro propósito lo haremos del siguiente modo:

```shell
joan@gk55:~$ ps -ef | awk '{ if($NF == "firefox") print $0}'
joan       12642       1  0 09:42 ?        00:00:00 /bin/sh -c firefox
joan       12643   12642 14 09:42 ?        00:59:13 firefox
```

**Nota:** Podemos usar varios operadores como por ejemplo `==, !=, >, <` **Nota:** En apartados anteriores hemos visto que el último delimitador se define como `$NF`. **Nota:** `$0` hace referencia a todas las columnas.

Si únicamente quisiéramos imprimir el número de proceso y el último delimitador usaríamos el siguiente comando:

```shell
joan@gk55:~$ ps -ef | awk '{ if($NF == "firefox") print $2" "$NF}'
12642 firefox
12643 firefox
```

### Contar el número de líneas de un fichero o de la salida de un comando con NR (Número de registros)

Para mostrar el número de líneas que tiene un fichero o la salida de un comando usaremos el parámetro `NR`. Por lo tanto para imprimir el número de líneas que tiene el fichero `/etc/shells` lo haremos del siguiente modo:

```shell
joan@gk55:~$ awk '{print NR}' /etc/shells
1
2
3
4
5
6
7
8
```

Sí únicamente queremos obtener el número total de líneas añadiremos el parámetro `END` del siguiente modo:

```shell
joan@gk55:~$ awk 'END {print NR}' /etc/shells
8
```

**Nota**: Toda acción precedida del comando `END` solo se ejecutará una vez cuando se haya procesado la salida del fichero `/etc/shells`

### Imprimir la primera línea de un fichero o de la salida de un comando con la opción NR

La opción `NR` también no servirá para imprimir la línea de texto que nosotros queramos. Por ejemplo si queremos imprimir la primera línea de la salida del comando `ps -aux` lo haremos del siguiente modo:

```shell
joan@gk55:~$ ps -aux | awk 'NR==1{print $0}'
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
```

Y si únicamente queremos imprimir la segunda línea lo haremos así:

```shell
joan@gk55:~$ ps -aux | awk 'NR==2{print $0}'
root           1  0.0  0.3 163980  9400 ?        Ss   08:52   0:01 /sbin/init
```

### Imprimir todas las filas de un fichero a partir de una determinada línea mediante la opción NR

Para imprimir todas las líneas de la salida del comando `ps -aux` excepto las 2 primeras lo haremos de forma similar al apartado anterior, pero en vez de usar el operador `==` usaremos el operador `>`

```shell
joan@gk55:~$ ps -aux | awk 'NR>2{print $0}'
root           2  0.0  0.0      0     0 ?        S    08:52   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   08:52   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   08:52   0:00 [rcu_par_gp]
...
```

Lo que estamos haciendo con el comando ejecutado es imprimir todos los registros del fichero excepto el 1 y el 2 que corresponden obviamente a la primera y segunda línea.

### Imprimir todas las línea de un documento o salida que empiezan con un determinado carácter o string

Si quisiéramos imprimir todos los procesos que están corriendo en nuestro sistema operativo podemos usar el siguiente comando:

```shell
joan@gk55:~$ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.3 163980  9388 ?        Ss   08:52   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    08:52   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   08:52   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   08:52   0:00 [rcu_par_gp]
...
```

En la primera columna `$1` figura el usuario que ha iniciado el proceso. Si queremos imprimir los detalles de los procesos iniciados por usuario cuyo nombre empieza por `a` o por `m`, lo haremos del siguiente modo:

```shell
joan@gk55:~$ ps -aux | awk '$1 ~ /^[a,m]/ {print $0}'
avahi        451  0.0  0.1   7272  3132 ?        Ss   08:52   0:00 avahi-daemon: running [asus.local]
message+     455  0.0  0.1   8908  4572 ?        Ss   08:52   0:01 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
avahi        471  0.0  0.0   7092   316 ?        S    08:52   0:00 avahi-daemon: chroot helper
```

Si queremos el mismo resultado que acabamos de obtener mostrando la cabecera que en este caso será la primera línea lo haremos del siguiente modo:

```shell
joan@gk55:~$ ps -aux | awk 'NR==1{print $0}' && ps -aux | awk '$1 ~ /^[a,m]/ {print $0}'
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
avahi        451  0.0  0.1   7272  3096 ?        Ss   08:52   0:00 avahi-daemon: running [asus.local]
message+     455  0.0  0.1   8908  4572 ?        Ss   08:52   0:01 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
avahi        471  0.0  0.0   7092   304 ?        S    08:52   0:00 avahi-daemon: chroot helper
```

### Imprimir eliminando los x primero caracteres de cada línea mediante el parámetro substr

En mi caso la salida del comando `/etc/shells` es la siguiente:

```shell
joan@gk55:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
```

Si queremos imprimir la salida eliminando por ejemplo los 5 primeros caracteres de cada una de las líneas lo haremos del siguiente modo:

```shell
joan@gk55:~$ cat /etc/shells | awk '{print substr($0, 5)}'
etc/shells: valid login shells
/sh
/bash
/bin/bash
/rbash
/bin/rbash
/dash
/bin/dash
```

Como hemos visto anteriormente, si queremos borrar la primera línea de la salida del comando lo haremos mediante el parámetro `NR>1` del siguiente modo:

```shell
joan@gk55:~$ cat /etc/shells | awk 'NR>1 {print substr($0, 5)}'
/sh
/bash
/bin/bash
/rbash
/bin/rbash
/dash
/bin/dash
```

**Nota:** El primer parámetro del comando substr sirve para definir la columna en la que operamos. El segundo parámetro es para definir los caracteres que queremos recortar.

### Imprimir un rango de líneas determinadas mediante el comando awk y NR

Si en la salida del comando `cat /etc/shells` únicamente queremos imprimir las líneas o registros 2, 3 y 4 lo haremos del siguiente modo:

```shell
joan@gk55:~$ cat /etc/shells | awk 'NR==2, NR==4 {print $0}'
/bin/sh
/bin/bash
/usr/bin/bash
```

Si a la salida del comando que acabamos de ejecutar le quisiéramos añadir los número de línea lo haríamos del siguiente modo:

```shell
joan@gk55:~$ cat /etc/shells | awk 'NR==2, NR==4 {print NR, $0}'
2 /bin/sh
3 /bin/bash
4 /usr/bin/bash
```

### Saber en que posición de una línea aparece una palabra o combinación de letras determinada mediante match y RSTART

Para saber la posición en que aparece una palabra o cadena de caracteres usaremos los parámetros `match` y `RSTART`. Su funcionamiento lo veréis en el siguiente ejemplo.

Para ver la posición en que aparece la palabra `cpu` en la salida del comando `ps -aux` ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ ps -aux | awk 'match($0, /cpu/) {print $0 " Contiene la palabra \"cpu\" en la posición " RSTART}'
root           9  0.0  0.0      0     0 ?        I<   08:52   0:00 [mm_percpu_wq] Contiene la palabra "cpu" a la posición 75
root          15  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/0] Contiene la palabra "cpu" en la posición 69
root          16  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/1] Contiene la palabra "cpu" en la posición 69
root          21  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/2] Contiene la palabra "cpu" en la posición 69
```

La función `match($0, /cpu/)` busca todas las líneas que contienen un determinado patrón que en el caso del ejemplo es las letras cpu. Entonces mediante `print` y `RSTART` se imprimen o muestran en pantalla mostrando la posición en que aparece el patrón que estamos buscando.

### Mostrar la longitud de un patrón mediante el parámetro RLENGHT

En el ejemplo del apartado anterior añadiremos que se muestre la longitud del patrón o texto que estamos buscando. Lo haremos con el parámetro `RLENGTH` del siguiente modo:

```shell
joan@gk55:~$ ps -aux | awk 'match($0, /cpu/) {print $0 " La longitud de la expresión buscada es " RSTART" La longitud del parámetro buscado es " RLENGTH}'
root           9  0.0  0.0      0     0 ?        I<   08:52   0:00 [mm_percpu_wq] La longitud de la expresión buscada es 75 La longitud del parámetro buscado es 3
root          15  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/0] La longitud de la expresión buscada es 69 La longitud del parámetro buscado es 3
root          16  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/1] La longitud de la expresión buscada es 69 La longitud del parámetro buscado es 3
root          21  0.0  0.0      0     0 ?        S    08:52   0:00 [cpuhp/2] La longitud de la expresión buscada es 69 La longitud del parámetro buscado es 3
joan      411346  0.0  0.1  11300  3040 pts/0    R+   14:01   0:00 awk match($0, /cpu/) {print $0 " La longitud de la expresión buscada es " RSTART" La longitud del parámetro buscado es " RLENGTH} La longitud de la expresión buscada es 83 La longitud del parámetro buscado es 3
```

## REALIZAR OPERACIONES MATEMÁTICAS CON EL COMANDO AWK

Awk permite realizar operaciones matemáticas con los números de un fichero de texto o con los números que generan las salidas de los comandos. También es capaz de por ejemplo realizar operaciones matemáticas con valores almacenados en variables. Algunos ejemplos de lo que acabo de citar son los siguientes.

### Realizar operaciones matemáticas con variables y awk

Si tenemos una variable `a` al que le asignamos el valor 10 y una variable `b` al que le asignamos el valor 20 podemos ejecutar el siguiente comando:

```shell
joan@gk55:~$ awk -v a="10" -v b="20" 'BEGIN {print "La multiplicación de a x b es", a*b;}'
La multiplicación de a x b es 200
```

**Nota:** Tenemos que usar la `BEGIN` porque no hay ninguna salida de texto o fichero a procesar. Todo lo que está dentro de `BEGIN` se ejecuta antes de procesarse un fichero o la salida de un comando. En contraposición `END` es todo lo que se ejecuta después de haber sido procesado un fichero o una salida de comando.

**Nota:** La forma de asignar una variable en `awk` es `-v variable="valor"`

Awk también permite llamar a variables que tenemos declaradas en nuestra shell de bash. Para ello lo haremos del modo que se muestra en el siguiente ejemplo:

```shell
joan@gk55:~$ a=1.5
joan@gk55:~$ b=4
joan@gk55:~$ awk -v a="$a" -v b="$b" 'BEGIN {print "La multiplicación de a x b es", a*b;}'
La multiplicación de a x b es 6
```

### Calcular la raíz cuadrada de un número mediante awk

Del mismo modo que realizamos una suma podemos realizar restas, divisiones, multiplicaciones, etc. Así que se queremos obtener la raíz cuadrada de 400 lo haremos del siguiente modo:

```shell
joan@gk55:~$ awk 'BEGIN{print sqrt(400)}'
20
```

También podemos obtener un listado que nos proporcione resultados de la raíz cuadrada de una serie de números. Para ello podemos hacer uso de un bucle for de la siguiente forma:

```shell
joan@gk55:~$ awk 'BEGIN { for(i=1; i<=10; i++) print "La raíz cuadrada de", i*i, "es", i;}'
La raíz cuadrada de 1 es 1
La raíz cuadrada de 4 es 2
La raíz cuadrada de 9 es 3
La raíz cuadrada de 16 es 4
La raíz cuadrada de 25 es 5
La raíz cuadrada de 36 es 6
La raíz cuadrada de 49 es 7
La raíz cuadrada de 64 es 8
La raíz cuadrada de 81 es 9
La raíz cuadrada de 100 es 10
```

Y en el caso que quisieran obtener la raíz cuadrada de todos los números existentes entre el 0 y el 1 lo podrían hacer del siguiente modo:

```shell
joan@gk55:~$ awk 'BEGIN { for(i=0; i<=1; i=i+0.00001) print "La raíz cuadrada de", i*i, "es", i;}'
...
...
...
La raíz cuadrada de 0.99986 es 0.99993
La raíz cuadrada de 0.99988 es 0.99994
La raíz cuadrada de 0.9999 es 0.99995
La raíz cuadrada de 0.99992 es 0.99996
La raíz cuadrada de 0.99994 es 0.99997
La raíz cuadrada de 0.99996 es 0.99998
La raíz cuadrada de 0.99998 es 0.99999
```

## EJECUTAR SCRIPTS CON EL COMANDO AWK

Como dijimos en el inicio del artículo awk es un lenguaje de programación. Podemos construir funciones, bucles y generar scripts usando el lenguaje awk. En nuestro caso definiremos un script para trabajar con los resultados que nos devuelve el comando `df`.

```shell
joan@gk55:~$ sudo df
S.ficheros     bloques de 1K    Usados Disponibles Uso% Montado en
udev                 1467404         0     1467404   0% /dev
tmpfs                 298144       904      297240   1% /run
/dev/sda1           56648848  13155828    40582968  25% /
tmpfs                1490708    351400     1139308  24% /dev/shm
tmpfs                   5120         4        5116   1% /run/lock
compartida         499451928 415539964    83911964  84% /media
tmpfs                 298140        56      298084   1% /run/user/1000
```

Lo que hará el script será lo siguiente:

1. Mostrar solo las líneas que empiecen por la letra `t` y cuya capacidad disponible (Columna 4 "$4") sea mayor a 6000k
2. De las columnas que se pueden mostrar solo imprimiremos en pantalla la (Columna 1 "$1"), que corresponde a la unidad, y la suma de la columnas 2 y 3 que corresponderá al espacio disponible.

Para generar un script simple con nombre `capacidad.awk` ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ nano capacidad.awk
```

Una vez se abra el editor de textos escribiremos las instrucciones para conseguir nuestro propósito. En nuestro caso el código es el siguiente:

```shell
#!/usr/bin/awk -f

BEGIN { printf "%s\n","Voy a extraer las partes que me interesan del comando df" }
BEGIN { printf "%s\t%s\n","Unidad","Capacidad disponible" }
/^t/  && $4 > 6000 {print $1"\t"$2 + $3"K"}
```

Ahora ejecutaremos el script del siguiente modo y veremos que obtenemos el resultado deseado:

```shell
joan@gk55:~$ sudo df | awk -f capacidad.awk 

Voy a extraer las partes que me interesan del comando df

Unidad	Capacidad disponible
tmpfs	299048K
tmpfs	1845180K
tmpfs	298196K
```

Si también quisiéramos añadir las líneas que empiezan por `c` dejaríamos el script `capacidad.awk` del siguiente modo:

```shell
#!/usr/bin/awk -f

BEGIN { printf "%s\n","Voy a extraer las partes que me interesan del comando df" }
BEGIN { printf "%-12s %-3s\n","Unidad","Capacidad disponible" }
/^t/  && $4 > 6000 {printf "%-12s %-23s\n", $1, $2+$3"K"}
/^c/  && $4 > 6000 {printf "%-12s %-23d\n", $1, $2+$3"K"}
```

**Nota:** Si os fijáis en el contenido del script veremos que usamos la opción `printf` en lugar de `print`. De esta forma podemos dar formato a los datos de salida mostrados en pantalla. Los parámetros del comando `printf` harán lo siguiente:

`%-`: Justificar el texto a la izquierda. `%`: Justificar el texto a la derecha. `12s`: String de 12 caracteres. `23d`: 23 caracteres con formato numérico decimal. `\n:` Inserta una nueva línea.

Y el resultado obtenido seria el siguiente:

```shell
joan@gk55:~$ sudo df | awk -f capacidad.awk 
Voy a extraer las partes que me interesan del comando df
Unidad       Capacidad disponible
tmpfs        299048
tmpfs        1602408
compartida   923949580
tmpfs        298196
```

## BUSCAR Y REEMPLAZAR TEXTO CON CON EL COMANDO AWK Y GSUB

Mediante la función `gsub` podemos buscar y reemplazar texto con `awk` de forma similar a como lo podemos hacer con sed. La función gsub se utiliza del siguiente modo:

`gsub("letra_palabra_o_expresión_regular_a_buscar", "texto_que_reemplaza_el_buscado", lugar_donde_realizar_sustitución)`

### Buscar una expresión regular, palabra o letra y reemplazarla por otra

Por lo tanto si partimos del fichero de texto `geekland.txt` que tiene el siguiente contenido:

```;bash
Geekland es el mejor blog, de tecnología
```

Y queremos reemplazar todas las `G` por `g` ejecutaremos el siguiente comando y obtendremos el siguiente resultado:

```shell
joan@gk55:~$ awk '{ gsub("G","g",$0); print $0 }' geekland.txt
geekland es el mejor blog, de tecnología
```

### Buscar y reemplazar texto en únicamente una columna determinada

En nuestro caso la salida del comando `df -h` es la siguiente:

```shell
joan@gk55:~$ sudo df -h
S.ficheros     Tamaño Usados  Disp Uso% Montado en
udev             1,4G      0  1,4G   0% /dev
tmpfs            292M   916K  291M   1% /run
/dev/sda1         55G    13G   39G  25% /
tmpfs            1,5G   122M  1,4G   9% /dev/shm
tmpfs            5,0M   4,0K  5,0M   1% /run/lock
compartida       477G   408G   69G  86% /media
tmpfs            292M    68K  292M   1% /run/user/1000
```

Siguiendo el mismo procedimiento que en el apartado anterior, si en la segunda columna queremos reemplazar todas las `M` por G procederemos del siguiente modo:

```shell
joan@gk55:~$ sudo df -h | awk '{ gsub("M","G",$2); print $2 }'
Tamaño
1,4G
292G
55G
1,5G
5,0G
477G
292G
```

Si en vez de imprimir la primera columna queremos imprimir la primera y la segunda de forma que la tabulación sea perfecta usaremos `printf` del siguiente modo:

```shell
joan@gk55:~$ sudo df -h | awk '{ gsub("M","G",$2); printf "%-12s %-12s\n",$1,$2 }'
S.ficheros   Tamaño      
udev         1,4G        
tmpfs        292G        
/dev/sda1    55G         
tmpfs        1,5G        
tmpfs        5,0G        
compartida   477G        
tmpfs        292G 
```

## OTROS USOS QUE PODEMOS DAR AL COMANDO AWK

Hemos visto que el comando awk tiene multitud de usos. A continuación y para finalizar el artículo mostraremos un par de usos simples no detallados anteriormente.

### Imprimir el contenido de un fichero con el comando awk

`Awk` se puede utilizar de forma similar al comando `cat` para mostrar el contenido de los ficheros de nuestro equipo. A modo de ejemplo para visualizar el contenido del fichero `functions.php` lo podemos hacer del siguiente modo.

```shell
joan@gk55:~/Nube$ awk '{print}' functions.php 
<?php
/**
 * Twenty Nineteen functions and definitions
 *
 * @link https://developer.wordpress.org/themes/basics/theme-functions/
 *
 * @package WordPress
 .....
```

### Ejecutar un comando de bash mediante el comando awk

Awk permite ejecutar comandos bash sin ningún tipo de problema. Si por ejemplo queremos usar el comando `pwd` para imprimir la ruta en la que estamos ejecutaremos el siguiente comando:

```shell
joan@gk55:~$ awk 'BEGIN { system("pwd") }'
/home/joan
```

Si creen que hay usos interesantes del comando awk que no he citado les ánimo a escribir en los comentarios del blog.

#### Fuentes

[https://www.um.es/innova/OCW/informatica-para-universitarios/ipu\_docs/la\_shell/awk.pdf](https://www.um.es/innova/OCW/informatica-para-universitarios/ipu_docs/la_shell/awk.pdf)

[https://www.baeldung.com/linux/awk-guide](https://www.baeldung.com/linux/awk-guide)
