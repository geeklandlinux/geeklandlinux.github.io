---
title: "Cambiar de directorio de forma rápida con pushd y popd"
date: 2022-08-21
categories: 
  - "linux-2"
tags: 
  - "cd"
  - "popd"
  - "pushd"
  - "terminal"
cover:
  image: "images/Cambiar-de-directorio-de-forma-rapida-y-sencilla-en-Linux.jpeg"
  relative: true
---

Hay varías formas para cambiar de directorio de forma rápida y sencilla en Linux. Algunas bastante conocidas son:

1. Usar el autocompletado de rutas mediante la tecla `TAB`.
2. Usar el [historial de comandos]({{< relref "/posts/como-usar-y-configurar-el-historial-de-comandos-history-en-linux" >}}) mediante el atajo de teclado `Ctrl+r` y de esta forma acceder a comandos ejecutados en el pasado.
3. Comandos para ir directamente al directorio `/home` o al directorio anterior.
4. Etc

No obstante existen otros métodos menos conocidos como por ejemplo gestionar una pila de rutas con los comandos `pushd`, `popd`, `cd` y `dirs`. A continuación veremos este método que seguro que a muchos de vosotros les será de gran utilidad.<!--more-->

## FORMA EN LA QUE PUSHD Y POPD NOS PERMITE CAMBIAR DE DIRECTORIO DE FORMA RÁPIDA Y SENCILLA

El comando `pushd` nos permitirá crear una lista o pila de directorios enumerada. Una vez creada la pila o lista de directorios el comando `cd` nos permitirá ir de forma rápida y sencilla a cualquier directorio almacenado en la pila o lista de directorios. De esta forma podremos cambiar de directorio de forma rápida y sencilla.

Además el comando `dirs` nos permitirá listar los directorios que tenemos en la pila de directorios en todo momento.

La forma más sencilla de entender lo que acabo de decir es mediante el siguiente ejemplo.

## CAMBIAR ENTRE DIRECTORIOS MEDIANTE LOS COMANDOS PUSDHD Y POPD

Imaginemos el caso que tenemos que estar constantemente intercambiando entre los siguientes directorios o rutas:

```shell
1. /home/geekland
2. /var/log
3. /var/tmp
4. ~/.config
5. /etc
6. /usr 
```

Si por ejemplo queréis cambiar del directorio `1` al directorio `2` y después del directorio `2` al directorio `6` les recomiendo que lo hagan del siguiente modo.

### Crear una pila o lista con las rutas que necesitamos acceder (sin acceder a la rutas que introducimos)

Lo primero que tenemos que realizar es crear una lista o pila de directorios. Para introducir los 6 directorios mencionados en el inicio de este apartado lo haremos del siguiente modo:

El primer directorio es `/home/geekland`. Por lo tanto lo primero a realizar es dirigirse al directorio `/home/geekland` ejecutando el siguiente comando en la terminal:

```shell
❯:~$ cd /home/geekland
```

A continuación introduciremos el resto de directorios del siguiente modo:

```shell
❯:~$ pushd -n /var/log
~ /var/log
```

**Nota:** Si os fijáis la salida del comando indica los directorios que tenemos en la pila. En este ejemplo los 2 directorios que ya tenemos en la pila son `~` y `/var/log`

**Nota:** La estructura que usamos para añadir el directorio en la pila es `pushd -n` seguido del `directorio que queremos añadir a la pila`. La opción `-n` hará que en el momento de introducir el directorio en la pila nos quedemos fijos en el directorio actual.

**Nota:** Cada una de las nuevas rutas añadida se almacenarán en la posición `1` de la pila de directorios.

Para añadir el resto de directorios en la pila seguiremos usando el comando `pushd -n` seguido de la `directorio que queremos añadir a la pila` del siguiente modo:

```shell
❯:~$ pushd -n /var/tmp
~ /var/tmp /var/log
```

`❯:~$ pushd -n ~/.config ~ ~/.config /var/tmp /var/log`

`❯:~$ pushd -n /etc ~ /etc ~/.config /var/tmp /var/log`

`❯:~$ pushd -n /usr ~ /usr /etc ~/.config /var/tmp /var/log`

Si os fijáis la salida del último comando muestra la totalidad de directorios a los que necesitamos acceder. Por lo tanto hemos terminado nuestra tarea.

### Listar las rutas de la pila

Cada vez que ejecutamos un comando `popd` o `pushd` veremos los directorios que tenemos almacenados en la pila. Pero también podemos consultar los directorios presentes en la pila con el comando `dirs`. El comando `dirs` nos permite obtener la totalidad de directorios almacenados en la pila de forma enumerada. Esto será de grandísima utilidad para que después podamos acceder al directorio que nosotros queremos con suma facilidad.

Si simplemente ejecutamos el comando `dirs` obtendremos los directorios almacenados en la pila:

```shell
❯:~$ dirs
~ /usr /etc ~/.config /var/tmp /var/log
```

Si ahora queremos obtener los directorios presentes a la pila de forma enumerada añadiremos la opción `-v` del siguiente modo:

```shell
❯:~$ dirs -v
 0  ~
 1  /usr
 2  /etc
 3  ~/.config
 4  /var/tmp
 5  /var/log
```

Si finalmente queremos imprimir la ruta completa de los directorios añadiremos la opción `-l` del siguiente modo:

```shell
❯:~$ dirs -v -l
 0  /home/geekland
 1  /usr
 2  /etc
 3  /home/geekland/.config
 4  /var/tmp
 5  /var/log
```

**Nota:** La posición número `0` de la pila se irá reescribiendo y siempre será el directorio de trabajo actual.

### Duplicar la ruta raíz, o la posición 0 de la lista, en el caso que lo consideren necesario

Si observamos la última pila de directorios vemos que tenemos la totalidad de directorios en la pila. Pero si os fijáis el directorio `/home/geekland` está en la posición `0`. Esto es un problema porque la posición `0` se sobrescribe cada vez que cambiamos de directorio. La posición `0` siempre será la ruta del directorio en que estamos ubicados actualmente.

Por lo tanto les recomiendo duplicar el directorio almacenado en la posición `0` del siguiente modo:

```shell
❯:~$ pushd .
~ ~ /usr /etc ~/.config /var/tmp /var/log
```

Si ahora volvemos a consultar los directorios que tenemos en la pila vemos que entre la posición `1` y la posición `6` están presentes la totalidad de directorios a los que necesitamos acceder de forma frecuente.

```shell
❯:~$ dirs -v -l
 0  /home/geekland
 1  /home/geekland
 2  /usr
 3  /etc
 4  /home/geekland/.config
 5  /var/tmp
 6  /var/log
```

### Cambiar al directorio que tenemos almacenado en la posición 3 de la pila de directorios

Si estamos en la ruta `/home/geekland` y queremos cambiar al directorio `/etc` tan solo tenemos que ver la posición que el directorio `/etc` ocupa en la pila de directorios. En nuestro caso ocupa la posición `3`. Por lo tanto para acceder al directorio `/etc` tan solo tendremos que ejecutar el siguiente comando:

```shell
❯:~$ cd ~3
```

Justo al ejecutar el comando nos dirigiremos al directorio `/etc`. Si ahora imprimimos nuestra pila veremos que los directorios presentes en la pila son los siguientes:

```shell
❯:/etc$ dirs -v -l
 0  /etc
 1  /home/geekland
 2  /usr
 3  /etc
 4  /home/geekland/.config
 5  /var/tmp
 6  /var/log
```

Si analizamos los resultados vemos:

1. Antes el directorio `0` era `/home/geekland`. Ahora es `/etc`. Esto es así porque el directorio `0` siempre mostrará el directorio en el que nos encontramos ubicados.
2. El resto de directorios serán exactamente iguales que los anteriores. Esto es sumamente importante porque esto es lo que nos permite asociar un directorio a un número. Por lo tanto el directorio `3` siempre será el `/etc` y el directorio `6` siempre será el `/var/log`.

Si ahora nos queremos dirigir al directorio `6`, que es el `/var/log`, ejecutaremos el siguiente comando en la terminal:

```shell
❯:~$ cd ~6
```

Y la pila de directorios quedará del siguiente modo:

```shell
❯:/var/log$ dirs -v -l
 0  /var/log
 1  /home/geekland
 2  /usr
 3  /etc
 4  /home/geekland/.config
 5  /var/tmp
 6  /var/log
```

Y al igual que antes:

1. El directorio `0` ahora será `/var/log` porque es el directorio en el que nos encontramos ubicados.
2. El resto de directorios del `1` al `6` serán exactamente los mismos que antes.

### Ir intercambiando entre el directorio almacenado en la posición 0 y 1 de la pila de directorios

Siguiendo con el ejemplo anterior podemos ver que estamos ubicados en la ruta `/var/log`

```shell
❯:/var/log$ pwd
/var/log
```

Los directorios que tenemos que almacenados en la pila son los siguientes:

 `0  /var/log  1  /home/geekland  2  /usr  3  /etc  4  /home/geekland/.config  5  /var/tmp  6  /var/log`

Si ahora queremos que intercambiar varias veces entre los directorios almacenados en la posición `0` y `1` de nuestra pila lo podemos hacer de varias formas. La primera de ellas es identificando las posiciones de los directorios `/var/log` y `/home/geekland`. En nuestro caso estos directorios están ubicados en las posiciones `1` y `6`de la pila. Por lo tanto si estamos en el directorio `/var/log` y queremos ir al `/home/geekland` ejecutaremos el comando:

```shell
❯:/var/log$ cd ~1
```

y si ahora queremos volver al `/var/log` ejecutaremos el comando:

```shell
❯:~$ cd ~6
```

Otra forma para conseguir el mismo propósito sería mediante el comando `pushd`. Pero esta forma no me gusta porque modifica las posiciones de la pila de directorios y en mi caso siempre me gusta tener asociado un directorio a un número. Pero si lo queréis hacer de esta forma tened en cuenta que cada vez que ejecutéis el comando `pushd` el directorio almacenado en la posición `0` se irá a la posición `1` y el que está en la posición `1` a la `0`. Por lo tanto si nuestra pila de directorios es la siguiente:

 `0  /var/log  1  /home/geekland  2  /usr  3  /etc  4  /home/geekland/.config  5  /var/tmp  6  /var/log`

y ejecutamos el comando `pushd` nos dirigiremos automáticamente al directorio `/home/geekland`

```shell
❯:/var/log$ pushd
~ /var/log /usr /etc ~/.config /var/tmp /var/log

❯:~$
```

Si ahora miramos la pila de directorios veremos que se han intercambiado. El directorio que antes estaba en la posición `0` ahora está en la posición `1` y el que estaba en la posición `1` ahora está la posición `0`.

```shell
❯:~$ dirs -v -l
 0  /home/geekland
 1  /var/log
 2  /usr
 3  /etc
 4  /home/geekland/.config
 5  /var/tmp
 6  /var/log
```

si ahora volvemos a ejecutar de nuevo el comando `pushd` volveremos a ubicarnos dentro del directorio `/var/log`

```shell
❯:~$ pushd
/var/log ~ /usr /etc ~/.config /var/tmp /var/log

❯:/var/log$
```

y además se volverán a intercambiar los directorios almacenados en la posición `0` y `1` de la pila.

```shell
❯:/var/log$ dirs -v -l
 0  /var/log
 1  /home/geekland
 2  /usr
 3  /etc
 4  /home/geekland/.config
 5  /var/tmp
 6  /var/log
```

### Eliminar un elemento de la pila de directorios

El comando `popd` lo podemos usar para eliminar un elemento de la pila. Si por ejemplo queremos eliminar el directorio almacenado en la posición `4` ejecutaremos el siguiente comando:

```shell
❯:/var/log$ popd +4
/var/log ~ /usr /etc /var/tmp /var/log
```

Si miramos los directorios almacenados en la pila vemos que se ha borrado la número `4`. Las rutas almacenadas serán las siguientes:

```shell
❯:/var/log$ dirs -v -l
 0  /var/log
 1  /home/geekland
 2  /usr
 3  /etc
 4  /var/tmp
 5  /var/log
```

### Eliminar el directorio almacenado en la posición 0 de la pila

Si queremos eliminar el directorio almacenado en la posición `0` de la pila tan solo tenemos que ejecutar el comando `popd` del siguiente modo:

```shell
❯:/var/log$ popd
~ /usr /etc /var/tmp /var/log
```

Si ahora vuelven a observar la pila de directorios verán que en vez de tener `6` posiciones tendrá `5` posiciones.

```shell
❯:~$ dirs -v -l
 0  /home/geekland
 1  /usr
 2  /etc
 3  /var/tmp
 4  /var/log
```

Si volvemos a ejecutar el comando `popd`

```shell
❯:~$ popd
/usr /etc /var/tmp /var/log
```

Entonces se volverá a borrar el directorio número `0` de la pila. Además la pila pasará de tener `5` elementos a tener `4` elementos.

```shell
❯:/usr$ dirs -v -l
 0  /usr
 1  /etc
 2  /var/tmp
 3  /var/log
```

Si continuamos ejecutando el comando `popd` borraremos la totalidad de directorios almacenados en la pila.

### Cambiar de directorio modificando el orden de la pila de directorios

También podemos usar el comando `pushd` para movernos de un directorio a otro a la vez que modificamos el orden de la pila de directorios. En mi caso no uso está funcionalidad, pero si vosotros la queréis usar lo podéis hacer del siguiente modo.

Si tenemos la siguiente pila de directorios:

 `0  /usr  1  /etc  2  /var/tmp  3  /var/log`

Y queremos acceder al directorio `2` que es el `/var/tmp` podemos ejecutar el siguiente comando:

```shell
❯:/usr$ pushd +2
/var/tmp /var/log /usr /etc

❯:/var/tmp$
```

Si después de ejecutar el comando observamos el orden de la pila de directorios vemos lo siguiente:

```shell
❯:/var/tmp$ dirs -v -l
 0  /var/tmp
 1  /var/log
 2  /usr
 3  /etc
```

Todos los directorios de la posición `2` en adelante se han desplazado a la parte superior. Por lo tanto, en el ejemplo en cuestión, los directorios que ocupaban la posición `2` y `3` ahora ocupan la posición `0` y `1`. Y los directorios que antes ocupaban las posiciones `0` y `1` ahora ocupan las posiciones `2` y `3`. Particularmente no me gusta esta forma de trabajar porque me gusta tener siempre asociado un número a un directorio.

### Crear una pila de directorios de forma automática

Si quieren pueden automatizar la creación de la pila de directorios. Si siempre se mueven por los mismos directorios, que por ejemplo podrían ser los siguientes:

```shell
1. /home/geekland
2. /var/log
3. /var/tmp
4. ~/.config
5. /etc
6. /usr 
```

Pueden abrir el fichero `.bashrc` mediante el siguiente comando:

```shell
❯:~$ nano ~/.bashrc
```

Cuando se abra el editor de texto nano añadiremos el siguiente código al final del fichero para que cada vez que abramos un emulador de terminal se cree nuestra pila de directorios de forma automática:

```shell
pushd -n /var/log
pushd -n /var/tmp
pushd -n ~/.config
pushd -n /etc
pushd -n /usr
pushd  .
```

Acto seguido guardamos los cambios y cerramos el fichero. A partir de ahora cada vez que abramos una terminal se nos creará una pila de directorios de forma automática.

## CONCLUSIONES SOBRE COMO CAMBIAR DE DIRECTORIO DE FORMA SENCILLA

La forma detallada en el artículo es una forma rápida y eficaz para cambiar de un directorio a otro de forma rápida. El método es especialmente efectivo en el caso que tengamos que intercambiar de forma prolongada entre los mismos directorios.

Hay otros usos de los comandos `pushd` y `popd`, pero lo que he detallado en este artículo es la forma que me parece mejor en mi caso.

#### Fuentes

[https://unix.stackexchange.com/questions/77077/how-do-i-use-pushd-and-popd-commands](https://unix.stackexchange.com/questions/77077/how-do-i-use-pushd-and-popd-commands)
