---
title: "Habilitar os-prober para que el grub pueda detectar los sistemas operativos"
date: 2023-06-24
categories: 
  - "linux-2"
tags: 
  - "grub"
  - "sistemas-operativos"
  - "solucion-de-problemas"
cover:
  image: "images/Habilitar-os-prober-para-que-el-grub-pueda-detectar-los-sistemas-operativos.jpg"
  relative: true
---

Debido a la configuración predeterminada del grub muchas distribuciones, como por ejemplo Debian, no es capaz de detectar el resto de sistemas operativos instalados en el equipo. Por este motivo en el siguiente artículo veremos como solucionar el problema que el grub no pueda detectar el resto de sistemas operativos instalados en nuestro equipo.<!--more-->

## ¿POR QUÉ EL GRUB NO CONSIGUE DETECTAR LOS OTROS SISTEMAS OPERATIVOS?

Muchas distribuciones deshabilitan la utilidad `os-prober` por temas de seguridad. Al deshabilitar `os-prober` el grub, o gestor de arranque, no será capaz de detectar y añadir las entradas del resto de sistemas operativos en el fichero de configuración `/boot/grub/grub.cfg`. Por lo tanto para solucionar el problema citado en el inicio del artículo tan solo tendremos habilitar `os-prober` del siguiente modo.

## HACER QUE EL GRUB VUELVA A DETECTAR LOS SISTEMAS OPERATIVOS HABILITANDO OS-PROBER

Lo primero a realizar es asegurar que tenemos instalado el paquete `os-prober`. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> ❯ sudo apt install os-prober
> ```

La función de `os-prober` es detectar el resto de sistema operativos instalados en nuestro equipo y añadir sus entradas en el fichero de configuración del grub. Para comprobar que funciona pueden ejecutar el siguiente comando:

> ```shell
> ❯ sudo os-prober
> [sudo] contraseña para joan: 
> /dev/sda8@/EFI/Microsoft/Boot/bootmgfw.efi:Windows Boot Manager:Windows:efi
> ```

**Nota:** Si se fijan os-prober ha detectado el sistema operativo Windows 10. Esto es así porque aparte de Debían mi equipo tiene un dual boot con Windows 10.

Acto seguido editaremos la configuración del gestor de arranque grub ejecutando el siguiente comando en la terminal:

> ```shell
> ❯ sudo  nano  /etc/default/grub
> ```

Cuando se abra el editor de texto introduciremos la siguiente línea en el fichero de configuración del grub. De esta forma aseguraremos que `os-prober` se pueda ejecutar sin problemas.

```shell
GRUB_DISABLE_OS_PROBER=false
```

**Nota:** Asegúrense que el fichero de configuración **no contenga** la línea `GRUB_DISABLE_OS_PROBER=true`. En el caso que la encuentren deben modificar `true` por `false`

Una vez introducida la línea guardaremos los cambios y cerramos el fichero. Finalmente tan solo tenemos que regenerar el fichero de configuración del grub ejecutando el siguiente comando en la terminal:

```shell
❯ sudo update-grub
Generating grub configuration file ...
Found background image: /usr/share/images/desktop-base/desktop-grub.png
Found linux image: /boot/vmlinuz-5.18.0-1-amd64
Found initrd image: /boot/initrd.img-5.18.0-1-amd64
Found linux image: /boot/vmlinuz-5.17.0-1-amd64
Found initrd image: /boot/initrd.img-5.17.0-1-amd64
Found linux image: /boot/vmlinuz-5.16.0-6-amd64
Found initrd image: /boot/initrd.img-5.16.0-6-amd64
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot entries.
Found Windows Boot Manager on /dev/sda8@/EFI/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for UEFI Firmware Settings ...
done
```

**Nota:** Si se fijan en la salida del comando podrán ver que ahora si se detectan otros sistemas operativos, como por ejemplo Windows.

Una vez regenerada la configuración, tan solo tienen que reiniciar el equipo y entonces podrán comprobar que ahora el grub detecta la totalidad de los sistemas operativos.

## QUE HACER SI EL PROBLEMA SIGUE SIN SOLUCIONARSE

En el caso que el método que acabamos de citar no les funcione pueden intentar reinstalar el grub siguiendo las instrucciones del siguiente [enlace]({{< relref "/posts/recuperar-el-grub" >}}).

### Fuentes

[https://www.baeldung.com/linux/grub-bootloader-add-new-os](https://www.baeldung.com/linux/grub-bootloader-add-new-os)
