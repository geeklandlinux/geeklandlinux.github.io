---
title: "Cómo gestionar los módulos del kernel en Linux de forma sencilla"
date: 2017-01-05
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "kernel"
  - "modulos-del-kernel"
cover:
  image: "images/Cómo-gestionar-módulos-del-kernel-Linux.png"
  relative: true
---

En el siguiente artículo veremos la totalidad de puntos necesarios para gestionar de forma adecuada los módulos del kernel de Linux. A continuación empezamos con la explicación.<!--more-->

## ¿QUÉ SON Y QUE FUNCIÓN TIENEN LOS MÓDULOS DEL KERNEL?

El kernel de Linux es modular porque permite insertar y eliminar código bajo demanda con el fin de añadir o quitar una funcionalidad.

Partiendo de esta base decimos que los módulos del kernel son fragmentos de código, abierto o cerrado, que nosotros podemos añadir o quitar del Kernel con el fin de añadir o quitar una funcionalidad.

Algunas de las funcionalidades que podemos añadir al núcleo mediante los módulos del kernel son las siguientes:

1. Usar los drivers privativos de nuestra tarjeta gráfica.
2. Registrar las temperaturas de componentes de nuestro ordenador.
3. Que los ventiladores de nuestro ordenador sean gestionados por el software creado por el fabricante de nuestro ordenador.
4. Hacer funcionar nuestro tarjeta de red wifi.
5. etc.

###### Nota: Básicamente los módulos del kernel nos permiten hacer funcionar nuestro hardware, o en el caso que ya funcione podemos hacer que lo haga de forma más eficiente.

## ¿QUÉ MÓDULOS DEL KERNEL TENEMOS DISPONIBLES?

Los módulos del kernel son archivos terminados con la extensión .ko que se almacenan en la ubicación /lib/modules/versión\_del\_kernel.

Por lo tanto ejecutando el siguiente comando en la terminal podemos averiguar la totalidad de módulos que tenemos disponibles.

> ```
> ls -R /lib/modules/$(uname -r)
> ```

###### Nota: el parámetro uname -r permite consultar los módulos de kernel para el kernel que estamos usando.

## SABER LOS MÓDULOS DEL KERNEL QUE ESTAMOS USANDO

Para obtener una lista de la totalidad de módulos del kernel que estamos usando tenemos que ejecutar el siguiente comando en la terminal:

> ```
> lsmod
> ```

Este comando nos mostrará la siguiente información:

 
|   **Campo**   |   **Información mostrada**   |
| --- | --- |
|   _Module_   |   Los nombres de los módulos que están activos.   |
|   _Size_   |   El tamaño que ocupa cada uno de los módulos cargados en la memoria.   |
|   _Used_   |   Indica si un módulo está siendo usado por otros módulo.   |

Otro comando alternativo que mostrará la misma información es el siguiente:

> ```
> cat /proc/modules
> ```

## CONOCER EL MÓDULO O DRIVER QUE ESTÁ USANDO UN DETERMINADO HARDWARE

En ocasiones nos puede interesar conocer el nombre del módulo que está usando un hardware determinado.

### Si el hardware está integrado en nuestro ordenador

Si queremos conocer el nombre módulo que está usando mi tarjeta gráfica abro una terminal y ejecuto el siguiente comando:

> ```
> lspci -v
> ```

Si analizamos los resultados obtenidos veremos lo siguiente:

> ```
> 02:00.0 VGA compatible controller: NVIDIA Corporation GT218 [GeForce 8400 GS Rev. 3] (rev a2) (prog-if 00 [VGA controller])
>     Subsystem: Device 196e:080b
>     Flags: bus master, fast devsel, latency 0, IRQ 26
>     Memory at fd000000 (32-bit, non-prefetchable) [size=16M]
>     Memory at d0000000 (64-bit, prefetchable) [size=256M]
>     Memory at ce000000 (64-bit, prefetchable) [size=32M]
>     I/O ports at ec00 [size=128]
>     [virtual] Expansion ROM at 000c0000 [disabled] [size=128K]
>     Capabilities: <access denied>
>     Kernel driver in use: nvidia
>     Kernel modules: nvidia
> ```

Si observamos el campo Kernel driver in use de nuestra tarjeta gráfica vemos que estamos usando el módulo nvidia. De esta forma puedo afirmar que estoy usando el driver privativo de nvidia.

Una vez conocemos el nombre del módulo podemos ver si está activado ejecutando el siguiente comando en la terminal:

> ```
> joan@debian:~$ lsmod | grep nvidia
> nvidia 10563584 44
> drm 360448 4 nvidia
> ```

Por o tanto está activado y funcionando juntamente con las dependencias de este módulo.

### Si el hardware usado está conectado vía usb

Si queremos conocer el módulo que utiliza un dispositivo de hardware conectado vía usb, como por ejemplo el teclado del ordenador, abrimos una terminal y ejecutamos el comando:

> ```
> usb-devices
> ```

Si analizamos los resultado obtenidos vemos lo siguiente:

> ```
> T: Bus=01 Lev=01 Prnt=01 Port=00 Cnt=01 Dev#= 2 Spd=1.5 MxCh= 0
>  D: Ver= 1.10 Cls=00(>ifc ) Sub=00 Prot=00 MxPS= 8 #Cfgs= 1
>  P: Vendor=046d ProdID=c31c Rev=64.00
>  S: Manufacturer=Logitech
>  S: Product=USB Keyboard
>  C: #Ifs= 2 Cfg#= 1 Atr=a0 MxPwr=90mA
>  I: If#= 0 Alt= 0 #EPs= 1 Cls=03(HID ) Sub=01 Prot=01 Driver=usbhid
>  I: If#= 1 Alt= 0 #EPs= 1 Cls=03(HID ) Sub=00 Prot=00 Driver=usbhid
> ```

Por lo tanto puedo concluir mi teclado está usando el módulo del kernel usbhid.

###### Nota: El comando usb-devices forma parte del paquete usbutils. Por lo tanto para poder utilizar el comando usb-devices hay que tener instalado el paquete usbutils.

## OBTENER INFORMACIÓN DE LOS MÓDULOS DEL KERNEL

Si queremos obtener información de uno de los módulos del kernel tan solo tenemos que ejecutar el comando modinfo seguido del nombre del módulo o de la ruta del módulo.

Por lo tanto si queremos obtener información acerca del módulo de nvidia ejecutamos el siguiente comando en la terminal:

> ```
> sudo modinfo /lib/modules/4.8.0-2-amd64/updates/dkms/nvidia-legacy-340xx.ko
> ```

Después de ejecutar el módulo obtendremos gran cantidad de información. Parte de la información que obtendremos es la siguiente:

 
|   **Campo**   |   **Información mostrada**   |
| --- | --- |
|   _filename_   |   La ruta donde se almacena el módulo.   |
|   _alias_   |   El tipo de licencia del módulo.   |
|   _licence_   |   El alias que recibe el módulo que en mi caso es char-major-195-\*   |
|   _version_   |   La versión del driver de Nvidia.   |
|   _description_   |   Una descripción de la funcionalidad del módulo.   |
|   _depends_   |   Las dependencias que tiene el módulo nvidia.   |
|   _intree_   |   Si el resultado es y entonces el driver que analizamos forma parte del propio kernel y no es un módulo del kernel.   |
|   _author_   |   Nombre del autor del driver.   |
|   _parm_   |   Se indican opciones de configuración que podemos aplicar a los módulos mientras se cargan.   |
|   _etc_   |   etc.   |

Para obtener más información de las posibilidades de modinfo abran una terminal y ejecuten el comando:

> ```
> man modinfo
> ```

## CONOCER LAS DEPENDENCIAS DE UN MÓDULO DEL KERNEL

Los módulos del kernel tienen dependencias. Esto quiere decir que para que un módulo funcione de forma adecuada necesita que otros módulos también estén activados.

Para conocer las dependencias de un módulo tan solo tienen que ejecutar el comando modprove --show-depends seguido del nombre del módulo.

De este modo, si quieren conocer las dependencias del driver realtek tan solo hay que ejecutar el siguiente comando:

> ```
> joan@debian:~$ sudo modprobe --show-depends realtek
> insmod /lib/modules/4.8.0-2-amd64/kernel/drivers/net/phy/libphy.ko 
> insmod /lib/modules/4.8.0-2-amd64/kernel/drivers/net/phy/realtek.ko
> ```

Por lo tanto tanto queda claro que en mi caso el módulo realtek tiene un módulo dependiente que es el libphy.

El archivo /lib/modules/version\_kernel/modules.dep es generado automáticamente por la utilidad depmod y contiene todo el árbol de dependencias de los módulos.

## MOSTRAR LA CONFIGURACIÓN DE UN MÓDULO DEL KERNEL

Para ver un listado de los parámetros de configuración de todos los módulos del kernel hay que ejecutar els siguiente en la terminal:

> ```
> sudo modprobe -c | less
> ```

Si únicamente queremos conocer la configuración sobre un determinado módulo, como por ejemplo el nouveau, ejecutamos el siguiente comando en la terminal:

> ```
> joan@debian:~$ sudo modprobe -c | grep nouveau
> blacklist nouveau
> alias pci:v000010DEd*sv*sd*bc03sc*i* nouveau
> alias pci:v000012D2d*sv*sd*bc03sc*i* nouveau
> ```

Viendo el texto podemos ver los alias del módulo nouveau y también podemos ver que esta completamente deshabilitado porque lo hemos introducido a la lista negra.

## CARGAR UN MÓDULO AL KERNEL JUNTO CON SUS DEPENDENCIAS

Cargar un módulo en el kernel conjuntamente con sus dependencias es muy sencillo. Pongamos el caso que queremos cargar el módulo vfat.

El primer paso a realizar es comprobar si el módulo está cargado. Para ello ejecutamos el comando lsmod | grep seguido del nombre del módulo:

> ```
> lsmod | grep vfat
> ```

Si el comando no nos proporciona ningún resultado quiere decir que el módulo no está cargado y por lo tanto lo podemos cargar sin problema alguno.

Seguidamente actualizamos la base de datos de dependencias de módulos ejecutando el siguiente comando en la terminal:

> ```
> sudo depmod
> ```

Finalmente cargamos el módulo ejecutando el comando sudo modprobe seguido del nombre del módulo:

> ```
> sudo modprobe -v vfat
> ```

Al ejecutar el comando se cargará el módulo vfat juntamente con todas sus dependencias en el kernel de nuestro equipo.

Como hemos visto anteriormente hay módulos que permiten introducir parámetros de configuración cuando los cargamos. Para conocer los parámetros que se pueden usar consulten el apartado obtener información de los módulos del kernel.

Modprobe tiene multitud de opciones para su ejecución. Para obtener información adicional sobre este comando abran una terminal y ejecuten el comando man modprobe.

## DESHABILITAR UN MÓDULO DEL KERNEL JUNTO CON SUS DEPENDENCIAS

Si después de cargar el módulo vfat nos arrepentimos y queremos deshabilitarlo tenemos que seguir los siguientes pasos:

En la terminal ejecutamos el comando sudo modprobe -rv seguido del nombre del módulo que queremos descargar. Por lo tanto para descargar el módulo vfat ejecutamos el siguiente comando en la terminal:

> ```
> sudo modprobe -rv vfat
> ```

Al ejecutar este comando se descargará el módulo vfat y todas sus dependencias del Kernel.

###### Nota: No apliquen el comando de este apartado a no ser que sea necesario. Si lo aplican y no lo vuelven a activar perderán la posibilidad de trabajar en sistemas de ficheros vfat.

## PONER A LA LISTA NEGRA UN MÓDULO DEL KERNEL

Puede darse el caso que existan 2 módulos activos realizando la misma función y esto puede generar conflictos.

Un caso típico y conocido es tener activados los drivers privativos de nvidia conjuntamente con los libres.

Para solucionar este punto disponemos de 2 soluciones:

1. Descargar uno de los 2 módulos. Para ello hay que seguir las instrucciones descritas en el apartado anterior.
2. Poner a la lista negra uno de los 2 módulos. De esta forma el módulo que está en la lista negra no se cargará nunca.

Para aplicar la segunda solución abrimos una terminal y ejecutamos el siguiente comando:

> ```
> sudo nano /etc/modprobe.d/blacklist.conf
> ```

Una vez se abra el editor de textos escribiremos el text blacklist seguido del nombre del módulo a bloquear. Por lo tanto para poner el módulo nouveau a la lista negra escribimos:

> ```
> blacklist nouveau
> ```

A continuación guardamos los cambios y cerramos el fichero. Si ninguno de los módulos activos depende de nouveau, la próxima vez que arranquemos el ordenador no se cargará el módulo nouveau.

Si queremos bloquear de forma absoluta el módulo nouveau podemos sustituir el comando blacklist nouveau por:

> ```
> install nouveau /bin/false
> ```

## AVERIGUAR SI EL DRIVER DE UN HARDWARE ESTÁ INCLUIDO EN EL KERNEL O FORMA PARTE DE UN MÓDULO EXTERNO

En ocasiones puede resultar útil saber si un driver está incluido directamente en el Kernel o lo está a través de un módulo.

Para ello accedemos a la terminal y ejecutamos el siguiente comando:

> ```
> cat /lib/modules/$(uname -r)/modules.builtin
> ```

La salida de este comando contendrá la totalidad de drivers que están incluidos directamente en el kernel.

Estos drivers estáticos en el kernel estarán siempre cargados en memoria listos para ser usados. Por lo tanto si tenemos cargados los drivers de un hardware que no usamos estamos desperdiciando recursos de nuestro ordenador. Por este motivo algunos usuarios de Linux configuran y compilan su propio Kernel. De este modo solo tienen cargado en memoria lo justo e imprescindible para que funcione su equipo.

Compilar un kernel no es tarea fácil. Hay usuarios que presumen de compilar su propio kernel, pero la gran mayoría de estos usuarios se dedican a compilar un kernel a partir del archivo de configuración del kernel de su distro. De esto modo no obtienen ningún beneficio de la compilación del kernel. Lo más difícil y el paso crucial para configurar un kernel es optimizar su archivo de configuración.

###### Nota: Los comandos rmmod e insmod no se detallan en el tutorial porque modprobe hace la misma tarea de forma más simple y efectiva.
