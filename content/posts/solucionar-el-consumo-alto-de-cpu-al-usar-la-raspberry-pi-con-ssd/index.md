---
title: "Solucionar el consumo alto de cpu al usar la Raspberry Pi con SSD"
date: 2021-02-27
categories: 
  - "raspberry-pi"
tags: 
  - "configuracion"
  - "disco-duro"
cover:
  image: "images/consumo-alto-de-cpu-al-usar-raspberry-pi-con-ssd.webp"
  relative: true
---

En mi caso uso un disco duro SSD en la Raspberry Pi 4. Seguí los siguientes pasos para [reemplazar la tarjeta MicroSD por un dispositivo de almacenamiento SSD]({{< relref "/posts/como-arrancar-la-raspberry-pi-4-desde-un-disco-ssd" >}}). Al seguir los pasos todo funcionaba perfecto, pero si noté un consumo alto de CPU. Mientras los valores de uptime deberían ser aproximadamente cero en mi caso eran los siguientes:<!--more-->

> ```shell
> pi@raspberrypi:~ $ uptime
>  09:54:17 up 5 min,  1 user,  load average: 0,60, 0,52, 0,44 
> ```

Acto seguido intenté visualizar los procesos que estaban consumiendo CPU. Los resultados obtenidos fueron los siguientes:

> ```shell
> pi@raspberrypi:~ $ ps -eo pcpu,pid,user,args --no-headers| sort -t. -nk1,2 -k4,4 -r |head -n 5
>  0.8   492 root     rclone mount --config /home/pi/.config/rclone/rclone.conf --allow-non-empty --dir-cache-time 15m --allow-other jellyfin: /media/pendrive/media/
>  0.6 10847 root     [kworker/2:2-mm_percpu_wq]
>  0.3 10211 root     [kworker/2:3-events]
>  0.2  9522 root     [kworker/2:1-events]
>  0.2  4264 root     [kworker/2:0-events]
> ```

**Nota**: Además el led verde de la Raspberry Pi estaba apagándose y encendiéndose constantemente.

## ¿POR QUÉ LA RASPBERRY PI CONSUME MÁS CPU AL TRABAJAR CON UN DISPOSITIVO DE ALMACENAMIENTO SSD?

Después de investigar en Internet descubrí que la Raspberry Pi detecta que no tenemos insertada ninguna tarjeta MicroSD. Esto ocasiona que la Raspberry Pi esté comprobando constantemente si hemos insertado una tarjeta MicroSD en nuestro dispositivo y esto conlleva un consumo elevado de CPU que en algunos casos reportados citan que es alrededor de un 10%.

Para solucionar este problema tenemos un par de soluciones.

## EVITAR EL CONSUMO EXCESIVO DE CPU CUANDO USAMOS LA RASPBERRY PI CON UN DISPOSITIVO DE ALMACENAMIENTO SSD O USB

Existen un par de soluciones que son válidas tanto para la Raspberry Pi 4 como para la Raspberry Pi 3.

**La primera de las soluciones** es insertar una tarjeta MicroSD sin contenido en la ranura de la Raspberry Pi y reiniciar el sistema operativo. De esta forma la Raspberry Pi detectará que hay una tarjeta MicroSD insertada y no se harán las comprobaciones constantes que consumen CPU.

**La segunda de las opciones,** y que es la que utilizo en mi caso, es añadir el parámetro `dtparam=sd_poll_once` en el fichero de configuración `/boot/config.txt`. Para ello ejecutaremos el siguiente comando en la terminal.

> ```shell
> pi@raspberrypi:~ $ sudo nano /boot/config.txt
> ```

Una vez se abra el editor de textos nano, justo al principio o al final del fichero añadiremos el siguiente parámetro:

> ```shell
> dtparam=sd_poll_once
> ```

A continuación guardaremos los cambios y reiniciaremos la Raspberry Pi. Una vez reiniciada observarán que los valores de carga son mucho bajos. En mi caso los valores obtenidos son los siguientes:

> ```shell
> pi@raspberrypi:~ $ uptime
>  09:54:17 up 5 min,  1 user,  load average: 0,02, 0,11, 0,06
> ```

Otro punto que observaremos es que el led de color verde de la Raspberry Pi ha dejado de funcionar. Esto es así porque mediante el comando que introducimos en `/boot/config.txt` hacemos que la Raspberry Pi únicamente verifique en el arranque si tenemos insertada una tarjeta MicroSD.

## CONCLUSIONES

En el caso que estén usando la Raspberry Pi y la arranquen mediante un dispositivo SSD o USB es probable que tengan el problema citado en este artículo. Por lo tanto les recomiendo que realicen las comprobaciones oportunas y sigan los consejos detallados en este artículo. De este modo dejarán de desaprovechar los recursos de su Raspberry Pi.

#### Fuente

[https://jamesachambers.com/raspberry-pi-cheap-ssd-upgrade-30/](https://jamesachambers.com/raspberry-pi-cheap-ssd-upgrade-30/)
