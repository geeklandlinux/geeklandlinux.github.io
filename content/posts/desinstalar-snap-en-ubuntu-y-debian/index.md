---
title: "Desinstalar snap por completo en un servidor Ubuntu o Debian"
date: 2020-09-29
categories: 
  - "linux-2"
tags: 
  - "debian"
  - "snap"
  - "ubuntu"
cover:
  image: "images/desinstalar-snap-por-completo-Ubuntu-y-Debian.png"
  relative: true
---

En mi caso tengo un Ubuntu Server 20.04 en un VPS con bajos recursos. El VPS tiene 1GB de RAM y resulta que Snap está consumiendo una parte importante de esta RAM. La verdad me parece una mala elección que un sistema operativo con opciones mínimas y destinado a trabajar como servidor venga con Snap activado por defecto. Afortunadamente podemos desinstalar snap siguiendo las instrucciones que verán a continuación.<!--more-->

## LISTAR Y DESINSTALAR LA TOTALIDAD DE PAQUETES SNAP QUE TENEMOS INSTALADOS

El primero paso consiste en listar la totalidad de paquetes snap que tenemos instalados. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> ubuntu@ubuntu-20-04:~$ snap list
> Name    Version   Rev    Tracking         Publisher   Notes
> core18  20200724  1885   latest/stable    canonical✓  base
> lxd     4.6       17320  latest/stable/…  canonical✓  -
> snapd   2.46.1    9279   latest/stable    canonical✓  snapd
> ```

En mi caso tengo instalados `core18`, `lxd` y `snapd`. Obviamente ninguno de los paquetes instalado me es necesario. Para desinstalarlos hay que ejecutar los siguientes comandos en la terminal:

> ```shell
> ubuntu@ubuntu-20-04:~$ sudo snap remove lxd
> lxd removed
> 
> ubuntu@ubuntu-20-04:~$ sudo snap remove core18
> core18 removed
> 
> ubuntu@ubuntu-20-04:~$ sudo snap remove snapd
> snapd removed
> ```

**Nota**: Si en vuestro caso os aparecen más paquetes que a mi no hay problema. Desinstaladlos todos porque el objetivo que tenemos es desinstalar completamente snap.

## ELIMINAR EL DEMONIO SNAPD DEL SISTEMA OPERATIVO

A continuación ya podremos desinstalar el demonio Snapd de nuestro sistema operativo. De este modo la próxima vez que reiniciéis vuestro equipo ya no estaremos malgastando la memoria RAM. El comando a usar para desinstalar snap será el siguiente:

> ```shell
> ubuntu@ubuntu-20-04:~$ sudo apt purge snapd
> Reading package lists... Done
> Building dependency tree       
> Reading state information... Done
> The following package was automatically installed and is no longer required:
>   squashfs-tools
> Use 'sudo apt autoremove' to remove it.
> The following packages will be REMOVED:
>   snapd*
> 0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
> After this operation, 122 MB disk space will be freed.
> Do you want to continue? [Y/n] 
> (Reading database ... 147160 files and directories currently installed.)
> Removing snapd (2.46.1+20.04) ...
> Processing triggers for man-db (2.9.1-1) ...
> Processing triggers for dbus (1.12.16-2ubuntu2.1) ...
> Processing triggers for mime-support (3.64ubuntu1) ...
> (Reading database ... 147076 files and directories currently installed.)
> Purging configuration files for snapd (2.46.1+20.04) ...
> Final directory cleanup
> Discarding preserved snap namespaces
> Removing extra snap-confine apparmor rules
> Removing snapd cache
> Removing snapd state
> ```

## ELIMINAR LA TOTALIDAD DE FICHEROS QUE SNAP ALMACENO EN NUESTRO DISPOSITIVO DE ALMACENAMIENTO

Finalmente borraremos la totalidad de información que snap almacenó en nuestro dispositivo de almacenamiento. Para ello ejecutaremos los siguientes comandos en la terminal:

> ```shell
> ubuntu@ubuntu-20-04:~$ rm -rf ~/snap
> ubuntu@ubuntu-20-04:~$ sudo rm -rf /snap
> ubuntu@ubuntu-20-04:~$ sudo rm -rf /var/snap
> ubuntu@ubuntu-20-04:~$ sudo rm -rf /var/lib/snapd
> ```

## REINICIAR EL EQUIPO DESPUÉS DE DESINSTALAR SNAP

Una vez seguidos todos los pasos tan solo tienen que reiniciar su equipo o servidor. Una vez reiniciado el equipo verán que el consumo de RAM del equipo será sensiblemente menor. Tienen que tener presente que si no usan un programa o un servicio es mejor desinstalarlo ya que en caso contrario estarán consumiendo recursos inútilmente.
