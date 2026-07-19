---
title: "Recomendación de software libre para cifrar archivos en la nube"
date: 2020-02-29
categories: 
  - "seguridad-informatica"
tags: 
  - "cifrado"
  - "cryfs"
  - "ecryptfs"
  - "encfs"
  - "gocryptfs"
  - "servicios-en-la-nube"
  - "veracrypt"
coverImage: "opciones-cifrar-archivos-en-la-nube.png"
---

Da igual que guardemos nuestro contenido en una [nube]({{< relref "/posts/que-son-los-servicios-en-la-nube" >}}) pública o privada. En cualquiera de los 2 casos tendremos que cifrar la información en el caso que sea sensible. Para cifrar archivos en la nube tenemos opciones como por ejemplo EncFS, CryFS, eCryptfs, gocryptfs y Veracrypt. A continuación veremos cual de ellos es la mejor opción para cifrar archivos en la nube.<!--more-->

## CARACTERÍSTICAS INDISPENSABLES DE UN SOFTWARE PARA CIFRAR ARCHIVOS EN LA NUBE

Para cifrar archivos en la nube de forma práctica se tienen que cumplir los siguientes requisitos:

### Tiene que cifrar archivo por archivo

Considero que un software para cifrar archivos en la nube tiene que cifrar archivo por archivo. Por lo tanto, si ciframos 10 ficheros de un directorio, como mínimo tenemos que visualizar 10 ficheros cifrados.

Existe software de cifrado, como por ejemplo **Veracrypt**, que **cifra todo el contenido en un solo fichero**. Esta forma de trabajar es claramente **un inconveniente** si la cantidad de información cifrada es grande. Los motivos son los siguientes:

1. **El riesgo de perder la información es mayor**: Si se corrompe el fichero cifrado perderemos la totalidad del contenido que teníamos cifrado.
2. **Las copias de seguridad son más lentas**: Si tenemos 1GB de información cifrada y únicamente modificamos 4 archivos que en total tienen un peso de 1MB, cuando hagamos la copia de seguridad se deberá respaldar 1GB de contenido.
3. **La sincronización con la nube será más lenta**: Si tenemos un volumen cifrado de 1GB y únicamente modificamos un archivo de 4k implica que deberemos sincronizar 1GB de información.
4. **La cantidad de lecturas y escrituras mayor**: Por las razones detalladas tanto en el punto 2 como en el 3 la cantidad de lecturas y escrituras realizadas por nuestro disco duro será mucho mayor.

**Nota:** Este comportamiento también dependerá de la nube que usemos.

Partiendo de esta premisa las opciones más apropiadas serían usar los siguientes software:

EncFS, CryFS, eCryptfs y gocryptfs

### Se tiene que integrar con las nubes públicas o privadas que utilicemos

Además el software de cifrado tiene trabajar de forma adecuada con las nubes públicas y privadas. No tiene que generar problemas de sincronización ni conflictos usándolo de forma habitual.

Bajo esta condición las opciones que funcionan sin ningún tipo de problema son:

EncFS, CryFS, Veracrypt y GocryptFS

## CARACTERÍSTICAS DESEABLES DE UN SOFTWARE PARA CIFRAR ARCHIVOS EN LA NUBE

Otros requisitos importantes que debería cumplir la solución de cifrado son los siguientes.

### Poder acceder a los datos cifrados desde cualquier dispositivo

Este punto dependerá de las necesidades de los usuarios. No obstante un punto importante a considerar es que el contenido cifrado en la nube se pueda acceder y editar en la totalidad de sistemas operativos. En este apartado los software que ganan por goleada son EncFS y Veracrypt. Si usamos EncFS o Veracrypt podremos consultar nuestros datos cifrados sin problemas desde los siguientes sistemas operativos:

1. Android
2. Linux
3. FreeBSD
4. OpenBSD
5. MacOS
6. Windows
7. Etc.

El resto de software analizado solo permite consultar el contenido cifrado desde uno o 2 sistemas operativos. Y lo peor de todo es que no se pueden usar ni en Android ni en iOS. Para más información respecto a este punto pueden consultar la tabla comparativa que encontrarán más adelante.

### Seguridad ofrecida por el software de cifrado

En este punto los ganadores son **CryFS y Veracrypt**. Ambos software tienen las siguiente virtudes que no tienen los demás:

1. Son los únicos que **cifran la estructura de directorios** del volumen cifrado.
2. Son las únicas opciones que **cifran los metadatos** de cada uno de los archivos cifrados.

Como gran perdedor en este apartado tenemos a EncFS. **EncFS no cifra la estructura de los directorios y no cifra los metadatos de los archivos**. Este hecho puede dar una idea al atacante del contenido que estamos almacenando de forma cifrada. Pero es que además en una [auditoria de seguridad](https://defuse.ca/audits/encfs.htm "Ver los resultados de la auditoria realizada a EncFS") elaborada en Febrero de 2014 se detectaron otras vulnerabilidades de seguridad. Las conclusiones de la auditoria fueron las siguientes:

> “Cuando un fichero cifrado es modificado varias veces y el atacante tiene acceso a cada una de las versiones cifradas modificadas. Entonces el atacante tendrá más posibilidades de crackear el password para descifrar el contenido.”

A raíz de los resultados de la auditoría han salido actualizaciones que resuelven parte de las vulnerabilidades, pero a día de hoy y 6 años después aun no se han resuelto todos los problemas. No obstante **los desarrolladores de EncFS tienen el compromiso de resolver todas las vulnerabilidades a partir de la versión 2.0**. El problema es que nadie sabe cuando saldrá la versión 2.0

**Nota:** No cifrar los metadatos de los ficheros y mantener la estructura de los directorios puede tener puntos positivos. El principal punto positivo es que podremos realizar copias de seguridad cifradas desde el origen con rsync actualizando únicamente los archivos que han recibido modificaciones.

## TABLA RESUMEN DE CARACTERÍSTICAS DE LOS SOFTWARE ANALIZADOS

A continuación les dejo una tabla comparativa de las distintas opciones que estamos analizando para cifrar archivos en la nube.

     
|  |   **EncFS**   |   **CryFS**   |   **gocryptfs**   |   **eCryptfs**   |   **Veracrypt**   |
| --- | --- | --- | --- | --- | --- |
|   Sistemas operativos   |   Android, Linux, Windows, MacOS  iOS   |   Linux, MacOS, Windows+- |   Linux, MacOS, Windows+- |   Linux   |   Android, Linux, Windows, MacOS   |
|   Cifra el contenido de los ficheros   |   Sí   |   Sí   |   Sí   |   Sí   |   Sí   |
|   Cifra metadatos de los archivos   |   No   |   Sí   |   No   |   No   |   Sí   |
|   Cifra estructura de los directorios   |   No   |   Sí   |   No   |   No   |   Sí   |
|   Vulnerabilidades de seguridad   |   Conocidas   |   No   |   No   |   No   |   No   |
|   Sincroniza archivo por archivo   |   Sí   |   Sí   |   Sí   |   Sí   |   No   |
|   Montar volumen cifrado a partir de un directorio   |   Sí   |   No   |   Sí   |   No   |   No   |
|   Pequeños cambios implican que solo las partes modificadas tengan que ser resubidas a la nube   |   Sí   |   Sí   |   Sí   |   Sí   |   No   |

+-: Soporte experimental y en algunos casos no usable.

**Fuente:** [https://www.cryfs.org/comparison/](https://www.cryfs.org/comparison/)

## ¿QUÉ SOFTWARE OS RECOMIENDARIA PARA CIFRAR ARCHIVOS EN LA NUBE?

El software ideal será en función de las necesidades de cada usuario. Por lo tanto mi recomendación en función de las distintas necesidades es la siguiente.

### Casos en los que recomiendo usar CryFS para cifrar archivos en la nube

**Si siempre usáis Linux, MacOS o FreeBSD y no precisáis acceder al contenido cifrado desde otros sistemas operativos usad CryFS**. Técnicamente y sobre el papel es el software más seguro y recomendable para cifrar archivos en la nube.

En mi caso no uso esta opción porque CryFS no me da ninguna solución para consultar mis archivos cifrados ni en Windows ni en Android. CryFS tampoco permite montar un volumen de contenido cifrado a partir de un directorio no cifrado. Si lo hiciera seria un opción bastante útil para realizar copias de seguridad cifradas. Además no es una opción que destaque por su velocidad.

### ¿Cuándo recomendaría usar EncFS?

Si simplemente queréis guardar vuestra información personal de forma cifrada y que esté disponible y accesible en la totalidad de sus sistemas operativos en entonces usen EncFS.

Además EncFS es un opción muy interesante para realizar copias de seguridad cifradas en local o en la nube. Los motivos son los siguientes:

1. Cifra archivo por archivo guardando sus metadatos. Por lo tanto con rsync o rclone podremos hacer un copia de seguridad actualizando únicamente los ficheros que se han modificado.
2. EncFS permite montar un volumen cifrado a partir de un directorio sin cifrar. Esta opción es muy útil en el caso que por ejemplo queramos subir una copia de seguridad en la nube cifrada desde el origen.

Por todos estos motivos en mi caso estoy usando EncFS.

Recuerden que EncFS presenta vulnerabilidades de seguridad. Por lo tanto si el contenido que queréis cifrar es altamente sensible es mejor que uséis Veracrypt o CryFS. A pesar de tener vulnerabilidades existen medidas que podemos tomar para dificultar que un atacante descifre nuestros ficheros. Además los desarrolladores de EncFS se comprometieron todos los problemas a partir de la versión 2.0

### ¿Cúando recomendaría usar Veracrypt?

No es el software que recomendaría para cifrar archivos en la nube. No obstante, si son usuarios de Windows y tenéis que cifrar información extremadamente sensible y que ocupan poco tamaño pueden plantearse usar Veracrypt.

En cuando al rendimiento no os tenéis que preocupar. El funcionamiento de Veracrypt será sensiblemente más rápido que EncFS y especialmente CryFS. Durante el artículo ya he citado los principales inconvenientes de Veracrypt.

### ¿Y que puedo decir de gocryptfs?

Es un software a tener muy en cuenta. Además según una [auditoria](https://defuse.ca/audits/gocryptfs.htm "Ver los resultados de la auditoria de seguridad realizada a gocryptfs") realizada en Marzo del 2017 es más o menos seguro.

Recomendaría el uso de gocryptfs a todos los usuarios que no necesiten consultar sus datos cifrados en sistemas operativos móviles.

También considero que es la **mejor opción para los usuarios que quieran guardar una copia de seguridad cifrada en la nube desde su origen**. Al igual que EncFS, gocryptfs permite montar un volumen cifrado a partir de un directorio con ficheros. Por lo tanto:

1. Podemos trabajar con nuestros documentos de forma habitual.
2. En el momento de realizar la copia de seguridad podemos montar un volumen cifrado que contendrá toda la información que queremos respaldar, pero cifrada.
3. Una vez montado el volumen cifrado podemos realizar una copia de seguridad local o en la nube del contenido cifrado.
4. Una vez finalizada la copia de seguridad podemos desmontar el volumen cifrado y seguir trabajando con nuestros documentos.

Es muy posible que en mi caso intente reemplazar EncFS por gocryptfs.

### ¿Recomendaría usar eCryptfs para cifrar archivos en la nube?

eCryptfs es una magnífica herramienta integrada en el Kernel de Linux. Es conocida porque es la herramienta que usa Ubuntu para cifrar las particiones de nuestro sistema operativo y su rendimiento es espectacular. No obstante no es una solución adecuada para cifrar nuestros archivos en la nube. El motivo es que eCryptfs no es un software diseñado para cifrar archivos en la nube. Si intentan usar eCryptfs para sincronizar ficheros entre distintos equipos verán un comportamiento errático. Por ejemplo que un archivo se reemplaza por una versión anterior a la actual, etc.

Por lo tanto **nadie debería usar eCryptfs para cifrar archivos en la nube**.

eCryptfs fue diseñado asumiendo que únicamente un solo usuario puede modificar archivos de la unidad cifrada. Pero en el caso de sincronizar archivos en la nube son varios los usuarios/software que pueden modificar el contenido del directorio cifrado.

###### ADVERTENCIA

Tened en cuenta que gran parte de las herramientas citadas se desarrollaron y se mantienen para ser usadas en sistemas operativos GNU-Linux. Esto quiere decir que estas herramientas funcionarán perfecto en GNU-Linux.

No obstante en otros sistemas operativos podéis encontrar problemas. En ocasiones las aplicaciones para otros sistemas operativos son creadas por terceros no vinculados con el proyecto original. Esto puede causar que el programa no se actualice con frecuencia y que no funcione lo bien que debería funcionar.

## COMO USAR ESTOS PROGRAMAS PARA CIFRAR ARCHIVOS EN LA NUBE

Para un uso básico de EncFS, CryFS y gocryptFS les recomiendo visitar el siguiente enlace.

https://geekland.eu/cifrar-archivos-en-linux-con-encfs-cryfs-y-gocryptfs-usando-la-terminal/

###### FUENTES

[https://en.wikipedia.org/wiki/Comparison\_of\_disk\_encryption\_software](https://en.wikipedia.org/wiki/Comparison_of_disk_encryption_software) [https://www.cryfs.org/](https://www.cryfs.org/)
