---
title: "Configurar el cliente SSH para facilitar la conexión remota a los equipos"
date: 2020-01-11
categories: 
  - "linux-2"
  - "raspberry-pi"
tags: 
  - "configuracion"
  - "ssh"
coverImage: "configurar-el-cliente-ssh-facilitar-conexion.png"
---

Quien tenga varios ordenadores y servidores y se conecte a ellos vía SSH sabrá que resulta difícil recordar los parámetros de conexión. Por este motivo a continuación detallaré el procedimiento para que todos los usuarios puedan configurar el cliente SSH de su distribución Linux y de esta forma facilitar la conexión remota a sus equipos y servidores.<!--more-->

Algunos de los parámetros difíciles de recordar son:

1. La IP.
2. El nombre de usuario.
3. El puerto en que está escuchando SSH.
4. Etc.

Para solucionar este problema podemos usar el fichero de configuración que tienen la totalidad de clientes OpenSSH. La ubicación y nombre del fichero de configuración en GNU Linux es:

> ```
> ~/.ssh/config
> ```

Por lo tanto el fichero de configuración es un archivo llamado config ubicado dentro del directorio oculto ssh que está en nuestra home.

## CONFIGURAR EL CLIENTE SSH PARA CONECTARNOS DE FORMA SIMPLE A NUESTROS EQUIPOS Y SERVIDORES

Es probable que el fichero de configuración que acabo de mencionar no esté disponible en su equipo. Para crearlo ejecutaremos el siguiente comando en la terminal:

> ```
> touch ~/.ssh/config
> ```

A continuación otorgaremos los permisos apropiados para asegurar que las conexiones ssh se puedan realizar de forma correcta y segura. Para ello tenemos que ejecutar los siguientes comandos en la terminal:

> ```
> chmod 700 ~/.ssh
> chmod 700 ~/.ssh/id_rsa
> chmod 700 ~/.ssh/config
> chmod 600 ~/.ssh/known_hosts
> chmod 600 ~/.ssh/authorized_keys
> ```

Una vez creado el fichero lo editaremos ejecutando el siguiente comando:

> ```
> nano ~/.ssh/config
> ```

El código a introducir dentro del fichero dependerá de vuestras necesidades y del tipo de autenticación que usen. A continuación les detallo el código que deben usar en función del tipo de autenticación que usan.

### Facilitar la conexión a un servidor ssh con autenticación por contraseña

Si el comando que usan para conectarse al servidor ssh es del siguiente tipo:

> ```
> ssh -p 2224 pi@geekland.duckdns.org
> ```

Entonces el código a introducir dentro del fichero ~/.ssh/config para configurar el cliente SSH es el siguiente:

> ```
> Host rp
>   Hostname geekland.duckdns.org
>   User pi
>   Port 2224
> ```

Una vez introducido el código podemos guardar los cambios y cerrar el fichero. Acto seguido podremos conectarnos a nuestro servidor usando este simple comando:

> ```
> ssh rp
> ```

El significado de todos y cada uno de los parámetros introducidos al fichero de configuración es el siguiente:

- Host rp: El parámetro Host sirve para dar un nombre al servidor al que nos vamos a conectar. Podéis poner un nombre cualquiera que os ayude a recordar el equipo al que os queréis conectar. En mi caso me quiero conectar a mi Raspberry Pi, por esto motivo uso el nombre rp.
- Hostname geekland.duckdns.org: Detallamos la IP del servidor al que nos queremos conectar. La IP puede ser local o pública. La IP se puede indicar de forma numérica o mediante un dominio que apunte a una IP.
- User pi: En el parámetro User escribimos el nombre de usuario con el que nos queremos loguear.
- Port 2224: Detallamos el puerto en que está escuchando el servidor ssh. En la mayoría de casos está escuchando en el puerto 22, pero en mi caso por temas de seguridad uso el 2224.

### Facilitar la conexión a un servidor ssh con autenticación por claves

Si usamos un sistema de autenticación por claves, el comando que usamos para loguearnos al servidor será del siguiente tipo:

> ```
> ssh -i ~/.ssh/id_rsa -p 22 ubuntu@120.444.220.130
> ```

Por lo tanto, el código que deberemos usar en el fichero de configuración ~/.ssh/config será el siguiente:

> ```
> Host vps
>   Hostname 120.444.220.130
>   IdentityFile ~/.ssh/id_rsa
>   IdentitiesOnly yes
>   User ubuntu
>   Port 22
> ```

Una vez introducido el código guardamos los cambios. El significado de los parámetros introducidos es el siguiente:

- Host vps: En mi caso el parámetro Host tiene como valor vps. vps es el nombre o alias que usaré para referirme al servidor al que me quiero conectar.
- Hostname 120.444.220.130: Detallamos la IP del servidor al que nos queremos conectar. La IP puede ser local o pública. La IP se puede indicar de forma numérica o mediante un dominio que apunte a una IP.
- IdentityFile ~/.ssh/id\_rsa: El parámetro IdentityFile indica la ubicación de la clave privada del cliente que usamos para loguearnos al servidor ssh. En mi caso la clave usada es la id\_rsa que está ubicada en ~/.ssh/
- IdentitiesOnly yes: Al establecer el valor yes únicamente se tendrá en cuenta la clave privada especificada en el parámetro IdentityFile.
- User ubuntu: Especifico que me quiero loguear remotamente con el usuario ubuntu.
- Port 22: Especifico que el puerto en que está escuchando el servidor ssh es el 22.

Una vez finalizada la configuración nos podremos conectar a nuestro equipo de forma remota usando este simple comando:

> ```
> ssh vps
> ```

###### Nota: pueden consultar el siguiente artículo en el caso que no sepan como [configurar el acceso SSH mediante un par de claves]({{< relref "/posts/conectarse-servidor-ssh-sin-contrasena" >}})

## PARÁMETROS ADICIONALES QUE PODEMOS USAR PARA CONFIGURAR EL CLIENTE SSH

A lo largo del artículo han visto los siguientes parámetros para facilitar la conexión a nuestro servidor usando SSH:

1. Host
2. Hostname
3. IdentityFile
4. IdentitiesOnly
5. User
6. Port

No obstante existen otros parámetros que podemos configurar en el cliente SSH. Algunos de ellos son los siguientes:

- **StrictHostKeyChecking:** Permite configurar si queremos añadir de forma automática las claves DSA del equipo al que nos conectamos al fichero ~/.ssh/known\_hosts. El valor por defecto es “ask”, de este modo si es la primera que nos conectamos a un servidor se nos preguntará si queremos introducir la clave en el fichero ~/.ssh/known\_hosts. Si cambiásemos al valor a “no” la seguridad seria más baja ya que se añadirán las claves al fichero ~/.ssh/known\_hosts de forma automática.
- **VisualHostKey:** Permite mostrar el [fingerprint del equipo]({{< relref "/posts/fingerprint-servidor-ssh" >}}) remoto al que nos conectamos en formato ASCII. De este modo podremos tener una indicación sencilla/visual que no no estamos conectando a un equipo fraudulento. Los valores que pueda tomar esta variable son “yes” y “no”.
- **Compression:** Parámetro que permite habilitar la compresión en el intercambio de datos entre servidor y cliente. La opción puede resultar útil en situaciones en que la conexión a internet sea lenta. Los valores que puede tomar esta variable son “yes” y “no”.
- **ServerAliveInterval:** Establece el intervalo en segundos después del cual sino se ha recibido ningún dato del servidor se enviará un mensaje a través del túnel SSH para comprobar que el servidor aún está activo. Los posibles valores de esta variable son “yes” y “no”.
- **Protocol:** Indicamos la versión del protocolo SSH a usar para conectarnos al servidor. Los valores posibles son “1”, “2” o “2,1”. El valor por defecto es “2,1”, pero si quieren incrementar la seguridad pueden establecer el valor 2.
- Etc.

Ejemplo de aplicación de los parámetros que acabamos de ver Si queremos generar una configuración que aplique a la totalidad de equipos remotos a los que nos vamos a conectar podemos añadir el siguiente código al fichero de configuración ~/.ssh/config:

> ```
> Host *
>   StrictHostKeyChecking ask
>   ServerAliveInterval 120
>   Compression no
>   Protocol 2
>   VisualHostKey yes
> ```

###### Nota: Todas los parámetros dentro de Host \* se aplican a la totalidad de conexiones SSH que realicemos.

Una vez añadida y guardada esta información, en todas y cada una de las conexiones a un servidor SSH:

1. Se nos informará en el caso que nos estemos conectando a un equipo por primera vez.
2. Cada 120 segundos de inactividad se hará una comprobación que el equipo que actúa como servidor está activo.
3. El contenido entre cliente y servidor no será comprimido.
4. El protocolo usado para la conexión SSH será el 2.
5. En el momento de la conexión veremos una representación gráfica del fingerprint del servidor.

## CONFIGURACIÓN QUE ESTOY USANDO EN MI CLIENTE OPENSSH

Después de todo lo explicado os dejo la configuración que yo mismo tengo introducida en mi equipo:

> ```
> # Conexión a mi Raspberry Pi
> Host rp
>   Hostname geekland.duckdns.org
>   User pi
>   Port 2224
> 
> # Conexión a mi Raspberry Pi
> Host vps
>   Hostname 120.444.220.130
>   IdentityFile ~/.ssh/id_rsa
>   IdentitiesOnly yes
>   User ubuntu
>   Port 22
> 
> # Parámetros que aplican a mi VPS y Raspberry Pi
> Host *
>   StrictHostKeyChecking ask
>   ServerAliveInterval 120
>   Compression no
>   Protocol 2
>   VisualHostKey yes
> ```

## INFORMACIÓN ADICIONAL

Si quieren obtener información sobre otros parámetros a usar para configurar el cliente SSH pueden ejecutar el siguiente comando en la terminal de Linux:

> ```
> man 5 ssh_config
> ```

Si lo preferís también pueden visitar la siguiente [URL](https://linux.die.net/man/5/ssh_config "Consultar parámetros adicionales para configurar el cliente SSH").

## CONCLUSIONES

Si configuramos nuestro cliente SSH de forma adecuada las conexiones a nuestros equipos serán mucho más rápidas y simples. Además prácticamente no tendremos que recordar ningún parámetro para establecer la conexión con el cliente. Simplemente tendremos que recordar el nombre del host.

###### FUENTES

[https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client](https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client)

[https://www.ssh.com/ssh/config](https://www.ssh.com/ssh/config)
