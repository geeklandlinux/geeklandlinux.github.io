---
title: "Que es el fingerprint de un servidor SSH y que utilidad tiene"
date: 2017-10-07
categories: 
  - "redes"
tags: 
  - "fingerprint"
  - "seguridad"
  - "ssh"
coverImage: "que-es-fingerprint-servidor-SSH.jpg"
---

La primera vez que nos conectamos a un servidor SSH obtenemos mensajes de advertencia sobre el fingerprint similares al siguiente:

> ```
> The authenticity of host '192.168.1.123 (192.168.1.123)' can't be established.
> ECDSA key fingerprint is SHA256:TiqZXJITgH/kUE9WoNP4TF2xkescmKxFIe33TpsQRC7.
> Are you sure you want to continue connecting (yes/no)?
> ```

<!--more-->Esta advertencia es para prevenir que nos conectemos a un servidor SSH malicioso y evitar ataques man in the middle. En lenguaje llano, la advertencia nos está preguntando si estamos seguros de conectarnos al servidor SSH que tiene el fingerprint SHA256:RiqYXJITgH/kUE9WoNP4TF2xkescmKxFIe33TpsQRC8.

## ¿QUÉ ES EL FINGERPRINT DE UN SERVIDOR SSH?

En el momento que se generan las claves pública y privada de un servidor SSH, se genera una huella digital única para cada una de estas llaves. Esta huella digital única es el fingerprint y su principal función es identificar de forma inequívoca un servidor.

## ¿QUÉ UTILIDAD TIENE EL FINGERPRINT DE UN SERVIDOR SSH?

En el momento que nos conectamos al servidor SSH se inicia un proceso autenticación. El proceso es el siguiente:

1. Cuando el cliente se conecta al servidor consulta el archivo ~/.ssh/known\_hosts. El fichero ~/.ssh/known\_hosts es una base de datos de fingerprints de los servidores SSH a los que nos hemos conectado.
2. Si el fingerprint del servidor SSH al que nos conectamos está almacenado en ~/.ssh/known\_hosts, querrá decir que no es la primera vez que nos conectamos al servidor. Por lo tanto a priori se trata de un servidor conocido y seguro al que nos podremos conectar sin ningún tipo de problema.
3. En el caso que el fingerprint del servidor SSH no esté almacenado en nuestro ~/.ssh/known\_hosts nos aparecerá el mensaje de advertencia que hemos visto al inicio del artículo. En este caso deberemos proceder según se indica en los puntos 4 y 5.
4. **Si no es la primera vez que nos conectamos al servidor** y nos aparece la advertencia tenemos que responder no y abortar la conexión. Si nos encontramos con esta situación pueden estar pasando 2 cosas. **La primera** de ellas no es peligrosa y es que el administrador del servidor haya generado un par de llaves nuevo. Si este es el caso nos podemos conectar al servidor sin ningún tipo de problema.  **La segunda** es que estemos recibiendo un ataque del tipo man in the middle, y que el servidor al que nos estamos conectando sea un servidor malicioso que pretende robar nuestras credenciales. Una vez robadas nuestras credenciales el atacante podrá conectarse al servidor real en nombre nuestro. Si este es el caso aborten de inmediato la conexión al servidor.
5. **Si aparece el mensaje de advertencia y es la primera vez que nos conectamos al servidor** no nos debemos preocupar. Tan solo tenemos que comprobar que el fingerprint de la advertencia corresponda al fingerprint del servidor SSH. _Si el fingerprint es correcto_ escribiremos yes, presionaremos la tecla Enter y seguidamente nos conectaremos al servidor. En el momento que nos hayamos conectado al servidor, el fingerprint del servidor se almacenará en nuestro archivo ~/.ssh/known\_hosts. _En el caso que el fingerprint no sea correcto_ debemos responder no y abortar la conexión de inmediato.

Por lo tanto queda claro que el fingerprint es una mecanismo para asegurar que nos estamos conectado al servidor al que realmente queremos conectarnos.

## ¿CÓMO PODEMOS SABER EL FINGERPRINT DE NUESTRO SERVIDOR SSH?

Nos dirigimos al directorio donde se almacenan las claves privadas y públicas de nuestro servidor. Para ello ejecutamos el siguiente comando en la terminal:

> ```
> cd /etc/ssh
> ```

Para visualizar nuestras claves privadas ejecutamos el siguiente comando en la terminal:

> ```
> ls -l
> ```

El resultado obtenido en mi caso es el siguiente:

| root@debian:/etc/ssh$ ls -ls total 576 \-rw-r--r-- 1 root root 553122 ene 3 2017 moduli \-rw-r--r-- 1 root root 1723 ene 3 2017 ssh\_config \-rw-r--r-- 1 root root 2513 may 10 2015 sshd\_config \-rw------- 1 root root 668 jul 17 2014 ssh\_host\_ed25519\_key \-rw-r--r-- 1 root root 601 jul 17 2014 ssh\_host\_ed25519\_key.pub \-rw------- 1 root root 227 jul 17 2014 ssh\_host\_ecdsa\_key \-rw-r--r-- 1 root root 173 jul 17 2014 ssh\_host\_ecdsa\_key.pub \-rw------- 1 root root 1679 jul 17 2014 ssh\_host\_rsa\_key \-rw-r--r-- 1 root root 393 jul 17 2014 ssh\_host\_rsa\_key.pub |
| :-- |

El fingerprint de cada una de las llaves se almacena en los ficheros que terminan con la extensión **.pub**.

Si en el inicio del post consultan el mensaje de advertencia que obtengo al conectarme al servidor verán que se está usando la llave ECDSA. Por lo tanto consultaré el fingerprint de la llave ssh\_host\_ecdsa\_key.pub ejecutado el siguiente comando en la terminal:

> ```
> ssh-keygen -l -f ssh_host_ecdsa_key.pub
> ```

Cada una de los términos del comando tiene el siguiente significado:

**ssh-keygen:** Utilidad que uso para conseguir mi propósito. **\-l:** Opción que indica a ssh-keygen que muestre el fingerprint. **\-f:** Opción para poder especificar el nombre de una llave. **ssh\_host\_ecdsa\_key.pub:** Nombre de la llave que quiero averiguar el fingerprint.

El resultado obtenido de ejecutar el comando es el siguiente:

| root@debian:ssh-keygen -l -f ssh\_host\_ecdsa\_key.pub SHA256:TiqZXJITgH/kUE9WoNP4TF2xkescmKxFIe33TpsQRC7 root@debian (ECDSA) |
| :-- |

El fingerprint mostrado es el mismo que el que aparece en el mensaje de advertencia de conexión al servidor. Por lo tanto nos podemos conectar al servidor sin ningún tipo de temor.

## COMO VER LA FINGERPRINT DE LOS SERVIDORES A LOS QUE ME HE CONECTADO

Como hemos dicho anteriormente, el archivo ~/.ssh/known\_hosts almacena la totalidad de fingerprints de los servidores a los que nos hemos conectado.

Por lo tanto para consultar la totalidad de fingerprints tan solo tenemos que ejecutar el siguiente comando en la terminal:

> ```
> cat ~/.ssh/known_hosts
> ```

Procediendo de la forma que describimos en el artículo conseguiremos obtener mayor seguridad en el momento de conectarnos a servidores SSH.
