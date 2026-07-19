---
title: "Cómo desactivar servicios no deseados en Linux con Systemd"
date: 2023-08-13
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "sysadmin"
  - "systemd"
coverImage: "Como-detener-y-deshabilitar-servicios-no-deseados-del-sistema-Linux.jpg"
---

Las distribuciones Linux incluyen varios servicios o demonios habilitados por defecto, algunos de los cuales podrían no ser utilizados por los usuarios. Estos servicios innecesarios se ejecutan en segundo plano y consumen recursos del sistema. En este artículo, aprenderás cómo listar todos los servicios en ejecución y cómo desactivar aquellos que no se están utilizando mediante [systemd]({{< relref "/posts/systemctl-administrar-servicios-linux" >}}).<!--more-->

**Nota:** En GNU/Linux, los servicios o "demonios" se refieren a procesos que se ejecutan en segundo plano para realizar diversas tareas del sistema. Estos demonios o servicios son administrados por el sistema de inicio, que en las distribuciones más modernas suele ser systemd. Para listar los demonios en ejecución y activarlos o desactivarlos, sigue los pasos que se describen a continuación.

### LISTAR LA TOTALIDAD DE SERVICIOS O DEMONIOS EN EJECUCIÓN

Lo primero que tenemos que realizar es listar la totalidad de servicios o demonios gestionados por systemd. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> systemctl list-unit-files --type service --all
> ```

El resultado obtenido será algo parecido a lo que verán a continuación:

> ```shell
> ❯ systemctl list-unit-files --type service --all
> UNIT FILE                                  STATE           PRESET  
> acpid.service                              disabled        enabled 
> alsa-restore.service                       static          - 
> alsa-state.service                         static          - 
> alsa-utils.service                         masked          enabled 
> anacron.service                            disabled        enabled 
> apparmor.service                           enabled         enabled 
> apt-daily-upgrade.service                  static          - 
> apt-daily.service                          static          - 
> autovt@.service                            alias           - 
> avahi-daemon.service                       enabled         enabled 
> blueman-mechanism.service                  enabled         enabled 
> bluetooth.service                          enabled         enabled 
> bolt.service                               static          - 
> colord.service                             static          - 
> configure-printer@.service                 static          - 
> console-getty.service                      disabled        disabled
> console-setup.service                      enabled         enabled 
> ...
> ```

La columna "STATE" en la salida del comando `systemctl list-units --type=service` indica el estado actual del servicio. Los posibles estados y sus significados son los siguientes:

- **enabled:** El servicio está habilitado y se inicia automáticamente durante el arranque.
- **disabled:** El servicio está deshabilitado y no se inicia de forma automática durante el arranque.
- **masked:** El servicio está completamente deshabilitado y no se puede iniciar de ningún modo sin previamente desenmascararlo. Este estado se utiliza para evitar que un servicio se inicie accidentalmente o para indicar que el servicio está deshabilitado de forma intencional.
- **static:** Servicios que están configurados para iniciarse automáticamente al iniciar el sistema. Estos servicios pueden estar activos o inactivos, pero siempre están disponibles para cuando se necesite usarlos. Estos servicios no se pueden activar ni desactivar, pero se pueden enmascarar. Normalmente estos servicios son esenciales para el correcto funcionamiento del sistema.
- **alias:** Indica que el servicio es un alias o un enlace simbólico a otro servicio. Esto significa que el servicio en sí mismo no existe, pero se utiliza para referirse a otro servicio con un nombre diferente.
- **generated:** Se refiere a los servicios generados automáticamente por el sistema o por otras herramientas. Estos servicios pueden ser generados dinámicamente según las necesidades del sistema o como parte de la configuración de otros servicios.
- **Indirect:** El servicio en sí no está habilitado directamente, pero su activación depende de otro servicio o unidad. En otras palabras, un servicio con estado "indirect" no se inicia por sí mismo, sino que se activa como resultado de la activación de otro servicio relacionado.

La columna "PRESET" en la salida del comando `systemctl list-units --type=service` indica la configuración de inicio predeterminada de un servicio en particular. Los posibles estados y sus significados son los siguientes:

- **enabled**: El servicio está habilitado según la política de inicio predeterminada.
- **disabled**: El servicio está deshabilitado según la política de inicio predeterminada.
- **\-**: No se proporciona información sobre la configuración de inicio predeterminada para esta unidad.

### IDENTIFICAR QUE HACE CADA UNOS DE LOS DEMONIOS O SERVICIOS Y DECIDIR SI LOS QUEREMOS DESACTIVAR O PARAR

Una vez que conocemos la totalidad de servicios presentes en nuestro sistema Linux, es importante saber lo que hacen para decidir si son necesarios para nosotros. Para descubrir lo que hace cada uno de los servicios, existen varias herramientas disponibles como por ejemplo Bing Chat, Bard, Perplexity, etc.

Algunos servicios que en mi caso están activos y no necesito son los siguientes:

| Demonio | Función |
| --- | --- |
| cups-browsed.service | Su función principal es descubrir y conectar automáticamente impresoras compartidas en una red local y hacerlas disponibles para los usuarios locales del sistema |
| cups.service | Su función es proporcionar la infraestructura necesaria para administrar y manejar las impresoras y trabajos de impresión en el sistema operativo |
| ModemManager.service | Administrar y controlar los módems conectados al sistema, como los módems USB 3G/4G/LTE, módems internos de portátiles, módems GSM, entre otros dispositivos de comunicación móvil. |
| openvpn.service | Demonio del cliente de OpenVPN. |
| redsocks.service | Demonio del proxy transparente redsocks. |

A continuación procederé a desactivar los servicios del siguiente modo.

**Nota:** Hay que ser extremadamente cauteloso a la hora de deshabilitar servicios. Hay servicios que son esenciales y si los deshabilitamos podemos dejar inservible nuestro sistema operativo.

### DESACTIVAR LOS SERVICIOS O DEMONIOS QUE NO USAMOS PARA AHORRAR RECURSOS

En el apartado anterior he listado una serie de servicios que tengo levantados y no estoy usando. Así que para parar el servicio `cups-browsed.service` ejecutaremos el siguiente comando:

> ```shell
> ❯ sudo systemctl stop cups-browsed.service
> ```

Y para hacer que no se vuelva a levantar al iniciar el sistema ejecutaremos el siguiente comando:

> ```shell
> ❯ sudo systemctl disable cups-browsed.service
> Synchronizing state of cups-browsed.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install disable cups-browsed
> Removed "/etc/systemd/system/multi-user.target.wants/cups-browsed.service".
> ```

Del mismo modo que deshabilitamos el demonio `cups-browsed.service` haremos lo mismo con el resto de servicios.

> ```shell
> ❯ sudo systemctl stop cups.service
> ❯ sudo systemctl disable cups.service
> Synchronizing state of cups.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install disable cups
> Removed "/etc/systemd/system/sockets.target.wants/cups.socket".
> Removed "/etc/systemd/system/printer.target.wants/cups.service".
> Removed "/etc/systemd/system/multi-user.target.wants/cups.service".
> Removed "/etc/systemd/system/multi-user.target.wants/cups.path".
> 
> ❯ sudo systemctl stop ModemManager.service
> ❯ sudo systemctl disable ModemManager.service
> Removed "/etc/systemd/system/dbus-org.freedesktop.ModemManager1.service".
> Removed "/etc/systemd/system/multi-user.target.wants/ModemManager.service".
> 
> ❯ sudo systemctl stop redsocks.service
> ❯ sudo systemctl disable redsocks.service
> Synchronizing state of redsocks.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install disable redsocks
> Removed "/etc/systemd/system/multi-user.target.wants/redsocks.service".
> 
> ❯ sudo systemctl stop openvpn.service
> ❯ sudo systemctl disable openvpn.service
> Synchronizing state of openvpn.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install disable openvpn
> Removed "/etc/systemd/system/multi-user.target.wants/openvpn.service".
> ```

**Nota:** A la hora de desactivar servicios del sistema hay que ser extremadamente cautelosos. Si se desactiva un servicio esencial puede darse el caso que nuestro sistema operativo deje de funcionar.

Si algún día precisarán usar algunos de los demonios deshabilitados, como por ejemplo `cups.service` y `cups-browsed.service` se podrían volver a levantar y habilitar del siguiente modo:

> ```shell
> ❯ sudo systemctl enable cups.service
> [sudo] contraseña para joan: 
> Synchronizing state of cups.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install enable cups
> Created symlink /etc/systemd/system/printer.target.wants/cups.service → /lib/systemd/system/cups.service.
> Created symlink /etc/systemd/system/multi-user.target.wants/cups.service → /lib/systemd/system/cups.service.
> Created symlink /etc/systemd/system/sockets.target.wants/cups.socket → /lib/systemd/system/cups.socket.
> Created symlink /etc/systemd/system/multi-user.target.wants/cups.path → /lib/systemd/system/cups.path.
> 
> ❯ sudo systemctl start cups.service
> ```
> 
> ```shell
> ❯ sudo systemctl enable cups-browsed.service
> Synchronizing state of cups-browsed.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install enable cups-browsed
> Created symlink /etc/systemd/system/multi-user.target.wants/cups-browsed.service → /lib/systemd/system/cups-browsed.service.
> 
> ❯ sudo systemctl start cups-browsed.service
> ```

Paralelamente he visto que el servicio de `anacron.service` está desactivado, y en mi caso lo quiero activar. Para ello ejecutaré el siguiente comando:

> ```shell
> ❯ sudo systemctl enable anacron.service
> Synchronizing state of anacron.service with SysV service script with /lib/systemd/systemd-sysv-install.
> Executing: /lib/systemd/systemd-sysv-install enable anacron
> Created symlink /etc/systemd/system/multi-user.target.wants/anacron.service → /lib/systemd/system/anacron.service.
> 
> ❯ sudo systemctl start anacron.service
> ```

### COMPROBAR QUE LOS SERVICIOS SE HAN DESACTIVADO

Una vez desactivados los servicios volveremos a ejecutar el comando `systemctl list-unit-files --type service --all` para verificar que sea así

> ```shell
> UNIT FILE                                  STATE           PRESET  
> avahi-daemon.service                       enabled         enabled 
> blueman-mechanism.service                  enabled         enabled 
> bluetooth.service                          enabled         enabled   
> colord.service                             static          - 
> configure-printer@.service                 static          - 
> cups-browsed.service                       disabled        enabled 
> cups.service                               disabled        enabled 
> nordfileshared.service                     disabled        enabled 
> nordvpn.service                            generated       - 
> nordvpnd.service                           enabled         enabled 
> openvpn-client@.service                    disabled        enabled 
> openvpn-server@.service                    disabled        enabled 
> openvpn.service                            disabled        enabled 
> openvpn@.service                           disabled        enabled 
> redsocks.service                           disabled        enabled 
> rescue.service                             static          - 
> ....
> ```

Y efectivamente vemos que se han desactivado y activado los servicios según los comandos ejecutados anteriormente. De esta forma podremos optimizar el rendimiento de nuestra distribución Linux. Aparte de desactivar los servicios necesarios también es importante controlar a la perfección las aplicaciones que se levanta al iniciar el sistema operativo. Este último tema lo abordaremos en futuros artículos.
