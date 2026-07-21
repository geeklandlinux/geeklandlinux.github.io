---
title: "12 formas de mejorar la seguridad de un servidor linux"
date: 2022-12-14
categories: 
  - "linux-2"
  - "seguridad-informatica"
tags: 
  - "fail2ban"
  - "firewall"
  - "raspberry-pi"
  - "seguridad"
  - "ssh"
  - "vps"
cover:
  image: "images/12-formas-de-mejorar-la-seguridad-de-un-servidor-linux.jpeg"
  relative: true
---

En mi caso siempre que puedo utilizo servicios autoalojados en mi Raspberry Pi o en mi servidor VPS. Algunos de los servicios que utilizo actualmente son Tiny Tiny RRS, Shaarli, Whoogle, AudioBookShelf, Trilium, FileBrowser, etc. Lo hago por temas de privacidad, porque funcionan igual de bien que los servicios de pago o servicios que se dedican a recopilar nuestros datos, y porque en muchas ocasiones los puedo adaptar mejor a mis necesidades. No obstante a la hora de autoalojar un servicio es sumamente importante securizar nuestro servidor VPS o nuestra Raspberry Pi para evitar recibir ataques de hackers o bots. Por esto motivo a continuación les dejo una serie de recomendaciones para mejorar la seguridad de su servidor VPS y/o Raspberry Pi y de esta forma evitar ataques de hackers y de bots.<!--more-->

## UTILIZAR CERTIFICADOS SSL PARA LOS SERVICIOS AUTOALOJADOS PARA LA SEGURIDAD DE SU SERVIDOR

Hoy en día es indispensable que la totalidad de servicios abiertos a Internet utilicen un certificado SSL. Los motivos son los siguientes:

1. Dan la seguridad que el servicio web al que nos conectamos no es fraudulento. De esta forma evitamos que un atacante dirija a los usuarios de nuestro servicio a una web clonada maliciosa que seguramente intentará robar las credenciales de los usuarios.
2. El certificado SSL hará que la totalidad de tráfico entre el usuario y el servidor que aloja el servicio autoalojado esté cifrado. Por lo tanto, si un atacante intercepta el tráfico entre el cliente y el servidor no podrá ver nada ni obtener las credenciales de acceso al servicio porque el contenido estará cifrado. Además al existir una capa de cifrado estaremos protegiendo la integridad de los datos entre el servidor y el cliente.

En el caso que uséis multitud de servicios autoloajados, se recomienda encarecidamente [comprar un certificado SSL](https://www.ssl2buy.com/), como Comodo, RapidSSL o DigiCert.

**Nota:** Un [certificado SSL Wildcard](https://www.ssl2buy.com/wildcard-ssl-certificate) nos permitirá cifrar el tráfico de la totalidad de nuestros servicios mediante un solo certificado. Por lo tanto cuantos más servicios uséis más recomendable y económico será usar un certificado SSL Wildcard.

Les recomiendo que cliquen en el siguiente enlace para obtener más información sobre el [funcionamiento de los certificados SSL]({{< relref "/posts/motivos-instalar-certificado-ssl" >}}).

## NO USAR LOS USUARIOS Y LAS CONTRASEÑAS POR DEFECTO PARA MEJORAR LA SEGURIDAD DE UN SERVIDOR LINUX

Muchos equipos, VPS o servicios web que instalamos acostumbran a crear usuarios y/o contraseñas por defecto. Por ejemplo hasta hace poco la totalidad de usuarios que instalaban Raspberry OS tenían el usuario `pi` y la contraseña `raspberry`. Esto sin duda era un riesgo de seguridad muy importante y que hay que evitar si o si. Por lo tanto les recomiendo encarecidamente que sigan las siguientes recomendaciones para mejorar la seguridad de su servidor.

### Generar un nuevo usuario para administrar los servicios en el servidor

En el momento de instalar el sistema operativo o en el momento de acceder al VPS generen un nuevo usuario y una nueva contraseña. Para por ejemplo generar nuevo usuario llamado `geekland` con los privilegios necesarios para instalar, configurar y gestionar los servicios lo haremos del siguiente modo.

Crearemos el usuario `geekland` ejecutando el siguiente comando en la terminal:

```shell
sudo adduser geekland
```

**Nota:** Justo después de ejecutar el comando tendréis que definir la contraseña para este usuario. Por favor elijan una contraseña que sea segura.

Finalmente añadiremos el usuario `geekland` a los grupos `adm` y `sudo`. De este modo el usuario `geekland` tendrá los permisos necesarios para gestionar el servidor que autoaloja los servicios. Para conseguir lo que acabo de citar ejecutaremos los siguientes comandos:

```shell
sudo gpasswd -a geekland adm
sudo gpasswd -a geekland sudo
```

**Nota:** Si queremos que el usuario `geekland` tenga los mismos permisos que uno de los usuarios por defecto, como por ejemplo podría ser `pi` podríamos ejecutar el siguiente comando en la terminal:

 `groups | sed 's/pi //g' | sed 's/ /,/g' | xargs -I{} sudo usermod -a -G {} geekland`

Una vez realizado los pasos que he citado os recomiendo iniciar sesión con el nuevo usuario que acabáis de crear y comprobar que todo funciona de forma adecuada.

### Borrar los usuarios por defecto del servidor para mejorar la seguridad del servidor

Una vez hemos comprobado que el nuevo usuario funciona de forma correcta llega la hora de borrar los usuarios por defecto del sistema operativo. En el caso que nuestro equipo tenga un usuario por defecto llamado `pi` y estemos logueados con el usuario `geekland` haremos lo siguiente:

Si lo creen oportuno copiaremos el contenido de la partición `/home` del usuario `pi` al usuario `geekland`. Para ello ejecutaremos el siguiente comando:

```shell
sudo cp -R /home/pi/. /home/$USER
```

Acto seguido cambiaremos el propietario de los archivos que acabamos de copiar. Para que el propietario de los ficheros que acabamos de copiar sea el usuario `geekland` ejecutaremos el siguiente comando en la terminal:

```shell
sudo chown -R $USER: /home/$USER
```

Finalmente eliminaremos todo rastro del usuario `pi` ejecutando el siguiente comando en la terminal:

```shell
sudo deluser --remove-home pi
```

### Generar nuevos usuarios y contraseñas para los servicios web que autoalojamos

Prácticamente la totalidad de servicios web que autoalojaremos ofrecerán un panel de administración o fichero de configuración para gestionar los usuarios y las credenciales de los usuarios. Accedan a él y realicen las siguientes acciones:

1. Crear un nuevo usuario con los permisos que crean necesarios.
2. Dar una contraseña robusta al nuevo usuario que acaban de crear.
3. Borrar la totalidad de usuarios por defecto que ofrezca el servicio. También es altamente recomendable borrar los usuarios que nunca más volverán a usar el servicio.

## SECURIZAR EL ACCESO VÍA SSH PARA MEJORAR LA SEGURIDAD DE UN SERVIDOR LINUX

En prácticamente la totalidad de ocasiones accederemos a nuestra Raspberry Pi o a nuestro servidor VPS vía SSH. Por lo tanto es sumamente importante securizar el acceso vía SSH. Una guía rápida y básica para realizar lo que estoy proponiendo es la siguiente:

Accedemos al fichero de configuración de ssh ejecutando el siguiente comando en la terminal:

```shell
sudo nano /etc/ssh/ssh_config
```

Una vez se abra el editor de textos nano realizaremos las siguientes modificaciones.

### Cambiar el puerto por defecto de ssh

El puerto por defecto de ssh es el 22. Es recomendable cambiarlo porque habrán multitud de bots intentando acceder a su equipo usando el puerto 22. Para realizar la modificación buscaremos la siguiente línea en el fichero de configuración:

```shell
#   Port 22
```

Acto seguido si queremos que el nuevo puerto de acceso a ssh sea el 2324 modificaremos la línea para que quede del siguiente modo:

```shell
Port 2324
```

### Deshabilitar passwords vacíos para acceder ssh

Es importante forzar a que los usuarios tengan que definir una contraseña para acceder al servidor vía ssh. El hecho de poder loguearse y acceder al servidor web sin introducir ningúna contraseña es un riesgo de seguridad importante. Para forzar que los usuarios tengan que definir y usar una contraseña añadiremos la siguiente línea al fichero de configuración:

```shell
PermitEmptyPasswords no
```

### Deshabilitar el acceso root

También es importante evitar que nadie pueda conectarse vía ssh al servidor usando el usuario root. Para ello en el fichero de configuración de ssh añadiremos la siguiente línea:

```shell
PermitRootLogin no
```

### Inhabilitar el acceso mediante contraseña

La autenticación mediante contraseña es segura. Pero aún es más segura la autenticación mediante un sistema de claves público-privada. Por lo tanto en el caso que quieran loguearse mediante claves deberán seguir los siguientes pasos.

Inicialmente deshabilitaremos la autenticación mediante contraseña. Para ello añadiremos la siguiente línea al fichero de configuración de ssh:

```shell
PasswordAuthentication no
```

Una vez realizadas la totalidad de modificaciones al fichero de configuración guardan los cambios y cierran el fichero. Acto seguido sigan las siguientes instrucciones para poderse loguear mediante claves asimétricas.

https://geeklandlinux.github.io/posts/conectarse-servidor-ssh-sin-contrasena/

### Comprobar el funcionamiento de la nueva configuración

Una vez finalizado el proceso de configuración asegúrense de guardar los cambios y de cerrar el fichero de configuración de ssh. Acto seguido deberán reiniciar el servicio ssh ejecutando el siguiente comando en la terminal:

```shell
service ssh restart
```

A partir de estos momentos ya pueden testear que la configuración aplicada funciona de forma correcta. Recuerden que hemos definido que el nuevo puerto para el servidor ssh será el `2324`, por lo tanto para acceder al servidor tendrán que ejecutar un comando del siguiente tipo:

```shell
ssh -p 2324 usuario@ip_servidor
```

**Nota:** Deberéis reemplazar `usuario` e `ip_servidor` por los valores de vuestro servidor.

**Nota:** Existen otras acciones para securizar el acceso vía ssh. No obstante los detallaré en un futuro artículo.

## CONFIGURAR EL FIREWALL DEL EQUIPO PARA EVITAR ACCESOS INDESEADOS

Con el fin de evitar accesos indeseados es altamente recomendable activar el firewall del equipo que aloja los servicios. Hay muchas formas de configurar el firewall de un equipo Linux, pero en principio la solución más fácil para los principiantes sería usar ufw.

Para instalar ufw en el equipo tendrán que ejecutar el siguiente comando en la terminal:

```shell
sudo apt install ufw
```

Una vez instalado tendrán que configurar las reglas del firewall. En la mayoría de casos el siguiente set de comandos debería ser suficiente para realizar una configuración básica y que sea útil para un usuario que simplemente quiere autoalojar pocos servicios para uso personal.

### Definir la política por defecto del Firewall o cortafuegos para mejorar la seguridad de nuestro servidor

En mi caso lo primero que realizo es definir las siguientes reglas:

1. Que todo el tráfico saliente esté permitido.
2. Que la totalidad del tráfico entrante esté bloqueado.

Para aplicar las 2 reglas que acabamos de citar tan solo hay que ejecutar los siguientes comandos en la terminal:

```shell
sudo ufw default allow outgoing
sudo ufw default deny incoming
```

Una vez aplicada la política por defecto deberemos ir permitiendo las entradas únicamente a los servicios que nos interesan. En mi caso en este artículo solo mostraré como:

1. Habilitar la entrada a los usuarios que quieran acceder al servidor vía ssh.
2. Habilitar la entrada a los usuarios que quieran acceder a los servicios que autoalojaremos en nuestro equipo.
3. Permitir el tráfico entrante y saliente dentro de nuestra red local.

### Habilitar ssh en el firewall

En mi caso el servidor ssh está escuchando en el puerto 2324. Por lo tanto para permitir el acceso a la totalidad de usuarios a través de ssh tendré que ejecutar el siguiente comando:

```shell
sudo ufw allow 2324/tcp
```

La regla que acabo de definir será válida para la gran mayoría de usuarios. Pero para los más paranoicos en términos de seguridad podrían hacer que únicamente una determinada IP tuviera acceso al servidor. Por ejemplo en el caso que el servidor tuviera la IP `132.42.45.25` y solo quisiera que la IP `223.343.232.40` se conectará al servidor vía ssh debería ejecutar el siguiente comando:

```shell
sudo ufw allow proto tcp from 223.343.232.40 to 132.42.45.25 port 2234
```

### Habilitar el acceso web en el firewall

Acto seguido tendríamos que definir las reglas para dar acceso al servidor web que proporcionará el servicio a los clientes. Para ello ejecutaremos los siguientes comandos en la terminal:

```shell
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Habilitar el tráfico dentro de la red local

También puede resultar útil habilitar la totalidad de tráfico dentro de la red local. En mi caso la red local es la `10.0.0.0/24`. Por lo tanto para habilitar la totalidad de tráfico dentro de la red local ejecutaré el siguiente comando:

```
sudo ufw allow from 10.0.0.0/24
```

Acto seguido tendrán que ir dando definiendo las reglas del firewall en función de sus necesidades. Si por ejemplo uno de los servicios que tienen que alojar usa el puerto 1194 deberán abrir el puerto 1194.

### Activar el Firewall ufw

Una vez definidas la totalidad de reglas ejecutaremos los siguientes comandos para asegurar que el firewall esté activo y también para asegurar que se levanta cada vez que reiniciemos el equipo:

```shell
sudo ufw start
sudo ufw enable
```

## USAR FAIL2BAN PARA EVITAR ATAQUES DE FUERZA BRUTA

Fail2ban tiene la función de bloquear a la totalidad de usuarios que introduzcan repetidamente de forma errónea las credenciales para acceder a un servicio web como por ejemplo podría ser un lector de feeds. Para instalar y configurar fail2ban les recomiendo los siguientes enlaces:

[https://geeklandlinux.github.io/posts/instalar-configurar-y-usar-fail2ban-para-evitar-ataques-de-fuerza-bruta/]({{< relref "/posts/instalar-configurar-y-usar-fail2ban-para-evitar-ataques-de-fuerza-bruta" >}})

[https://geeklandlinux.github.io/posts/como-consultar-logs-de-fail2ban/]({{< relref "/posts/como-consultar-logs-de-fail2ban" >}})

[https://geeklandlinux.github.io/posts/usar-fail2ban-con-traefik-para-proteger-servicios-que-corren-en-docker/]({{< relref "/posts/usar-fail2ban-con-traefik-para-proteger-servicios-que-corren-en-docker" >}})

Si instalan y configuran de forma adecuada fail2ban cualquier usuario que por ejemplo introduzca x veces mal las credenciales de acceso a un servicio será bloqueado vía IP durante x segundos. De esta forma podremos mitigar ataques de fuerza bruta a nuestros servicios web.

## ELIMINAR O DESACTIVAR APLICACIONES, SERVICIOS Y PROTOCOLOS DE RED QUE NO SE UTILICEN

Es importante conocer la totalidad de servicios y protocolos de red que tenemos activados así como los puertos que están usando. Si algunos de los servicios o protocolos de red no se usan les recomiendo encarecidamente que los eliminen. Para analizar los servicios y puertos que están en ejecución pueden ejecutar el siguiente comando:

```shell
sudo netstat -plunt
```

**Nota:** Un comando alternativo al recomendado podría ser `sudo ss -atpu`

Acto seguido obtendrán un resultado similar al siguiente:

```shell
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      1406970/docker-prox 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      942/sshd: /usr/sbin 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1407042/docker-prox 
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init              
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      1406946/docker-prox 
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      824/systemd-resolve 
tcp        0      0 0.0.0.0:13378           0.0.0.0:*               LISTEN      1407198/docker-prox 
tcp6       0      0 :::443                  :::*                    LISTEN      1406976/docker-prox 
...
```

Seguidamente deberán analizar cada una de las líneas y como mínimo deberían preguntarse lo siguiente:

1. ¿Necesitamos el servicio que se está ejecutando?
2. ¿El servicio se está ejecutando en las interfaces de red que nosotros queremos?
3. ¿Tenemos securizado cada uno de los servicios expuestos?

En base a cada una de las respuestas deberán tomar las decisiones oportunas. Por ejemplo:

1. Si el servicio no lo usan es mejor eliminarlo.
2. Si un determinado servicio está abierto a todo el mundo `0.0.0.0:*` y no debe ser así corrijanlo.
3. Intentar aplicar medidas para mejorar la seguridad del servicio.

Para finalizar solo decir que en el caso que quieran eliminar alguno de los servicios, como por ejemplo ngnix, tan solo tendrían que ejecutar el siguiente comando en la terminal:

```shell
sudo apt purge nginx
```

## INSTALAR Y USAR SELINUX

También es altamente recomendable [instalar y configurar SElinux](https://www.linode.com/docs/guides/how-to-install-selinux-on-ubuntu-22-04/) (Security Enhanced Linux). SElinux añadirá una política de control de acceso MAC (Mandatory Access Control) mucho más avanzada que por ejemplo AppArmor. De este modo SELinux definirá las políticas para por ejemplo gestionar el acceso de un proceso a un fichero de la siguiente forma:

1. Tenemos un fichero que será accedido por un proceso. Tanto el fichero como el proceso tendrán una serie de atributos.
2. Existirá una política que en función de los atributos del proceso y del fichero definirá si el proceso puede acceder al fichero o no. Este sistema de control de acceso es mucho más complejo que el DAC en que el propietario del fichero es el encargado de definir los usuarios que tienen derecho a consultar o modificar un fichero.

**Nota:** Recuerden que antes de instalar SElinux deberán desinstalar AppArmor.

La comprensión y el proceso de instalación y configuración de SElinux son complejos. Por lo tanto dejaré este tema para un futuro artículo. O en el caso que no puedan esperar pueden consultar el siguiente [enlace](https://opensource.com/article/18/7/sysadmin-guide-selinux).

## USAR CONTRASEÑAS COMPLEJAS Y QUE NO SEAN LAS PREDETERMINADAS PARA MEJORAR LA SEGURIDAD DE NUESTRO SERVIDOR

Todo el mundo debería tener muy presente que una gestión adecuada de las contraseñas es muy importante. En el siguiente enlace podrán encontrar una serie de consejos para crear y gestionar las contraseñas de acceso:

https://geeklandlinux.github.io/posts/buenas-practicas-gestion-uso-contrasenas/

## RESTRINGIR EL ACCESO POR ZONA GEOGRÁFICA O POR IP

Un modo de incrementar la seguridad de un servicio web es únicamente dar acceso a quien necesita acceder al servicio. Si tenemos los conocimientos suficientes podremos realizar las siguientes acciones:

1. Hacer que un servicio web únicamente pueda ser accedido por una determinada IP, o por un rango de IP.
2. Hacer que únicamente los habitantes de un determinado país tengan acceso a nuestro servicio web autoalojado.
3. Hacer que el servicio únicamente sea accesible para los usuarios que puedan conectarse a una determinada red VPN.

Si usan Docker y Traefik pueden seguir los consejos detallados en el siguiente enlace para restringir el acceso por rangos de IP o por país.

https://geeklandlinux.github.io/posts/limitar-acceso-servicio-o-web-por-ip-con-traefik/

Si pretenden que el acceso al servicio sea mediante una red VPN tienen multitud de opciones disponibles. Por ejemplo podrían usar Tailscale:

https://geeklandlinux.github.io/posts/tailscale-conecta-equipos-a-una-red-privada-virtual-vpn-facilmente/

## ACTUALIZAR PERIÓDICAMENTE LOS SERVICIOS QUE USAMOS ASÍ COMO EL SISTEMA OPERATIVO

Actualizar el equipo/servidor y el servicio web es fundamental para incrementar la seguridad ya que muchas de las actualizaciones están orientadas a corregir vulnerabilidades. Para facilitar las actualizaciones de los equipos y de los servicios web pueden seguir las siguientes recomendaciones.

### Actualizar el equipo que aloja el servicio web

Para actualizar el equipo que aloja el servicio web lo pueden hacer manualmente, pero no es lo más recomendable porque requiere de tiempo y de constancia. Lo ideal en este caso será automatizar el proceso de actualización siguiendo los consejos que dejo en el siguiente enlace:

https://geeklandlinux.github.io/posts/actualizaciones-automaticas-en-nuestro-equipo-con-unattended-upgrades/

### Actualizar el servicio web para mejorar la seguridad de nuestro servidor

Existen servicios web que ya traen las herramientas necesarias para actualizar el software de forma sencilla con tan solo presionar un botón. En caso que esta opción no esté disponible tendremos que realizar un proceso de actualización manual que en diversos casos acostumbra a ser complejo.

Una opción a tener en cuenta para actualizar de forma sencilla los servicios es usar Docker. Si usan contenedores Docker verán que pasar de una versión de software a otra es cuestión de segundos e incluso hay herramientas como Watchtower que permiten actualizar los contenedores de forma automática.

## UTILIZAR LOS SERVICIOS DENTRO DE CONTENEDORES O DENTRO ENTORNOS AISLADOS PARA MEJORAR LA SEGURIDAD DE NUESTRO SERVIDOR

Si instalamos los servicios mediante contenedores, como por ejemplo Docker, conseguiremos incrementar la seguridad de los servicios siempre y cuando los contenedores se hayan construido de forma adecuada. Esto es así porque Docker permitirá que los servicios puedan correr en ambientes de ejecución aislados. Por lo tanto en caso de existir un problema de seguridad habrá más posibilidades que únicamente afecten a uno de los servicios y no a todos.

Además de Docker existen otros sistemas para ejecutar los servicios en un entorno aislado. Por ejemplo aislamiento mediante virtualización o con chroot.

## OTROS MÉTODOS PARA MEJORAR LA SEGURIDAD DE NUESTRO SERVIDOR

Finalmente solo comentar que existen otros mecanismos de defensa que aplicar. Algunos de ellos pueden resultar más complejos de aplicar que otros, pero si buscan información en la red seguro que encontrarán las instrucciones oportunas para su implantación. Algunos de estos mecanismos son:

1. Implementar medidas de detección de intrusos (ODS) y aún mejor, medidas de prevención de intrusos (IDS). Estás medidas se pueden aplicar tanto vía software como vía hardware.
2. La arquitectura de la red en que autoalojan sus servicios es importante. Dependiendo de su diseño su nivel de seguridad será mayor o menor.
3. Implementar HoneyPot como mecanismo de defensa frente a los posibles atacantes.
4. Aplicar un sistema de doble factor de autenticación en el caso que sea posible. De este modo obtendrán una capa adicional de protección en el proceso de autenticación.

En el caso que vosotros apliquéis mecanismos de seguridad distintos a los mencionados en el artículo les ruego que los dejen en los comentarios de este artículo.
