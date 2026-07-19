---
title: "Servidor gráfico se desconecta aleatoriamente en XFCE 4.14 (Solución)"
date: 2020-05-01
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "xfce"
coverImage: "servidor-grafico-xfce-se-desconecta.png"
---

Desde la actualización a XFCE 4.14 he tenido problemas con el servidor gráfico de mi equipo. En mi caso de forma totalmente aleatoria se desconectaba el servidor gráfico de mi sistema operativo y la única forma de volver a levantarlo era reiniciarlo y perder todo el trabajo que no estuviera guardado.<!--more-->

Si miraba los logs del sistema operativo veía el siguiente mensaje de error:

> ```
> debian acpid: client "1150[0:0] has disconnected"
> ```

Según el [foro oficial de XFCE](https://forum.xfce.org/viewtopic.php?id=13233 "Enlace al foro oficial de XFCE donde ofrecen también la solución al problema") otros usuarios han experimentado problemas similares al mio. Algunos de ellos son los siguientes:

1. Alto consumo de CPU al conectar un monitor externo.
2. El puntero del ratón en ocasiones queda congelado.
3. Problemas para mostrar los efectos de transparencia.
4. Pantallazos negros.
5. Se producen problemas gráficos al aparecen menú contextual del gestor de archivos Thunar.

## ¿POR QUÉ SE DESCONECTA EL SERVIDOR GRÁFICO?

Para evitar problemas de tearing los desarrolladores del gestor de ventanas xfwm4 introdujeron 3 modos de vblank al pasar de la versión 4.12 a la versión 4.14 de XFCE. Al parecer estos cambios son los que están generando los problemas.

Básicamente el problema es que el modo predeterminado de vblank no funciona de forma adecuada con todas las tarjetas gráficas. Por lo tanto para solucionar el problema deberemos reemplazar el modo en que se realiza el proceso de vblank.

## EVITAR QUE EL SERVIDOR GRÁFICO SE DESCONECTE DE FORMA ALEATORIA EN XFCE 4.14

Para solucionar el problema tenemos que saber la marca de nuestra tarjeta gráfica. En mi caso uso una Nvidia. Si no sabéis la marca deberéis ejecutar el siguiente comando en la terminal:

> ```
> joan@debian:~$ sudo lspci -nn | grep VGA
> 02:00.0 VGA compatible controller [0300]: NVIDIA Corporation GT218 [GeForce 8400 GS Rev. 3] [10de:10c3] (rev a2)
> ```

A continuación tenemos que conocer el modo adecuado de vblank para nuestra tarjeta gráfica. Para ello pueden usar la siguiente guía:

 
|   **Modo vblank**   |   **Tarjeta gráfica**   |
| --- | --- |
|   glx o auto   |   Es el modo predeterminado. Acostumbra a funcionar en tarjetas gráfica **Intel**. También puede funcionar en **algunos modelos de tarjetas Nvidia y Ati**.   |
|   xpresent   |   Funciona en tarjetas **gráficas recientes** de las marca **Ati/amd**.   |
|   off   |   Deshabilitada vblank. Esta configuración funciona bien en la mayoría de tarjetas gráficas **Nvidia**.   |

En mi caso tengo una tarjeta Nvidia y con el modo predeterminado auto el servidor gráfico se desconecta. Por lo tanto la única opción que tengo para solucionar el problema es habilitar el modo off. Para ello tan solo tengo solo tengo que ejecutar el siguiente comando en la terminal:

> ```
> xfconf-query -c xfwm4 -p /general/vblank_mode -t string -s off --create
> ```

**Nota:** Si quisieran usar el modo xpresent tan solo tendrían que reempazar off por xpresent.

Una vez ejecutado el comando reinicien su ordenador. En mi caso después de aplicar este cambio todo funciona a la perfección y el servidor gráfico nunca más se ha vuelto a desconectar.

## COMPROBAR QUE EL NUEVO MODO DE VBLANK ESTÁ ACTIVADO

Una vez reiniciado el ordenador pueden ejecutar el siguiente comando para comprobar el modo de vblank que están usando.

> ```
> joan@debian:~$ xfconf-query -c xfwm4 -p /general/vblank_mode 
> off
> ```
