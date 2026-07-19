---
title: "Programar la ejecución de tareas con Systemd Timers y reemplazar Cron"
date: 2020-06-21
categories: 
  - "linux-2"
tags: 
  - "systemctl"
  - "systemd"
  - "systemd-timers"
cover:
  image: "images/Systemd-Timers-para-programar-ejecucion-tareas.jpg"
  relative: true
---

En su día detallamos el [uso de cron y anacron]({{< relref "/posts/planificar-tareas-con-cron-y-anacron-en-linux" >}}) para programar la ejecución de tareas y scripts a una hora determinada. Cron y Anacron funcionan perfectamente y de hecho es el método que acostumbro a utilizar. No obstante hay alternativas para reemplazar Cron y una de ellas son los Systemd Timers. Los Timers de Systemd pueden ser una alternativa válida ya que la mayoría de distribuciones Linux incorporan Systemd.<!--more-->

## ¿QUÉ SON Y COMO FUNCIONAN LOS SYSTEMD TIMERS?

Tal y como hemos comentado en la introducción, los Systemd timers permiten automatizar el inicio de un proceso, programa, script o comando a una hora determinada o en intervalos de tiempo determinados.

## BENEFICIOS QUE APORTAN LOS SYSTEMD TIMERS RESPECTO CRON

Tanto Cron como Systemd Timer funcionan perfecto. Teóricamente los Systemd Timers aportan los siguientes beneficios respecto Cron:

1. La **depuración de errores es más fácil**. Todos los errores producidos en la ejecución de un trabajo quedan registrados en el Journal de Systemd.
2. Proporcionan un **mayor control de cuando se ejecutará la tarea**. Los Systemd Timers nos permite configurar que para que se inicie una tarea antes tiene que estar levantada la red o un servicio de base de datos como por ejemplo MariaDB.
3. Los trabajos ejecutados con Systemd Timers pueden gestionarse mediante una característica del Kernel llamada Control Groups. Esto permite **asociar de forma inequívoca todos los procesos que genera o arranca un servicio**.

Como contrapartida los Systemd Timers son más difíciles de configurar. Por este motivo en mi caso sigo y seguiré usando Cron.

## PLANIFICAR LA EJECUCIÓN DE UNA TAREA CON SYSTEMD TIMERS

Programar una tarea con un systemd timer es sensiblemente más complicado que con Cron. Mientras en Cron trabajamos sobre dos ficheros, en Systemd se acostumbra a trabajar en 3 ficheros:

1. El primer fichero se trata del **script que tenemos que ejecutar**. Si se trata de correr un simple comando obviamente no es necesario crear ningún script ya que lo podemos introducir directamente en el servicio de Systemd.
2. El segundo fichero es para configurar **el servicio de Systemd** que iniciará el script o comando que queramos ejecutar.
3. Finalmente, el tercer fichero será para crear el **timer** que iniciará el servicio de systemd en el momento que nosotros queramos.

Una vez vista la introducción pasaremos a ver un ejemplo de creación de un Timer de Systemd. Los pasos a seguir son los siguientes.

## CREAR EL SCRIPT QUE TENEMOS QUE EJECUTAR EN UN MOMENTO DETERMINADO

**Nota**: Si unicamente queréis ejecutar un comando en un momento determinado no es necesario crear un script. Si solo queréis ejecutar un simple comando podéis pasar al siguiente apartado.

A modo de ejemplo crearé un simple script que registrará la fecha en que un determinado usuario ejecutará el script. Para ello ejecutaré el siguiente comando en la terminal:

`sudo nano /opt/mi_script.sh`

Cuando se abra el editor de texto pegaré el siguiente contenido para conseguir mi propósito:

`#!/bin/bash echo "$(whoami) $(date)" >> /home/joan/milog.log`

Una vez creado el script guardaremos los cambios y cerraremos el fichero. Seguidamente daré permisos de ejecución al script ejecutando el siguiente comando en la terminal:

`sudo chmod +x /opt/mi_script.sh`

Finalmente podremos ejecutar el script para ver que funciona. Para ello ejecutaremos el siguiente comando en la terminal:

`/opt/mi_script.sh`

Una vez ejecutado el script comprobaré que el fichero milog.log ha registrado correctamente el usuario que ha ejecutado el script y la hora en que lo hizo. Para ello ejecutaré el siguiente comando en la terminal:

`joan@debian:~$ cat milog.log root vie jun 19 17:28:39 CEST 2020`

## CREAR EL SERVICIO DE SYSTEMD QUE INICIARÁ EL SCRIPT QUE ACABAMOS DE CREAR

El siguiente paso consiste en crear el servicio de Systemd que ejecutará el script que acabamos de crear. En mi caso todos los servicios de Systemd los acostumbro a crear en la siguiente ubicación:

`/etc/systemd/system/`

Al servicio de Systemd le podemos poner cualquier nombre que tenga relación con la ejecución del script y que termine con la extensión .service. En mi caso crearé el servicio registro.service ejecutando el siguiente comando en la terminal:

`sudo nano /etc/systemd/system/registro.service`

Una vez se abra el editor de texto nano pegaré el siguiente código:

`[Unit] Description= Servicio de prueba para geekland Requires=network.target After=network.target  [Service] Type=simple ExecStart=/opt/mi_script.sh User=joan`

Una vez creado el servicio guardamos los cambios y cerramos el fichero. El significado de cada uno de los campos es el que detallo a continuación:

- La variable **Description** es para definir la utilidad del servicio. Por lo tanto podemos poner una frase cualquiera que nos ayude a recordar la utilidad del servicio que estamos definiendo.
- La directiva **Requires** indica que para que se inicie el servicio la red tiene que estar levantada. Esta directiva está estableciendo una dependencia. Para ejecutar un simple script esta directiva no es necesaria, pero al tratarse de un ejemplo didáctico la he introducido.
- Con **After=network.target** indicamos que la red se inicie antes que nuestro servicio. Para ver la totalidad de servicios existentes podemos usar el siguiente comando `systemctl list-units --type service --all`. Al igual que con la directiva anterior, no es necesario especificar la directiva After para la ejecución de scripts simples.
- La variable **Type** define el tipo de arranque del servicio. **simple** es el valor por defecto y hace que justo después de ejecutar el script, systemd considere que ya se ha ejecutado de forma satisfactoria. Otros tipos de arranque que pueden usarse son **forking**, **oneshot**, **dbus**, **notify**, **idle**, etc. Pueden encontrar una explicación de lo que hace cada arranque en el siguiente [enlace](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files "Explicación sobre los distintos métodos para iniciar una tarea").
- En en apartado **ExecStart** tenemos que detallar el comando que queremos que se ejecute o la ruta del script que queremos ejecutar. Por lo tanto en mi caso tengo que escribir `/opt/mi_script.sh`. Si al terminar la ejecución del script quisiéramos ejecutar otra acción/script lo podríamos hacer usando la directiva **ExecStop**.
- El campo **User** sirve para definir el usuario que ejecutará el script. Por lo tanto en mi caso será el usuario joan quien ejecutará el script que hemos definido en el apartado anterior. Si no definimos ningún usuario entonces será el usuario root quien ejecuta el script.

Para comprobar que el servicio que acabamos de crear funciona ejecutaremos el siguiente comando para que Systemd pueda detectar el nuevo servicio que acabamos de crear:

`sudo systemctl daemon-reload`

Para iniciar el servicio tan solo tenemos que ejecutar el siguiente comando:

`sudo systemctl start registro.service`

Después de ejecutar este comando se ejecutará el script. Para comprobar que el script se ha ejecutado tan solo tenemos que consultar el fichero en que se tenia que realizar el registro:

`joan@debian:~$ cat milog.log joan sáb jun 20 09:02:56 CEST 2020`

Vemos que el registro se ha realizado correctamente y además tal y como especificamos ha sido realizado por el usuario joan.

## CREAR EL TIMER QUE INICIARÁ EL SERVICIO DE SYSTEMD

Una vez comprobado que se puede ejecutar el script mediante el servicio crearemos un timer para que el servicio y script se ejecuten cuando nosotros queramos. Para ello en la misma ubicación que creamos el servicio crearemos un fichero con el mismo nombre que el servicio, pero con la extensión .timer. Por lo tanto ejecutaremos el siguiente comando en la terminal:

`sudo nano /etc/systemd/system/registro.timer`

**Nota:** Para identificar fácilmente los servicios y los timers es recomendable que usen el mismo nombre. La única diferencia entre ellos será la extensión.

**Nota:** Existe la posibilidad de crear timers que se ejecuten a una hora determinado o que se ejecuten después de períodos relativos de tiempo. A continuación veremos las dos opciones.

### Crear un timer para ser ejecutado después de un período relativo de tiempo

Una vez se abra el editor de texto nano pegaremos el siguiente código para definir el momento en que se realizará la tarea que queremos ejecutar.

`[Unit] Description= Timer de prueba para geekland  [Timer] Unit=registro.service OnBootSec=5min OnUnitActiveSec=15min  [Install] WantedBy=timers.target`

Una vez creado el timer guardamos los cambios y cerramos el fichero. El significado de cada uno de los campos es el que detallo a continuación:

- En la variable **Description** ponemos una descripción que ayude a identificar la utilidad del timer. Por lo tanto en mi caso pongo la misma descripción que en el servicio de Systemd.
- En **Unit** tenemos que introducir el nombre del servicio que tiene que ejecutar el timer. Por lo tanto en nuestro caso escribimos registro.service. En mi caso si quisiera no seria necesario introducir la variable Unit porque el Timer y el servicio de Systemd tienen el mismo nombre.
- El parámetro **OnBootSec=5min** indica que queremos que el script se ejecute 5 minutos después de arrancar el sistema operativo.
- El parámetro **OnUnitActiveSec=15min** hará que nuestro script se vaya ejecutando cada 15 minutos después de que se haya arrancado el sistema operativo. Si queremos indicar valores en semanas podemos reemplazar 15min por 2w.
- **Wantedby** es para indicar cuando se activará el Timer para ejecutar la tarea. En mi caso Wantedby tiene el valor timers.target. Por lo tanto el timer se activa justo al iniciar nuestro equipo. Otros valores de Wantedby que podemos usar son **multi-user.target**, **graphical.target**, **bluetooth.target**, etc. Cada target activará el timer en determinadas circunstancias. Por lo tanto si reemplazamos timers.target por graphical.target, el timer para activar la ejecución del script se iniciará en el momento que se levante la interfaz gráfica y si no se levanta la interfaz gráfica nunca se ejecutará el script. Para ver todos los target disponibles pueden ejecutar el siguiente comando en la terminal `systemctl list-units --type target`.

### Crear un timer para ser ejecutado a una hora determinada

Si por lo contrario queremos ejecutar nuestras tareas a una fecha y hora determinada deberemos cambiar los parámetros OnBootSec y OnUnitActiveSec por **OnCalendar**. Por lo tanto, el código del timer pasará a ser del siguiente modo:

`[Unit] Description= Timer de prueba para geekland  [Timer] Unit=registro.service OnCalendar=Thu *-*-* 17:00:00 Persistent=true  [Install] WantedBy=timers.target`

Observen lo siguiente:

- La línea en que definimos **Persistent=true** es opcional. Si añadimos esta línea y definimos su valor como true entonces si el ordenador no está encendido el jueves a las 17:00 horas no pasará nada porque el trabajo se ejecutará justo después de iniciar el ordenador. Por lo tanto añadiendo Persistent=true conseguiremos la misma funcionalidad que nos ofrece Anacron.
- La sintaxis del parámetro **Oncalendar** para definir la hora o las horas en que se tiene que ejecutar el script es la siguiente. `OnCalendar=Dia_semana año-mes-Dia Hora:minuto:segundo`

Por lo tanto en función del momento en que queremos ejecutar el script tendremos que modificar el parámetro OnCalendar del siguiente modo:

| Hora a iniciar | Parámetro para configurar el inicio |
| --- | --- |
| Todos los jueves que sean día 5 a las 17:00 horas y 50 minutos | `OnCalendar=Thu *-*-5 17:50:00` |
| Todos los días de Lunes a Viernes a las 4 de la madrugada. | `OnCalendar=Mon..Fri *-*-* 04:00:00` |
| Cada 5 Minutos | `OnCalendar=*-*-* *:0/5` |
| Los 10 primeros días del mes a las 14:30 siempre y cuando sea Lunes o Martes | `OnCalendar=Mon,Tue *-*-01..10 14:30:00` |
| Cada Minuto | `OnCalendar=*-*-* *:*:00` |
| Cada Hora | `OnCalendar=*-*-* *:00:00` |
| A diario | `OnCalendar=*-*-* 00:00:00` |
| De forma semanal los Domingos a las 12 de la noche | `OnCalendar=Sun *-*-* 00:00:00` |
| Se ejecuta una vez al mes | `OnCalendar=*-*-01 00:00:00` |
| Unicamente una vez al año | `OnCalendar=*-01-01 00:00:00` |
| Una vez cada 4 meses | `OnCalendar=*-01,04,07,10-01 00:00:00` |
| Una vez cada 6 meses | `OnCalendar=*-01,07-01 00:00:00` |

## ACTIVAR EL TIMER QUE ACABAMOS DE CREAR

A estas alturas y solo nos falta habilitar e iniciar el timer. Para ello recargaremos la configuración de Systemd ejecutando el siguiente comando en la terminal:

`sudo systemctl daemon-reload`

Acto seguido habilitaremos e iniciaremos el timer ejecutando los siguientes comandos en la terminal:

`sudo systemctl enable registro.timer sudo systemctl start registro.timer`

Finalmente podemos comprobar que el timer esta activo del siguiente modo:

`joan@debian:~$ sudo systemctl status registro.timer ● registro.timer - Timer de prueba para geekland      Loaded: loaded (/etc/systemd/system/registro.timer; enabled; vendor preset: enabled)      Active: active (waiting) since Sat 2020-06-20 11:26:19 CEST; 58s ago     Trigger: Sat 2020-06-20 11:41:19 CEST; 14min left    Triggers: ● registro.service`

###### Fuentes

Consulten las fuentes que les dejo a continuación. Los Systemd Units disponen de multitud de parámetros de configuración que no se han comentado en este artículo. Los siguientes enlaces les ayudarán a configurar de forma más precisa el inicio de una tarea.

[https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files) [https://wiki.archlinux.org/index.php/Systemd/Tim](https://wiki.archlinux.org/index.php/Systemd/Timers) [https://www.freedesktop.org/software/systemd/man/systemd.timer.html](https://www.freedesktop.org/software/systemd/man/systemd.timer.html) [https://www.freedesktop.org/software/systemd/man/systemd.s.html](https://www.freedesktop.org/software/systemd/man/systemd.special.html) [https://www.freedesktop.org/software/systemd/man/systemd.u.html](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) [https://doc.opensuse.org/documentation/leap/archive/42.2/reference/html/book.opensuse.reference/cha.systemd.html](https://doc.opensuse.org/documentation/leap/archive/42.2/reference/html/book.opensuse.reference/cha.systemd.html) [https://wiki.archlinux.org/index.php/Systemd\_(Espa%C3%B1ol)/Timers\_(Espa%C3%B1ol)](https://wiki.archlinux.org/index.php/Systemd_\(Espa%C3%B1ol\)/Timers_\(Espa%C3%B1ol\))
