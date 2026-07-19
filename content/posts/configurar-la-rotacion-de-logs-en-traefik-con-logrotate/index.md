---
title: "Configurar la rotación de Logs en Traefik con logrotate"
date: 2021-04-24
categories: 
  - "docker"
  - "linux-2"
tags: 
  - "logrotate"
  - "logs"
  - "traefik"
coverImage: "configurar-la-rotacion-de-logs-en-traefik.png"
---

La semana pasada vimos como [activar y consultar los logs del proxy inverso Traefik usando Docker]({{< relref "/posts/usar-fail2ban-con-traefik-para-proteger-servicios-que-corren-en-docker" >}}). No obstante el log generado por Traefik irá creciendo hasta que con el paso del tiempo acabe teniendo un tamaño desproporcionado y llene todo el disco. Para evitar este problema lo único que tenemos que hacer es configurar la rotación de logs en Traefik mediante logrotate de la siguiente forma.<!--more-->

## CONFIGURAR LA ROTACIÓN DE LOGS EN TRAEFIK

Aunque sepas como funciona el sistema de rotación de Linux hay que tener en cuenta que el log de Traefik que queremos rotar está dentro de un contenedor Docker. Por lo tanto el procedimiento que deberemos seguir es ligeramente diferente al procedimiento normal. Para editar la política de rotación de logs de Traefik ejecutaremos el siguiente comando en la terminal:

> ```nash
> pi@raspberrypi:~ $ nano /etc/logrotate.d/traefik
> ```

Una vez se abra el editor de textos nano pegaremos el siguiente código para definir la política de rotación de logs:

> ```shell
> /var/log/traefik/*.log {
>   weekly
>   size 1M
>   rotate 5
>   missingok
>   notifempty
>   postrotate
>     docker kill --signal="USR1" traefik
>   endscript
> }
> ```

Una vez pegado el código guardamos los cambios y cerramos el fichero. El significado de todos y cada uno de los parámetros para rotar el log de Traefik es el siguiente:

| Parámetro de configuración | Función |
| --- | --- |
| `/var/log/traefik/*.log {` | Definimos la ruta del log o logs que queremos rotar. |
| `weekly` | Una vez a la semana se intentarán rotar los logs. |
| `size 1M` | Para que se realice la rotación del log/s como mínimo tiene que tener un tamaño de un Mega. |
| `rotate 5` | El número máximo de ficheros de logs almacenados serán 5. Por lo tanto en nuestro caso solo almacenaremos los logs de las últimas 5 semanas. |
| `missingok` | No se producirá ningún error en el caso que no exista un fichero de log. |
| `notifempty` | Si el fichero de logs está vacío no se rotará el log. |
| `postrotate` | Permite ejecutar un comando o script después de efectuarse la rotación de logs. |
| `docker kill --signal="USR1" traefik` | Después de especificar postrotate podemos ejecutar el comando `docker kill --signal="USR1" traefik`. El comando es para dar la señal `USR1` al contenedor Docker traefik. `USR1` es la señal que los desarrolladores de Traefik han definido para que se realice la rotación de logs. |
| `endscript` | Indicamos que ha terminado la ejecución del comando o script anterior. |

A partir de estos momentos ya no tenemos que preocuparnos. La rotación de logs en Traefik está configurada y según la política que hemos definido se rotarán semanalmente.

## FORZAR LA ROTACIÓN DE LOGS EN TRAEFIK PARA COMPROBAR EL FUNCIONAMIENTO

Para comprobar el funcionamiento de la configuración que acabamos de aplicar podemos esperar una semana o podemos forzar la ejecución de la tarea que realiza la rotación de logs. Para forzar la tarea que realiza la rotación de logs tan solo tenemos que ejecutar cualquiera de los 2 comandos que les dejo a continuación:

> ```shell
> sudo logrotate /etc/logrotate.conf --debug 
> sudo logrotate /etc/logrotate.conf
> ```

Como pueden ver en mi caso la rotación de logs se está realizando de forma adecuada.

> ```shell
> pi@raspberrypi:~ $ cd /var/log/traefik/
> pi@raspberrypi:/var/log/traefik $ ls -la
> total 1380
> drwxr-xr-x 2 root root    4096 abr 23 00:00 .
> drwxr-xr-x 5 root root    4096 abr 24 00:00 ..
> -rw-r--r-- 1 root root  205840 abr 24 10:56 access.log
> -rw-r--r-- 1 root root 1184842 abr 22 23:20 access.log.1
> ```

### Fuentes

[https://doc.traefik.io/traefik/v2.0/observability/logs/](https://doc.traefik.io/traefik/v2.0/observability/logs/)
