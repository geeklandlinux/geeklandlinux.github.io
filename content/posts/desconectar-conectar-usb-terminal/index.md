---
title: "Desconectar y conectar un dispositivo USB con la terminal en Linux"
date: 2017-09-24
categories: 
  - "linux-2"
tags: 
  - "tips"
  - "usb"
  - "wifi"
cover:
  image: "images/conectar-desconectar-usb-terminal.jpg"
  relative: true
---

Hace unos meses que de forma aleatoria pierdo la conexión WiFi de mi ordenador. La únicas formas de volver a recuperar la conexión son reiniciar el ordenador o desconectar y volver conectar mi tarjeta de red inalámbrica USB. Para evitar tener que desconectar y conectar físicamente mi dispositivo USB, a continuación veremos como podemos conectar y desconectar un dispositivo USB mediante la terminal.<!--more-->

## IDENTIFICAR EL DISPOSITIVO USB QUE QUEREMOS DESCONECTAR Y/O CONECTAR

Inicialmente vamos a recopilar la información de los dispositivos USB disponibles en nuestro equipo. Para ello ejecutaremos el siguiente comando en la terminal:

> ```
> sudo lsusb -t
> ```

El resultado obtenido en mi caso es el siguiente:

| /: Bus 05.Port 1: Dev 1, Class=root\_hub, Driver=ehci-pci/8p, 480M \|\_\_ Port 3: Dev 10, If 0, Class=Vendor Specific Class, Driver=r8712u, 480M \|\_\_ Port 7: Dev 6, If 0, Class=Mass Storage, Driver=usb-storage, 480M /: Bus 04.Port 1: Dev 1, Class=root\_hub, Driver=uhci\_hcd/2p, 12M /: Bus 03.Port 1: Dev 1, Class=root\_hub, Driver=uhci\_hcd/2p, 12M /: Bus 02.Port 1: Dev 1, Class=root\_hub, Driver=uhci\_hcd/2p, 12M \|\_\_ Port 2: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M \|\_\_ Port 2: Dev 2, If 1, Class=Human Interface Device, Driver=usbhid, 1.5M /: Bus 01.Port 1: Dev 1, Class=root\_hub, Driver=uhci\_hcd/2p, 12M \|\_\_ Port 1: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M \|\_\_ Port 2: Dev 3, If 0, Class=Audio, Driver=snd-usb-audio, 12M \|\_\_ Port 2: Dev 3, If 1, Class=Audio, Driver=snd-usb-audio, 12M |
| :-- |

A partir de la salida de este comando deberéis ser capaces de identificar el dispositivo USB que queréis desconectar.

En mi caso la tarjeta de red inalámbrica USB corresponde al Bus 05 y al puerto número 3. Por lo tanto el Bus 05 y el puerto 3 es el que deberemos desconectar y conectar.

###### Nota: Si la información proporcionada por el comando no es suficiente pueden ejecutar el comando lsusb -v. Este comando les proporcionará mayor información para identificar los dispositivos USB.

## DESCONECTAR Y CONECTAR UN USB MEDIANTE LA TERMINAL

Una vez identificado el dispositivo USB lo podemos desconectar y conectar del siguiente modo.

### Desconectar un dispositivo usb usando la terminal

Para desconectar el dispositivo USB tenemos que ejecutar un comando del siguiente tipo en la terminal:

> ```
> echo 'numerobus-numeropuerto' |sudo tee /sys/bus/usb/drivers/usb/unbind
> ```

En mi caso la tarjeta de red inalámbrica corresponde al bus 05 y al puerto 3. Por lo tanto ejecutaré el siguiente comando en la terminal:

> ```
> echo '5-3' |sudo tee /sys/bus/usb/drivers/usb/unbind
> ```

Una vez ejecutado el comando nuestro dispositivo USB se desconectará.

### Conectar un dispositivo usb usando la terminal

Una vez desconectado lo podremos volver a conectar usando un comando del siguiente tipo:

> ```
> echo 'numerobus-numeropuerto' |sudo tee /sys/bus/usb/drivers/usb/bind
> ```

Hemos visto que la tarjeta de red inalámbrica corresponde al bus 05 y al puerto 3. Por lo tanto ejecutaré el siguiente comando en la terminal:

> ```
> echo '5-3' |sudo tee /sys/bus/usb/drivers/usb/unbind
> ```

Después de ejecutar el comando mi tarjeta de red inalámbrica se activará y volveré a disponer de conexión a internet.

## AUTOMATIZAR EL PROCESO DE DESCONECTAR Y CONECTAR UN DISPOSITIVO USB

Si lo consideramos oportuno podemos automatizar el proceso que acabamos de ver. Para ello tan solo tenemos que crear un script del siguiente modo.

Inicialmente creamos el archivo restart\_wifi.sh ejecutando el siguiente comando en la terminal:

> ```
> touch ~/restart_wifi.sh
> ```

Una vez creado el archivo lo abriremos ejecutando el siguiente comando en la terminal:

> ```
> nano ~/restart_wifi.sh
> ```

A continuación escribiremos el código encargado de desconectar y conectar nuestro dispositivo USB. En mi caso el código es el siguiente:

> ```
> #!/bin/bash
> echo '5-3' |sudo tee /sys/bus/usb/drivers/usb/unbind
> echo '5-3' |sudo tee /sys/bus/usb/drivers/usb/bind
> ```

Una vez introducido el código guardamos los cambios y cerramos el fichero. Finalmente daremos permisos de ejecución al script ejecutando el siguiente comando en la terminal:

> ```
> sudo chmod +x ~/restart_wifi.sh
> ```

En estos momentos podremos ejecutar el script ejecutando el siguiente comando en la terminal:

> ```
> sh ~/restart_wifi.sh
> ```

Así de forma fácil y sencilla podremos desconectar y conectar un dispositivo USB únicamente usando la terminal.

### Ejecutar el script que acabamos de crear con un doble click

Si nos molesta tener que ejecutar un comando en la terminal podemos usar el script haciendo doble click sobre un archivo .desktop. Para ello deberemos proceder del siguientes modo.

Inicialmente creamos el archivo .desktop Reiniciar Wifi que es el que usaremos para desconectar y conectar nuestro dispositivo USB. Para ello ejecutamos el siguiente comando en la terminal:

> ```
> touch ~/Escritorio/"Reiniciar Wifi.desktop"
> ```

Seguidamente editamos el archivo que acabamos de crear ejecutando el siguiente comando en la terminal:

> ```
> nano ~/Escritorio/"Reiniciar Wifi.desktop"
> ```

A continuación pegamos el siguiente código dentro del editor de texto:

> ```
> [Desktop Entry]
> #Nombre de la aplicación
> Name=Reiniciar Wifi
> #Comentario que aparece al seleccionar el lanzador
> Comment=Reinicia mi dispositivo WiFi
> #Comando que usaríamos para ejecutar el script desde la terminal
> Exec=sh '/home/joan/restart_wifi.sh'
> #Ruta del icono que queremos que tenga el lanzador
> Icon=/usr/share/icons/Numix/128/devices/network-wireless.svg
> #Para que el programa se ejecute en una terminal
> Terminal=true
> #Indicación del tipo de archivo .desktop
> Type=Application
> #Para que el programa se ubique en el apartado Accesorios de nuestro menú
> Categories=Utility
> #Desactivar la notificación de inicio del script
> StartupNotify=false
> ```

###### Nota: Los comentarios del código contienen la explicación de cada una de las líneas.

###### Nota: La líneas de color rojo son las únicas susceptibles de modificación

Una vez introducido el código guardamos los cambios y cerramos el fichero.

Finalmente damos permisos de ejecución al archivo .desktop ejecutando el siguiente comando en la terminal:

> ```
> chmod +x ~/Escritorio/"Reiniciar Wifi.desktop"
> ```

A partir de estos momentos, cada vez que haga doble click en el archivo Reiniciar Wifi.desktop se desconectará y conectará el dispositivo USB que hayamos definido.
