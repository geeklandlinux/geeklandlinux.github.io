---
title: "Configurar la pantalla del ordenador con xrandr en Linux"
date: 2023-07-16
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "pantalla"
  - "xrandr"
coverImage: "Configurar-la-pantalla-del-ordenador-con-xrandr-en-Linux.jpg"
---

En el mundo actual, tener una pantalla correctamente configurada es esencial para una experiencia informática óptima. Ya sea que esté utilizando un solo monitor o una configuración de múltiples monitores, la utilidad xrandr es una herramienta poderosa que puede ayudarlo a configurar la pantalla en un sistema Linux que ejecuta el servidor gráfico X.Org. En este artículo de blog, detallaremos como podemos usar la utilidad xrandr para configurar la pantalla o pantallas de su ordenador.<!--more-->

## ¿QUÉ ES XRANDR?

Xrandr es una utilidad de línea de comandos **para el sistema X org** que permite administrar la configuración de su pantalla. Algunos de los parámetros de nuestro monitor que permite configurar son:

1. La resolución.
2. La orientación.
3. La frecuencia.
4. El escalado.
5. El brillo.
6. El parámetro gamma.
7. etc.

Es particularmente útil para configurar múltiples monitores y configuraciones de pantalla personalizadas en administradores de ventanas como i3 o en entornos de escritorio que usen el servidor gráfico Xorg.

## ¿CUANDO PODEMOS USAR LA UTILIDAD XRANDR PARA CONFIGURAR LA PANTALLA?

Siempre y cuando uséis el servidor gráfico X org podréis usar Xrandr. En el caso que uséis un escritorio con el servidor gráfico Wayland tendréis que usar la utilidad wlr-randr.

## OBTENER INFORMACIÓN DEL MONITOR O PANTALLA

Antes de configurar nuestro monitor tenemos que tener claras sus características y especificaciones. De esta forma podremos extraer el máximo rendimiento. En mi caso después de investigar en las especificaciones de mi monitor y en Internet he llegado a las siguientes conclusiones:

1. Resolución máxima de mi monitor es de 1920x1080
2. La Frecuencia máxima de mi monitor es 144Hz
3. Los DPI ideales de mi monitor son 82.

**Nota:** Los DPI ideales vienen determinados por el tamaño y la resolución del monitor. En mi caso el monitor que tengo es de 27" y la resolución máxima es de 1920x1080. Por lo tanto si visitan la siguiente [página Web](https://dpi.lv/) verán que el número ideal es de 82.

## OBTENER INFORMACIÓN DE COMO EL SISTEMA OPERATIVO RECONOCE NUESTRO MONITOR

Mediante el comando `xrandr` obtendremos información la siguiente información:

1. Nombre con que se reconocen los monitores conectados a nuestro ordenador.
2. Nombre del puerto en el que está conectado nuestro monitor.
3. Resoluciones y frecuencias disponibles en cada uno de nuestros monitores.
4. La resolución y frecuencia actual de nuestro monitor.

Para obtener la información que acabamos de citar tan solo tenemos que ejecutar el comando `xrand` del siguiente modo.

> ```shell
> ❯ xrandr
> Screen 0: minimum 320 x 200, current 1920 x 1080, maximum 16384 x 16384
> HDMI-1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
>    1920x1080     60.00*+ 144.00   120.00   119.88   119.98    74.97    50.00    59.94  
>    1920x1080i    60.00    50.00    59.94  
>    1280x1024     75.02    60.02  
>    1280x720      60.00    50.00    59.94  
>    1024x768     119.99    99.97    75.03    70.07    60.00  
>    832x624       74.55  
>    800x600      119.97    99.99    72.19    75.00    60.32    56.25  
>    720x576       50.00  
>    720x480       60.00    59.94  
>    640x480      119.99   100.00    75.00    72.81    66.67    60.00    59.94  
>    720x400       70.08  
> HDMI-2 disconnected (normal left inverted right x axis y axis)
> ```

Después de ver la salida del comando llegamos a las siguientes conclusiones:

1. El monitor se reconoce con el nombre `Screen 0`. Únicamente tengo un monitor.
2. El monitor se comunica con nuestro ordenador mediante el puerto `HDMI-1`
3. La resolución máxima que puedo utilizar es 1920x1080. Además ya tengo configurado el sistema operativo para que pueda usar al resolución máxima
4. La frecuencia de actualización que estoy usando es 60Hz. Pero veo que la frecuencia máxima que puede usar mi monitor de es de 144Hz.

A continuación ejecutaremos el comando `xdpyinfo | grep -B2 resolution` para ver los los puntos por pulgada (DPI) de nuestro monitor.

> ```shell
> ❯ xdpyinfo | grep -B2 resolution
> screen #0:
>   dimensions:    1920x1080 pixels (508x285 millimeters)
>   resolution:    96x96 dots per inch
> ```

Tal y como pueden en la salida del comando estoy usando 96 DPI.

## CONFIGURAR LA RESOLUCIÓN DE LA PANTALLA

Acabamos de ver que mi monitor tiene y está usando una resolución de 1920x1080. Por lo tanto estoy usando la resolución máxima y no sería necesario configurar este parámetro. No obstante si lo quieren hacer tan solo tiene que ejecutar un comando del siguiente tipo:

> ```shell
> xrandr --output SALIDA_DEL_MONITOR --mode ANCHOxALTO
> ```

Por lo tanto para configurar la resolución a 1920x1080 tendré que ejecutar el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --mode 1920x1080
> ```

## CONFIGURAR LA FRECUENCIA DEL MONITOR

Anteriormente hemos visto que la frecuencia con la que está trabajando nuestro monitor es 60Hz. En mi caso quiere que trabaje a la frecuencia máxima de 144Hz y a una resolución de 1920x1080. Para ello ejecutaré un comando del siguiente tipo

> ```shell
> xrandr --output SALIDA_DEL_MONITOR --mode ANCHOxALTO --rate FRECUENCIA_DESEADA
> ```

Por lo tanto en mi caso tendré que ejecutar el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00
> ```

Si ahora volvemos a ejecutar el comando xrandr podremos ver que efectivamente estamos usando una frecuencia de 144Hz.

> ```shell
> ❯ xrandr -q
> Screen 0: minimum 320 x 200, current 1920 x 1080, maximum 16384 x 16384
> HDMI-1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
>    1920x1080     60.00 + 144.00*  120.00   119.88   119.98    74.97    50.00    59.94  
>    1920x1080i    60.00    50.00    59.94  
>    1280x1024     75.02    60.02  
>    1280x720      60.00    50.00    59.94  
>    1024x768     119.99    99.97    75.03    70.07    60.00  
>    832x624       74.55  
>    800x600      119.97    99.99    72.19    75.00    60.32    56.25  
>    720x576       50.00  
>    720x480       60.00    59.94  
>    640x480      119.99   100.00    75.00    72.81    66.67    60.00    59.94  
>    720x400       70.08  
> HDMI-2 disconnected (normal left inverted right x axis y axis)
> ```

Una frecuencia de 144 Hz no ofrece ventajas significativas en el caso que se realicen tareas ofimáticas y el consumo de energía del monitor será mayor. No obstante prefiero usar 144 Hz porque el parpadeo de la pantalla que percibirá nuestro ojo será menor y por lo tanto ayudará a disminuir la fatiga visual.

## MODIFICAR LOS DPI DE NUESTRO MONITOR

Vimos que los DPI de nuestro monitor son 96. El número ideal es de 82. Para cambiar este parámetro y además asegurar que tenemos una resolución de 1920x1080 y una frecuencia de 144 Hz tendremos que ejecutar un comando del siguiente tipo:

> ```shell
> xrandr --output SALIDA_DEL_MONITOR --mode ANCHOxALTO --rate FRECUENCIA_DESEADA --dpi NUMERO_DE_DPI
> ```

Por lo tanto tendré que ejecutar el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 82
> ```

Si ahora ejecuto el comando `xdpyinfo | grep -B2 resolution` veré que efectivamente los DPI son 82.

> ```shell
> ❯ xdpyinfo | grep -B2 resolution
> screen #0:
>   dimensions:    1920x1080 pixels (594x334 millimeters)
>   resolution:    82x82 dots per inch
> ```

## MODIFICAR EL ESCALADO O FACTOR DE ESCALA DE NUESTRO MONITOR

Si la imagen mostrada por el monitor es demasiado grande o demasiado pequeña puede ser recomendable realizar un escalado. El escalado se puede realizar tanto en el eje de las X como en el eje de las Y.

Para mantener la configuración anterior y realizar un escalado podemos aplicar un comando del siguiente tipo:

> ```shell
> xrandr --output SALIDA_DEL_MONITOR --mode ANCHOxALTO --rate FRECUENCIA_DESEADA --dpi NUMERO_DE_DPI --scale ESCALADO_XxESCALADO_Y
> ```

Como en mi caso no quiero ejecutar ningún tipo de escalado uso el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 82 --scale 1x1
> ```

**Nota:** En el caso que quisieran disminuir el tamaño de los objetos mostrados en pantalla podrían ejecutar el comando `xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 82 --scale 1.1x1.1`. En el caso que los quisieran incrementar `xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 82 --scale 0.9x0.9`

## ¿CÓMO ROTAR LA PANTALLA CON XRANDR?

Puede darse el caso que alguien tenga que rotar el monitor 90, 180 o 270º. Para ello tan solo tendrán que usar el parámetro `rotate` del siguiente modo:

Para girar el monitor 90 grados hacia la derecha tan solo tendrán que ejecutar el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --rotate right
> ```

Si quieren volver a la ortientación estándard tendrán que ejecutar el comando:

> ```shell
> ❯ xrandr --output HDMI-1 --rotate normal
> ```

Otros valores que puede tomar el parámetro `rotate` son `left` e `inverted`

## CAMBIAR EL BRILLO Y EL PARÁMETRO GAMMA DE LA PANTALLA

En principio el brillo de monitor me gusta y por lo tanto lo dejaré tal cual con el valor 1. No obstante en mi caso me gustaría tener un pelín más de contraste y un tono más oscuro. Por lo tanto fijaré el valor de gamma en `0.9`. Para hacer lo que acabo de citar tendré que ejecutar el siguiente comando en la terminal:

> ```shell
> ❯ xrandr --output HDMI-1 --brightness 1 --gamma 0.9
> ```

Si no les gusta el resultado obtenido pueden experimentar con otros valores de brillo y gamma.

## CONFIGURAR LA PANTALLA EN CASO DE TENER MÚLTIPLES MONITORES

También podemos usar xrandr en el caso que tengamos más de un monitor. Supongamos que tenemos los siguientes monitores:

1. Monitor conectado al puerto DP-1: 1920x1080, 144 Hz, 82 dpi
2. Monitor conectado al puerto HDMI-1: 1920x1080, 144 Hz, 96 dpi

### Configurar la pantalla principal o primaria

En el caso que tengan 2 monitores lo primero que tendrán que realizar es configurar el monitor principal o primario. En mi caso quiero que le monitor conectado el Display port 1 (DP-1) sea el principal. Por lo tanto ejecutaré el siguiente comando:

> ```shell
> ❯ xrandr --output DP-1 --primary
> ```

Acto seguido configuraremos su resolución, frecuencia y DPI ejecutando el siguiente comando:

> ```shell
> ❯ xrandr --output DP-1 --mode 1920x1080 --rate 144.00 --dpi 82 --scale 1x1
> ```

### Configurar el monitor o pantalla secundaria

Acto seguido configuraremos el monitor secundario que es el que está conectado al puerto `HDMI-1`. El monitor secundario tendrá una resolución de 1920x1080, una frecuencia de 144Hz y 96 dpi. Como el monitor secundario `HDMI-1` está a la derecha del monitor principal `DP-1` ejecutaré el siguiente comando:

> ```shell
> ❯ xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 96 --scale 1x1 --right-of DP-1
> ```

**Nota:** El parámetro `--right-of` indica la posición del monitor secundario respecto al principal. Otros parámetros alternativos a `--right-of` son:

- `--left-of`
- `--above`
- `--below`

## COMO HACER QUE LA CONFIGURACIÓN DE XRANDR SEA PERSISTENTE

La configuración que acabamos de establecer no se mantendrá si reiniciamos el ordenador. Si queremos que la configuración se aplique cada arrancamos el ordenador tenemos varias soluciones. Como en mi caso uso el escritorio i3 tan solo tendré que añadir las siguientes líneas en el fichero de configuración ubicado en `~/.config/i3`.

```shell
# Configuración de la pantalla
exec --no-startup-id xrandr --output HDMI-1 --mode 1920x1080 --rate 144.00 --dpi 82 --scale 1x1
exec --no-startup-id xrandr --output HDMI-1 --brightness 1 --gamma 0.9
```

De este modo, cada vez que iniciemos el ordenador el monitor que esté conectado al puerto HDMI-1 tendrá:

1. Una resolución de 1920x1080.
2. Una frecuencia de 144 Hz
3. 82 dpi
4. El parámetro gamma se modificará de 1 a 0.9

**Nota:** En el caso que no usen el escritorio i3 pueden crear un script que se ejecute en el momento de iniciar le ordenador.

## CONCLUSIONES

En este artículo, discutimos cómo usar la utilidad xrandr para configurar su pantalla en el entorno de escritorio i3 en Linux. Siguiendo estos pasos, puede configurar fácilmente su configuración de pantalla, administrar múltiples monitores y crear un espacio de trabajo personalizado adaptado a sus necesidades. Si quieren profundizar más sobre el uso de xrandr les recomiendo que abran una terminal y ejecuten el comando `xrandr --help`

### Fuentes

[https://linuxconfig.org/how-to-configure-your-monitors-with-xrandr-in-linux](https://linuxconfig.org/how-to-configure-your-monitors-with-xrandr-in-linux)

[https://wiki.archlinux.org/title/Xrandr](https://wiki.archlinux.org/title/Xrandr)
