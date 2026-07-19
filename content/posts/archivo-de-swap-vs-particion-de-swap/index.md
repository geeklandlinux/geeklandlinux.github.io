---
title: "Archivo de swap versus partición de swap en GNU Linux"
date: 2016-12-28
categories: 
  - "linux-2"
tags: 
  - "archivo-de-swap"
  - "particion-de-swap"
  - "swap"
coverImage: "Archivo-de-swap-versus-Partición-Swap.jpg"
---

Una de las novedades de Ubuntu 17.04 será que en vez de usar una partición de Swap usará un archivo de Swap. Esto se ha anunciado a bombo y platillo como si hubieran descubierto las Americas y la realidad es que no tiene nada novedoso.

En Linux [hace muchísimos años que existen los archivos de Swap]({{< relref "/posts/crear-un-archivo-de-swap" >}}). Y si hasta el momento ninguna de las distros los ha usado será por algo.<!--more-->

## ¿POR QUÉ UBUNTU QUIERE USAR UN ARCHIVO DE SWAP?

Pienso que el único motivo es facilitar la vida a los usuarios más noveles.

Usar un archivo de Swap no aporta ninguna ventaja técnica ni absolutamente nada nuevo. Incluso me atrevería a decir que técnicamente es una solución peor por los siguientes motivos.

## ANÁLISIS DE LOS ARCHIVOS DE SWAP VERSUS LAS PARTICIONES DE SWAP

En principio no existe ninguna diferencia de rendimiento entre un archivo de Swap y una partición de Swap.

No obstante el archivo de Swap es técnicamente peor ya que presenta o puede presentar los siguientes inconvenientes.

### El archivo de Swap puede presentar fragmentación

Si creamos un archivo de Swap cuando tenemos el disco duro muy lleno existen posibilidades que creemos un archivo de Swap fragmentado.

Si el archivo de Swap está fragmentado implica que la paginación en disco se hará de forma más lenta perdiendo rendimiento.

Por este motivo tenemos que tener en cuenta los siguientes aspectos:

1. Si tenemos el disco duro lleno y necesitamos más Swap lo más conveniente es crear una partición de Swap.
2. No se recomienda ampliar un archivo de Swap. Si andamos cortos de memoria Swap lo ideal es crear una partición Swap o crear un archivo nuevo de Swap con una prioridad de uso baja.

Analizando el caso de Ubuntu este punto no será un problema porque la partición se crea en el momento que instalamos el sistema operativo. Por lo tanto los bloques que conformarán la partición Swap serán contiguos y no existirá fragmentación.

###### Nota: Una partición Swap nunca tendrá problemas de fragmentación.

###### Nota:  Este apartado solo afecta a la gente que usa discos duros mecánicos

### La velocidad de escritura y lectura es muy similar

La escritura en una partición de Swap es directa sin tener que seguir todo el proceso para escribir en un sistema de archivos.

En cambio en un archivo de Swap el proceso de escritura es sobre un sistema de archivos. No obstante todo este proceso es gestionado de forma especial por el Kernel de Linux.

El kernel de Linux mapea en todo momento lo que hay en el archivo de Swap. A partir de aquí entra y sale contenido de la memoria Swap de forma directamente omitiendo todas las restricciones que supone la escritura en un sistema de archivos.

### La partición de Swap permite aprovechar mejor el espacio en el disco duro

En el caso que usemos varias distros en el mismo equipo podemos usar la misma partición Swap en todas las distros. De esta forma podemos ahorrar espacio en disco.

En cambio si usamos archivos de Swap cada distro tendrá que usar su propio archivo de Swap provocando que necesitamos más espacio en nuestro disco duro.

Hoy en día este punto no es muy importante porque normalmente las capacidades de nuestros discos duros es elevada y siempre podemos usar dispositivos de almacenamiento externos o la nube.

### El archivo de swap es menos seguro frente posibles apagones y fallos imprevistos

Se que es forzar mucho la máquina, pero si usamos un archivo de Swap hay más posibilidades que se corrompa el sistema de ficheros en caso de un apagón o de un fallo imprevisto.

### No podemos saber la posición que ocupa el archivo de Swap en el disco

Existen usuarios que crean la partición swap al inicio de sus discos duros. Lo hacen así por que las velocidades de transferencia en el inicio del disco son mayores.

La práctica que acabo de citar solo es aplicable si usamos particiones de swap. Si usamos un archivo de swap no podremos controlar la posición en el disco duro en que se crea el espacio de intercambio.

### Los archivos de Swap son más flexible

Obviamente los archivos de swap son más flexibles que las particiones de swap.

Los archivos de swap permiten cambiar su tamaño o crear nuevos archivos de forma extremadamente fácil, sencilla y rápida.

## CONCLUSIONES

La principal ventaja de una partición de Swap es que nunca tendrá fragmentación y su funcionamiento es más adecuado técnicamente hablando.

Los puntos fuertes de un archivo de Swap serán su flexibilidad y la facilidad que por ejemplo pueden proporcionar a los usuarios noveles que usen Ubuntu 17.04. Si el archivo de swap se crea en un sistema fresco y con suficiente espacio en el disco duro tendrá el mismo rendimiento que una partición swap. Además cada año que pasa los espacios de intercambio son menos necesarios porque los ordenadores disponen de más memoria RAM.

Los archivos de swap no me parecen la opción técnica más adecuada. No obstante los archivos de swap funcionan de forma adecuada y considero que son útiles en casos en los que una partición de swap se nos quede corta.

Lo que quiere realizar Ubuntu no supondrá ninguna afectación en el rendimiento de esta distribución. Además el archivo de Swap seguro que facilitará la vida a los usuarios que usen por primera vez el sistema del pingüino.
