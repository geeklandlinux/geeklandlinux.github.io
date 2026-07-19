---
title: "Explicación de las carpetas que contienen archivos temporales en Linux"
date: 2016-12-08
categories: 
  - "linux-2"
tags: 
  - "archivos-temporales"
coverImage: "Características-carpetas-contienen-archivos-temporales-Linux.png"
---

En Linux, al igual que en otros sistemas operativos, existen los archivos temporales del sistema. Estos archivos se almacenan en las siguientes carpetas:

1. /tmp
2. /var/tmp

Únicamente cito las carpetas que albergan la gran mayoría de archivos temporales de nuestro sistema.<!--more-->

Si quieren consultar otras carpetas que contienen archivos temporales, o que almacenan información que podemos prescindir de ella, pueden consultar los archivos de configuración de la carpeta /usr/lib/tmpfiles.d/

Algunas de las carpetas adicionales que encontrarán son las siguientes:

1. /var/run
2. /var/spool
3. /var/log/journal
4. /run/shm
5. /var/cache/man
6. etc

Asimismo hay multitud de programas instalados en nuestro ordenador que almacenan archivos de configuración, archivos temporales, archivos de cache, registros e historiales en la partición /home de nuestro ordenador.

## ¿QUÉ DIFERENCIA HAY ENTRE LA CARPETA /TMP Y LA /VAR/TMP?

El propósito y fin de ambas carpetas es exactamente el mismo. Su función es almacenar archivos temporales.

Las características básicas de los archivos temporales de cada una de las carpetas son las siguientes:

### Características de los archivos temporales de la carpeta /tmp

Las características de los archivos almacenados en la carpeta /tmp son la siguientes:

1. El tiempo que los archivos temporales estarán almacenados es corto o muy corto.
2. El tiempo de almacenamiento dependerá de la distro que usemos. No obstante la gran mayoría de distros borran la carpeta /tmp cada vez que encendemos o reiniciamos el ordenador.
3. Por los motivos descritos en el punto 2 hay usuarios que montan la partición /tmp en la memoria RAM de su propio ordenador.
4. La carpeta /tmp siempre estará disponible en las etapas iniciales del arranque del sistema. Por lo tanto la totalidad de scripts de inicio harán uso de la carpeta /tmp para almacenar los archivos temporales.

### Características de los archivos temporales de la carpeta /var/tmp

Por contrapartida los archivos de la carpeta /var/tmp disponen de las siguientes propiedades:

1. Su tiempo de almacenamiento es mucho más elevado.
2. Los archivos presentes en esta carpeta no se borran cada vez que encendemos o apagamos el ordenador. El tiempo de almacenamiento de estos archivos depende de la distribución que usemos.
3. En mi caso uso Debian y de forma estándar no borra nunca el contenido de la carpeta /var/tmp. Otras distribuciones como por ejemplo RHEL 7 los archivos se borran si tienen más de 30 días de inactividad.
4. Los usuarios siempre deberían montar la carpeta /var/tmp en un disco duro normal y corriente.
5. Algunos ejemplos de datos que se almacenan en esta ubicación son archivos para restaurar el estado de un programa o archivo en caso de error o que nuestro ordenador se cuelgue.
6. La carpeta /var/tmp puede formar parte de un punto de montaje. Por lo tanto está carpeta no estará disponible en las fases iniciales del arranque. La carpeta no estará disponible hasta que se hayan montado los puntos de montaje.

### Diferencia entre la carpeta /tmp y la carpeta /var/tmp

Por lo tanto la conclusión final es que la carpeta /tmp contendrá archivos temporales que se borrarán cada vez que reiniciamos el ordenador.

La carpeta /var/tmp contiene archivos temporales que se recomiendan almacenar durante un periodo de tiempo determinado. Por lo tanto el contenido de esta carpeta no se borrará cada vez que reiniciemos nuestro ordenador.

## ¿NOS TENEMOS QUE PREOCUPAR DE BORRAR LOS ARCHIVOS TEMPORALES?

En principio un usuario normal no debe preocuparse de borrar los archivos temporales que no estén en la partición /home.

Normalmente las distribuciones que usamos están configuradas para borrar de forma automática los archivos temporales innecesarios.

No obstante si gestionamos un servidor la situación es diferente. Un servidor funciona de forma ininterrumpida durante largos periodos de tiempo. Por lo tanto existe la posibilidad que la carpeta /tmp, /var/tmp u otra se llenen provocando que nos quedemos sin espacio en el disco duro.

Para evitar este problema podemos seguir los consejos que se muestran en el siguiente enlace:

[Borrar los archivos temporales de forma periódica y automática con Systemd]({{< relref "/posts/borrar-los-archivos-temporales-automaticamente" >}})

[Forzar el borrado de los archivos temporales de forma segura]({{< relref "/posts/eliminar-archivos-temporales-servicio-systemd" >}})
