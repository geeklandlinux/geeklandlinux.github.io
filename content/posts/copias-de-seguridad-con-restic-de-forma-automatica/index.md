---
title: "Como hacer copias de seguridad cifradas con Restic de forma automática"
date: 2020-11-14
categories: 
  - "seguridad-informatica"
tags: 
  - "backup"
  - "restic"
  - "seguridad"
  - "software-libre"
coverImage: "montar-un-sistema-de-copias-de-seguridad-con-restic.jpg"
---

El objetivo del siguiente artículo es mostrar el sistema que uso para realizar mis copias de seguridad. Verán que una vez implementados todos los pasos podrán realizar las copias de seguridad cifradas o en la nube con el software Restic de forma completamente automática y solo tendrán que preocuparse de comprobar que las copias de seguridad puedan restaurarse correctamente.<!--more-->

## ¿POR QUÉ RESTIC ES UNA BUENA OPCIÓN PARA REALIZAR COPIAS DE SEGURIDAD?

En mi caso he decidido realizar mis copias de seguridad con el software Restic por los siguientes motivos:

1. Es una **solución multiplaforma.** Se puede instalar y usar en prácticamente todos los sistemas operativos incluyendo Linux, Windows, MacOS, FreeBSD, OpenBSD, Solus, etc.
2. Permite guardar **copias de seguridad cifradas** locales y en la nube. El algoritmo de cifrado usado es el AES256-Poly1305-AES.
3. Las **copias de seguridad cifradas se pueden realizar en un medio de almacenamiento local o remoto**. Por lo tanto podemos almacenar copias de seguridad en un equipo local o en servidor remoto sftp, Microsoft Azure, AWS S3, BackBlaze B2, Google cloud, OpenStack Swift, etc. Además Restic **se puede usar conjuntamente con rclone**, por lo tanto podemos [subir nuestras copias de respaldo a multitud de nubes](https://restic.net/blog/2018-04-01/rclone-backend/).
4. **Restic realiza copias de seguridad completas, pero a la práctica son incrementales**. Cada una de las copias de seguridad realizadas es independiente de la otra. No obstante implementa un sistema de de-duplicación que hará que no se almacene contenido duplicado en disco. Si tenemos una copia seguridad y realizamos una segunda copia, el contenido común entre la primera y la segunda copia de seguridad solo se almacenará una vez en disco. De esta forma podremos realizar copias de seguridad sin necesidad de disponer de gran cantidad de almacenamiento.
5. **Permite visualizar los datos almacenados en la copia de seguridad en nuestro gestor de archivos** sin necesidad de tener que restaurarlos. De esta forma podemos navegar tranquilamente por el contenido de nuestra copia de seguridad y restaurar solo los ficheros que deseemos de forma sencilla y sin tener que usar prácticamente comandos.
6. El **proceso de restauración de una copia de seguridad es rápido y sencillo**. Mediante un simple comando podremos restaurar la copias de seguridad.
7. Es una herramienta de línea de comandos. Por lo tanto **permite automatizar las copias de seguridad incluso en servidores sin entorno gráfico**.
8. Tiene una [**documentación excelente**](https://restic.readthedocs.io/en/latest/manual_rest.html). Si la leéis podréis implementar fácilmente un sistema de copias de seguridad rápido, eficiente y seguro.
9. Dispone de **herramientas para comprobar que las copias de seguridad almacenadas están en buen estado**.
10. **Podemos establecer una política para la eliminación de snapshots antiguos**. De este modo podremos automatizar el proceso de copia de seguridad sin tener que preocuparnos de eliminar las copias de seguridad antiguas.

## TIPO DE COPIAS DE SEGURIDAD QUE REALIZARÉ EN MI CASO

En el ejemplo que veremos a continuación automatizaremos el proceso de copia de seguridad de toda la información de mi Raspberry Pi a un servidor VPS remoto que uso para almacenar las copias de seguridad. Las copias de seguridad que realizaremos serán completas y cifradas.

La representación esquemática de mi estrategia de copias de seguridad con Restic es la siguiente:

[![Esquema de mi sistema de copias de seguridad](images/esquema-copia-seguridad-con-restic.png "Esquema de mi sistema de copias de seguridad")](images/esquema-copia-seguridad-con-restic.png)

**Nota:** La parte dentro del recuadro rojo corresponde a la copia de seguridad de mi Raspberry Pi.

Por lo tanto en mi caso tengo un servidor remoto de copias de seguridad que es el que almacenará las copias de seguridad de todos mis equipos.

## INSTALAR EL SOFTWARE RESTIC PARA REALIZAR LAS COPIAS DE SEGURIDAD

En mi caso y para facilitar la tarea recomiendo instalar Restic a través de los repositorios de su distribución. Para ello en mi caso opero de la siguiente forma

### Instalar Restic en el equipo que contiene el contenido a respaldar (Raspberry Pi)

En el equipo que contiene la información a respaldar, que en mi caso es una Raspberry Pi, ejecutamos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo apt install restic
> ```

**Nota**: Para instalar restic en otros sistemas operativos que usen un gestor de paquetes diferente a apt [sigan las intrucciones del siguiente enlace](https://restic.readthedocs.io/en/latest/020_installation.html).

### Instalar Restic en el equipo que almacenará la copia de seguridad (Servidor VPS)

En el ordenador que almacenará las copias de seguridad, que en mi caso es un Ubuntu Server 20.04, ejecutamos el siguiente comando:

> ```shell
> ubuntu@opvpn-webdav-server:~$ sudo apt install restic
> ```

Si quieren instalar Restic en otros sistemas operativos que no usan el gestor de paquetes deb pueden seguir las instrucciones del siguiente [enlace](https://restic.readthedocs.io/en/latest/020_installation.html).

## ACTUALIZAR EL SOFTWARE RESTIC A LA ÚLTIMA VERSIÓN

Hemos instalado Restic desde los repositorios de nuestra distribución. Por lo tanto no estaremos usando la última versión de Restic. Si quieren usar la última versión en el equipo que contiene la información a respaldar ejecuten el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ sudo restic self-update
> writing restic to /usr/bin/restic
> find latest release of restic at GitHub
> latest version is 0.10.0
> download SHA256SUMS
> download SHA256SUMS.asc
> GPG signature verification succeeded
> download restic_0.10.0_linux_arm.bz2
> downloaded restic_0.10.0_linux_arm.bz2
> saved 15138816 bytes in /usr/bin/restic
> successfully updated restic to version 0.10.0
> ```

Para actualizar Restic en el servidor que almacenará la copia de seguridad deberán ejecutar el mismo comando que acabamos de ejecutar:

> ```shell
> ubuntu@opvpn-webdav-server:~$ sudo restic self-update
> writing restic to /usr/bin/restic
> find latest release of restic at GitHub
> latest version is 0.10.0
> download SHA256SUMS
> download SHA256SUMS.asc
> GPG signature verification succeeded
> download restic_0.10.0_linux_amd64.bz2
> downloaded restic_0.10.0_linux_amd64.bz2
> saved 18378752 bytes in /usr/bin/restic
> successfully updated restic to version 0.10.0
> ```

De esta forma rápida y simple ya tenemos la última versión de Restic disponible en todos nuestros equipos.

## CREAR UN REPOSITORIO DE RESTIC PARA ALMACENAR LAS COPIAS DE SEGURIDAD

A continuación, en el servidor VPS que almacenará las copias de seguridad crearé un directorio que almacenará las copias de seguridad de mi Raspberry Pi. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> ubuntu@opvpn-webdav-server:~$ mkdir /home/ubuntu/rpi4_bk
> ```

Ahora sobre el directorio `/home/ubuntu/rpi4_bk/` tenemos que crear un repositorio de Restic. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ init
> enter password for new repository: mi_contraseña
> enter password again: mi_contraseña
> created restic repository ee979ca4cb at /home/ubuntu/rpi4_bk/
> 
> Please note that knowledge of your password is required to access
> the repository. Losing your password means that your data is
> irrecoverably lost.
> ```

**Nota**: Durante la creación del repositorio tendremos que definir una contraseña. Esta contraseña la debéis recordar y además tiene que ser una contraseña robusta. Necesitaremos introducir la contraseña en cualquier operación que implique modificar o consultar los datos almacenados en la copia de seguridad. Por lo tanto necesitaremos introducir la contraseña al realizar, restaurar, borrar y consultar la copia de seguridad.

Si necesitan más información pueden visitar el siguiente enlace en el que se explica de forma más profunda el proceso de [crear un repositorio](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html).

## CONFIGURAR RESTIC PARA QUE SEA CAPAZ DE LEER Y RESPALDAR LA TOTALIDAD DE FICHEROS DE NUESTRO EQUIPO

Si ejecutamos Restic con nuestro usuario tendremos el problema que no podremos leer ni respaldar ficheros y directorios cuyo propietario sea `root`.

Una solución para evitar el problema que acabamos de citar es ejecutar Restic como usuario root. La otra es crear un nuevo usuario sin privilegios de administrador que sea capaz de respaldar la totalidad de ficheros de nuestro equipo.

### Crear un nuevo usuario sin privilegios de administrador para crear las copias de seguridad

Lo primero que tenemos que hacer es crear un nuevo usuario llamado `restic` ejecutando el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo useradd -m restic
> ```

### Definir un password para el nuevo usuario

A continuación definiremos un password para el usuario `restic` que acabamos de crear. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo passwd restic
> Nueva contraseña: mi_contraseña
> Vuelva a escribir la nueva contraseña: mi_contraseña
> passwd: contraseña actualizada correctamente
> ```

### Modificar las propiedades del fichero binario de Restic

Acto seguido cambiaremos los permisos y propiedades del binario de restic para que únicamente pueda ser ejecutado por los usuarios `root` y `restic`. Para ello ejecutaremos los siguientes comandos en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo chown root:restic /usr/bin/restic
> pi@raspberrypi:~ $ sudo chmod 750 /usr/bin/restic
> ```

Finalmente mediante `setcap` y la propiedad `cap_dac_read_search=+ep` haremos que cuando se ejecute el binario de Restic no se realice ningún tipo de comprobación de si tenemos permisos de lectura para respaldar un fichero o directorio. Para conseguir este propósito ejecutaremos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ sudo setcap cap_dac_read_search=+ep /usr/bin/restic
> ```

Fuente: [https://restic.readthedocs.io/en/latest/080\_examples.html#backing-up-your-system-without-running-restic-as-root](https://restic.readthedocs.io/en/latest/080_examples.html#backing-up-your-system-without-running-restic-as-root)

## HACER QUE EL USUARIO RESTIC TENGA ACCESO A LA UBICACIÓN REMOTA EN QUE SE ALMACENARÁN LAS COPIAS DE SEGURIDAD

Si pretenden realizar una copia remota vía ssh, el usuario `restic` que acabamos de crear tendrá que tener acceso al servidor remoto que almacenará las copias de seguridad. Para conseguir nuestro propósito nos loguearemos al usuario restic ejecutando el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ su restic
> Contraseña: mi_contraseña
> ```

Seguidamente ejecutaremos el siguiente comando para dirigirnos a la ubicación home de restic:

> **`restic@raspberrypi:/home/pi $ cd`**

A continuación crearemos el directorio `.ssh` que será el encargado de almacenar la configuración y las claves para podernos conectar al servidor que almacenará las copias de seguridad. Para ello ejecutaremos los siguientes comandos en la terminal:

> ```shell
> restic@raspberrypi:~ $ mkdir .ssh
> restic@raspberrypi:~ $ cd .ssh
> ```

Acto seguido crearemos el directorio que almacenará la clave privada que en mi caso usaré para autenticarme en el momento de conectarme al servidor remoto que almacenará las copias de seguridad:

> ```shell
> restic@raspberrypi:~/.ssh $ mkdir clave
> ```

Seguidamente accedemos dentro del directorio que acabamos de crear:

> ```shell
> restic@raspberrypi:~/.ssh $ cd clave
> ```

A continuación creamos el fichero `ubuntu_server` que es el que almacenará nuestra clave. Para ello ejecutamos el siguiente comando:

> ```shell
> restic@raspberrypi:~/.ssh/clave $ touch ubuntu_server
> ```

El siguiente paso consistirá en copiar la clave privada en el fichero que acabamos de crear. Para ello ejecutaremos el siguiente comando:

> ```shell
> restic@raspberrypi:~/.ssh/clave $ nano ubuntu_server
> ```

Cuando se abra el editor de textos copiaremos la clave que en mi caso es la siguiente:

```shell
-----BEGIN OPENSSH PRIVATE KEY-----
kfodsktgoretkpremgdfçpobmtqeçpmbgeapmgeqomgrefgFGGFDGDEwerfmew+pgorert
ipgmemg&$67568,6j64_J/u-p,h.DGGtegheogteopgtjogithjwoghtghoewoigregeh/
orewpt7787689GDFgjpirwnq.fswqpow788861461r23me290rewrewGFRWGREGRETRETR
porgepogrjeRFEREGR WV2o3xcmjgtrVHJTBKPGCGEWCOKXOEWXGErfxpqinfveronpotr
FOIRFWmewfmoidm09450439+44984rewcdrec,jecoirtgvmoirgjcov,rjovcjoir%jTY
kfodsktgoretkpremgdfçpobmtqeçpmbgeapmgeqomgrefgFGGFDGDEwerfmew+pgorert
ipgmemg&$67568,6j64_J/u-p,h.DGGtegheogteopgtjogithjwoghtghoewoigregeh/
orewpt7787689GDFgjpirwnq.fswqpow788861461r23me290rewrewGFRWGREGRETRETR
porgepogrjeRFEREGR WV2o3xcmjgtrVHJTBKPGCGEWCOKXOEWXGErfxpqinfveronpotr
FOIRFWmewfmoidm09450439+44984rewcdrec,jecoirtgvmoirgjcov,rjovcjoir%jTY
ykHHwemEq8lhsaSJYWKbgsWvTGLwPPXLONtoB7AAAAAwEAAQAAAQBBKnwYolkVrnEHurw7
5eYa7vGDQ5xwrZvDvcwWcP52ZoMFBn1JDS3o+87vy0KnW+oD3oQUPEo/sVz3bgIiouHPD3
yHsv0uOskJ+0ArDTgJl4NMRjGkaVGKbGNW8lv89BV2rEqmhPaDj08DwhiUo7MSztUx7CGd
5L6cIXUoYZB4tSsKs0CGNqL9fIQsk6on+VnD0n6dB7sP3M5WK9mM35veDR1q2ezyeMynvI
ksBWfkz9CSiAirhZ/GHweVbKfp1KDbnP6nk1BBQaxAjNXBvkcbcjywnmp+HHHOtkhtSV5C
kfodsktgoretkpremgdfçpobmtqeçpmbgeapmgeqomgrefgFGGFDGDEwerfmew+pgorert
ipgmemg&$67568,6j64_J/u-p,h.DGGtegheogteopgtjogithjwoghtghoewoigregeh/
orewpt7787689GDFgjpirwnq.fswqpow788861461r23me290rewrewGFRWGREGRETRETR
porgepogrjeRFEREGR WV2o3xcmjgtrVHJTBKPGCGEWCOKXOEWXGErfxpqinfveronpotr
EM8VdpBTFJAAAAgQDzi0bcrHkHTMTKJplXF61ePPgNmv/Xo3i4l0WPSfyiqXXQ4rwLjRv0
kfodsktgoretkpremgdfçpobmtqeçpmbgeapmgeqomgrefgFGGFDGDEwerfmew+pgorert
ipgmemg&$67568,6j64_J/u-p,h.DGGtegheogteopgtjogithjwoghtghoewoigregeh/
orewpt7787689GDFgjpirwnq.fswqpow788861461r23me290rewrewGFRWGREGRETRETR
porgepogrjeRFEREGR WV2o3xcmjgtrVHJTBKPGCGEWCOKXOEWXGErfxpqinfveronpotr
uPMrewrtewdPi2EAAb0AAAANdWJrewbnR1X3NlcnZlcgECAwQFBg==
-----END OPENSSH PRIVATE KEY-----
```

Una vez pegada la clave guardan los cambios y cierran el fichero.

Para poder usar la clave y además almacenarla en el equipo de forma segura cambiaremos su usuario, grupo y permisos ejecutando los siguientes comandos en la terminal:

> **`restic@raspberrypi:~/.ssh/clave $ chown restic:restic ubuntu_server restic@raspberrypi:~/.ssh/clave $ chmod 0400 ubuntu_server`**

A continuación nos dirigiremos a la ruta `/home/restic/.ssh` ejecutando el siguiente comando:

> ```shell
> restic@raspberrypi:~/.ssh/clave $ cd ..
> ```

Seguidamente crearemos y editaremos el fichero `config` para introducir la configuración necesaria para que el usuario restic se pueda conectar al servidor remoto que almacenará las copias de seguridad. Para ello ejecutaremos el siguiente comando en la terminal:

> **`restic@raspberrypi:~/.ssh $ nano config`**

Una vez se abra el editor de textos nano introduciremos la configuración pertinente para que se puedan conectar al servidor que almacenará las copias de seguridad:

> **`Host vps2      Hostname 88.333.444.107      IdentityFile /home/restic/.ssh/clave/ubuntu_server      IdentitiesOnly yes      User ubuntu      Port 22`**

Finalmente guardaremos los cambios y cerraremos el fichero. A partir de estos momentos el usuario restic podrá acceder al servidor que almacenará las copias de seguridad vía ssh y sin tener que introducir ningún tipo de contraseña.

**Nota:** Si no acaban de comprender lo que se realiza en este apartado les recomiendo visiten el siguiente enlace para [configurar el acceso a un servidor remoto mediante SSH]({{< relref "/posts/configurar-cliente-ssh-para-facilitar-la-conexion-remota-a-nuestros-equipos" >}}).

Fuente: [https://restic.readthedocs.io/en/latest/080\_examples.html#motivation](https://restic.readthedocs.io/en/latest/080_examples.html#motivation)

## DEFINIR EL CONTENIDO QUE QUEREMOS RESPALDAR EN LA COPIA DE SEGURIDAD

A continuación creamos 2 ficheros de texto que contendrán los ficheros y directorios que queremos respaldar. Para ello procederemos del siguiente modo:

### Definir los ficheros y directorios que queremos introducir en las copias de seguridad

En la ubicación `~/restic` crearemos el fichero `files_to_backup.txt` que detallará la totalidad de ficheros y directorios que queremos respaldar. Para ello ejecutaremos los siguientes comandos:

> ```shell
> restic@raspberrypi:~ $ cd /home/restic/restic
> restic@raspberrypi:~ $ mkdir restic
> restic@raspberrypi:~ $ cd restic
> restic@raspberrypi:~/restic $ touch files_to_backup.txt
> restic@raspberrypi:~/restic $ nano files_to_backup.txt
> ```

Una vez se abra el editor de texto detallaremos todos los directorios y ficheros que queremos respaldar. En mi caso el contenido que he copiado es el siguiente:

> ```shell
> /home/pi
> /media/nextcloud/twitter
> /media/nextcloud/podcast
> /media/nextcloud/services/jellyfin/
> ```

Una vez definido el contenido a respaldar guardamos los cambios y cerramos el fichero.

**Nota**: En cualquier momento podemos modificar el contenido del fichero `files_to_backup.txt` para añadir o sacar contenido de nuestra copia de seguridad.

### Definir los ficheros y directorios que queremos excluir en las copias de seguridad

De la misma forma que hemos definido los archivos y directorios que queremos incluir en la copia de seguridad también podemos excluir ficheros y directorios de la copia de seguridad. Para ello en la ubicación `~/restic` crearemos el fichero `files_to_exclude.txt` que detallará la totalidad de ficheros y directorios que no queremos respaldar. Para ello ejecutaremos los siguientes comandos:

> ```shell
> restic@raspberrypi:~ $ cd /home/restic/restic
> restic@raspberrypi:~/restic $ touch files_to_exclude.txt
> restic@raspberrypi:~/restic $~ nano files_to_exclude.txt
> ```

Cuando se abra el editor de texto nano definiremos los ficheros y directorios que no queremos respaldar. En mi caso los valores introducidos son:

> ```shell
> /home/pi/videos
> *.txt
> ```

Finalmente guardamos los cambios y cerramos el fichero. De este modo nuestra copia de seguridad contendrá:

1. La totalidad de ficheros y directorios de nuestra partición `/home/pi` excepto el directorio `/home/pi/videos`.
2. Los ficheros y directorios de las ubicaciones `/media/nextcloud/twitter`, `/media/nextcloud/podcast` y `/media/nextcloud/services/jellyfin/`
3. La copia no contendrá ningún archivo que tenga la extensión `.txt`.

## REALIZAR LA COPIA DE SEGURIDAD DE FORMA MANUAL CON RESTIC

Una vez finalizada la configuración ya podemos realizar la copia de seguridad. Para ello tan solo tenemos que ejecutar un comando del siguiente tipo:

> `**sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ backup --tag raspberry --tag docker -v --exclude-file=/home/restic/restic/files_to_exclude.txt --files-from=/home/restic/restic/files_to_backup.txt**`

El significado de todos y cada uno de los parámetros del comando es el siguiente:

- `sudo -u restic`: Como inicio la copia de seguridad con el usuario Pi definimos que el usuario restic es quien ejecutará copia de seguridad.
- `restic`: Invocamos el binario de restic para realizar la copia de seguridad.
- `-r sftp:vps2:/home/ubuntu/rpi4_bk/`: Indicamos la ubicación remota del repositorio que almacenará la copia de seguridad. En nuestro caso la copia de seguridad se almacenará en un servidor sftp con nombre vps2. El repositorio en que se almacenará la copia es `/home/ubuntu/rpi4_bk/`.
- `backup`: Para indicar a restic que queremos realizar una copia de seguridad.
- `--tag raspberry` : Añadimos la etiqueta `raspberry` a la copia de seguridad. Las etiquetas son útiles para por ejemplo aplicar acciones únicamente a los snapshots o copias de seguridad que contengan una determinada etiqueta.
- `--tag docker` : Añadimos la etiqueta docker a la copia de seguridad.
- `-v --exclude-file=/home/restic/restic/files_to_exclude.txt --files-from=/home/restic/restic/files_to_backup.txt`: Definimos los ficheros que queremos respaldar.

El resultados de aplicar el comando que acabamos de detallar es el siguiente:

> ```shell
> pi@raspberrypi:~ $ sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ backup --tag raspberry --tag docker -v --exclude-file=/home/restic/restic/files_to_exclude.txt --files-from=/home/restic/restic/files_to_backup.txt
> open repository
> enter password for repository: mi_contraseña
> repository ee979ca4 opened successfully, password is correct
> lock repository
> load index files
> start scan on [/home/pi /media/nextcloud/twitter /media/nextcloud/podcast /media/nextcloud/services/jellyfin]
> start backup on [/home/pi /media/nextcloud/twitter /media/nextcloud/podcast /media/nextcloud/services/jellyfin]
> scan finished in 3.478s: 17872 files, 1.728 GiB
> [0:35] 14.74%  3592 files 260.779 MiB, total 17872 files 1.728 GiB, 0 errors ETA 3:25
> /media/nextcloud/services/jellyfin/config/cache/images/resized-images/7/7d3f157f-f229[0:35] 14.75%  3593 files 260.968 MiB, total 17872 files 1.728 GiB, 0 errors ETA 3:25
> /media/nextcloud/services/jellyfin/config/cache/images/resized-images/7/7d3f157f-f229[0:35] 14.76%  3596 files 261.134 MiB, total 17872 files 1.728 GiB, 0 errors ETA 3:25
> [0:46] 17.60%  4087 files 311.441 MiB, total 17872 files 1.728 GiB, 0 errors ETA 3:36
> uploaded intermediate index 0ee25081
> uploaded intermediate index e740956e
> 
> Files:       17872 new,     0 changed,     0 unmodified
> Dirs:            4 new,     0 changed,     0 unmodified
> Data Blobs:  15666 new
> Tree Blobs:      5 new
> Added to the repo: 1.585 GiB
> 
> processed 17872 files, 1.728 GiB in 6:41
> snapshot 6eb6d1e2 saved
> ```

A partir de estos momentos la copia de seguridad ya está realizada. Si leen más adelante verán como se puede automatizar el proceso.

**Nota:** Para obtener más información de como realizar un backup pueden visitar el siguiente [enlace](https://restic.readthedocs.io/en/stable/040_backup.html).

## COMPROBAR QUE LA COPIA DE SEGURIDAD SE HA REALIZADO CORRECTAMENTE

Para confirmar que la copia de seguridad se ha realizado con éxito lo podemos hacer des del servidor de copias de seguridad o desde del equipo en que se ha realizado la copia de seguridad.

Para listar las copias de seguridad des del servidor en que se almacenan las copias de seguridad ejecutamos el siguiente comando:

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ snapshots
> enter password for repository: mi_contraseña
> password is correct
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 3161b4f6  2020-10-25 20:00:11  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin                                                      
> ----------------------------------------------------------------------
> 2 snapshots
> ```

**Nota**: El comando que acabamos de ejecutar muestra que se han realizado 2 copias de seguridad o snapshots. Si restauro el snapshot con ID `3161b4f6` restaurare los archivos respaldados al estado que tenían el día 25-10-2020 a las 20:00:11. Si restauro la copia con ID `6eb6d1e2` restaurare los archivos respaldados al estado que tenían el día 25-10-2020 a las 20:02:53

Una vez listadas las copias de seguridad podemos comprobar su integridad ejecutando el siguiente comando en la terminal:

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ check
> enter password for repository: mi_contraseña
> password is correct
> create exclusive lock for repository
> load indexes
> check all packs
> check snapshots, trees and blobs
> no errors were found
> ```

Si queremos realizar las mismas comprobaciones des del equipo que contiene los ficheros que hemos respaldado ejecutaremos los siguientes comandos. Para listar los backup existentes:

> ```shell
> pi@raspberrypi:~ $ sudo restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ snapshots 
> enter password for repository: mi_contraseña
> password is correct
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 3161b4f6  2020-10-25 20:00:11  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> ----------------------------------------------------------------------
> 2 snapshots
> ```

Para comprobar la integridad de las copias de seguridad:

> ```shell
> pi@raspberrypi:~ $ sudo restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ check
> using temporary cache in /tmp/restic-check-cache-778788374
> enter password for repository: mi_contraseña
> repository ee979ca4 opened successfully, password is correct
> created new cache in /tmp/restic-check-cache-778788374
> create exclusive lock for repository
> load indexes
> check all packs
> check snapshots, trees and blobs
> no errors were found
> ```

## ELIMINAR COPIAS DE SEGURIDAD O SNAPSHOTS EN RESTIC

Las copias de seguridad se pueden eliminar de forma manual o estableciendo una política o reglas. Para eliminarlas de forma manual tendrán que proceder del siguiente modo.

### Eliminar snapshots o copias de seguridad de forma manual en Restic

Si desde el servidor de copias de seguridad listamos los snapshots disponibles veremos que tenemos 2. El primer snapshot tendrá la ID `3161b4f6` y el segundo tendrá la ID `6eb6d1e2`.

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ snapshots
> enter password for repository: mi_contraseña
> password is correct
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 3161b4f6  2020-10-25 20:00:11  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> ----------------------------------------------------------------------
> 2 snapshots
> ```

Si queremos eliminar la copia de seguridad con ID `3161b4f6` ejecutaremos el siguiente comando en la terminal.

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ forget 3161b4f6
> ```

En estos momentos los datos se han eliminado del índice de Restic, pero aun están en el disco. Para eliminarlos completamente del disco tendremos que ejecutar el siguiente comando:

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ prune
> ```

Si ahora listamos las copias de seguridad disponibles veremos que solamente tenemos una.

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic -r /home/ubuntu/rpi4_bk/ snapshots
> enter password for repository: mi_contraseña
> password is correct
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> ----------------------------------------------------------------------
> 1 snapshot
> ```

Si quisiéramos borrar la copia desde del cliente que contiene el contenido a respaldar deberíamos haber ejecutado los siguientes comandos. Para listar las copias de seguridad presentes:

> ```shell
> pi@raspberrypi:~ $ sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ snapshots
> enter password for repository: mi_contraseña
> password is correct
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 3161b4f6  2020-10-25 20:00:11  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> ----------------------------------------------------------------------
> 2 snapshots
> ```

Para eliminar del registro y del disco la copia con ID `3161b4f6` con tan solo un comando deberíamos ejecutar:

> ```shell
> pi@raspberrypi:~ $ sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ forget 3161b4f6 prune
> ```

Fuente: [https://restic.readthedocs.io/en/latest/060\_forget.html](https://restic.readthedocs.io/en/latest/060_forget.html)

### Eliminar snapshots o copias de seguridad mediante una política o reglas

Restic permite eliminar copias de seguridad basándose en las siguientes políticas o reglas:

1. Número de copias de seguridad que queremos almacenar.
2. Borrar las copias de seguridad que tienen más de una determinada antigüedad.
3. Borrar todas las copias de seguridad excepto aquellas que contengan una o varias etiquetas que nosotros podemos definir.
4. etc.

En mi caso quiero que el número máximo de copias de seguridad almacenadas sean 6. Por lo tanto ejecutaré el siguiente comando para conseguir mi propósito:

> ```shell
> ubuntu@opvpn-webdav-server:~$ restic forget -r /home/ubuntu/rpi4_bk/ --group-by host --keep-last 6 --prune
> enter password for repository: mi_contraseña
> password is correct
> snapshots for (host [raspberrypi]):
> 
> keep 1 snapshots:
> ID        Date                 Host         Tags           Directory
> ----------------------------------------------------------------------
> 6eb6d1e2  2020-10-25 20:02:53  raspberrypi  raspberry  ┌── /home/pi
>                                             docker     │   /media/nextcloud/twitter
>                                                        │   /media/nextcloud/podcast
>                                                        └── /media/nextcloud/services/jellyfin
> ----------------------------------------------------------------------
> 1 snapshot
> 0 snapshots have been removed, running prune
> ```

Al existir un solo snapshoot no ha pasado absolutamente nada. Se ha conservado el existente y no se ha eliminado ninguno. Si en el momento de ejecutar el comando hubieran habido 10 copias de seguridad se habrían borrado las 4 más antiguas y se habrían mantenido las 6 más actuales.

Para ver como aplicar otras reglas pueden visitar el siguiente [enlace](https://restic.readthedocs.io/en/latest/060_forget.html).

## SCRIPT PARA AUTOMATIZAR LAS COPIAS DE SEGURIDAD

A lo largo de este artículo hemos configurado Restic para realizar una copia de seguridad de forma manual. Cuando hayan comprobado que la copia de seguridad se realiza correctamente de forma manual pueden automatizar su proceso del siguiente modo:

### Crear el fichero que contendrá la clave del repositorio /home/ubuntu/rpi4\_bk/

Para automatizar el proceso de copia de seguridad tenemos que evitar tener que introducir la contraseña cada vez que operamos con nuestro repositorio. Para ello tendremos que realizar lo siguiente:

Inicialmente crearemos un archivo con nombre `pass` que es el que contendrá el password del repositorio `/home/ubuntu/rpi4_bk/`.

> ```shell
> restic@raspberrypi:~/restic $ touch pass
> ```

A continuación introduciremos la contraseña del repositorio `/home/ubuntu/rpi4_bk/` dentro del fichero `pass`.

> ```shell
> restic@raspberrypi:~/restic $ echo mi_password > pass 
> ```

Seguidamente otorgaremos los permisos 0400 al archivo pass. De este modo, el archivo pass únicamente podrá ser leído por el usuario `restic` y el usuario `root`.

> ```shell
> restic@raspberrypi:~/restic $ chmod 0400 pass
> ```

### Crear el archivo que mostrará si la copia de seguridad se ha realizado con éxito

Ahora crearemos el archivo `raspberry.txt`. Este archivo almacenará lo que Restic mostraría en pantalla si realizáramos la copia manualmente. De esta forma podremos saber si la copia de seguridad se ha realizado con éxito. Para ello ejecutaremos el siguiente comando:

> ```shell
> restic@raspberrypi:~/restic $ touch raspberry.txt
> ```

A continuación otorgaremos los permisos 622 para asegurar que todos los usuarios pertinentes puedan escribir en el fichero `raspberry.txt`.

> **`restic@raspberrypi:~/restic $ chmod 622 raspberry.txt`**

A partir de estos momentos ya podemos generar el script para realizar la copia de seguridad.

### Crear el script para ejecutar de forma automática las copias de seguridad

El script que crearemos para realizar las copias de seguridad realizará las siguientes acciones:

1. La copia de seguridad de los ficheros y directorios que queremos respaldar.
2. Que el número máximo de copias de seguridad almacenadas sean 6.
3. Una vez finalizada la copia de seguridad se realizará una comprobación de su integridad.
4. Los resultados obtenidos de la copia de seguridad se enviarán nuestro canal de Telegram. De esta forma mirando a Telegram podremos ver si la copia de seguridad se ha realizado correctamente.

Para crear el script dentro de la ubicación `/home/restic/restic` crearemos el fichero run\_backup.sh del siguiente modo:

> **`restic@raspberrypi:~/restic $ nano run_backup.sh`**

Una vez se abra el editor de textos nano pegaremos el siguiente código:

> ```shell
> #!/bin/bash
> 
> ##Fecha copia de seguridad
> echo -e "Backup $HOSTNAME realizado el $(date +'%m/%d/%Y a las %R')" >/home/restic/restic/raspberry.txt 2>&1
> 
> ##Realiza la copia de seguridad
> sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ backup --password-file="/home/restic/restic/pass" --tag raspberry --tag docker -v --exclude-file=/home/restic/restic/files_to_exclude.txt --files-from=/home/restic/restic/files_to_backup.txt >>/home/restic/restic/raspberry.txt 2>&1
> 
> ##Para hacer que únicamente se almacenen las últimas 6 copias de seguridad
> sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ forget --password-file="/home/restic/restic/pass" --group-by host --keep-last 6 --prune >>/home/restic/restic/raspberry.txt 2>&1
> 
> ##Comprobar la integridad de la copia de seguridad
> sudo -u restic restic -r sftp:vps2:/home/ubuntu/rpi4_bk/ check --password-file="/home/restic/restic/pass" >>/home/restic/restic/raspberry.txt 2>&1
> 
> ##Enviar notificación a Telegram con el resultado de la copia de seguridad
> ##sudo -u restic curl -X  POST "https://api.telegram.org/bot"1182342334:TPOS5R_SKA_Or12tOu1VfE2REitd15kWyZ9"/sendDocument" -F chat_id="-11199944432421" -F caption="Copia "$HOSTNAME" fecha $(echo -e "$(date +'%m/%d/%Y a las %R')")" -F document="@/home/restic/restic/raspberry.txt"
> ```

**Nota**: El apartado `Enviar notificación a Telegram con el resultado de la copia de seguridad` está comentado. Esto es así porque para que funcione primero hay que crear un bot en Telegram, obtener un token, obtener el ID del chat al que quieren enviar el resultado y finalmente adaptar el comando para publicar a Telegram.

Una vez pegado el código guardan los cambios y cierran el fichero.

Finalmente damos permisos de ejecución al script que acabamos de generar mediante el siguiente comando:

> ```shell
> restic@raspberrypi:~/restic $ chmod +x run_backup.sh
> ```

### Programar la ejecución de la copia de seguridad mediante cron

Ahora únicamente nos falta usar cron para que el script se ejecute cuando nosotros queramos. El script en mi caso quiero que lo corra el usuario `pi`. Por lo tanto nos loguearemos con el usuario pi con el siguiente comando:

> ```shell
> restic@raspberrypi:~/restic $ su pi
> Contraseña: mi_contraseña
> ```

A continuación abriremos el archivo crontab del usuario pi ejecutando el siguiente comando:

> ```shell
> pi@raspberrypi:/home/restic/restic $ crontab -e
> ```

Finalmente introduciremos el contenido pertinente para que se ejecute el script para realizar la copia de seguridad todos los sábados a la 1:05 horas.

> **`# For more information see the manual pages of crontab(5) and cron(8) #  # m h  dom mon dow   command  #copia de seguridad Raspberry 5 1 * * 6  bash /home/restic/restic/run_backup.sh`**

**Nota**: En mi caso es más que suficiente realizar la copia de seguridad una vez por semana. Si para vosotros no es suficiente podéis [modificar el contenido del archivo crontab para adaptarlo a sus necesarias]({{< relref "/posts/planificar-tareas-con-cron-y-anacron-en-linux" >}}).

Una vez pegado el código salen del fichero presionando la combinación de teclas `CTRL+X`. A partir de estos momentos todos los sábados a la 01:05 de la madrugada se realizará una copia de seguridad del contenido de mi Raspberry Pi.

## RESTAURAR COPIA DE SEGURIDAD CON RESTIC

Para visualizar y restaurar la copia de seguridad que hemos realizar en Restic deberemos seguir las indicaciones del siguiente enlace:

https://geekland.eu/restaurar-copia-de-seguridad-con-restic/

Además en futuros artículos veremos como:

- Borrar completamente un repositorio de Restic.
