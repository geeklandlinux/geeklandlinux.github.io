---
title: "Crear entornos virtuales de desarrollo Python en Linux"
date: 2023-03-18
categories: 
  - "linux-2"
tags: 
  - "entornos-virtuales"
  - "programar"
  - "python"
cover:
  image: "images/Crear-entornos-virtuales-de-desarrollo-Python-en-Linux.jpg"
  relative: true
---

En siguiente artículo hablaremos de los entornos virtuales de programación en Phyton. Concretamente abordaremos su definición y sus diferentes utilidades, para posteriormente adentrarnos en el proceso de creación y eliminación de los entornos de desarrollo virtuales de Python.<!--more-->

## ¿QUÉ ES UN ENTORNO VIRTUAL DE DESARROLLO EN PYTHON?

Un entorno virtual de programación en Python es un ambiente aislado en el cual se pueden instalar paquetes y librerías de Python específicas para un proyecto en particular, sin afectar al sistema operativo y otras aplicaciones instaladas en la máquina.

Esto permite a los desarrolladores de software trabajar en diferentes proyectos de Python, cada uno con sus propias dependencias y versiones de paquetes, sin preocuparse por conflictos entre versiones de paquetes y librerías.

## UTILIDADES QUE TIENE UN ENTORNO VIRTUAL DE DESARROLLO EN PYTHON

Las utilidades son las siguientes:

1. **Reproducibilidad del código:** Podemos asegurar que nuestro código se ejecutará correctamente en cualquier momento, sin importar el sistema operativo en el que se encuentre. Esto se logra mediante la creación de un entorno virtual, el cual utiliza versiones específicas de Python y las librerías de Python que necesitamos. De esta manera, podemos garantizar la compatibilidad del código incluso si actualizamos el sistema operativo o la versión de Python.

2. **Evitar conflictos de dependencias:** Si trabaja en varios proyectos de Python al mismo tiempo, es probable que necesite diferentes versiones de las mismas bibliotecas o dependencias. Un entorno virtual puede ayudar a evitar conflictos de dependencias al mantener los requerimientos específicos de cada proyecto en su propio entorno aislado.

3. **Instalar paquetes y dependencias de manera local:** Permite instalar paquetes y dependencias de Python de manera local, lo que significa que puede trabajar sin necesidad de acceso de administrador a su sistema operativo o afectar a otros proyectos de Python en su sistema.

4. **Facilitar la colaboración:** Facilitar la colaboración con otros desarrolladores. Al compartir su entorno virtual con otros miembros del equipo, puede garantizar que todos estén trabajando con las mismas versiones de las bibliotecas y dependencias.

En resumen, un entorno virtual de Python es una herramienta importante para desarrolladores de Python que ayuda a garantizar la reproducibilidad y la portabilidad de su código, a evitar conflictos de dependencias y a facilitar la colaboración en equipo.

## CREAR ENTORNOS VIRTUALES EN PYTHON

Existen diversas herramientas para crear entornos virtuales de Python. Algunas de ellas son venv, conda, virtualenv, Pyenv, etc, No obstante en este artículo nos centraremos en el uso de venv.

### Instalar los paquetes para crear y configurar el entornos virtuales de desarrollo de Python

Lo primero que debemos hacer es instalar el paquete `python3-venv`. Este paquete es necesario para poder crear y gestionar entornos virtuales de Python en nuestro sistema. Si estás utilizando una distribución con el gestor de paquetes apt, podrás instalar `python3-venv` ejecutando el siguiente comando en la terminal:

> ```shell
> sudo apt install python3-venv
> ```

**Nota:** En el caso de usar el gestor de paquetes dnf el comando será `sudo dnf install python3-venv`

**Nota:** En el caso de usar el gestor de paquetes Pacman el comando será `sudo pacman -S python-venv`

### Definir y crear la ubicación donde se almacenarán los ficheros y configuración del entorno virtual de Python

Acto seguido definiremos y crearemos la ubicación donde se almacenará el entorno virtual de Python. En mi caso quiero el entorno virtual en `~/python_3_11_2`, por lo tanto ejecutaré el siguiente comando:

> ```shell
> mkdir -p ~/python_3_11_2
> ```

### Crear el entorno virtual de desarrollo en Python

Para crear el entorno de desarrollo en Python tan solo tendré que ejecutar un comando del siguiente tipo:

> ```shell
> python3 -m venv ubicación_entorno_virtual
> ```

Como la ubicación del entorno virtual es `~/python_3_11_2` ejecutaré el siguiente comando:

> ```shell
> python3 -m venv ~/python_3_11_2
> ```

Una vez que hayas ejecutado el comando para crear el entorno virtual, se creará un entorno virtual con la versión de Python que tengas instalada en tu equipo. Al acceder a la ubicación del entorno virtual, verás la siguiente estructura de ficheros y directorios:

> ```shell
> 1 ├── bin
> 2 ├── include
> 3 ├── lib
> 4 ├── lib64 -> lib
> 5 └── pyvenv.cfg
> ```

Cada uno de estos elementos contiene:

- `bin` : Scripts, y enlaces simbólicos necesarios para activar y desactivar el entorno virtual en el sistema operativo correspondiente. También contiene los binarios de los paquetes que instalaremos en el entorno de desarrollo.
- `include`: Directorio contiene los archivos de cabecera de Python necesarios para compilar módulos de Python que puedas necesitar para tu proyecto.
- `lib`: Este directorio contiene las bibliotecas y módulos de Python instalados en el entorno virtual.
- `lib64`: Directorio que incluye las bibliotecas de Python compiladas para sistemas de 64 bits. Estas bibliotecas pueden ser módulos nativos de Python o paquetes de terceros que se utilizan en el desarrollo de aplicaciones.
- `pyvenv.cfg`: Fichero que contiene la configuración del entorno virtual, incluyendo la versión de Python utilizada, la ubicación del directorio del intérprete de Python, entre otros detalles.

### Crear el entorno virtual de desarrollo en Python con una versión específica de Python

El entorno de desarrollo que acabamos de crear usa la versión 3.11.2 de Python. Si quisiéramos usar otra versión, como por ejemplo la versión 3.9, primero tendríamos que instalarla en el sistema operativo. Acto seguido tendrían que crear el entorno virtual ejecutando un comando del siguiente tipo:

> ```shell
> python3 -m venv --system-site-packages -p ubicación_binario_python ubicación_del_entorno_virtual
> ```

Por lo tanto para crear un entorno de desarrollo de python que use la versión 3.9 en el directorio `~/python_3_9` deberíamos ejecutar el siguiente comando:

> ```shell
> python3 -m venv --system-site-packages -p /usr/bin/python3.9 ~/python_3_9
> ```

### Activar el entorno de desarrollo de Python

Para activar y empezar a operar dentro del entorno de desarrollo que acabamos de crear tan solo tendremos que ejecutar un comando del siguiente tipo:

> ```shell
> source ubicación_entorno_desarrollo/bin/activate
> ```

Como la ubicación de mi entorno de desarrollo es `~/python_3_11_2` ejecutaré el siguiente comando:

> ```shell
> source ~/python_3_11_2/bin/activate
> ```

Después de ejecutar el comando, ingresaremos al entorno de desarrollo. Si instalamos una librería o programa mediante el gestor de paquetes pip, se instalará exclusivamente en el entorno de desarrollo. Esto significa que solo podremos solo usarlo mientras estemos dentro del entorno de desarrollo.

### Instalar paquetes y librerías dentro del entorno de desarrollo

Después de activar el entorno de desarrollo, es posible instalar librerías y programas dentro del mismo mediante el gestor de paquetes pip. Por ejemplo, para instalar youtube-dlp, solo se necesita ejecutar el siguiente comando:

> ```shell
> pip3 install yt-dlp
> ```

Una vez finalizada la instalación podrán usar youtube-dlp. Pero únicamente cuando esté activado el entorno de desarrollo en el que han instalado youtube-dlp.

## MOVERSE ENTRE DISTINTOS ENTORNOS DE DESARROLLO DE PYTHON

Si ahora estamos dentro del entorno de desarrollo `~/python_3_11_2` y queremos movernos a uno diferente como por ejemplo `~/python_3_9` tan solo tendremos que ejecutar un comando del siguiente tipo:

> ```shell
> source ubicación_entorno_desarrollo/bin/activate
> ```

Como el entorno de desarrollo que quiero activar se ubica en `~/python_3_9` ejecutaré el siguiente comando:

> ```shell
> source ~/python_3_9/bin/activate
> ```

## SALIR DEL ENTORNO VIRTUAL DE DESARROLLO PYTHON QUE TENEMOS ACTIVADO

Si finalmente queremos salir del entorno de desarrollo que tenemos activado tan solo tentemos que ejecutar el siguiente comando:

> ```shell
> deactivate
> ```

## LISTAR LOS ENTORNOS VIRTUALES PYTHON PRESENTES EN NUESTRO EQUIPO

Para listar de forma recursiva la totalidad de entornos de desarrollo virtuales presentes en una ubicación del sistema operativo, como por ejemplo `/home/joan`, tan solo tenemos que ejecutar el siguiente comando:

> ```shell
> ❯ find /home/joan -type f -name "pyvenv.cfg"
> /home/joan/chatgpt/pyvenv.cfg
> /home/joan/python_3_11_2/pyvenv.cfg
> /home/joan/python_3_9/pyvenv.cfg
> ```

De esta forma se puede ver que en mi caso tengo 3 entornos de desarrollo. Sus ubicaciones son:

1. /home/joan/chatgp
2. /home/joan/python\_3\_11\_2
3. /home/joan/python\_3\_9

## ELIMINAR ENTORNOS VIRTUALES DE PYTHON

Para eliminar cualquiera de los entornos virtuales tan solo tendremos que ejecutar un comando del siguiente tipo:

> ```shell
> rm -rf ubicación_entorno_virtual
> ```

Por lo tanto para borrar el entorno virtual `~/python_3_11_2` ejecutaremos el siguiente comando:

> ```shell
> rm -rf ~/python_3_11_2
> ```
