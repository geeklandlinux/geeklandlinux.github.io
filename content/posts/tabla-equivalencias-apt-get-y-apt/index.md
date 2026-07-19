---
title: "Tabla de equivalencias entre los comandos apt-get y apt"
date: 2017-04-06
categories: 
  - "linux-2"
tags: 
  - "apt"
  - "apt-get"
  - "equivalencias"
coverImage: "equivalencias-comandos-apt-y-apt-get.png"
---

En el artículo que escribí recientemente vimos las [similitudes y diferencias existentes]({{< relref "/posts/apt-y-apt-get-similitudes-y-diferencias" >}}) entre apt-get y apt. Para los lectores que estén interesados en probar apt, a continuación les mostraré una tabla de equivalencias entre estos 2 comandos. Además también encontrarán una explicación detallada de la función que realizan cada uno de los comandos que hablaremos.<!--more-->

## TABLA DE EQUIVALENCIAS ENTRE LOS COMANDOS APT-GET Y APT

A continuación les presento una tabla equivalencias que les ayudará a usar apt sin ningún tipo de problema:

  
|   **Comando apt-get**   |   **Comando equivalente apt**   |   **Función del comando**   |
| --- | --- | --- |
|   apt-get update   |   apt update   |   Actualizar los repositorios de nuestra distribución. Los repositorios se acostumbran a configurar en el archivo /etc/apt/sources.list   |
|   apt-get install   |   apt install   |   Instalar uno o varios paquetes en nuestro sistema operativo.   |
|   apt-get upgrade   |   apt upgrade   |   Actualizar los paquetes y programas que tenemos instalados. Con apt-get, si los paquetes requieren nuevas dependencias no se actualizarán. Con apt, si los paquetes actualizables requieren de nuevas dependencias, se instalarán las nuevas dependencias y se actualizarán los paquetes. Utilizando estos comando nunca desinstalaremos ningún paquete.   |
|   apt-get dist-upgrade   |   apt dist-upgrade   |   Actualizamos la totalidad de paquetes que tenemos instalados. Durante el proceso de actualización se instalarán y actualizarán las dependencias necesarias. En caso que sea necesario se pueden desinstalar paquetes de nuestro sistema para resolver conflictos de dependencias.   |
|   apt-get full-upgrade   |   apt full-upgrade   |   Actualizamos la totalidad de paquetes que tenemos instalados. Durante el proceso de actualización se instalarán y actualizarán las dependencias necesarias. En caso que sea necesario se pueden desinstalar paquetes de nuestro sistema para resolver conflictos de dependencias.   |
|   apt-get remove   |   apt remove   |   Desinstalar uno o varios paquetes manteniendo sus archivos de configuración.   |
|   apt-get purge   |   apt purge   |   Desinstalar los paquetes y los archivos de configuración de las utilidades desinstaladas.   |
|   apt-get autoremove   |   apt autoremove   |   Para desinstalar las dependencias de programas que en su día eran necesarias, pero ahora ya no son necesarias.   |
|   apt-mark   |   apt-mark   |   Herramienta para etiquetar si los paquetes han sido instalados manualmente o automáticamente. El paquete apt-mark también tiene otras funcionalidades como por ejemplo retener y desretener paquetes, etc.   |
|   apt-get build-dep   |   apt build-dep   |   Instalar todos los paquetes necesarios para poder compilar un determinado programa o paquete.   |
|   apt-get clean   |   apt clean   |   Borrar los paquetes binarios almacenados en  las ubicaciones /var/cache/apt/archive y /var/cache/apt/archives/partial/.   |
|   apt-get autoclean   |   apt autoclean   |   Eliminar paquetes binarios que ya no pueden ser descargados de los repositorios de la ubicación /var/cache/apt/archive.   |
|   apt-get check   |   \- |   Verificar que se cumplen todas las dependencias del sistema- |
|   apt-get source   |   apt source   |   Permite descargar el código fuente de un programa o paquete en el directorio.   |
|   apt-get download   |   apt download   |   Descargar paquetes binarios de nuestros repositorios en el directorio actual.   |
|   apt-get changelog   |   apt changelog   |   Descarga y muestra en pantalla el registro de cambios de un paquete.   |
|   apt-get indextargets   |   \- |   Muestra información detallada de los repositorios que tenemos en el sistema operativo.   |
|   apt-cache search   |   apt search   |   Buscar todos los paquetes que contienen un nombre determinado.   |
|   apt-cache depends   |   apt depends   |   Mostrar la totalidad de dependencias de un paquete del sistema.   |
|   apt-cache rdepends   |   apt rdepends   |   Mostrar las dependencias inversas de un paquete.   |
|   apt-cache policy   |   apt policy   |   Muestra la prioridad de instalación de cada uno de los repositorios y sus ramas.   |
|   nano /etc/apt/sources.list   |   apt edit-sources   |   Editar el contenido de nuestros repositorios en el fichero /etc/apt/sources.list.   |
|   apt-cache show   |   apt show   |   Muestra la información y características sobre un determinado paquete presente en nuestros repositorios.   |
|   apt-cache showsrc   |   apt showsrc   |   Mostrar detalles de los paquetes fuente de nuestros repositorios.   |
|   apt-show-versions   |   apt list   |   Obtener listados de los paquetes instalados, actualizables, que se han actualizado manualmente, etc.   |

La tabla que les acabo de detallar contiene la totalidad de comandos existentes en la actualidad. En el caso que encuentren a faltar algún comando les agradecería que lo comunicaran en los comentarios de este artículo.

###### Nota: Algunos de los comandos detallados no están documentados. El motivo es que simplemente se crearon para que exista una equivalencia entre ambos comandos.

###### Nota: Algunos de los comandos que se citan como equivalentes no son realmente equivalentes. No obstante los considero como equivalentes porque la función que realizan es prácticamente equivalente.

## NOTAS FINALES

Mediante la tabla de equivalencias y la descripción de la función de cada uno de los comandos, todos los usuarios deberían ser capaces de usar ambos comandos sin ningún tipo de problema.  De este modo, mediante la línea de comandos todo el mundo podrá gestionar los paquetes de su distribución GNU-Linux.

Si analizan la sintaxis y el uso de ambos comandos verán que son muy semejantes, por lo tanto les será sumamente fácil usar cualquiera de los 2 comandos. Como vimos en artículos anteriores es indiferente usar un comando u otro, por lo tanto usen el que mas les convenga.

Si precisan de mayor información para poder usar estos dos comandos pueden consultar pueden consultar el [manual del administrador de Debian](https://debian-handbook.info/browse/es-ES/stable/ "Consultar el manual del administrador de Debian"). Concretamente pueden revisar los puntos 5 y 6 del manual.
