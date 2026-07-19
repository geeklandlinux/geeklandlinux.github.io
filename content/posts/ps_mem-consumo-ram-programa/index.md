---
title: "PS_MEM para ver el consumo de memoria RAM de un programa"
date: 2017-11-04
categories: 
  - "linux-2"
tags: 
  - "consejos"
  - "memoria"
  - "ram"
cover:
  image: "images/consumo-memoria-ram-de-un-programa.png"
  relative: true
---

A veces resulta difícil saber de forma exacta la memoria RAM que consume un programa. Esto es debido a que los monitores del sistema nos acostumbran a proporcionar datos por procesos y hay que tener en cuenta que un mismo programa puede lanzar varios procesos que consumen memoria y recursos. Una claro ejemplo de lo que estoy comentando es la siguiente salida del comando top:<!--more-->

|   ``` top - 17:40:46 up 7:25,   1 user,    load average: 0,20, 0,63, 0,79 Tasks: 212 total,   1 running,   211 sleeping,   0 stopped,   0 zombie  %Cpu(s): 6,1 us,   3,7 sy,   0,0 ni,   85,2 id,   4,9 wa,   0,0 hi,   0,2 si,   0,0 st  KiB Mem : 3275348 total,   96012 free,   2059148 used,   1120188 buff/cache  KiB Swap: 1116480 total,   1060188 free,   56292 used.   943472 avail Mem   PID   USER  PR  NI VIRT    RES     SHR    S  %CPU  %MEM  TIME+     COMMAND  739   root  20  0  553148  170884  67924  S  3,6   5,2   31:18.80  Xorg  2803  joan  20  0  1466368 281860  103248 S  3,3   8,6   27:42.77  chrome  2290  joan  20  0  1137612 20148   16868  S  2,6   0,6   11:01.44  conky  1754  joan  20  0  599820  44960   33060  S  1,3   1,4   0:00.56   xfce4-terminal  3295  joan  20  0  1044308 180712  83416  S  0,7   5,5   15:31.74  chrome  1592  joan  20  0  184056  20956   17392  S  0,3   0,6   2:27.08   xfwm4  3596  joan  20  0  803632  81564   56760  S  0,3   2,5   1:19.26   chrome  21596 joan  20  0  972676  160996  78556  S  0,3   4,9   0:12.28   chrome  24732 joan  20  0  996232  204444  87592  S  0,3   6,2   0:40.91   chrome  31922 joan  20  0  637044  48988   27788  S  0,3   1,5   1:54.88   chrome  … ```   |
| --- |

¿Cuanta memoria RAM está consumiendo Google Chrome?

Pues la verdad no lo se porque Google Chrome es un programa que puede tener abiertos entre 15 y 20 procesos.

Como los monitores del sistema tradicionales no nos sirven usaremos otros métodos.

## VER LA MEMORIA RAM QUE CONSUME UN PROGRAMA CON PS\_MEM

ps\_mem es un script de Python para conocer la memoria RAM que consumen los programas de nuestro equipo.

Para descargar el script tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> wget https://raw.githubusercontent.com/pixelb/ps_mem/master/ps_mem.py
> ```

###### [Web de desarrollador](https://www.pixelbeat.org/ "blog del autor del script ps_mem")

Una vez descargado podemos consultar sus instrucciones de uso ejecutando el siguiente comando en la terminal:

> ```
> sudo python ps_mem.py --help
> ```

Si queremos obtener el consumo detallado de memoria RAM de los programas que estamos usando ejecutaremos el siguiente comando:

> ```
> sudo python ps_mem.py -S
> ```

Una muestra de los resultados obtenidos en mi caso son los siguientes:

|   ``` Private       + Shared       = RAM used     Swap used     Program  ...  9.3 MiB     + 626.0 KiB    = 9.9 MiB         0.0 KiB            0.0 KiB     wicd  8.1 MiB     + 2.1 MiB       = 10.2 MiB       0.0 KiB            0.0 KiB     Thunar  8.5 MiB     + 1.9 MiB       = 10.5 MiB       0.0 KiB            0.0 KiB     panel-66-weather  13.4 MiB   + 715.0 KiB    = 14.1 MiB       0.0 KiB            0.0 KiB     tracker-store  12.3 MiB   + 3.0 MiB       = 15.2 MiB       0.0 KiB            0.0 KiB     tracker-needle  16.7 MiB   + 2.1 MiB       = 18.9 MiB       0.0 KiB            0.0 KiB     wicd-client  15.9 MiB   + 4.0 MiB       = 19.9 MiB       0.0 KiB            0.0 KiB     xfce4-terminal  15.0 MiB   + 5.2 MiB       = 20.3 MiB       0.0 KiB            0.0 KiB     xfdesktop  25.2 MiB   + 45.5 KiB      = 25.3 MiB       0.0 KiB            0.0 KiB     preload  41.3 MiB   + 326.5 KiB    = 41.7 MiB       0.0 KiB            0.0 KiB     tor  51.3 MiB   + 22.2 MiB     = 73.4 MiB       0.0 KiB            0.0 KiB     Xorg  76.4 MiB   + 17.7 MiB     = 94.1 MiB       0.0 KiB            0.0 KiB     evince  131.0 MiB + 634.0 KiB    = 131.6 MiB     0.0 KiB            0.0 KiB     dropbox  198.1 MiB + 4.0 MiB       = 202.1 MiB     0.0 KiB            0.0 KiB     soffice.bin  228.2 MiB + 35.2 MiB     = 263.3 MiB     0.0 KiB            0.0 KiB     reditr_app (5)  691.6 MiB + 123.8 MiB   = 815.4 MiB     0.0 KiB            0.0 KiB     chrome (12)  ------------------------------------------------------------- 2.0 GiB      0.0 KiB        224.0 KiB  ============================================================= ```   |
| --- |

En este caso de forma muy precisa vemos que el navegador Google Chrome tiene abiertos 12 procesos. Estos 12 procesos tienen un consumo de memoria RAM de 815,4 MB

## INSTALAR EL SCRIPT PS\_MEM

Si nos gusta ps\_mem lo podemos instalar en nuestro equipo de forma permanente. De esta forma podremos usar ps\_mem independiente de la ubicación en que estemos.

Para la instalación de ps\_mem primero tenemos que instalar el python-pip. Para ello ejecutamos el siguiente comando en la terminal:

> ```
> sudo apt-get install python-pip
> ```

###### Nota: El comando se deberá adaptar en función del gestor de paquetes que usen.

###### Nota: En archlinux, el paquete pythin-pop equivale al paquete python2-pip.

Una vez instalado este paquete instalaremos ps\_mem. Para ello, desde la ubicación donde nos descargamos el script ejecutamos el siguiente comando en la terminal:

> ```
> sudo pip install ps_mem
> ```

Una vez instalado el programa lo podremos usar sin necesidad estar en la misma ubicación del script.

Por lo tanto si queremos consultar el consumo de RAM de nuestros programas tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> sudo ps_mem
> ```

Si además quisiéramos que la información del consumo se vaya actualizando cada 10 segundos ejecutaríamos el siguiente comando en la terminal

> ```
> sudo ps_mem -w 10
> ```

Si algún día quieren desinstalar ps\_mem tan solo tendrán que ejecutar el siguiente comando en la terminal:

> ```
> sudo pip uninstall ps_mem
> ```
