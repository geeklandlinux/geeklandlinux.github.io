---
title: "Cómo activar TRIM de forma correcta en Linux: fstrim, LVM y dm-crypt"
date: 2019-02-24
categories: 
  - "linux-2"
tags: 
  - "disco-duro"
  - "trim"
coverImage: "activar-trim-correctamente-linux.png"
---

En pasados artículos vimos [la utilidad de TRIM]({{< relref "/posts/trim-debemos-activarlo-ssd" >}}) en las unidades de almacenamiento SSD. A continuación veremos como podemos activar TRIM de forma correcta en cualquier sistema operativo GNU-Linux.<!--more-->

## REQUISITOS PARA PODER ACTIVAR TRIM

Para activar TRIM deberemos cumplir con las siguientes condiciones:

1. El **sistema operativo tiene que soportar TRIM**. Todas los sistemas operativos a partir de Windows 7 y las distros linux con un kernel igual o posterior al 2.6.28 soportan TRIM.
2. Disponer de **una unidad SSD con un firmware que soporte TRIM**. La totalidad de unidades SSD actuales tienen pleno soporte.
3. El sistema de archivos de vuestro equipo tiene que ser **btrfs, ext3, ext4 gfs2m jfs, vfat, f2fs, ntfs, ocfs2 o xfs**.

Hoy en día prácticamente todos los equipos y sistemas operativos son aptos para usar TRIM.

###### Nota: Puede usarse el comando lsblk --discard para comprobar si nuestra unidad ssd soporta TRIM.

## ¿CÓMO ACTIVAR TRIM DE FORMA CORRECTA EN LINUX?

Existen dos modos de activar TRIM. Los modos son los siguientes:

1. **TRIM Continuo:** En modo continuo ejecutará TRIM cada vez que borremos un archivo de nuestra unidad de almacenamiento.
2. **TRIM periódico:** Se aplicará TRIM de forma periódica y automática en la fecha y hora que especifiquemos. Normalmente es suficiente ejecutar TRIM una vez por semana.

### ¿Hay que usar TRIM en modo continuo o en modo periódico?

En mi caso recomiendo activar TRIM de forma periódica. Activar TRIM en modo continuo genera los siguientes inconvenientes:

1. **Reducción de la vida útil de la unidad de almacenamiento**. TRIM en modo continuo implica lecturas y escrituras adicionales a nuestra unidad de almacenamiento.
2. **Rendimiento inferior al esperado**. En determinados casos se pueden producir congelaciones y corrupción del sistema de archivos. Por ejemplo, en mi caso de forma aleatoria se me congela Chrome durante segundos.
3. **Imposibilidad de recuperar archivos borrados**. Si borramos un fichero por equivocación nos será prácticamente imposible recuperarlo.

Por lo tanto, en este artículo aprenderemos a configurar y activar TRIM para que únicamente se ejecute de forma periódica.

Al usar TRIM en modo periódico únicamente recomiendo no andar justos de capacidad de almacenamiento en nuestro SSD.

### Desactivar el TRIM continuo en el caso que esté habilitado

**Grandes distribuciones** como por ejemplo Ubuntu, Manjaro y Debian **habilitan TRIM continuo por defecto**. Para desactivar el TRIM continuo abran una terminal y ejecuten el siguiente comando:

> ```
> sudo nano /etc/fstab
> ```

Cuando se abra el editor de textos comprueben si las entradas del fichero fstab contienen el término discard. En mi caso las entradas son las siguientes:

| \# <file system>                                                       <mount point> <type> <options>                     <dump> <pass> UUID=d4fbbf2b-9fec-45f1-a91f-1eb0970558dd    /boot               ext4      defaults,noatime,discard   0            2 UUID=bb5e07f9-4b5c-4a18-ad20-0dd94fa5369f   /                      ext4      defaults,noatime,discard   0            1 UUID=864d1126-2867-4902-8f4d-5974a43e1279 /home             ext4      defaults,noatime,discard   0            2 UUID=82d8bd4f-4700-421d-a421-049b4664bf7a  swap              swap     defaults,noatime,discard   0            2 tmpfs /tmp tmpfs defaults,noatime,mode=1777 0 0 UUID=8626904F269041DB /media/DATOS ntfs-3g default\_permissions,uid=1000 0 0 |
| :-- |

Tal y como pueden ver aparece el término discard. Por lo tanto tengo TRIM continuo habilitado. Para deshabilitarlo tan solo hay que borrar el término discard de cada una de las entradas. Por lo tanto las entradas de fstab tienen que ser las siguientes:

| \# <file system>                                                        <mount point> <type> <options>              <dump> <pass> UUID=d4fbbf2b-9fec-45f1-a91f-1eb0970558dd     /boot                ext4      defaults,noatime    0            2 UUID=bb5e07f9-4b5c-4a18-ad20-0dd94fa5369f    /                       ext4      defaults,noatime    0            1 UUID=864d1126-2867-4902-8f4d-5974a43e1279  /home              ext4      defaults,noatime    0            2 UUID=82d8bd4f-4700-421d-a421-049b4664bf7a   swap               swap     defaults,noatime    0            2 tmpfs /tmp tmpfs defaults,noatime,mode=1777 0 0 UUID=8626904F269041DB /media/DATOS ntfs-3g default\_permissions,uid=1000 0 0 |
| :-- |

###### Nota: Es importante que la opción noatime (no last access time) este presente en las entradas de fstab. De este modo debería mejorar el rendimiento de nuestro SSD. Con la opción noatime no se realizará ninguna escritura a la unidad SSD para registrar la fecha de último acceso a un fichero.

Una vez modificado el contenido del fichero fstab guardamos los cambios, cerramos el fichero y reiniciamos nuestro equipo. De este modo, TRIM ya no se aplicará de forma continua.

### Activar TRIM periódico en Linux usando systemd

Para poder usar fstrim tenemos que asegurar que el paquete util-linux esté instalado. Para ello ejecutamos el siguiente comando en la terminal:

> ```
> sudo apt install util-linux
> ```

###### Nota: En el caso que no usen el gestor de paquete apt deberán modificar el comando que acabamos de ejecutar.

En el caso que sean usuarios de Debian 8 y 9 ejecuten el siguiente comando para crear los servicios de Systemd:

> ```
> sudo cp /usr/share/doc/util-linux/examples/fstrim.{service,timer} /etc/systemd/system
> ```

A continuación, todos los usuarios de cualquier distro deberán ejecutar el siguiente comando para habilitar TRIM:

> ```
> sudo systemctl enable fstrim.timer
> ```

Acto seguido ejecutaremos el siguiente comando para iniciar el servicio:

> ```
> sudo systemctl start fstrim.timer
> ```

En estos momento el soporte TRIM debería estar habilitado. Para comprobar que está habilitado tan solo tenemos que ejecutar el siguiente comando:

> ```
> sudo systemctl status fstrim.timer
> ```

El resultado obtenido en mi caso es el siguiente:

> ```
> ● fstrim.timer - Discard unused blocks once a week
> Loaded: loaded (/lib/systemd/system/fstrim.timer; enabled; vendor preset: enabled)
> Active: active (waiting) since Fri 2019-02-22 19:47:05 CET; 22min ago
> Trigger: Mon 2019-02-25 00:00:00 CET; 2 days left
> Docs: man:fstrim
> ```

Si analizamos la salida del comando vemos la palabras active y waiting. Por lo tanto TRIM está habilitado. Otra información que podremos encontrar es la siguiente:

1. Se ejecutará TRIM una vez por semana en todas las particiones montadas de nuestra unidad de almacenamiento SSD.
2. Se aplicará todos los Lunes a las 00:00 horas.
3. La hora en que se ha levantado el servicio fstrim.

###### Nota: La gran mayoría de distribuciones Linux actuales utilizan systemd.

###### Nota: Si aplicamos TRIM de forma periódica es recomendable no andar justos de espacio de almacenamiento.

### Forzar la ejecución de TRIM

Si durante 2 o 3 días se han escrito o borrado una gran cantidad de datos en el SSD puede ser conveniente forzar un TRIM antes de la fecha programada. Para ello tan solo tenemos que abrir una terminal y ejecutaremos el siguiente comando:

> ```
> sudo systemctl enable fstrim.timer --now
> ```

Si lo prefieren también pueden ejecutar el siguiente comando:

> ```
> sudo fstrim -v -a
> ```

El ejecutar estos comandos se aplicará TRIM a la totalidad de sistemas de ficheros que tengamos montados.

Sí únicamente queremos aplicar TRIM en una de las particiones que tengamos montadas podemos ejecutar un comando del siguiente tipo:

> ```
> sudo fstrim -v partición_en_la_que_queremos_aplicar_trim
> ```

Por lo tanto en función de la partición que queramos aplicar TRIM ejecutaremos los siguientes comandos:

| **Partición** | **Comando** |
| :-- | :-- |
| root | sudo fstrim -v / |
| home | sudo fstrim -v /home |
| boot | sudo fstrim -v /boot |

De este modo se iniciará el proceso para que la controladora de nuestra unidad SSD conozca la totalidad de bloques de la unidad de almacenamiento que pueden ser borrados.

###### Nota: Forzar la ejecución de TRIM con la opción -v permite comprobar que no hayan errores durante el proceso.

### Activar TRIM en volúmenes LVM

Para activar TRIM en volúmenes LVM deberemos ejecutar el siguiente comando en la terminal:

> ```
> sudo nano /etc/lvm/lvm.conf
> ```

Cuando se abra el editor de textos cambiaremos el valor issue\_discards de 0 a 1. De tal forma que quede del siguiente modo:

> ```
> # [...]
> devices {
>    # [...]
>    issue_discards = 1
>    # [...]
> }
> # [...]
> ```

Una vez aplicados los cambios los guardamos y reiniciamos el ordenador.

### Activar TRIM en particiones cifradas con dm-crypt

Para habilitar TRIM en particiones cifradas realizaremos lo siguiente. Abriremos una terminal y ejecutaremos el siguiente comando:

> ```
> sudo nano /etc/crypttab
> ```

Cuando se abra el editor de textos añadiremos la opción discard en el apartado <options>.

| <target name>   <source device> <key file>  <options> sda3\_crypt         /dev/sda3             none          luks,discard |
| :-- |

Una vez realizadas las modificaciones guardamos los cambios y cerramos el fichero.

Finalmente actualizaremos la configuración de arranque del sistema ejecutando el siguiente comando en la terminal:

> ```
> sudo update-initramfs -u -k all
> ```

###### Nota: Activar TRIM en particiones cifradas tiene [implicaciones de seguridad](https://asalor.blogspot.com/2011/08/trim-dm-crypt-problems.html "Explicación de los problemas de seguridad que implica activar TRIM en una partición cifrada").

### Ver la periodicidad con que se ejecuta TRIM

Para ver la periodicidad con que se ejecutará TRIM tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> cat /etc/systemd/system/timers.target.wants/fstrim.timer
> ```

En mi caso el resultado obtenido es el siguiente:

> ```
> [Unit]
> Description=Discard unused blocks once a week
> Documentation=man:fstrim
> 
> [Timer]
> OnCalendar=weekly
> AccuracySec=1h
> Persistent=true
> 
> [Install]
> WantedBy=timers.target
> ```

Analizando la salida del comando podemos observar lo siguiente:

1. TRIM se ejecuta una vez a la semana. La frecuencia es más que correcta y no recomiendo que se modifique. Si quisierais cambiar la frecuencia tan solo tendríamos que reemplazar weekly por otro valor como por ejemplo Mon,Tue \*-\*-01..04 12:00:00. Está ejemplo ejecutaría TRIM los 4 primeros días del mes siempre y cuando sean Lunes o Martes a las 12 del mediodía.
2. La precisión temporal con que se ejecutará la tarea TRIM será de ± 1 hora.
3. El valor de persistencia está fijado en True. De este modo, aunque el ordenador no esté abierto a la hora que se tiene que ejecutar, TRIM se ejecutará al abrirlo.

###### Nota: Para ver la totalidad de valores que podemos introducir en el fichero de configuración podemos ejecutar el comando man systemd.timer.

### Comprobar la última vez que se ejecuto TRIM

Para comprobar la última vez que se aplico TRIM ejecutamos el siguiente comando en la terminal:

> ```
> systemctl list-timers fstrim.timer --all
> ```

El resultado obtenido en mi caso es el siguiente:

> ```
> NEXT LEFT LAST PASSED UNIT ACTIVATES
> Mon 2019-02-18 00:00:00 CET 1 day 13h left Mon 2019-02-11 08:39:42 CET 5 days ago fstrim.timer fstrim.service
> ```

Si analizamos la salida del comando vemos lo siguiente:

1. Trim se ejecutará el próximo Lunes a las 00:00 horas.
2. Falta 1 día y 13 ahora para que se ejecute Trim.
3. La última vez en que se ejecuto fue el pasado Lunes a la 08:39:42h. No se ejecuto a las 00:00 porque en este momento el ordenador estaba apagado. Por lo tanto cuando TRIM se ejecuto cuando abrí el ordenador

### Consultar los logs de TRIM

Si queremos consultar los logs de TRIM podemos ejecutar el siguiente comando en la terminal:

> ```
> sudo journalctl -u fstrim.timer
> ```

Cada vez que reiniciemos el ordenador o servidor se borrará el log de fstrim.timer. Si queréis que el log sea persistente tendréis que realizar lo siguiente.

Inicialmente crearemos la carpeta que almacenará los logs ejecutando el siguiente comando en la terminal:

> ```
> sudo -p mkdir /var/log/journal
> ```

Acto seguido indicamos que los logs del espacio volátil /run/log/journal/ se almacenen en /var/log/journal ejecutando el siguiente comando:

> ```
> sudo systemd-tmpfiles --create --prefix /var/log/journal
> ```

Finalmente reiniciamos el servicio systemd-journald ejecutando el siguiente comando:

> ```
> sudo systemctl restart systemd-journald
> ```

###### Nota: Aplicando la persistencia de logs implicará que estaremos duplicando logs.

###### FUENTES

[https://wiki.debian.org/SSDOptimization](https://wiki.debian.org/SSDOptimization)

[https://wiki.archlinux.org/index.php/Solid\_state\_drive](https://wiki.archlinux.org/index.php/Solid_state_drive)
