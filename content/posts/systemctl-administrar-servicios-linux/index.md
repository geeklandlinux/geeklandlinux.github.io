---
title: "Systemctl para administrar servicios en Linux con Systemd"
date: 2017-06-29
categories: 
  - "linux-2"
tags: 
  - "servicios"
  - "systemctl"
  - "systemd"
coverImage: "administrar-servicios-en-systemd-con-systemctl.jpg"
---

Guste más o guste menos, systemd ha llegado para quedarse y prueba de ello es que la totalidad de distribuciones Linux grandes, excepto Gentoo, lo están usando como sistema de inicio predeterminado Por este motivo en el presente artículo veremos que es un servicio y como usar el comando systemctl para que todo usuario básico, medio o avanzado pueda gestionar la totalidad de servicios de su ordenador de forma fácil y sencilla.<!--more-->

## ¿QUÉ ES UN SERVICIO EN LINUX?

Un servicio no es más que un programa que se ejecuta, o está esperando ser ejecutado, en segundo plano.

Los servicios o procesos tienen las siguientes características:

1. No acostumbran a tener una interfaz gráfica para que los usuarios puedan interactuar de forma directa con ellos.
2. Los servicios o demonios normalmente son inicializados con el arranque del sistema.
3. Normalmente están a la espera de que pase un evento para a posteriori poder realizar su función.
4. Toda la actividad que generan los servicios queda registrada en los logs del sistema mediante syslog y/o journalctl.

Algunos ejemplos de servicios o demonios que conoce la gran mayoría de gente son los siguientes:

1. apache
2. httpd
3. cron
4. dnsmasq
5. etc

### Como se gestionan los servicio o demonios en GNU Linux

Los servicios o demonios se gestionan mediante los demonios Init o Systemd.

El primer servicio que inicia el Kernel de Linux es Init o Systemd. Seguidamente, init o systemd son los encargados de cargar el resto de servicios del sistema operativo. Por lo tanto Init o Systemd son los padres de todos los demonios o servicios que se inicializan en nuestro sistema operativo.

Init y Systemd siempre están activos hasta que el sistema se apaga. Mientras estos servicios estén activos los podremos usar para gestionar los servicios que se inician y paran en nuestro ordenador o servidor.

En el caso que systemd o init no se inicien, nuestro ordenador nunca podrá llegar a arrancar. Por lo tanto son demonios extremadamente importantes.

## OBTENER LISTADOS DE LOS SERVICIOS MEDIANTE SYSTEMCTL

Seguidamente les dejo un enlace en el que encontrarán varios comandos para obtener listados que nos darán [información sobre el estado de los servicios]({{< relref "/posts/conocer-estado-servicio-systemd" >}}) y unidades de nuestro sistema operativo.

## CONSEGUIR AYUDA E INFORMACIÓN SOBRE UN SERVICIO O UNIDAD DETERMINADA

Si queremos obtener información detallada sobre un servicio tan solo tenemos que ejecutar el comando systemctl help seguido del nombre del servicio que queremos analizar.

A modo de ejemplo, si queremos obtener información de como usar el servicio emergency tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> systemctl help emergency
> ```

## CONSULTAR LAS DEPENDENCIAS DE UN SERVICIO O UNA UNIDAD

Para conocer las dependencias de un servicio podemos usar varios comandos. Algunos de ellos son los siguientes:

### Averiguar las dependencias de un servicio o una unidad

Para ver el árbol de dependencias de un servicio o una unidad tenemos que ejecutar el comando systemctl list-dependencies seguido del nombre de servicio que queremos analizar las dependencias.

Por lo tanto, para conocer la totalidad de dependencias que Cron necesita para realizar su función tenemos que ejecutar el siguiente comando:

> ```
> systemctl list-dependencies cron
> ```

Y el resultado obtenido es del siguiente tipo:

`cron.service ● ├─system.slice ● └─sysinit.target ● ├─console-setup.service ● ├─dev-hugepages.mount ● ├─dev-mqueue.mount ● ├─keyboard-setup.service ● ├─kmod-static-nodes.service …`

### Ver los servicios y unidades que se ejecutan después de arrancar un servicio o proceso

Para conocer la totalidad de servicios que se usarán después de iniciar un servicio tenemos que ejecutar el comando systemctl list-dependencies --before seguido del nombre del servicio o unidad.

Por lo tanto, para conocer los servicios que se usaran después de ejecutar cron tenemos que ejecutar el siguiente comando en la terminal:

> ```
> systemctl list-dependencies --before cron
> ```

Si lo hacemos obtendremos el siguiente resultado:

`cron.service ● ├─multi-user.target ● │ ├─systemd-update-utmp-runlevel.service ● │ └─graphical.target ● │ └─systemd-update-utmp-runlevel.service ...`

### Ver los servicios y unidades que tienen que estar activos antes de ejecutar un servicio

Para saber los servicios y unidades que tienen que estar activos antes de ejecutar un servicio tenemos que ejecutar el comando systemctl list-dependencies --after seguido del nombre de un servicio o unidad.

A modo de ejemplo, para conocer los servicios y unidades que tienen que estar activos antes de iniciar cron tienen ejecutar el siguiente comando en la terminal:

> ```
> systemctl list-dependencies --after cron
> ```

El resultado que he obtenido en mi caso es el siguiente:

`cron.service ● ├─system.slice ● ├─systemd-journald.socket ● ├─basic.target ● │ ├─-.mount ● │ ├─tmp.mount ● │ ├─paths.target ● │ │ ├─acpid.path ● │ │ ├─systemd-ask-password-console.path ...`

## GESTIONAR SERVICIOS DE NUESTRO SISTEMA OPERATIVO

A continuación veremos los comandos básicos para poder gestionar la totalidad de nuestros servicios y unidades con Systemd.

### Ver el estado de un servicio determinado

Visiten el siguiente enlace para averiguar el [estado de un servicio determinado]({{< relref "/posts/conocer-estado-servicio-systemd" >}}).

### Iniciar un servicio con systemd

En el caso que un servicio esté inactivo lo podemos iniciar muy fácilmente.

Para iniciarlo abrimos una terminal y ejecutamos el comando systemctl start seguido del nombre del servicio que queremos iniciar.

Por lo tanto para iniciar el servicio dnsmasq tenemos que ejecutar el siguiente comando:

> ```
> sudo systemctl start dnsmasq.service
> ```

Una vez ejecutado el comando se iniciará el servicio. Para comprobar que el servicio se ha iniciado podemos ejecutar el siguiente comando:

> ```
> sudo systemctl status dnsmasq.service
> ```

### Reiniciar un servicio con systemd

En el caso que por cualquier incidencia queramos reiniciar un servicio tenemos que seguir las siguientes instrucciones:

Abrimos una terminal y ejecutamos el comando systemctl restart seguido del nombre del proceso que queremos reiniciar.

Por lo tanto, para reiniciar el servicio dnsmasq tenemos que ejecutar el siguiente comando:

> ```
> sudo systemctl restart dnsmasq.service
> ```

Una vez ejecutado el comando el servicio se reiniciará.

### Recargar la configuración de un servicio sin llegar a reiniciarlo

Al cambiar la configuración de cualquier servicio no se aplicará de forma instantánea. Para que se aplique la nueva configuración tenemos 3 opciones:

1. Reiniciar el ordenador o servidor.
2. Reiniciar el servicio siguiendo el método aplicado en el apartado anterior.
3. Recargar la configuración del servicio sin reiniciarlo ni pararlo en ningún momento.

Para recargar la configuración de un servicio sin llegar a pararlo ni a reiniciarlo ejecutamos el comando systemctl reload seguido del nombre del servicio que queremos recargar.

De este modo, para recargar la configuración de dnsmasq tenemos que ejecutar el siguiente comando en la terminal:

> ```
> systemctl reload dnsmasq.service
> ```

Al ejecutar el comando se aplicarán los cambios realizados en los archivos de configuración del servicio sin tan siquiera llegar a parar el servicio.

### Como parar un servicio con systemd

En el momento que queramos parar un servicio abrimos una terminal.

Una vez abierta ejecutamos el comando systemctl stop seguido del nombre del proceso que queremos parar.

Por lo tanto para parar el servicio dnsmasq tan solo tengo que ejecutar el siguiente comando:

> ```
> systemctl stop dnsmasq.service
> ```

Después de ejecutar el comando el servicio se parará y por lo tanto pasará a estar inactivo.

### Habilitar permanentemente un servicio en el arranque

Para que un servicio arranque de forma automática al iniciar el ordenador tenemos que habilitarlo.

Para habilitarlo tenemos que ejecutar el comando systemctl enable seguido del nombre del proceso que queremos habilitar.

A modo de ejemplo, para habilitar el servicio bluetooth.service tenemos que ejecutar el siguiente comando:

> ```
> sudo systemctl enable bluetooth.service
> ```

En el momento de ejecutar el proceso se creará un enlace simbólico de /lib/systemd/system/bluetooth.service a /etc/systemd/system/dbus-org.bluez.service que hará que el servicio pueda iniciarse cuando arrancamos el ordenador.

### Deshabilitar permanentemente un servicio

Si queremos que al arrancar el ordenador no se cargue un servicio realizaremos lo siguiente.

Para deshabilitar un servicio ejecutamos el comando systemctl disable seguido del nombre de proceso que queremos deshabilitar.

Por lo tanto para deshabilitar el servicio bluetooth.service ejecutamos el siguiente comando en la terminal:

> ```
> sudo systemctl disable bluetooth.service
> ```

Una vez ejecutado el comando se deshabilitará el servicio. Por lo tanto cuando arranquemos el ordenador bluetooth.service no se activará.

Para deshabilitar el servicio, systemd borra el enlace simbólico /etc/systemd/system/dbus-org.bluez.service que se creo que en el apartado anterior.

Aunque el servicio esté deshabilitado, cualquier usuario con permisos de administrador lo podrá iniciar de forma manual una vez iniciada la sesión.

###### Nota: Hay que tener precaución al deshabilitar servicios. Si no saben que realiza el servicio que deshabilitan pueden presentarse inconvenientes graves.

### Enmascarar y desenmascarar un servicio con Systemctl

En el apartado anterior hemos visto como deshabilitar un servicio. Si además de deshabilitarlo queremos que no se pueda iniciar manualmente ni automáticamente después de iniciar la sesión podemos enmascararlo.

**Para enmascarar un servicio** abrimos una terminal. Una vez abierta ejecutamos el comando systemctl mask seguido del nombre del proceso que queremos enmascarar.

Por lo tanto, si queremos enmascarar el servicio bluetooth.service ejecutamos el siguiente comando:

> ```
> sudo systemctl mask bluetooth.service
> ```

De este modo nadie podrá iniciar el proceso manualmente o automáticamente a no ser que lo desenmascare previamente.

**Para desenmascarar un servicio** tan solo tenemos que ejecutar el comando o systemctl unmask seguido del nombre del proceso que queremos desenmascarar.

Por lo tanto, en el caso que queramos desenmascarar el servicio que acabamos enmascarar tenemos que ejecutar el siguiente comando en la terminal:

> ```
> sudo systemctl unmask bluetooth.service
> ```

Para terminar deseo que después de leer este artículo tengáis las nociones necesarias para empezar a administrar los servicios de vuestro ordenador.
