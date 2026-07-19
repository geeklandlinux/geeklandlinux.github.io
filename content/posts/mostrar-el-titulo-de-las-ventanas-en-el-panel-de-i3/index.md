---
title: "Mostrar el título de las ventanas en el panel de i3"
date: 2023-01-29
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "gestor-de-ventanas"
  - "i3"
  - "i3blocks"
  - "i3status"
cover:
  image: "images/titulo-de-las-ventanas-en-el-panel-de-i3.png"
  relative: true
---

La gran mayoría de [configuraciones del escritorio i3]({{< relref "/posts/instalar-configurar-y-usar-el-gestor-de-ventanas-i3-en-linux" >}}) prescinden de las barras de título de las ventanas y por esto motivo no se pueden ver los títulos de las ventanas. Una solución para los usuarios que quieren ver los títulos de las ventanas es añadir el título en el panel del escritorio del modo que citaremos a continuación.<!--more-->

**Nota:** El método mostrado a continuación es válido para los usuarios de i3 que usen los paneles i3blocks, i3status y polybar. Polybar ya dispone de módulos de serie para mostrar el título de las ventanas en el panel. Por lo tanto en Polybar podéis aplicar esta solución, pero existen otras opciones posiblemente mejores.

## MOSTRAR EL TÍTULO DE LA VENTANAS EN EL PANEL DEL GESTOR DE VENTANAS DE I3

Buscando por la red he encontrado diversas soluciones y scripts como por ejemplo [el siguiente](https://github.com/vivien/i3blocks-contrib/blob/master/i3-focusedwindow/i3-focusedwindow "script alternativo para conseguir el mismo resultado"). Funciona perfecto, pero el problema es que tiene un consumo de CPU exageradamente alto. Justo después de ejecutar el script el uso de CPU se incremente desmesuradamente hasta el punto de perjudicar al rendimiento del equipo:

![Consumo de CPU exageradamente alto](images/consumo-de-cpu.webp "Consumo de CPU exageradamente alto")

Como solución he aplicado el método que verán a continuación. El método que verán a continuación es útil y no afectará al rendimiento del equipo ya que su consumo de CPU es bajo.

Lo primero que tenemos que asegurar es que el paquete jq esté instalado en nuestro equipo. Para ello ejecutaremos el siguiente comando en la terminal:

```shell
sudo apt install jq
```

A continuación accederemos al fichero de configuración de nuestro panel, que en mi caso es i3blocks. Para ello en mi caso tengo que ejecutar el siguiente comando en la terminal:

```shell
nano ~/.config/i3/i3blocks.conf
```

Una vez se abra el editor de texto pegan el siguiente bloque de código en la posición que quieran que aparezca el título de la ventana:

```shell
[i3-focusedwindow]
command=i3-msg -t subscribe -m '[ "window" ]'| jq --unbuffered -Mrc '. | select(.container.focused == true).container.window_properties.title'
color=#5294e2
interval=persistent
```

Una vez pegado el código guardan los cambios y cierran el fichero. Reinician su entorno de escritorio o su equipo y acto seguido podrán observar el panel de su escritorio muestra el título de las ventanas.

![Título de las ventanas en el panel de i3](images/titulo-de-las-ventanas-en-el-panel-de-i3.webp "Título de las ventanas en el panel de i3")

Si en vuestro casos tenéis soluciones más originales o más eficientes les ruego que lo mencionen en los comentarios del blog.
