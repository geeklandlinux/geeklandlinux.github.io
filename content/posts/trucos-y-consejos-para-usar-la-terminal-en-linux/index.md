---
title: "Trucos y consejos para usar la terminal en Linux"
date: 2022-09-12
categories: 
  - "linux-2"
tags: 
  - "terminal"
  - "trucos"
coverImage: "trucos-para-usar-la-terminal-de-forma-eficiente-en-linux.jpeg"
---

A continuación veremos una serie de trucos y consejos que todo usuario de la terminal debería saber. Después de leer este artículo los usuarios deberían ser capaces de desenvolverse mucho mejor en la terminal. Algunos de los trucos para la terminal que verán a continuación no se acostumbran a comentar en exceso y nos proporcionarán velocidad y facilidad a la hora de realizar nuestras tareas.<!--more-->

## TRUCOS PARA MOVERSE ENTRE DIRECTORIOS EN LA TERMINAL DE LINUX

Algunos de los trucos para la terminal para moverse de un directorio a otro de forma rápida y sencilla son los que detallan a continuación:

### Volver al directorio anterior

Imaginemos que estamos en la ruta `/var/log`

```shell
❯:/var/log$ pwd
/var/log
```

Seguidamente ejecutamos el siguiente comando para dirigirnos al directorio `/etc`:

```shell
❯:/var/log$ cd /etc
```

Si ahora queremos volver de forma rápida y sencilla al directorio anterior tan solo tendremos que ejecutar el siguiente comando:

```shell
❯:/etc$ cd -
```

**Nota:** El carácter `-` es equivalente a escribir la ruta anterior donde estábamos anteriormente.

### Trucos para dirigirse de forma inmediata al directorio /home/usuario en la terminal

Para dirigirse de forma inmediata al directorio `/home/usuario` lo podemos realizar mediante el siguiente comando:

```shell
❯:/var/log$ cd
```

Otro comando alternativo que podemos usar es:

```shell
cd ~
```

**Nota:** El comando `cd` es equivalente a `cd ~`. El símbolo `~` hace referencia a la ubicación `/home/usuario`.

### Moverse entre directorios en la terminal usando la opción pushd y popd

En el caso que tengáis que estar constantemente cambiando de directorio en la terminal y estos directorios siempre acostumbren a ser los mismos les aconsejo que sigan las recomendaciones que les dejo en el siguiente enlace:

https://geekland.eu/cambiar-de-directorio-de-forma-rapida-y-sencilla-en-linux/

## TRUCOS PARA EJECUTAR COMANDOS DE LA FORMA MÁS RÁPIDA Y SENCILLA EN LA TERMINAL

Si lo que pretenden es ejecutar comandos en la terminal de forma rápida, sencilla, evitando errores y sin tener que escribir en exceso les recomiendo que apliquen los siguientes trucos.

### Ejecutar de nuevo el último comando en la terminal

La terminal de Linux permite repetir el último comando ejecutado mediante 2 símbolos de exclamación `!!`. Imaginemos que queremos editar el fichero `.config/mpv/mpv.conf`. Por lo tanto ejecutaremos el siguiente comando:

```shell
❯:~$ nano .config/mpv/mpv.conf
```

A continuación editamos el contenido del fichero y lo cerramos. Si ahora queremos volver a editar el mismo fichero no hará falta que ejecutemos de nuevo el comando `nano .config/mpv/mpv.conf`. Tan solo tendremos que ejecutar el comando `!!` del siguiente modo:

```shell
❯:~$ !!
```

Una vez ejecutado el comando verán que se abre el editor de texto nano. De esta forma podemos evitar la introducción de comandos relativamente largos.

Otra utilidad típica de `!!` es ejecutar comandos con el usuario `root` que anteriormente se ejecutaron sin permisos de administrador. Imaginad el caso que un usuario ejecuta el siguiente comando:

```shell
❯:~$ apt update
Leyendo lista de paquetes... Hecho
E: No se pudo abrir el fichero de bloqueo «/var/lib/apt/lists/lock» - open (13: Permiso denegado)
....
```

Obviamente el comando no se ha ejecutado porque requiere de permisos de administrador. Para ejecutar el mismo comando con permisos de administrador sin tener que escribir de nuevo todo el comando pueden usar `!!` del siguiente modo:

```shell
❯:~$ sudo !!
sudo apt update
Des:1 https://download.docker.com/linux/ubuntu jammy InRelease [48,9 kB]
....
```

### Ejecutar el penúltimo y el antepenúltimo comando en la terminal de Linux

Del mismo modo que podemos ejecutar el último comando también podemos ejecutar el penúltimo y antepenúltimo comando.

Imaginemos que ejecutamos el siguiente comando:

```shell
❯:~$ ls
```

Acto seguido ejecutamos otro comando:

```shell
❯:~$ sudo apt update
```

Si ahora queremos volver a ejecutar el penúltimo comando no es necesario ejecutar el comando `ls`. Tan solo tenemos que ejecutar el siguiente comando:

```shell
❯:~$ !-2
```

Si en vez del penúltimo quisiéramos ejecutar el antepenúltimo:

```shell
!-3
```

y así sucesivamente. Para ver el comando que corresponde a cada uno de los números pueden ejecutar el comando `history` del siguiente modo:

```shell
❯:~$ history
   28  grep --color -E '[0-9]{1,3}' /etc/hosts
   29  grep --color -E '[0-9]{2}' /etc/hosts
   30  grep --color -E '[0-9]{3}' /etc/hosts
   31  grep --color -E '[0-9]{4}' /etc/hosts
   32  grep --color -E '[0-9]{1-3}' /etc/hosts
   33  grep --color -E '[0-9]{1,2}' /etc/hosts
...
  523  ls
  524  !
  525  !
  526  ls
  527  history
```

Una vez se el número que corresponde a cada comando puedo ejecutar los comandos del siguiente modo. Para ejecutar el comando `ls` ejecutaré el siguiente comando:

```shell
❯:~$ !526
```

### Ejecutar el comando anterior sin la primera palabra

Imaginemos el caso que de forma errónea he ejecutado el siguiente script con permisos de administrador.

```shell
❯:~$ sudo bash firefox_update.sh
```

Si quiero ejecutar exactamente el mismo comando omitiendo la primera palabra que es sudo tan solo tendré que ejecutar el comando `!*` del siguiente modo:

```shell
❯:~$ !*
bash firefox_update.sh
```

En el caso que quisiera omitir las 2 primeras palabras entonces ejecutaríamos el siguiente comando:

```shell
❯:~$ !!*
firefox_update.sh
```

### Trucos para encadenar la ejecución de uno o varios comandos en la terminal

Para actualizar los paquetes del sistema operativo con el gestor de paquetes apt tenemos que ejecutar los siguientes comandos.

Para refrescar los repositorios:

```shell
sudo apt update
```

Finalmente para actualizar los paquetes del sistema operativo:

```shell
sudo apt upgrade
```

Para combinar las 2 acciones que acabamos de ver en un solo comando usaremos el operador `;` del siguiente modo:

```shell
comando_1 ; comando_2
```

Por lo tanto en el ejemplo que estábamos viendo ejecutaríamos el siguiente comando:

```shell
sudo apt update ; sudo apt upgrade
```

Por lo tanto mediante la ejecución de un solo comando seremos capaces de actualizar nuestro sistema operativo.

En el caso que queramos encadenar la ejecución de comandos asegurando que en el caso que uno de los comandos falle se paralice la ejecución del resto de reemplazaremos el operador `;` por `&&` del siguiente modo:

```shell
sudo apt update && sudo apt upgrade
```

Para comprobar que se paralice la ejecución de los comandos podéis ejecutar el siguiente comando:

```shell
❯ ls /etc/geekland && echo "geekland"
"/etc/no_existe": No such file or directory (os error 2)
```

Como el directorio `/etc/geekland` no existe se produce un error y al producirse el error ya no se ejecutará el segundo comando. Por lo tanto en pantalla no veremos escrita la palabra `geekland`

Si ahora cambiamos `&&` por `;` veremos que se ejecutará el primer comando y nos dará error, pero aunque de error se ejecutará el segundo comando:

```shell
❯ ls /etc/geekland ; echo "geekland"
"/etc/no_existe": No such file or directory (os error 2)
geekland
```

### Ejecutar comandos usando el historial de comandos de la terminal de Linux

Linux almacena la totalidad de comandos ejecutados en el fichero de texto `~/.bash_history`. La totalidad de comandos almacenados en este fichero formarán el [historial de comandos ejecutados]({{< relref "/posts/como-usar-y-configurar-el-historial-de-comandos-history-en-linux" >}}) de la terminal de Linux.

Si siguen las instrucciones del siguiente enlace podrán recuperar de forma rápida y sencilla cualquier comando que ya hayan previamente ejecutado en el pasado sin tener que teclear y sin que tener que recordar la totalidad del comando:

https://geekland.eu/como-usar-y-configurar-el-historial-de-comandos-history-en-linux/

De esta forma podremos ejecutar comandos largos de forma extremadamente rápida y sin tener que recordar la totalidad del comando.

### Autocompletar un comando o una ruta mediante la tecla `TAB`

La tecla `TAB` sirve para autocompletar comandos y rutas y es muy útil para incrementar la velocidad en el uso de la terminal. Además también nos ayudará a cometer muchos menos errores en la introducción de rutas y comandos.

Imaginemos que queremos acceder a la ruta `/home/joan`. La forma más rápida de acceder a esta ruta será escribiendo lo siguiente:

```shell
❯:~$ cd /h▣
```

Una vez estemos en este punto presionaremos la tecla `TAB` y la ruta `/h` se autocompletará a `/home/`:

```shell
❯:~$ cd /home/▣
```

Acto seguido escribimos la letra `j` de joan y presionamos de nuevo la tecla `TAB`.

```shell
❯:~$ cd /home/j▣
```

Como dentro del directorio joan no hay ningún otro directorio que empiece por `j` se autocompletará la ruta del siguiente modo:

```shell
❯:~$ cd /home/joan/▣
```

En el caso que dentro del directorio `/home` existieran los directorios `joan` y `jorge` tendríamos que actuar del siguiente modo. Inicialmente teclearíamos los siguiente:

```shell
❯:~$ cd /home/j▣
```

Al presionar la tecla `TAB` no pasaría absolutamente nada porque dentro de `/home` existen 2 directorios que empiezan por `j`. En esto caso tendríamos que presionar 2 veces consecutivas la tecla `TAB` para obtener el siguiente resultado:

```shell
❯:~$ cd /home/j▣
joan/    jorge/
❯:~$
```

Por lo tanto vemos que existen 2 directorios con el nombre `joan` y `jorge`. Además ambos empiezan por `jo`. Por lo tanto si queremos acceder dentro del directorio `jorge` tendremos que teclear `or`.

```shell
❯:~$ cd /home/jor▣
```

y seguidamente presionar `TAB`. Entonces se autocompletará la ruta completa:

```shell
❯:~$ cd /home/jorge▣
```

**Nota:** Del mismo modo que hemos autocompletado rutas también podemos autocompletar comandos. Para mi este es uno de los mejores trucos para la terminal que pueden usar para incrementar enormemente su velocidad.

### Obtener comandos que hayamos ejecutado anteriormente

Presionando los cursores `↑` y `↓`, la terminal mostrará los últimos comandos presentes en el historial de comandos. Un ejemplo de su funcionamiento es el siguiente:

Si presionamos la tecla `↑` accederemos al último comando presente en el historial de comandos.

```shell
❯:~$ exit
```

Si volvemos a presionar la tecla `↑` accederemos al penúltimo comando presente en el historial de comandos.

```shell
❯:~$ clear
```

Si tenemos el penúltimo comando y queremos volver a mostrar el último comando almacenado en el historial de la terminal presionaremos la tecla `↓`.

```shell
❯:~$ exit
```

De esta forma tan sencilla podemos acceder a los comandos que ejecutados recientemente sin prácticamente tener que usar el teclado.

### Sustituir un texto por otro texto en la terminal

Imaginemos que ejecutamos el siguiente comando para refrescar los repositorios de nuestra distribución:

```shell
❯:~$ sudo apt update
```

Si ahora queremos actualizar el sistema operativo tendremos que ejecutar el comando `sudo apt dist-upgrade`. Si os fijáis la única diferencia entre el comando para refrescar los repositorios y el comando para actualizar el sistema operativo es cambiar `update` por `dist-upgrade`. Por lo tanto para no tener que escribir la totalidad del comando podemos ejecutar el siguiente comando:

```shell
❯:~$ ^update^dist-upgrade
sudo apt dist-upgrade
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Calculando la actualización... Hecho
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```

### Usar alias en Linux para ejecutar comandos de forma más rápida

Los alias también pueden ayudar a ejecutar comandos de una forma mucho más rápida y sencilla. Para saber que es un alias y saber como crearlos les recomiendo que visiten el siguiente enlace.

https://geekland.eu/alias-en-linux-que-son-como-crearlos-y-ejemplos/

## TRUCOS Y CONSEJOS PARA OCULTAR Y/O BORRAR EL CONTENIDO DE LA TERMINAL

Pueden usar los siguientes trucos o consejos para borrar u ocultar el contenido de la terminal.

### Ocultar el contenido que muestra el emulador de terminal

Para ocultar el texto que tenemos en la terminal tan solo tenemos que ejecutar el comando `clear` del siguiente modo:

```shell
❯:~$ clear
```

El comando `clear` no borra el contenido de la terminal. Lo que hace es desplazar el contenido de la terminal hacia arriba. Pero si posicionamos el puntero del ratón en el emulador de terminal y movemos la rueda del ratón veremos que podemos recuperar el contenido que se ha ocultado.

Otra forma de realizar exactamente lo mismo sin tener que ejecutar ningún comando es usar la combinación de teclas:

```shell
ctrl+l
```

### Borrar completamente el contenido de la terminal

En el apartado anteriormente simplemente estábamos ocultando el contenido mostrado por la terminal. Si en vez de ocultarlo quieren eliminarlo tendrán que ejecutar el comando `reset` del siguiente modo:

```shell
❯:~$ reset
```

Ahora da igual que hagamos scroll en la terminal. Aunque hagamos scroll no podremos recuperar el contenido borrado. Esto es así porque el comando `reset` borra todo el contenido de la sesión actual del emulador de terminal. El resultado de aplicar el comando `reset` es equivalente a cerrar la terminal y volverla a abrir.

### Borrar la totalidad de una línea que tenemos escrita en la terminal

Imaginemos que tenemos el siguiente comando a medio escribir:

```shell
❯:~$ sudo apt get ins▣
```

Si queremos borrar todo el texto desde la posición que tenemos posicionado el cursor hasta al inicio de la línea tendremos que presionar la combinación de teclas:

```shell
Ctrl+u
```

Por lo tanto si tenemos escrito el siguiente comando y el cursor esta posicionado justo antes de la `i` de install:

```shell
❯:~$ sudo apt get ▣install
```

Cuando presionemos el atajo de teclado `Ctrl+u` no se borrará toda la línea. Solo se borrará `sudo apt get` porque es lo que está a la izquierda del cursor. El resultado obtenido de presionar `Ctrl+u` será el siguiente:

```shell
❯:~$ install
```

Por lo tanto para borrar la totalidad de línea de un plumazo primero colocaremos el cursor al final de la línea mediante el atajo de teclado:

```shell
Ctrl+e
```

A continuación borraremos la totalidad de texto que está a la izquierda del cursor mediante el atajo de teclado:

```shell
Ctl+u
```

### Borrar el texto que tenemos a la derecha del cursor en la terminal

En el apartado anterior vimos que el atajo de teclado `Ctrl+u` sirve para borrar todo lo que tenemos a la izquierda del cursor. Para realizar lo opuesto, y por lo tanto borrar todo lo que tenemos a la derecha del cursor, tendremos que usar el atajo de teclado:

```shell
Ctrl+k
```

Por lo tanto si tenemos escrito el siguiente comando con el cursor posicionado justo después de la palabra get:

```shell
❯:~$ sudo apt get▣ install
```

Y presionamos el atajo de teclado `Ctrl+k` obtendremos el siguiente resultado:

```shell
❯:~$ sudo apt get
```

## ATAJOS DE TECLADO PARA REALIZAR MULTITUD DE TAREAS DE FORMA MÁS RÁPIDA

Conocer los atajos de teclado de la terminal les proporcionará velocidad y les permitirá tener acceso a una serie de funciones adicionales. Los atajos de teclado típicos que acostumbran a tener la gran mayoría de emuladores de terminal son los siguientes:

https://geekland.eu/atajos-de-teclado-para-emuladores-de-terminal-en-linux/

**Nota**: Los emuladores de terminal acostumbran a usar los atajos predeterminados de Emacs. No obstante si lo prefieren pueden modificar la configuración para usar los atajos de teclado de Vim.

## TRUCOS PARA SABER COMO MOVER EL CURSOR EN LA TERMINAL

Si quieren mover el cursor de la terminal de una posición a otra de forma rápida pueden usar los siguientes atajos de teclado.

### Ir al principio de la línea en la terminal de Linux

Si estamos en medio o al final de una línea y queremos posicionar el cursor justo al principio de la línea podéis usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `ctrl+a` | Mover el cursor al inicio de la línea. |
| `inicio` | Mover el cursor al inicio de la línea. |

### Posicionar el cursor al final de la línea en la terminal de Linux

Si por el contrario estamos al inicio de la línea, o en una posición intermedia, y queremos posicionar el cursor justo al final de la línea podéis usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `ctrl+e` | Mover el cursor al final de la línea. |
| `fin` | Mover el cursor al final de la línea. |

### Moverse saltando palabra por palabra en la terminal

Si lo que pretenden es que el cursor avance o retroceda palabra por palabra deberán usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `alt+f` | Para ir avanzando palabra por palabra. |
| `alt+b` | Para retroceder palabra por palabra. |

### Hacer scroll en la terminal mediante atajos de teclado

En ocasiones la terminal arroja salidas largas que son imposibles visualizar en una pantalla. Para retroceder o avanzar en la salida de un comando pueden usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `Ctrl + mayús + Repág` | Para retroceder o ir hacía arriba en la terminal. |
| `Ctrl + mayús + Avpág` | Para avanzar o ir hacía abajo en la terminal. |

## TRABAJAR CON PESTAÑAS EN LA TERMINAL

Trabajar con pestañas en la terminal puede resultar práctico para algunos usuarios. La forma más rápida de trabajar mediante pestañas en la terminal es usando atajos de teclado. Los atajos de teclado que en principio deberán usar son los siguientes.

### Abrir una pestaña en la terminal

Para abrir una pestaña en la terminal tan solo tendrán que usar el siguiente atajo de teclado:

```shell
Ctrl+mayús+t
```

Justo al usar el atajo de teclado se abrirá una nueva pestaña en la terminal.

### Moverse entre las distintas pestañas abiertas que podamos tener en la terminal

Si tenemos varias pestañas abiertas nos podemos mover de una pestaña a otra de forma rápida mediante el siguiente atajo de teclado.

```shell
Ctrl+mayús+cursores
```

Los atajos de teclado pueden variar en función del emulador de terminal que uséis. Si no os funciona este atajo de teclado pueden intentar usar los siguientes atajos:

| Atajos de teclado alternativos para moverse entre pestañas |
| --- |
| `ctrl+AvPág` |
| `ctrl+RePág` |
| `ctrl+mayús+AvPág` |
| `ctrl+mayús+RePág` |

**Nota**: Existen emuladores de terminal que permiten modificar los atajos de teclado.

### Cerrar una pestaña abierta en la terminal

Si lo que pretenden es cerrar alguna de las pestaña pueden usar el comando `exit`. También pueden usar los siguientes atajos de teclado:

| Atajos de teclado para cerrar una pestaña abierta en la terminal |
| --- |
| `ctrl+mayus+w` |
| `ctrl+d` |

## TRUCOS PARA MEJORAR LA ACCESIBILIDAD EN LA TERMINAL

Algunos consejos que les permitirán sentirse más cómodos y mejorar la accesibilidad en la terminal son los siguientes.

### Aumentar o disminuir la fuente de la terminal

En diversas ocasiones puede resultar útil aumentar o disminuir el tamaño del texto de la terminal. Para ello la forma más sencilla es usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `Ctrl+mayús +` | Para incrementar el tamaño del texto. |
| `Ctrl+ mayús -` | Disminuir el tamaño del texto. |

### Trabajar con la terminal a pantalla completa

En algunos casos puede resultar interesante poner el emulador de terminal a pantalla completa. Para ello tan solo hay que asegurar que la ventana del emulador de terminal esté activa y presionar el atajo de teclado `F11`.

### Bloquear y desbloquear la terminal

Si queremos bloquear la terminal para que nadie pueda introducir ningún comando podemos usar los siguientes atajos de teclado:

| Atajo de teclado | Función |
| --- | --- |
| `Ctrl+s` | Bloquear la terminal. |
| `Ctrl+q` | Desbloquear la terminal. |

### Usar un buen esquema de colores en la terminal

Configuren la terminal a su gusto. Prácticamente la totalidad de emuladores de terminal permiten modificar configuraciones como por ejemplo el esquema de colores y la tipografía a utilizar. Si lo hacen y eligen una buena configuración verán que la experiencia de uso será más satisfactoria.

### Mostrar las salidas de la terminal de una forma más ordenada

Mediante la utilidad `column` podemos mostrar la salida de la terminal de una forma mucho más ordenada y fácil de leer. Por ejemplo si ejecutamos el comando `mount` obtendremos la siguiente salida:

```shell
❯:~$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=3947964k,nr_inodes=986991,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=797812k,mode=755,inode64)
/dev/sda5 on / type ext4 (rw,noatime)
```

Si os fijáis la salida no es fácil de leer porque cada uno de los resultados mostrados se separa por espacios y no tienen una longitud común. Si cambiamos los espacios por columnas mediante la utilidad `column` obtendremos el siguiente resultado:

```shell
❯:~$ mount | column -t
sysfs       on  /sys                                       type  sysfs            (rw,nosuid,nodev,noexec,relatime)
proc        on  /proc                                      type  proc             (rw,nosuid,nodev,noexec,relatime)
udev        on  /dev                                       type  devtmpfs         (rw,nosuid,relatime,size=3947964k,nr_inodes=986991,mode=755,inode64)
devpts      on  /dev/pts                                   type  devpts           (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
```

**Nota:** El delimitador por defecto de `column` es el espacio. Si quieren pueden definir otro delimitador mediante la opción `-s`. Si queréis ver más opciones sobre la utilidad `column` pueden ejecutar el comando `man column` en la terminal.

## TRUCOS PARA GESTIONAR TRABAJOS EN LA TERMINAL DE LINUX CON JOBS, FG, BG, DISOWN Y KILL

La terminal de Linux ofrece capacidad de multitarea y podemos ejecutar y realizar distintas tareas de forma simultanea mediante las pestañas o mediante la gestión de trabajos. Para aprender a gestionar las tareas o trabajos de Linux les recomiendo que visiten el siguiente enlace:

https://geekland.eu/gestionar-trabajos-en-linux-terminal/

#### Fuentes

[https://www.youtube.com/watch?v=AVXYq8aL47Q](https://www.youtube.com/watch?v=AVXYq8aL47Q)
