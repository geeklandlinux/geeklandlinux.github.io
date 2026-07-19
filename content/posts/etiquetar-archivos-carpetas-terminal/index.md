---
title: "Etiquetar archivos y carpetas con la terminal en Linux"
date: 2017-05-30
categories: 
  - "linux-2"
tags: 
  - "buscar-archivos"
  - "etiquetar"
  - "gnome"
  - "terminal"
  - "tracker"
cover:
  image: "images/etiquetar-archivos-y-carpetas-con-la-terminal.png"
  relative: true
---

Siguiendo con la serie de post relacionados con Tracker, a continuación veremos como podemos etiquetar archivos y carpetas mediante la terminal usando [Tracker]({{< relref "/posts/configurar-tracker-para-buscar-archivos-por-contenido" >}}).

Básicamente existen 2 métodos para etiquetar archivos con tracker. El primero de ellos es usando la interfaz gráfica de Nautilus o de Tracker-needle. El segundo es usando la terminal.<!--more-->

## UTILIDAD DE ETIQUETAR LOS ARCHIVOS Y CARPETAS

Etiquetar nuestros archivos y carpetas es sumamente útil para los siguientes fines:

1. Organizar la información que tenemos almacenada en nuestro equipo.
2. Buscar los archivos y carpetas almacenadas en nuestro ordenador de forma mucho más rápida y efectiva.

Una vez conocidas las principales utilidades pasaremos a ver como podemos etiquetar nuestros archivos y carpetas usando la terminal.

## ETIQUETAR ARCHIVOS Y DIRECTORIOS MEDIANTE LA TERMINAL

A continuación veremos el procedimiento para etiquetar archivos y directorios en la terminal de Linux.

### Crear una etiqueta con la terminal

Si queremos crear una etiqueta sin asignarla a ningún archivo o carpeta tan solo hay que ejecutar el comando tracker tag --add= seguido del nombre de la etiqueta. Por lo tanto para crear la etiqueta 2015 tan solo deberíamos ejecutar el siguiente comando en la terminal:

> ```
> tracker tag --add=2015
> ```

### Etiquetar archivos y carpetas mediante la terminal

Etiquetar un archivo o una carpeta mediante la terminal es sumamente sencillo. Si queremos etiquetar la fotografía vacaciones\_2015.png con la etiqueta 2015 tenemos que ejecutar el siguiente comando:

> ```
> tracker tag --add=2015 /home/joan/Documentos/vacaciones_2015.png
> ```

Cada uno de los términos del comando realiza la siguiente función:

tracker tag: Parte del comando que indica a Tracker que queremos etiquetar un archivo o una carpeta. \--add=2015: Indica que queremos añadir una etiqueta con nombre 2015. /home/joan/Documentos/vacaciones\_2015.png: Indicamos la ruta del archivo que queremos que tenga la etiqueta 2015.

Una vez ejecutado el comando, la foto vacaciones\_2015.png dispondrá de las etiqueta 2015.

Para asignar la etiqueta 2015 a varios archivos y/o carpetas de forma simultanea tan solo hay ir añadiendo las rutas de los archivos y/o carpetas una detrás de otra. Por lo tanto para añadir la etiqueta 2015 en los archivos vacaciones\_2015.png y vacaciones\_2015\_1.png debemos ejecutar el siguiente comando:

> ```
> tracker tag --add=2015 '/home/joan/Documentos/vacaciones_2015.png' '/home/joan/Documentos/vacaciones_2015_1.png'
> ```

###### Nota: Recuerden que al arrastrar un grupo de archivos dentro de la terminal se copian las rutas de forma automática.

### Añadir una descripción a la etiqueta

En el hipotético caso que quisiéramos añadir una descripción a una etiqueta deberíamos usar la opción \--description.

Para añadir la descripción fotos del 2015 a la etiqueta 2015 tenemos que usar el siguiente comando:

> ```
> tracker tag --add=2015 --description=”fotos del 2015”
> ```

###### Nota: La parte roja del comando corresponde a la parte que añade la descripción a la etiqueta.

## ELIMINAR ETIQUETAS DE ARCHIVOS Y CARPETAS CON LA TERMINAL

Una vez conocido el proceso para crear las etiquetas tenemos que saber como eliminarlas en caso de necesidad.

### Eliminar una etiqueta presente en multitud de archivos y carpetas con la terminal

Si pretendemos eliminar una etiqueta de un plumazo sin importarnos los archivos y/o carpetas que la están usando hay que ejecutar un comando del siguiente tipo:

> ```
> tracker tag --delete=”nombre de la etiqueta a eliminar”
> ```

Por lo tanto, para eliminar la etiqueta 2015 de todos los archivos y carpetas tan solo deberíamos ejecutar el siguiente comando en la terminal:

> ```
> tracker tag --delete=2015
> ```

### Eliminar etiquetas de un archivo y/o carpeta en particular mediante la terminal

Si quieren eliminar una etiqueta de un archivo o carpeta en particular usaremos un comando del siguiente tipo:

> ```
> tracker tag --delete=”nombre de la etiqueta” ruta del archivo o carpeta
> ```

Por lo tanto si quiero eliminar la etiqueta 2015 del archivo vacaciones\_2015.png debo usar el siguiente comando:

> ```
> tracker tag --delete=”2015” /home/joan/Documentos/vacaciones_2015.png
> ```

## LLEVAR UN CONTROL DE LAS ETIQUETAS MEDIANTE LA TERMINAL

Si trabajamos con etiquetas es importante que las podamos controlar en todo momento. En caso contrario nuestra clasificación acabará siendo un caos.

Los comandos disponibles para poder llevar un control de nuestras etiquetas son los siguientes:

### Ver la totalidad de etiquetas disponibles

Para ver la totalidad de etiquetas disponibles con sus comentarios y saber el número de archivos y/o carpetas que contiene cada etiqueta deben ejecutar el siguiente comando en la terminal:

> ```
> tracker tag -t
> ```

### Ver los archivos que contienen cada una de las etiquetas en la terminal

Para consultar los archivos y directorios que contienen cada una de las etiquetas disponibles tenemos que ejecutar el siguiente comando en la terminal:

> ```
> tracker tag -t -s
> ```

### Consultar los archivos o carpetas que contiene una etiqueta

Si únicamente quieren ver los archivos y carpetas asociados a una etiqueta en concreto tienen que ejecutar el siguiente comando:

> ```
> tracker tag -t -s “nombre de la etiqueta”
> ```

### Averiguar las etiquetas que tiene asignadas un archivo o carpeta

Para averiguar las etiquetas que tiene asignadas un determinado archivo o carpeta ejecutamos el comando tracker tag seguido de la ruta del archivo o carpeta.

Por lo tanto si queremos analizar las etiquetas que tiene asignadas el archivo vacaciones\_2015.png tenemos que ejecutar el siguiente comando:

> ```
> tracker tag /home/joan/Documentos/vacaciones_2015.png
> ```

## ETIQUETAR ARCHIVOS Y CARPETAS USANDO NAUTILUS O TRACKER NEEDLE

En el siguiente enlace pueden ver como podemos etiquetar [archivos y carpetas de forma gráfica]({{< relref "/posts/etiquetar-archivos-y-carpetas-nautilus" >}}) usando Nautilus o Tracker-needle.

## USAR LAS ETIQUETAS PARA BUSCAR ARCHIVOS Y CARPETAS

Pueden consultar el siguiente enlace en el que encontrarán las indicaciones para [buscar nuestros archivos y carpetas mediante las etiquetas]({{< relref "/posts/buscar-archivos-y-carpetas-por-etiquetas" >}}) que hemos usando.
