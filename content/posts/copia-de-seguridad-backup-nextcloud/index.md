---
title: "Crear una copia de seguridad o Backup de la nube Nextcloud"
date: 2018-08-12
categories: 
  - "linux-2"
  - "raspberry-pi"
tags: 
  - "backup"
  - "mysql"
  - "nextcloud"
  - "raspbian"
cover:
  image: "images/crear-copia-de-seguridad-de-nextcloud.png"
  relative: true
---

A todos las personas que les toque administrar una nube Nextcloud deberían realizar una copia de seguridad para prevenir la pérdida de información.

Hay muchos sistemas y métodos para realizar copias de seguridad. En mi caso utilizo los siguientes 2 métodos:<!--more-->

1. Clonar Nextcloud a una nube pública usando Rclone. En lo que a privacidad se refiere no es la mejor de las opciones.
2. Realizar una copia de seguridad convencional mediante rsync o tar.

En este artículo realizaremos una copia de seguridad convencional mediante tar. En futuros artículos veremos como podemos usar Rclone para sincronizar los datos almacenados en Nextcloud con una nube pública.

###### Nota: El tutorial se puede aplicar a la totalidad de nubes Nextcloud que corren sobre un servidor de base de datos MariaDB o Mysql.

## ELEMENTOS QUE TENEMOS QUE INCLUIR EN LA COPIA DE SEGURIDAD

En mi caso siempre incluyo los siguientes elementos:

1. La base de datos de Nextcloud.
2. La totalidad de datos almacenados en la nube.
3. Los archivos de configuración de Nextcloud.

Para realizar la copia de seguridad de estos 3 elementos procederemos del siguiente modo.

## TIPO DE COPIA DE SEGURIDAD QUE CREAREMOS

Con sus ventajas y sus inconvenientes, las copias de seguridad que realizaremos son completas. Quien quiera trabajar con copias incrementales o diferenciales deberá modificar los comandos del siguiente artículo.

Para evitar que se llene el disco duro que borraremos las copias de seguridad con una antigüedad superior a 3 días.

## PARÁMETROS QUE NECESITAMOS CONOCER PARA CREAR LA COPIA DE SEGURIDAD

Para realizar la copia de seguridad de forma satisfactoria hay que conocer y tener claros los siguientes datos:

| **Parámetros** | **Valores a conocer** |
| :-- | :-- |
| Ruta de los archivos de configuración de Nextcloud. | /var/www/html/nextcloud |
| Ruta que almacena los datos de Nextcloud. | /media/nextcloud/data |
| Dirección en se almacenan los archivos de configuración. | /var/www/html/nextcloud |
| La dirección IP en que está funcionando Mysql o MariaDB. | localhost (Porque los comandos que ejecutaremos en el servidor que está corriendo MariaDB o Mysql) |
| Un usuario con privilegios de administración de la base de datos. | root |
| La contraseña del administrador de la base de datos. | mi\_contraseña |
| Nombre de la base de datos que queremos respaldar. | nextcloud |
| Ubicación donde queremos guardar las copias de seguridad. | La base de datos: media/usb/backups/nextcloud/database  Los datos de nuestra nube: media/usb/backups/nextcloud/data  Los archivos de configuración: media/usb/backups/nextcloud/ncserver   |
| Nombre que queremos dar a las copias de seguridad. | La base de datos: nextcloud-sqlbkp\_\`date +"%Y%m%d"\`.bak  Los datos de nuestra nube: nextcloud-databkp\_\`date +"%Y%m%d"\`.tar.gz  Los archivos de configuración: ncserver-databkp\_\`date +"%Y%m%d"\`.tar.gz   |

###### Nota: Las rutas mencionadas en la tabla serán distintas y en función de cada instalación de Nextcloud.

## ACTIVAR EL MODO MANTENIMIENTO PARA REALIZAR LA COPIA DE SEGURIDAD

Activaremos el modo mantenimiento para asegurar que ningún usuario está o accede a la nube durante la copia de seguridad. De esta forma evitaremos inconsistencias y problemas en las copias de seguridad que vamos a crear.

Para activar el modo mantenimiento tenemos que ejecutar el siguiente comando en la terminal:

> ```
> sudo -u www-data php /var/www/html/nextcloud/occ maintenance:mode --on
> ```

## REALIZAR UNA COPIA DE SEGURIDAD DE LA BASE DE DATOS DE NEXTCLOUD

Una vez activado el modo mantenimiento ya podemos realizar la copia de seguridad de nuestra base de datos. Para ello tenemos que ejecutar un comando del siguiente tipo en la terminal:

> ```
> mysqldump --single-transaction -h ip_servidor_mysql -u usuario_administrador_base_de_datos -p'contraseña_usuario_administrador_base_de_datos' nombre_base_de_datos_nextcloud > ruta_y_nombre_donde_crear_la_copia_base_de_datos
> ```

Por lo tanto, en mi caso deberé ejecutar el siguiente comando como usuario root:

> ```
> mysqldump --single-transaction -h localhost -u root -p'mi_contraseña' nextcloud > /media/usb/backups/nextcloud/database/nextcloud-sqlbkp_`date +"%Y%m%d"`.bak
> ```

Acto seguido se realizará la copia de seguridad de nuestra base de datos.

## COPIA DE SEGURIDAD DE LOS DATOS ALMACENADOS EN NEXTCLOUD

En mi caso tengo que copiar los datos almacenados en /media/nextcloud/data en el archivo comprimido nextcloud-databkp\_\`date +"%Y%m%d"\`.tar.gz que ubicaré en /media/usb/backups/nextcloud/data.

Para ello, como usuario root ejecutaré el siguiente comando en la terminal:

> ```
> tar -cpzf /media/usb/backups/nextcloud/data/nextcloud-databkp_`date +"%Y%m%d"`.tar.gz /media/nextcloud/data
> ```

Una vez ejecutado empezará a realizarse la copia de seguridad de nuestros datos. Una vez finalizada, la ubicación /media/usb/backups/nextcloud/data/ contendrá una archivo comprimido con la totalidad de nuestros datos.

## COPIA DE SEGURIDAD DE LOS ARCHIVOS DE CONFIGURACIÓN DE NEXTCLOUD

Crearemos un archivo comprimido con el nombre ncserver-databkp\_\`date +"%Y%m%d"\`.tar.gz que ubicaremos en /media/usb/backups/nextcloud/ncserver. Este archivo contendrá la totalidad de archivos de configuración de Nextcloud ubicados en /var/www/html/nextcloud.

Para ello, como usuario root, ejecutaré el siguiente comando en la terminal:

> ```
> tar -cpzf /media/usb/backups/nextcloud/ncserver/ncserver-databkp_`date +"%Y%m%d"`.tar.gz /var/www/html/nextcloud
> ```

Una vez ejecutado el comando se realizará una copia de seguridad de la totalidad de archivos de configuración de Nextcloud.

## DESACTIVAR EL MODO MANTENIMIENTO

A estas alturas ha finalizado el proceso de creación de la copia de seguridad. Por lo tanto desactivaremos el modo mantenimiento ejecutando el siguiente comando en la terminal:

> ```
> sudo -u www-data php /var/www/html/nextcloud/occ maintenance:mode --off
> ```

Una vez desactivado el modo mantenimiento, los usuarios podrán acceder de nuevo a la nube sin ningún tipo de problema.

## BORRAR LAS COPIAS DE SEGURIDAD CON UNA ANTIGÜEDAD SUPERIOR A 3 DÍAS

Para borrar las copias de seguridad con una antigüedad superior a 3 días tan solo tenemos que ejecutar los siguientes comandos:

> ```
> sudo find /media/usb/backups/nextcloud/database -mtime +3 -exec rm {} \;
> 
> sudo find /media/usb/backups/nextcloud/ncserver -mtime +3 -exec rm {} \;
> 
> sudo find /media/usb/backups/nextcloud/data -mtime +3 -exec rm {} \;
> ```

###### Nota: Las partes de color rojo corresponden a cada una de las rutas que almacenan nuestras copias de seguridad.

###### Nota: Las partes de color verde indican el tiempo máximo en días que queremos almacenar una copia de seguridad.

## AUTOMATIZAR LAS COPIAS DE SEGURIDAD DE NEXTCLOUD

Automatizar todo el proceso que acabamos de ver es extremadamente sencillo. Tan solo tenemos que crear un script del siguiente modo.

### Crear un script para realizar copias de seguridad de Nextcloud

Inicialmente crearemos una carpeta con el nombre scripts que almacenará el script. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> mkdir scripts
> ```

Seguidamente accederemos dentro de la carpeta que acabamos de crear ejecutando el siguiente comando en la terminal:

> ```
> cd scripts
> ```

A continuación crearemos el script ejecutando el siguiente comando en la terminal:

> ```
> nano nextcloudbkp.sh
> ```

Cuando se abra el editor de texto nano pegaremos el siguiente contenido:

> ```
> #!/bin/sh
> # Copia de seguridad de nextcloud
> # Activar el modo mantenimiento
> sudo -u www-data php /var/www/html/nextcloud/occ maintenance:mode --on
> # Copiar la base de datos
> mysqldump --single-transaction -h localhost -u root -p'mi_contraseña' nextcloud > /media/usb/backups/nextcloud/database/nextcloud-sqlbkp_`date +"%Y%m%d"`.bak
> # Copia de seguridad de los archivos de configuración
> tar -cpzf /media/usb/backups/nextcloud/ncserver/ncserver-databkp_`date +"%Y%m%d"`.tar.gz /var/www/html/nextcloud
> # Copia de seguridad de los datos de Nextcloud
> tar -cpzf /media/usb/backups/nextcloud/data/nextcloud-databkp_`date +"%Y%m%d"`.tar.gz /media/nextcloud/data
> # Desactivar el modo mantenimiento
> sudo -u www-data php /var/www/html/nextcloud/occ maintenance:mode --off
> # Borrar las copias de seguridad más antiguas de 3 días
> sudo find /media/usb/backups/nextcloud/database -mtime +3 -exec rm {} \;
> sudo find /media/usb/backups/nextcloud/ncserver -mtime +3 -exec rm {} \;
> sudo find /media/usb/backups/nextcloud/data -mtime +3 -exec rm {} \;
> ```

A continuación guardaremos los cambios y cerraremos el fichero. Finalmente tan solo tenemos que dar permisos de ejecución al script ejecutando el siguiente comando en la terminal:

> ```
> sudo chmod +x nextcloudbkp.sh
> ```

### Programar la ejecución de la copia de seguridad

El último paso consiste en programar la frecuencia con que se realizarán las copias de seguridad usando el programador de tareas cron.

Para programar la frecuencia con que se realizará la copia de seguridad ejecutamos el siguiente comando en la terminal:

> ```
> crontab -e
> ```

Cuando se abra el editor de textos pegamos el siguiente comando al final del archivo:

> ```
> 03 03 * * * sudo /home/pi/scripts/nextcloudbkp.sh >/dev/null 2>&1
> ```

Finalmente guardamos los cambios y cerramos el fichero.

De este modo todos los días a las 3 horas y 3 minutos de la madrugada se ejecutará el script que realizará la copia de seguridad de Nextcloud. Si quieren modificar la frecuencia de ejecución de la copia de seguridad tan solo tienen que modificar los parámetros de Cron.

## RESTAURAR LA COPIA DE SEGURIDAD QUE ACABAMOS DE CREAR

En futuros artículos detallaremos de forma sencilla como podemos restaurar la copia de seguridad que hemos creado en este artículo.
