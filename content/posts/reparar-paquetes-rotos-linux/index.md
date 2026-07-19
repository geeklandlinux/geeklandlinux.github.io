---
title: "Reparar paquetes rotos y dependencias incumplidas en Debian y Ubuntu"
date: 2019-05-24
categories: 
  - "linux-2"
tags: 
  - "apt"
  - "apt-get"
  - "paquetes-rotos"
  - "repositorios"
  - "synaptic"
coverImage: "reparar-paquetes-rotos-en-linux.jpg"
---

En mi caso tengo un paquete roto en mi equipo. En el caso que se encuentren con una situación similar deberán seguir las siguientes instrucciones para reparar los paquetes rotos y dependencias incumplidas. Pero antes de iniciar la explicación detallaremos que es un paquete roto y que son las dependencias incumplidas.<!--more-->

## ¿QUÉ ES UN PAQUETE ROTO?

En el momento de instalar un paquete puede darse el caso que no se instale correctamente o que no sea compatible con el resto de paquetes. Si esto pasa es posible que no podamos actualizar, instalar ni desinstalar ningún paquete de nuestro sistema operativo. El paquete responsable de que no podamos instalar y desinstalar paquetes es un paquete roto.

Los paquetes acostumbran a romperse en los siguientes casos:

1. Al instalar programas externos a los repositorios oficiales de nuestra distribución.
2. Si se interrumpe accidentalmente la instalación de un programa.
3. Si se interrumpe accidentalmente la actualización del sistema operativo.
4. Etc.

## ¿QUÉ SON LAS DEPENDENCIAS INCUMPLIDAS?

Al instalar un programa como LibreOffice se instalan multitud de paquetes, pero nosotros solo damos orden de instalar el paquete libreoffice. Esto es así por las dependencias de los paquetes. Es decir:

1. Damos la orden de instalar el paquete libreoffice.
2. El paquete libreoffice trae asociada la información que para funcionar correctamente tienen que instalarse una serie de paquetes adicionales. Estos paquetes adicionales se llaman dependencias. Obviamente, los paquetes adicionales o dependencias se instalarán de forma automática.
3. Las dependencias del paquete libreoffice también pueden tener nuevas dependencias y así sucesivamente.

Por lo tanto, en el momento que se haya instalado el paquete libreoffice y todas las dependencias podremos usar LibreOffice.

En el momento que un paquete requiere una dependencia que no está presente en los repositorios de nuestra distribución se generará una dependencia incumplida y por lo tanto no podremos instalar y usar el programa que estamos instalando. En el caso que tangamos paquetes rotos también se generarán dependencias incumplidas o errores de dependencias.

## PASOS A SEGUIR PARA REPARAR PAQUETES ROTOS Y SOLUCIONAR DEPENDENCIAS INCUMPLIDAS EN LINUX

En el caso que tengan paquetes rotos les propongo aplicar alguna de las soluciones que detallaré a continuación.

###### Nota: Al intentar reparar paquetes rotos se nos puede proponer eliminar una gran cantidad de paquetes para solucionar dependencias incumplidas. Por lo tanto, antes de aceptar alguna de las soluciones propuestas lean detenidamente lo que se realizará en el equipo.

### Reparar paquetes rotos con dpkg y el gestor de paquetes apt

Lo primero que haremos es intentar configurar paquetes que han sido desempaquetados, pero no configurados. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> sudo dpkg --configure -a
> ```

Si no se soluciona el problema limpiaremos la cache local de paquetes .deb ejecutando los siguiente comando en la terminal:

> ```
> sudo apt-get clean && sudo apt-get autoclean
> ```

Acto seguido regeneraremos la cache de los repositorios con el parámetro \--fix-missing. De este modo intentaremos detectar y reparar problemas de dependencias en nuestro sistema operativo. Por lo tanto en la terminal ejecutamos el siguiente comando:

> ```
> sudo apt-get update --fix-missing
> ```

Seguidamente usaremos apt para intentar corregir las dependencias rotas. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> sudo apt-get install -f
> ```

Después de ejecutar el comando se intentará buscar una solución para solucionar nuestro problema de dependencias incumplidas y paquetes rotos.

En el caso que sigan los problemas borraremos las lock files de apt y dpkg. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> sudo rm /var/lib/apt/lists/lock && sudo rm /var/cache/apt/archives/lock && sudo rm /var/lib/dpkg/lock
> ```

###### Nota: Las lock files tienen la función de evitar que 2 o más procesos accedan a los mismos datos.

Después de ejecutar este último comando vuelvan a repetir la totalidad de comandos citados en este apartado.

### Reparar paquetes rotos y dependencias con la terminal y dpkg

En el caso que la solución anterior no haya funcionado intentaremos eliminar el paquete roto que está generando problemas. Para ello ejecutaremos el siguiente comando:

> ```
> sudo apt-get remove
> ```

En el caso que no tengamos éxito averiguaremos el paquete que está generando problemas ejecutando el siguiente comando:

> ```
> sudo dpkg -l | grep ^..r
> ```

Acto seguido ejecutaremos el siguiente comando para intentar forzar la desinstalación del paquete que presenta problemas:

> ```
> sudo dpkg --remove --force-remove--reinstreq nombre_paquete
> ```

###### Nota: Deben remplazar nombre\_paquete por el paquete roto o que está generando problemas.

A continuación limpiaremos la cache local de nuestro repositorios ejecutando el siguiente comando:

> ```
> sudo apt-get clean && sudo apt-get autoclean
> ```

Finalmente intentaremos actualizar nuestro sistema operativo ejecutando el siguiente comando en la terminal:

> ```
> sudo apt-get update && sudo apt-get dist-upgrade
> ```

Si el sistema se actualiza sin problema habremos resuelto el problema de dependencias y paquetes rotos.

### Reparar paquetes rotos con Synaptic package manager

Otra opción para intentar solucionar el problema es mediante el programa Synaptic. Para instalarlo tan solo tienen que ejecutar el siguiente comando en la terminal:

> ```
> sudo apt-get install synaptic
> ```

Acto seguido abran el programa y sigan las siguientes instrucciones:

1. Cliquen en el botón Filtros.
2. A continuación seleccionen la opción Rotos.
3. Seleccionen el paquete que está dañado.
4. Acto seguido presionen sobre el botón derecho del ratón y cuando aparezca el menú contextual cliquen sobre la opción Marcar para desinstalar completamente. Acto seguido se intentará desinstalar el paquete que está generando todos los problemas.

[![Desinstalar paquetes rotos con Synaptic](images/desinstalar-paquetes-rotos-synaptic.png "Desinstalar paquetes rotos con Synaptic")](images/desinstalar-paquetes-rotos-synaptic.png "Desinstalar paquetes rotos con Synaptic")

En el caso que sigan con los mismos problemas pueden intentar lo siguiente. Justo al abrir el gestor de paquetes Synaptic entren dentro del menú Editar y cliquen sobre la opción Reparar paquete rotos. Acto seguido Synaptic interará arreglar los paquetes todos de nuestro sistema operativo.

[![Reparar paquetes rotos con Synaptic](images/reparar-paquetes-rotos-synaptic.png "Reparar paquetes rotos con Synaptic")](images/reparar-paquetes-rotos-synaptic.png "Reparar paquetes rotos con Synaptic")

### Último recurso para intentar solucionar problemas de paquetes rotos y dependencias

Si después de aplicar la totalidad de opciones siguen con problemas pueden intentar lo siguiente.

Editen el fichero /var/lib/dpkg/status ejecutando el siguiente comando en la terminal:

> ```
> sudo nano /var/lib/dpkg/status
> ```

###### Nota: El fichero /var/lib/dpkg/status contiene información del estado de los paquetes de nuestro sistema operativo.

Cuando se abra el editor de texto busquen el bloque de información que hace referencia al paquete roto que genera problemas. Una vez encontrado el bloque bórrenlo.

En mi caso, el paquete que genera problemas es caffeine, por lo tanto tendré que borrar todo el bloque que está dentro del rectángulo rojo.

[![Borrar el estado de los paquetes rotos](images/borrar-estado-paquetes-rotos.png "Borrar el estado de los paquetes rotos")](images/borrar-estado-paquetes-rotos.png "Borrar el estado de los paquetes rotos")

Una vez borrado el contenido guardamos los cambios y cerramos el fichero. Acto seguido podemos intentar actualizar nuestro sistema operativo ejecutando el siguiente comando:

> ```
> sudo apt-get clean && sudo apt-get update && sudo apt-get dist-upgrade
> ```

Después de seguir los pasos del artículo deberían tener su sistema operativo plenamente operativo. En caso contrario detallen su problema de forma adecuada en los comentarios del artículo dando la información necesaria para que alguien les pueda ayudar.
