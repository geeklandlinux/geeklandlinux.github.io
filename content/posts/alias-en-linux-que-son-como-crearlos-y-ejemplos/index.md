---
title: "Alias en Linux, Que són como crearlos y ejemplos"
date: 2022-09-03
categories: 
  - "linux-2"
tags: 
  - "alias"
  - "terminal"
cover:
  image: "images/alias-en-linux-y-ejemplos-de-como-crearlos.jpeg"
  relative: true
---

En ocasiones tenemos que introducir comandos largos y difíciles de recordar en la terminal. Esto acostumbra a generar errores y frustración, pero afortunadamente para solucionar este pequeño podremos usar los `alias` en Linux. Los `alias` nos ayudarán a simplificar la ejecución de un comando y de este modo el comando será más fácil de usar y de recordar. Por lo tanto en este artículo veremos que es un alias, que utilidades puede tener, como podemos crear un alias, como podemos borrar alias y como podemos listar los alias que tenemos configurados en nuestra terminal.<!--more-->

## ¿QUÉ ES UN ALIAS EN LINUX?

Cuando hablamos de un alías nos referimos a una abreviatura para ejecutar un comando. Por lo tanto un alías es una cadena de caracteres que nosotros definiremos y que servirá para ejecutar un comando determinado. Un ejemplo de lo que seria un alias es el siguiente:

Que al escribir la palabra `actualizar` y presionar la tecla `enter` se ejecute el comando `sudo apt update && sudo apt dist-upgrade -y`. De esta forma con tan solo escribir `actualizar` y presionando `enter` podremos actualizar nuestro sistema operativo de forma más rápida.

## ¿QUÉ UTILIDADES Y VENTAJAS TIENEN LOS ALIAS EN LINUX?

La principales utilidades y ventajas de los alias en GNU-Linux son las siguientes:

1. Reducir el texto escrito en la terminal para realizar una tarea. Al ser un texto más corto podremos ejecutar el comando de forma mucho más rápida.
2. Podremos usar la abreviatura que nosotros queramos para ejecutar un comando. Por lo tanto podremos recordar más fácilmente el comando a ejecutar para una tarea determinada.
3. Sustituir una herramienta por otra herramienta sin tener que cambiar la sintaxis. Por ejemplo podremos usar herramientas como `bat` y `exa` usando la misma sintaxis que con `cat` y `ls`.
4. Que al ejecutar un comando simple se ejecute con opciones adicionales. En otras palabra podemos conseguir que ejecutando el comando `ls` se ejecute el comando `ls -lah`. Por lo tanto cada vez que ejecutemos `ls` nos ahorraremos la introducción de las opciones `-lah`.
5. Existen menos posibilidades de error al introducir un comando en la terminal. El motivo es porque los comandos son cortos y fáciles de recordar ya que han sido creados por nosotros mismos.

## CREAR UN ALIAS EN LINUX

El sistema operativo GNU-Linux permite crear `alias` permanentes y `alias` que solo se aplicarán en la sesión actual del emulador de terminal. A continuación veremos ambos casos, pero lo primero que tenemos que tener claro es la sintaxis para crear un `alias`.

### Sintaxis para crear una alias

La sintaxis para crear un `alias` será del siguiente tipo:

```shell
alias abreviatura="comando_que_ejecuta_la_abreviatura"
```

Cada una de las partes de las sintaxis realiza la siguiente función:

`alias`: Parte del comando que indica que queremos crear un alias.

`abreviatura`: Es una cadena de caracteres cualquiera que ejecuta un comando, una serie de comandos o un script.

`comando_que_ejecuta_la_abreviatura`: Es el comando que se ejecutará cuando llamamos al `alias` o abreviatura que hayamos definido.

Una vez conocemos la sintaxis veremos un ejemplo para comprender mejor lo que estamos explicando.

### Ejemplo de creación de un alias

Si lo que pretendemos es actualizar nuestro sistema operativo con el comando `actualizar` lo haremos del siguiente modo:

1. Primero tenemos que tener claro el alias que queremos usar para actualizar nuestro sistema operativo. En mi caso es `actualizar`.
2. También tenemos que conocer los comandos que tenemos que usar para actualizar el sistema operativo. En mi caso los comandos son `sudo apt update && sudo apt dist-upgrade -y`

Una vez tenemos claros estos 2 puntos tan solo tenemos que usar la sintaxis que vimos en el apartado anterior. Por lo tanto abriremos un emulador de terminal y ejecutaremos el siguiente comando:

```shell
❯:~$ alias actualizar="sudo apt update && sudo apt dist-upgrade -y"
```

Una vez ejecutado el comando podremos actualizar el sistema operativo simplemente usando el comando `actualizar`. En mi caso si ejecuto el comando `actualizar` obtendré el siguiente resultado:

```shell
❯:~$ actualizar
Obj:1 http://deb.debian.org/debian testing InRelease
Obj:2 http://deb.debian.org/debian testing-updates InRelease
Obj:3 http://security.debian.org testing-security InRelease
Descargados 7.898 B en 3s (2.938 B/s)
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Todos los paquetes están actualizados.
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Calculando la actualización... Hecho
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```

Por lo tanto acabamos de crear un `alias` y además vemos que funciona. Pero si cierran el emulador de terminal y lo vuelven a abrir verán que el `alias` deja de funcionar:

```shell
❯:~$ actualizar
actualizar: command not found
```

Para hacer el `alias` permanente deberán realizar lo siguiente.

### Como hacer que un alias sea permanente

Para que el `alias` creado en el apartado anterior sea permanente deberemos realizar lo siguiente.

Lo primero es averiguar la shell o terminal que estamos usando. Para ello ejecutaremos el comando `echo "$SHELL"` en la terminal:

```shell
❯:~$ echo "$SHELL"
/bin/bash
```

**Nota:** En mi caso la salida del comando es `/bin/bash`. Por lo tanto estoy usando la terminal de bash.

En función de la terminal que estemos usando tenemos que ejecutar el siguiente comando:

| shell | comando |
| --- | --- |
| `/bin/bash` | `nano ~/.bashrc` |
| `/bin/zsh` | `nano ~/.zshrc` |
| `/bin/fish` | `nano ~/.config/fish/config.fish` |

Una vez se abra el editor de textos nano se van al final del fichero y añaden el alias del apartado anterior:

```shell
alias actualizar="sudo apt update && sudo apt dist-upgrade -y"
```

Acto seguido guardan los cambios, cierran el editor de textos nano y ejecutan el comando correspondiente para recargar la configuración de la terminal. El comando para recargar la configuración será el siguiente en función de la shell que estén usando:

| shell | comando |
| --- | --- |
| `/bin/bash` | `source ~/.bashrc` |
| `/bin/zsh` | `source ~/.zshrc` |
| `/bin/fish` | `source ~/.config/fish/config.fish` |

A partir de ahora, siempre que ejecutemos el comando `actualizar` se actualizará nuestro sistema operativo.

### Algunos ejemplos de alias que pueden ser útiles

Ahora ya sabemos como crear `alias` y además podemos ver su utilidad. A continuación les dejaré una serie de `alias` que pueden usar si lo creen conveniente:

| Alias | Función del alias |
| --- | --- |
| `alias ls="exa -lah"` | Permite reemplazar `ls` por `exa`. Es preciso instalar `exa` |
| `alias cat="bat"` | Reemplazar de forma sencilla `cat` por `bat`. Es preciso instalar `bat` |
| `alias rm="rm -i"` | Para borrar archivos con más seguridad desde la terminal. |
| `alias cp="cp -i"` | Antes de sobreescribir un fichero nos pedirá confirmación. |
| `alias mv="mv -i"` | Antes de sobreescribir un fichero nos pedirá confirmación. |
| `alias mkdir="mkdir -pv"` | Para facilitar la creación de directorios. |
| `alias mkdircd='function _mkdircd(){ mkdir -p "$1"; cd "$1"; };_mkdircd'` | Crear un directorio y acceder directamente al directorio creado. |
| `alias at="thunar $(pwd)"` | Abrir el explorador de archivos Thunar desde la terminal en la posición actual. |
| `alias gh="history \| grep"` | Encontrar un comando en el [historial de la terminal]({{< relref "/posts/como-usar-y-configurar-el-historial-de-comandos-history-en-linux" >}}) mediante el comando `gh` |
| `alias count="find . -type f \| wc -l"` | Usar el comando `count` para contar los ficheros que contiene un directorio. |
| `alias ports="netstat -tulanp"` | Listar los puertos abiertos en nuestro equipo. |
| `alias free="free -m -l -t -h"` | Ver el consumo de memoria de la forma que me interesa acortando el comando ejecutado. |
| `alias download="youtube-dl -f bestaudio --extract-audio --audio-format mp3` | Usar el comando `download` para descargar un audio de youtube mediante youtube-dl. |
| `alias syslog="sudo tail -n 10 /var/log/syslog"` | Mostrar las 10 últimas líneas del fichero de logs syslog mediante el comando `syslog` |
| `alias cpu10='watch -n 3 "free; echo; uptime; echo; ps aux --sort=-%cpu \| head -n 11; echo; who"'` | Cada 3 segundos mostrar los 10 procesos que consumen más CPU con el comando `cpu10` |
| `alias mem10='watch -n 3 "free; echo; uptime; echo; ps aux --sort=-%mem \| head -n 11; echo; who"'` | Cada 3 segundos mostrar los 10 procesos que consumen más RAM con el comando `mem10` |
| `alias ip="curl icanhazip.com"` | Saber nuestra ip pública desde la terminal con el comando `ip`. |
| `alias .1="cd .."` | `.1` para subir 1 nivel en la estructura de directorios. |
| `alias .2="cd ../.."` | `.2` para subir 2 niveles en la estructura de directorios. |
| `alias .3="cd ../../.."` | `.3` para subir 3 niveles en la estructura de directorios. |
| `alias grep="grep --color=auto"` | Para que grep resalte la palabra coincidente. |
| `alias df="df -h"` | Para leer más fácil la salida del comando `df`. |
| `alias tv="curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py \| python3 -"` | Realizar un test de velocidad de la conexión a Internet. |

**Nota:** La tabla solo muestra algunos de los `alias` que pueden crear. Si quieren compartir sus `alias` lo pueden hacer a través de los comentarios de este artículo.

### Precauciones a tener en el momento de crear un alias

Cuando configuramos un `alias` tenemos que ser cautelosos y evitar usar comandos existentes en nuestro sistema operativo. Por ejemplo si configuramos el siguiente `alias`:

```shell
alias sudo="ls"
```

Cada vez que ejecutemos un comando que contiene la palabra `sudo` la terminal interpretará `sudo` como `ls`. Esto implica que no podremos ejecutar comandos con permisos de administrador. Por ejemplo si configuramos el alias `alias sudo="ls"` e intentamos refrescar los repositorios de nuestra distribución ocurrirá lo siguiente:

```shell
❯:~$ sudo apt update
"apt": No such file or directory (os error 2)
"update": No such file or directory (os error 2)
```

## LISTAR LA TOTALIDAD DE ALIAS QUE TENEMOS CREADOS

Para listar la totalidad de alias definidos en nuestro sistema operativo tan solo tenemos que ejecutar el comando `alias` en la terminal:

```shell
❯:~$ alias
alias -- -='cd -'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias c='clear'
alias cat='bat'
alias h='history'
alias ls='exa -lah'
alias md='mkdir -p'
alias mv='mv -i'
alias pass='passgen'
alias py='python'
alias q='exit'
alias rd='rmdir'
alias rm='cp -i'
alias sl='ls'
alias snano='sudo nano'
alias syslog='sudo tail -n 10 /var/log/syslog'
alias vbpf='${VISUAL:-vim} ~/.bash_profile'
alias vbrc='${VISUAL:-vim} ~/.bashrc'
```

## BORRAR UN ALIAS EN LINUX

La forma de borrar un alias dependerá si se trata de un alias permanente o no permanente. En el caso de un `alias` temporal o no permanente lo haremos del siguiente modo.

### Eliminar un alias no permanente

Si queremos eliminar la totalidad de alias de un plumazo ejecutaremos el siguiente comando en la terminal:

```shell
❯:~$ unalias -a
```

Después de ejecutar el comando se borrarán la totalidad de alias temporales o no permanentes.

Si únicamente queremos borrar uno de los `alias` ejecutaremos el comando `unalias` seguido del nombre del `alias_que_queremos_borrar.` Por lo tanto para borrar el `alias` cuyo nombre es `actualizar` ejecutaremos el siguiente comando en la terminal:

```shell
❯:~$ unalias actualizar
```

En el caso que necesiten borrar un `alias` permanente deberán proceder del siguiente modo.

### Eliminar un alias permanente

En función de la shell que usen deberán usar el siguiente comando:

| shell | comando |
| --- | --- |
| `/bin/bash` | `nano ~/.bashrc` |
| `/bin/zsh` | `nano ~/.zshrc` |
| `/bin/fish` | `nano ~/.config/fish/config.fish` |

Cuando se abra el editor de textos nano deberán buscar el `alias` que quieren eliminar y borrarlo. Una vez borrado guardan los cambios, cierran el fichero y recargan la configuración de la terminal. El comando para recargar la configuración será en función de la shell que estén usando:

| shell | comando |
| --- | --- |
| `/bin/bash` | `source ~/.bashrc` |
| `/bin/zsh` | `source ~/.zshrc` |
| `/bin/fish` | `source ~/.config/fish/config.fish` |

## CONCLUSIÓN

Los alias son sumamente útiles y dan una idea de lo poderosa que es la terminal en Linux. Los alias nos permitirán ejecutar comandos de forma más rápida y tecleando menos. No obstante es una arma de doble filo porque si modificamos en exceso el comportamiento estándar de nuestro sistema operativo tendremos problemas cuando no trabajemos con nuestro equipo. Además no es una función especialmente interesante para los principiantes de Linux porque lo que un principiante tiene que hacer es aprender los comandos básicos del sistema operativo.

#### Fuentes

[https://phoenixnap.com/kb/alias-command](https://phoenixnap.com/kb/linux-alias-command)

[https://www.reddit.com/r/comments/c85me2/useful\_aliases\_that\_i\_have\_created/](https://www.reddit.com/r/linux/comments/c85me2/useful_aliases_that_i_have_created/)
