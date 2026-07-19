---
title: "Activar el trim en una raspberry pi arrancada por un ssd externo"
date: 2021-01-03
categories: 
  - "raspberry-pi"
tags: 
  - "configuracion"
  - "disco-duro"
  - "trim"
cover:
  image: "images/activar-el-trim-en-una-raspberry-pi-arrancada-por-un-ssd-externo.png"
  relative: true
---

Recientemente vimos como [arrancar nuestra Raspberry Pi a través de un dispositivo de almacenamiento SSD externo]({{< relref "/posts/como-arrancar-la-raspberry-pi-4-desde-un-disco-ssd" >}}). Para que el rendimiento del disco duro sea correcto y prolongar su vida útil es recomendable activar el TRIM. Para activar el TRIM en una Raspberry Pi arrancada por un SSD externo procederemos del siguiente modo.<!--more-->

**Nota**: Si no sabéis bien lo que estáis haciendo y solo queréis copiar y pegar las instrucciones recomiendo seguir el procedimiento que verán a continuación con solo un dispositivo de almacenamiento enchufado a la Raspebrry Pi.

## COMPROBAR SI TRIM ESTA ACTIVADO EN LA RASPBERRY PI

Para comprobar si TRIM está activado tienen que ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo fstrim -v /
> fstrim: /: the discard operation is not supported
> ```

**Nota**: El comando que acabamos de ejecutar es asumiendo que estamos arrancando la Raspberry Pi a través del ssd. Por este motivo como punto de montaje introducimos la partición root `/`.

En mi caso la salida del comando es `fstrim: /: the discard operation is not supported`. Por lo tanto TRIM no está habilitado. Para habilitarlo deberán seguir las indicaciones que encontrarán a continuación.

## ACTIVAR TRIM EN LA RASPBERRY PI 4 ARRANCADA A TRAVÉS DE UN SSD EXTERNO

Para activar TRIM de forma permanente deberán seguir las siguientes indicaciones:

### Verificar que TRIM es compatible con el firmware de nuestra unidad SSD

Ejecutamos el comando `lsblk -D` para ver el nombre con el que se detecta nuestra unidad SSD.

> ```shell
> pi@raspberrypi:~ $ lsblk -D
> NAME   DISC-ALN DISC-GRAN DISC-MAX DISC-ZERO
> sda           0        0B       0B         0
> ├─sda1        0        0B       0B         0
> └─sda2        0        0B       0B         0
> ```

Una vez sabemos que en mi caso se detecta como `/dev/sda` ejecutamos el siguiente comando para comprobar el valor de `Maximum unmap LBA count`. Este valor nos indica el número máximo de bloques que se podrán marcar como leídos.

> ```shell
> pi@raspberrypi:~ $ lsblk -D
> NAME   DISC-ALN DISC-GRAN DISC-MAX DISC-ZERO
> sda           0        0B       0B         0
> ├─sda1        0        0B       0B         0
> └─sda2        0        0B       0B         0
> pi@raspberrypi:~ $ sudo sg_vpd -p bl /dev/sda
> Block limits VPD page (SBC):
>   Write same non-zero (WSNZ): 0
>   Maximum compare and write length: 0 blocks [Command not implemented]
>   Optimal transfer length granularity: 8 blocks
>   Maximum transfer length: 65535 blocks
>   Optimal transfer length: 65535 blocks
>   Maximum prefetch transfer length: 65535 blocks
>   Maximum unmap LBA count: 8388480
>   Maximum unmap block descriptor count: 63
>   ...
> ```

Seguidamente ejecutamos el siguiente comando para averiguar el valor del parámetro `Unmap command supported (LBPU)`

> ```shell
> pi@raspberrypi:~ $ sudo sg_vpd -p lbpv /dev/sda
> Logical block provisioning VPD page (SBC):
>   Unmap command supported (LBPU): 1
>   Write same (16) with unmap bit supported (LBPWS): 0
>   Write same (10) with unmap bit supported (LBPWS10): 0
>   Logical block provisioning read zeros (LBPRZ): 0
>   Anchored LBAs supported (ANC_SUP): 1
>   Threshold exponent: 1
>   Descriptor present (DP): 0
>   Minimum percentage: 0 [not reported]
>   Provisioning type: 1 (resource provisioned)
>   Threshold percentage: 0 [percentages not supported]
> ```

Los valores de los parámetros `Maximum unmap LBA count` y `Unmap command supported (LBPU)` son mayores que cero. Por lo tanto podemos usar TRIM en nuestro dispositivo de almacenamiento. En otras palabras, nuestro dispositivo de almacenamiento tiene la capacidad de marcar los sectores que están libres para su uso.

### Hacer que el Kernel detecte nuestra unidad de Arranque externa SSD como un dispositivo capaz de realizar TRIM

Acto seguido ejecutaremos el siguiente comando para comprobar si el Kernel ha detectado que nuestro dispositivo de almacenamiento SSD externo es capaz de marcar los sectores del dispositivo de almacenamiento como libres.

> ```shell
> pi@raspberrypi:~ $ sudo find /sys/ -name provisioning_mode -exec grep -H . {} + | sort
> /sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb2/2-1/2-1:1.0/host0/target0:0:0/0:0:0:0/scsi_disk/0:0:0:0/provisioning_mode:full
> ```

Si os fijáis obtenemos las palabras `provisioning_mode:full` en la salida del comando . Por lo tanto el Kernel no sabe que nuestro disco duro es capaz de realizar TRIM. Para solucionar esto ejecutaremos los siguientes comandos:

> ```shell
> pi@raspberrypi:~ $ sudo su
> root@raspberrypi:/home/pi# echo unmap > /sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb2/2-1/2-1:1.0/host0/target0:0:0/0:0:0:0/scsi_disk/0:0:0:0/provisioning_mode
> ```

**Nota**: En vuestro caso deberéis reemplazar `/sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb2/2-1/2-1:1.0/host0/target0:0:0/0:0:0:0/scsi_disk/0:0:0:0/provisioning_mode` por el texto que hayas obtenido en el salida del comando que habéis ejecutado en el inicio de este apartado.

A continuación volvemos a realizar la misma comprobación que realizamos al inicio de este apartado. Por lo tanto ejecutamos los siguientes comandos:

> ```shell
> root@raspberrypi:/home/pi# exit
> exit
> pi@raspberrypi:~ $ sudo find /sys/ -name provisioning_mode -exec grep -H . {} + | sort
> /sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb2/2-1/2-1:1.0/host0/target0:0:0/0:0:0:0/scsi_disk/0:0:0:0/provisioning_mode:unmap
> ```

Ahora la salida del comando es `provisioning_mode:unmap`. Por lo tanto el kernel es capaz de detectar que nuestro disco puede marcar sectores como disponibles.

### Indicar el número máximo de bytes que se pueden marcar como libres en una sola operación

Tenemos que indicar el tamaño máximo de bytes que se pueden marcar como libres en una sola operación. Para ello lo primero que haremos es conocer el tamaño de un bloque de la unidad SSD ejecutando el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ sudo sg_readcap -l /dev/sda
> Read Capacity results:
>    Protection: prot_en=0, p_type=0, p_i_exponent=0
>    Logical block provisioning: lbpme=1, lbprz=0
>    Last LBA=234441647 (0xdf94baf), Number of logical blocks=234441648
>    Logical block length=512 bytes
>    Logical blocks per physical block exponent=3 [so physical block length=4096 bytes]
>    Lowest aligned LBA=0
> Hence:
>    Device size: 120034123776 bytes, 114473.5 MiB, 120.03 GB
> ```

Una vez obtenido el resultado vemos:

- El tamaño de un bloque es de **512 bytes**.
- En apartados anteriores vimos que el valor de `Maximum unmap LBA count` es **8388480**. Este valor nos indica el número máximo de bloques que se podrán marcar como libres en una sola operación.

Por lo tanto el número máximo de bytes que se pueden marcar como libres de utilización es:

> ```
> 8388480 bloques x 512 bytes = 4294901760 bytes
> ```

Para establecer este valor en la configuración ejecutaremos los siguientes comandos:

> **`pi@raspberrypi:~ $ sudo su root@raspberrypi:/home/pi# echo 4294901760 > /sys/block/sda/queue/discard_max_bytes`**

A partir de estos momentos ya podemos volver a ejecutar el comando `sudo fstrim -v /`. Ahora verán que TRIM se ejecuta sin problema alguno.

> ```shell
> pi@raspberrypi:~ $ sudo fstrim -v /
> ```

### Activar el trim en la Raspberry Pi cada vez que se arranque

Para que los cambios aplicados sean persistentes la próxima vez que arranquemos la Raspberry Pi identificaremos el ID de nuestro adaptador SATA a USB. Para ello ejecutaremos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ lsusb
> Bus 002 Device 002: ID 152d:0578 JMicron Technology Corp. / JMicron USA Technology Corp. JMS567 SATA 6Gb/s bridge
> Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
> Bus 001 Device 002: ID 2109:3431 VIA Labs, Inc. Hub
> Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
> ```

Una vez sabemos el identificador, que en mi caso es `152d:0578`, crearemos una regla udev ejecutando el siguiente comando en la terminal:

> ```shell
> sudo nano /etc/udev/rules.d/10-trim.rules
> ```

Cuando se abra el editor de textos nano pegaremos el siguiente código:

> ```shell
> ACTION=="add|change", ATTRS{idVendor}=="152d", ATTRS{idProduct}=="0578", SUBSYSTEM=="scsi_disk", ATTR{provisioning_mode}="unmap"
> ```

**Nota**: En vuestro caso tendréis que reemplazar los parámetros `` `**152d**` `` y `` `**0578**` `` por los de vuestro adaptador SATA a USB.

Una vez pegado el código guardaremos los cambios y cerraremos el fichero.

### Hacer que TRIM se ejecute de forma automática mediante un servicio de systemd

Finalmente habilitaremos TRIM mediante systemd ejecutando el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ sudo systemctl enable fstrim.timer
> ```

Con el servicio de systemd habilitado TRIM se ejecutará de forma totalmente automática una vez por semana. La periodicidad de ejecución se puede modificar, pero una vez por semana es adecuado para prácticamente la totalidad de usuarios.

#### Fuentes

[https://www.jeffgeerling.com/blog/2020/enabling-trim-on-external-ssd-on-raspberry-pi](https://www.jeffgeerling.com/blog/2020/enabling-trim-on-external-ssd-on-raspberry-pi)

[https://lemariva.com/blog/2020/08/raspberry-pi-4-ssd-booting-enabled-trim](https://lemariva.com/blog/2020/08/raspberry-pi-4-ssd-booting-enabled-trim)
