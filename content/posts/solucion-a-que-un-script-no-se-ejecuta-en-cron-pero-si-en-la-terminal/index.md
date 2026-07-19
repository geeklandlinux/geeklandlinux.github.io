---
title: "Solución a que un script no se ejecuta en cron pero si en la terminal"
date: 2022-05-08
categories: 
  - "linux-2"
tags: 
  - "cron"
  - "crontab"
  - "path"
  - "script"
  - "sysadmin"
coverImage: "script-no-se-ejecuta-en-cron-pero-si-en-la-terminal.png"
---

En mi caso tengo un script y cuando lo ejecuto manualmente funciona a la perfección. El problema es que cuando quiero programar su ejecución mediante cron el script no se ejecuta y/o no hace lo que hay que hacer. En el caso que se encuentren en un caso similar al mío les recomiendo que hagan lo siguiente.<!--more-->

## ¿POR QUÉ NO ES SE EJECUTA CORRECTAMENTE UN SCRIPT MEDIANTE CRON?

El primero motivo podría ser que la [sintaxis usada no sea correcta]({{< relref "/posts/planificar-tareas-con-cron-y-anacron-en-linux" >}}), pero este caso es poco probable porque si la sintaxis fuera incorrecta la terminal arrojaría un error y lo veríamos claramente.

El segunda opción es que nuestro script haga uso de un fichero binario cuyo `PATH` no está correctamente definido en Cron.

### ¿Pero que es el PATH o la ruta en Linux?

El `PATH` o la ruta es una variable de entorno cuya función es definir la ubicación de los distintos binarios de nuestro sistema operativo. Tenéis que tener presente que cada vez que ejecutamos un comando, la shell de Linux buscará en todos y cada uno de los directorios definidos en el `PATH` para encontrar el binario y de esta forma poder ejecutar el comando.

### ¿Cuál es el PATH de mi usuario?

Cada uno de los procesos de ejecución o cada una de la sesiones de usuario pueden tener un `PATH` diferente. Si por ejemplo queremos ver el `PATH` de nuestro usuario tan solo tenemos que abrir una terminal y ejecutar el siguiente comando:

> ```shell
> joan@ubuntu-20-04:~$ echo $PATH
> /home/joan/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
> ```

Por lo tanto después de ejecutar el comando vemos que el usuario `joan` tiene definido el siguiente `PATH`:

> ```shell
> /home/joan/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
> ```

En este caso el usuario `joan` podrá ejecutar cualquier binario que esté dentro de las rutas especificadas dentro de la variable de entorno `PATH`.

### ¿Cuál es el PATH establecido en las tareas que ejecuta CRON?

Para averiguar el `PATH` definido en cron tan solo tenemos que realizar lo siguiente.

Inicialmente ejecutamos el siguiente comando en la terminal:

> ```shell
> joan@ubuntu-20-04:~$ crontab -e
> ```

A continuación cuando se abra el editor de textos para editar el contenido de crontab añadimos la siguiente línea y guardamos los cambios.

> ```shell
> * * * * * env > /tmp/cronenv
> ```

Una vez guardados los cambios tenemos que esperar 1 minuto. Después de esperar un minuto consultaremos el contenido del fichero `/tmp/cronenv` para ver la totalidad de variables de entorno definidas en CRON. En mi caso las variables de entorno de cron son las siguientes:

> ```shell
> joan@ubuntu-20-04:~$ cat /tmp/cronenv
> HOME=/home/joan
> LOGNAME=joan
> PATH=/usr/bin:/bin
> LANG=es_ES.UTF-8
> SHELL=/bin/sh
> PWD=/home/joan
> ```

Si analizamos el contenido de la variable de entorno `PATH` vemos que únicamente contiene las rutas `/usr/bin` y `/bin`. Por lo tanto cron solo podrá ejecutar scripts que contengan comandos cuyos ficheros binarios estén ubicados en `/usr/bin` o `/bin`. Si os fijáis el `PATH` de la sesión de usuario de `joan` es totalmente diferente al `PATH` del cron y posiblemente este sea el motivo de nuestros problemas.

### Simular la ejecución de un script con las variables de entorno definidas en CRON

Para confirmar que el problema es el que acabo de citar ejecutaremos el script que nos da los problemas con las mismas variables de entorno definidas en cron. Para ello ejecutaremos el comando `env - $(cat /tmp/cronenv)` seguido del comando necesario para ejecutar el script que da problemas. Por lo tanto en mi caso el comando que tengo que ejecutar es el siguiente:

> ```shell
> joan@ubuntu-20-04:~$ env - $(cat /tmp/cronenv) bash /home/joan/scripts/audiobookyoutube/audioyoutube.sh
> ```

Después de ejecutar el script veo que en la salida estándar de la terminal se produce el siguiente error.

> ```shell
> /home/joan/scripts/audiobookyoutube/audioyoutube.sh: línea 66: yt-dlp: orden no encontrada
> ```

Por lo tanto el problema es que `yt-dlp` no se puede ejecutar. El motivo por el cual no se ejecuta es simple:

- En mi caso el binario de `yt-dlp` se halla en la ubicación `/home/joan/.local/bin`. Como hemos visto anteriormente está ruta no está presente en el `PATH` del cron. Por lo tanto es normal que el script falle en su ejecución.

## COMO SOLUCIONAR EL PROBLEMA DEL PATH EN CRON Y DE ESTA FORMA EJECUTAR EL SCRIPT DE FORMA SATISFACTORIA

Para solucionar el problema mostrado en esté artículo tan solo tenemos que añadir la ruta `/home/joan/.local/bin` en el `PATH` de cron. Para ello tenemos varias soluciones.

### Definir el PATH de cron en el fichero crontab

La primera de las soluciones es añadir el `PATH` de cron dentro del fichero crontab del usuario que ejecutará el script. Para ello tan solo tenemos que loguear con el usuario apropiado, abrir una terminal y ejecutar el siguiente comando:

> ```shell
> joan@ubuntu-20-04:~$ crontab -e
> ```

Una vez se abra el fichero crontab introduciremos una línea del siguiente tipo justo al inicio del fichero para definir el `PATH`:

> ```shell
> PATH=RUTA_BINARIOS1:RUTA_BINARIOS2:RUTA_BINARIOS3
> ```

Por lo tanto en mi caso añadiré la siguiente línea al inicio de mi fichero crontab:

> ```shell
> PATH=/usr/bin:/bin:/home/joan/.local/bin
> ```

Finalmente guardaremos los cambios y cerraremos el fichero. Ahora cuando se ejecute un script mediante cron se considerarán todos los binarios ubicados en la ruta `/home/joan/.local/bin`. Por lo tanto se podrá ejecutar `yt-dlp` y de este modo se debería ejecutar el script sin ningún tipo de problema. Por lo tanto hemos solucionado el problema que un script no se ejecuta en cron.

### Definir el PATH de ejecución dentro del script

Otra opción alternativa a la que acabamos de ver es definir el `PATH` dentro del script que queremos ejecutar. En mi caso el script que me da problemas es el `/home/joan/scripts/audiobookyoutube/audioyoutube.sh`. Por lo tanto lo único que tengo que realizar es abrirlo mediante el siguiente comando:

> ```shell
> joan@ubuntu-20-04:~$ nano /home/joan/scripts/audiobookyoutube/audioyoutube.sh
> ```

Una vez abierto defino el `PATH` justo después del Shebang. En mi caso lo he realizado del siguiente modo:

> ```shell
> #!/bin/bash
> PATH=/usr/bin:/bin:/home/joan/.local/bin
> ```

Una vez guardados las modificaciones realizadas en el script cron será capaz de ejecutar el script sin ningún tipo de problema.
