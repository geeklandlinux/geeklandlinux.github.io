---
title: "Actualizaciones automáticas en nuestro equipo con unattended-upgrades"
date: 2020-07-12
categories: 
  - "linux-2"
tags: 
  - "actualizar"
  - "apt"
  - "debian"
  - "ubuntu"
coverImage: "actualizaciones-automaticas-en-nuestro-servidor.jpg"
---

Si tiene un servidor o equipo cuyo sistema operativo usa el gestor de paquetes apt pueden actualizarlo de forma automática. Para obtener actualizaciones automáticas en su equipo deberán seguir las instrucciones que verán a continuación.<!--more-->

## ACTUALIZAR NUESTRO SERVIDOR O SISTEMA OPERATIVO DE FORMA AUTOMÁTICA MEDIANTE UNATTENDED-UPGRADES

Para actualizar nuestro sistema operativo de forma automática tan solo tenemos que instalar el paquete `unattended-upgrades`. Para ello tan solo tenemos que ejecutar el siguiente comando:

> `**sudo apt install unattended-upgrades**`

**Nota**: La función del paquete unattended-upgrades es automatizar el proceso de actualización de nuestro ordenador o servidor.

Una vez instalado el paquete activen las actualizaciones automáticas ejecutando el siguiente comando en la terminal:

> ```shell
> sudo dpkg-reconfigure -plow unattended-upgrades
> ```

## CONFIGURAR LAS ACTUALIZACIONES AUTOMÁTICAS MEDIANTE UNATTENDED-UPGRADES

El fichero de configuración de las actualizaciones desatendidas o automáticas es el siguiente:

> ```shell
> /etc/apt/apt.conf.d/50unattended-upgrades
> ```

En Debian y en Ubuntu la configuración estándar de unattended-upgrades solo aplicará las actualizaciones de seguridad. Si quieren modificar este comportamiento deberán ejecutar el siguiente comando para editar su archivo de configuración:

> ```shell
> sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
> ```

Si miran el contenido del fichero, dentro de la sección `**Unattended-Upgrade::Allowed-Origins {**` verán un contenido similar al siguiente:

> ```shell
> Unattended-Upgrade::Allowed-Origins {
>         "${distro_id}:${distro_codename}";
>         "${distro_id}:${distro_codename}-security";
>         // Extended Security Maintenance; doesn't necessarily exist for
>         // every release and this system may not have it installed, but if
>         // available, the policy for updates is such that unattended-upgrades
>         // should also install from here by default.
>         "${distro_id}ESMApps:${distro_codename}-apps-security";
>         "${distro_id}ESM:${distro_codename}-infra-security";
> //      "${distro_id}:${distro_codename}-updates";
> //      "${distro_id}:${distro_codename}-proposed";
> //      "${distro_id}:${distro_codename}-backports";
> };
> ```

**Nota:** En función de la distribución que uséis cambiará ligeramente el texto de este apartado. También verán que hay distribuciones que permiten mayor granularidad a la hora de definir las actualizaciones que se aplican. El ejemplo que veremos en este artículo es con un servidor Ubuntu 20.04.

Si os fijáis, la totalidad de líneas están comentadas a excepción de las líneas que hacen referencia a las actualizaciones de seguridad. Para añadir más tipos de actualizaciones tan solo tienen que descomentar las líneas correspondientes. Si descomentan las líneas que terminan por `updates`, `proposed` o `backports` obtendrán los siguientes tipos de actualizaciones automáticas:

- `**"${distro_id}:${distro_codename}-updates";**` Recomiendo descomentar esta línea. Al hacerlo se aplicarán actualizaciones no críticas no relacionadas con actualizaciones de seguridad. Activando esta opción se mantendrá el sistema operativo más actualizado y minimizaremos el riesgo que se rompan dependencias.
- `**"${distro_id}:${distro_codename}-proposed";**` En caso que administréis un servidor yo no activaría esta opción. Si aplican esta opción se instalarán paquetes que aún no han sido aun aceptados para trabajar en una versión estable.
- `**"${distro_id}:${distro_codename}-backports";**` Al igual que en el caso anterior no recomiendo activar esta opción para un servidor que esté en producción. Si la activan es posible que se actualicen paquetes y programas a una versión más actual. Pero con el riesgo de romper dependencias y crear inestabilidad en el sistema.

### Añadir repositorios adicionales a las actualizaciones automáticas

En el ejemplo que acabamos de ver solo se actualizarán de forma automática los paquetes que están presentes en los repositorios de Ubuntu. Si por ejemplo tienen un repositorio externo a Ubuntu, como por ejemplo el de Docker, y quieren que se actualice de forma automática lo deberán hacer de la siguiente forma.

**Tendremos que averiguar el origen y la etiqueta del repositorio** cuyos paquetes queremos que se actualicen de forma automática. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> grep -R 'Origin: \|Label: ' /var/lib/apt/lists
> ```

Si queremos filtrar más el contenido que aparece en pantalla ejecutaremos el siguiente comando:

> ```shell
> grep -R 'Origin: \|Label: ' /var/lib/apt/lists | grep docker
> ```

Si miramos la salida del comando que acabamos de aplicar veremos lo siguiente:

> ```shell
> /var/lib/apt/lists/download.docker.com_linux_ubuntu_dists_focal_InRelease:Label: Docker CE
> /var/lib/apt/lists/download.docker.com_linux_ubuntu_dists_focal_InRelease:Origin: Docker
> ```

Por lo tanto podemos concluir que el repositorio de Docker tiene el origen **`Docker`** y la etiqueta **`Docker CE`**. Por lo tanto en el fichero de configuración `/etc/apt/apt.conf.d/50unattended-upgrades` añadiremos la siguiente línea:

        **`"Docker:Docker CE";`**

Después de aplicar los cambios, la sección `**Unattended-Upgrade::Allowed-Origins {**` de mi fichero de configuración ha quedado de la siguiente forma:

> **`Unattended-Upgrade::Allowed-Origins {         "${distro_id}:${distro_codename}";         "${distro_id}:${distro_codename}-security";         // Extended Security Maintenance; doesn't necessarily exist for         // every release and this system may not have it installed, but if         // available, the policy for updates is such that unattended-upgrades         // should also install from here by default.         "${distro_id}ESMApps:${distro_codename}-apps-security";         "${distro_id}ESM:${distro_codename}-infra-security";         "${distro_id}:${distro_codename}-updates";         "Docker:Docker CE"; //      "${distro_id}:${distro_codename}-proposed"; //      "${distro_id}:${distro_codename}-backports"; };`**

Una vez finalizada la configuración guardamos los cambios y cerramos el fichero. A partir de estos momentos nuestro sistema operativo y Docker se actualizarán de forma automática.

### Evitar que ciertos paquetes se actualicen de forma automática

Si continúan observando el contenido del fichero de configuración verán que la sección `**Unattended-Upgrade::Package-Blacklist {**` permite definir los paquetes que no queremos que se actualicen de forma automática. A modo de ejemplo, si descomentamos las siguientes líneas:

> ```shell
> Unattended-Upgrade::Package-Blacklist {
>     // The following matches all packages starting with linux-
>   "linux-";
> 
>     // Use $ to explicitely define the end of a package name. Without
>     // the $, "libc6" would match all of them.
>   "libc6$";
>   "libc6-dev$";
>   "libc6-i686$";
> ```

- No se actualizaría ningún paquete cuyo nombre empiece por `linux-`. Por lo tanto por ejemplo no se actualizará el Kernel.
- No se actualizaría ningún paquete cuyo nombre termine por `libc6`, `libc6-dev` o `libc6-i686`.

### Obtener notificaciones vía email de las actualizaciones aplicadas

Para obtener notificaciones de las actualizaciones vía email tenemos que instalar un servidor de correo como por ejemplo mailx. Para ello ejecuten el siguiente comando en la terminal:

> ```shell
> sudo apt-get install bsd-mailx
> ```

Una vez instalado el paquete acceden de nuevo al archivo de configuración de las actualizaciones automáticas ejecutando el siguiente comando:

> ```shell
> sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
> ```

Una vez dentro del fichero de configuración buscan las siguientes líneas:

> ```shell
> Unattended-Upgrade::Mail "ejemplo@mail.com";
> Unattended-Upgrade::MailReport "on-change"; 
> ```

A continuación las modifican del siguiente modo:

- En la primera de las líneas tan solo tienen que reemplazar `ejemplo@mail.com` por la dirección de correo electrónico en que quieren recibir las notificaciones.
- En `Unattended-Upgrade::MailReport "on-change";` podemos seleccionar las opciones `on-change` y `only-on-error`. La opción opción `only-on-error` hará que unicamente se notifique vía email cuando hayan errores en la actualización automática. La opción `on-change` notificará sobre todos los paquetes instalados y desinstalados en el proceso de actualización automática.

**Nota**: Las notificaciones vía email son opcionales. No hace falta configurarlas si no las necesitamos. Para conocer los paquetes instalados y desinstalados durante las actualizaciones podemos consultar los logs del sistema.

### Otras opciones presentes en el fichero de configuración de actualizaciones automáticas

Si continúan leyendo verán que dentro del fichero de configuración se pueden controlar otros parámetros como por ejemplo:

- `**Unattended-Upgrade::InstallOnShutdown "false";**` Si está en true las actualizaciones se aplicarán al apagar nuestro servidor u ordenador. Esto hará el apagado del ordenador más lento.
- **`Unattended-Upgrade::Remove-Unused-Dependencies "false";`** Si el valor es true después de actualizar el sistema operativo se desinstalaran de forma automática todas las dependencias que no necesitamos.
- `**Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";**` Permite desinstalar los Kernel antiguos que no usamos de forma automática.
- `**Unattended-Upgrade::Automatic-Reboot "false";**` Si el valor es true permite reiniciar el servidor de forma automática en el caso que una actualización lo requiera.
- `**Unattended-Upgrade::Automatic-Reboot-Time "02:00";**` En conjunción con el parámetro anterior, permite que el reinicio del servidor se haga a la hora que nosotros queramos. Por lo tanto si después de actualizar el ordenador se tiene que reiniciar, no lo hará hasta que sean las 2 de la madrugada.
- `**Acquire::http::Dl-Limit "70";**` Para limitar el ancho de banda consumido en la actualización. Si fijamos 70, la velocidad de descarga de las actualizaciones será 70kb/segundo.
- etc.

## DEFINIR LA PERIODICIDAD DE LAS ACTUALIZACIONES AUTOMÁTICAS

Para definir la periodicidad de las actualizaciones automáticas deberán consultar el fichero `/etc/apt/apt.conf.d/20auto-upgrades` ejecutando el siguiente comando en la terminal:

> ```shell
> sudo nano /etc/apt/apt.conf.d/20auto-upgrades
> ```

Si observan el contenido del fichero **como mínimo** verán los siguientes parámetros de configuración:

> ```shell
> APT::Periodic::Update-Package-Lists "1";
> APT::Periodic::Unattended-Upgrade "1";
> ```

El significado de cada una de las líneas de forma respectiva es el siguiente:

- Como mínimo una vez al día se refrescarán los repositorios de nuestra distribución.
- Una vez al día se intentará actualizar el sistema operativo en función de la configuración que tengamos en el fichero `/etc/apt/apt.conf.d/50unattended-upgrades`.

**Nota:** Los números que aparecen en los parámetros de configuración indican la frecuencia en días en que se realizará cada una de las acciones.

### Parámetros de configuración adicionales para introducir en /etc/apt/apt.conf.d/20auto-upgrades

Otras opciones de configuración que pueden existir y **recomiendo introducir** en el fichero `/etc/apt/apt.conf.d/20auto-upgrades` son las siguientes:

> ```shell
> APT::Periodic::Update-Package-Lists "1";
> APT::Periodic::Unattended-Upgrade "1";
> APT::Periodic::Download-Upgradeable-Packages "1";
> APT::Periodic::AutocleanInterval "7";
> ```

La función de cada uno de los 2 nuevos parámetros de configuración que acabamos de introducir:

- A diario se comprobarán si existen actualizaciones y se descargarán los paquetes binarios de la actualización. Los paquetes descargados quedarán almacenados en la cache de apt y de este modo cuando realicemos una actualización manual será mucho más rápida.
- Cada 7 días se borrará la totalidad de paquetes que estén obsoletos en la cache del gestor de paquetes apt. De este modo la cache del gestor de paquetes apt no crecerá desmesuradamente y se mantendrá actualizada.

## COMPROBAR QUE SE ESTÁN APLICANDO LAS ACTUALIZACIONES SE SEGURIDAD

Para comprobar que las actualizaciones se están aplicando de forma correcta pueden usar las notificaciones vía email. No obstante también lo podemos comprobar mediante los logs del sistema. Los logs de las actualizaciones se almacenan en `/var/log/unattended-upgrades`. El contenido que hallaremos en esta ubicación es el siguiente:

> ```shell
> ubuntu@ubuntu-20-04:/var/log/unattended-upgrades$ ls
> unattended-upgrades-dpkg.log      unattended-upgrades.log
> unattended-upgrades-shutdown.log
> ```

Si quieren consultar los logs pueden ejecutar el siguiente comando en la terminal:

> ```shell
> less unattended-upgrades-dpkg.log
> ```

## COMPROBAR QUE LA CONFIGURACIÓN ES CORRECTA Y SE REALIZARÁN LAS ACTUALIZACIONES AUTOMÁTICAS

Para asegurar que la configuración aplicada es la correcta pueden ejecutar el siguiente comando en la terminal:

> ```shell
> sudo unattended-upgrade -d -v --dry-run
> ```

El comando que acabamos de aplicar simula el proceso de una actualización automática. Si durante la simulación no se producen errores entonces las actualizaciones automáticas se deberían realizar sin problema.

**Nota**: Si en vez de simular el comportamiento quieren forzar el proceso de actualización automático tan solo tiene que ejecutar el comando borrando el parámetro `--dry-run`

## VERIFICAR SI HAY BUGS ANTES DE APLICAR UNA ACTUALIZACIÓN AUTOMÁTICA

Existe una posibilidad muy remota que una de las actualizaciones automáticas pueda romper el sistema. Para intentar minimizar esta posibilidad podemos configurar que antes de instalar una actualización se compruebe si se han reportado bugs graves. Para ello tendrán que instalar apt-listbugs ejecutando el siguiente comando en la terminal:

> ```shell
> sudo apt install apt-listbugs
> ```

Una vez instalado pueden acceder a su fichero de configuración para modificar lo severo que tiene que ser el bug de la actualización para que no sea instalada. Para ello ejecutamos el siguiente comando en la terminal:

> **`sudo nano /etc/apt/apt.conf.d/10apt-listbugs`**

Dentro del fichero podemos modificar el contenido de la siguiente línea:

> ```shell
> AptListbugs::Severities "critical,grave,serious";
> ```

**Nota:** Los valores que podemos usar son critical, grave, serious, important, normal y minor.

En nuestro caso si lo dejamos tal cual si en el momento de realizar una actualización automática se detecta que el paquete a actualizar tiene una vulnerabilidad crítica, grave o seria no se actualizará.

**Nota**: En una distribución estable apt-listbugs prácticamente no tiene utilidad. Pero si estamos en versiones testing o inestables de algunas distribuciones nos puede ayudar a no tener sorpresas en las actualizaciones.

## SOLUCIONES ALTERNATIVAS A UNATTENDED-UPGRADES

Existen métodos alternativos para aplicar actualizaciones automáticas a nuestro sistema operativo. Una de estas soluciones es Apticron. Apticron será abordado en futuros artículos de este blog.

## CONCLUSIONES

Lo que han visto en este artículo es extremadamente útil para la gente que gestiona servidores. De este modo sin tener que preocuparnos de acceder diariamente al servidor podemos asegurar que se aplican la totalidad de actualizaciones de seguridad a nuestro sistema operativo. De este modo podremos evitar vulnerabilidades de seguridad y parchear nuestro sistema operativo de forma rápida y eficiente.

### Fuentes

[https://wiki.debian.org/UnattendedUpgrades](https://wiki.debian.org/UnattendedUpgrades)

[https://haydenjames.io/how-to-enable-unattended-upgrades-on-ubuntu-debian/](https://haydenjames.io/how-to-enable-unattended-upgrades-on-ubuntu-debian/)
