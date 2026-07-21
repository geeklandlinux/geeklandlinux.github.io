---
title: "Como instalar y usar el gestor de paquetes PIP"
date: 2023-04-01
categories: 
  - "linux-2"
  - "windows"
tags: 
  - "gestor-de-paquetes"
  - "pip"
  - "python"
cover:
  image: "images/Como-instalar-y-usar-el-gestor-de-paquetes-PIP.jpg"
  relative: true
---

Pip es un gestor de paquetes muy popular en el universo Python, que nos permite realizar la gestión de paquetes y librerías de Python de forma muy sencilla en nuestro sistema Linux. Con Pip, podemos instalar, actualizar, eliminar y buscar paquetes de Python, y nos proporciona una forma conveniente de gestionar dependencias y versiones entre diversas librerías de Python que tengamos en nuestro sistema. En este artículo, exploraremos en detalle cómo usar pip en Linux y Windows para instalar y gestionar paquetes de Python en su sistema.<!--more-->

## ¿QUÉ ES PIP?

Pip es un gestor de paquetes utilizado para instalar y administrar bibliotecas así como módulos de terceros en un proyecto de Python. Pip es el acrónimo de "Pip Installs Packages" y es una herramienta muy útil para instalar, actualizar, eliminar y buscar paquetes de Python.

Los paquetes Python instalado mediante pip proceden del siguiente repositorio:

[https://pypi.org/](https://pypi.org/)

Dentro de esta web podrán navegar y ver la totalidad de paquetes y librearías disponibles para su instalación.

## ¿CÓMO INSTALAR PIP USANDO PYTHON 3?

Para instalar el gestor de paquetes PIP tienen que proceder del siguiente modo en función del sistema operativo.

### Instalar el gestor de paquetes PIP En GNU-Linux

Para instalar pip en una distribución que use el gestor de paquetes apt deberán ejecutar el siguiente comando en la terminal:

> ```shell
> sudo apt install python3-pip
> ```

Si por lo contrario usan el gestor de paquetes dnf deberán ejecutar el siguiente comando:

> ```shell
> sudo dnf install python3 python3-wheel
> ```

Si usan Pacman:

> ```shell
> pacman -S python-pip
> ```

Finalmente si usan Zipper deberán ejecutar el siguiente comando:

> ```shell
> sudo zypper install python3-pip python3-setuptools python3-wheel
> ```

### Instalar el gestor de paquetes PIP en Windows

Para instalar PIP en Windows tenemos que proceder del siguiente modo:

1. Abrimos el navegador web y accedemos a [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) para descargar el fichero `get-pip.py`.
2. Acto seguido abrimos la consola de windows y nos dirigimos a la ubicación donde hemos descargado el fichero `get-pip.py`.
3. Finalmente ejecutamos el siguiente comando para que se instale el gestor de paquetes PIP.

> ```
> python3 get-pip.py
> ```

Para verificar que PIP se ha instalado correctamente tan solo hay que ejecutar el siguiente comando:

> ```
> pip --version
> ```

Si aparece información sobre la versión de PIP que has instalado, entonces la instalación ha sido exitosa.

**Nota: Obviamente antes de instalar PIP tenemos que tener instalado Python**

Una vez instalado el gestor de paquetes PIP podremos usarlo del siguiente modo.

## RESUMEN DE LA TOTALIDAD DE OPERACIONES QUE SE PUEDEN REALIZAR CON EL GESTOR DE PAQUETES PIP

Algunos de los comandos y operaciones más habituales del gestor de paquetes PIP son los que se muestran a continuación:

| Comando gestor paquetes pip | Explicación del comando |
| --- | --- |
| `pip install nombre_paquete` | Para instalar un programa o librería. |
| `pip install --pre nombre_paquete` | Instalar la versión beta de un programa o librería. |
| `pip download nombre_paquete` | Para descargar un paquete y la totalidad de sus dependencias sin instalar nada en el sistema operativo. |
| `pip uninstall nombre_paquete` | Para desinstalar un programa o librería. |
| `pip freeze` | Listar la totalidad de paquetes python instalados con pip indicando su versión y siguiendo siempre un formato determinado. |
| `pip list` | Muestra los mismos resultados que el comando `pip freeze`. Los resultados son más fáciles de leer, pero no siguen un formato tan rígido como `pip freeze` |
| `pip list --outdated` | Listar la totalidad de paquetes obsoletos y que por lo tanto tienen una versión más reciente disponible. |
| `pip show nombre_paquete` | Obtener información sobre un paquete instalado mediante pip. |
| `pip check` | Para verificar que los paquetes instalados están bien instalados y no tienen problemas de dependencias. |
| `pip install --upgrade nombre_paquete` | Para actualizar un paquete específico a la última versión. |
| `pip freeze \| grep -v '^\-e' \| cut -d = -f 1 \| xargs -n1 pip install -U` | Actualizar la totalidad de paquetes a la última versión. |
| `pip freeze --local \|grep -v '^\-e' \|cut -d = -f 1 \|xargs -n1 pip install -U` | Actualizar la totalidad de paquetes a la última versión de únicamente los paquetes instalados dentro del entorno virtual de desarrollo activo. |
| `pip freeze \| xargs pip uninstall -y` | Para eliminar la totalidad de paquetes Python instalados. |
| `pip cache dir` | Ver la ubicación del directorio que almacena copias en forma de cache de los paquetes que se descargan de los repositorios de PyPI (Python Package Index). |
| `pip cache purge` | Limpiar la cache de pip. |
| `pip --version` | Mostrar la versión de pip instalada en el sistema operativo. |
| `pip --help` | Mostrar ayuda para usar el comando pip. |

A continuación veremos algunos ejemplos de los comando que acabo de citar en la tabla.

## INSTALAR UN PROGRAMA O LIBRERÍA

Para instalar el programa youtube-dlp ejecutaremos el siguiente comando:

> ```shell
> pip install yt-dlp
> ```

## DESINSTALAR UN PROGRAMA O LIBRERÍA

Para desinstalar el programa que acabamos de instalar, que es youtube-dlp, ejecutaremos el siguiente comando:

> ```shell
> pip uninstal yt-dlp
> ```

## OBTENER INFORMACIÓN SOBRE UN PAQUETE QUE TENGAMOS INSTALADO

Para obtener información específica sobre un paquete que tenemos instalado en nuestro equipo tendremos que ejecutar un comando del siguiente tipo:

> ```shell
> pip show nombre_paquete
> ```

Por lo tanto para obtener información detallada del paquete `yt-dlp` ejecutaremos el siguiente comando:

> ```shell
> pip show yt-dlp
> Name: yt-dlp
> Version: 2023.3.4
> Summary: A youtube-dl fork with additional features and patches
> Home-page: https://github.com/yt-dlp/yt-dlp
> Author: 
> Author-email: 
> License: 
> Location: /home/joan/python_3_11_2/lib/python3.11/site-packages
> Requires: brotli, certifi, mutagen, pycryptodomex, websockets
> Required-by: 
> ```

Si observamos la salida del comando vemos que obtenemos la siguiente información:

- Nombre completo del paquete.
- Versión del paquete.
- Descripción del paquete.
- URL de origen del paquete.
- Requerimientos y dependencias del paquete.
- Ubicación donde se almacena el paquete.
- Dependencias que el paquete requiere para funcionar correctamente
- etc.

## LISTAR LA TOTALIDAD DE PAQUETES INSTALADOS MEDIANTE EL GESTOR DE PAQUETES PIP

Para listar la totalidad de paquetes instalados conjuntamente con su versión tan solo tenemos que ejecutar el comando `pip freeze` del siguiente modo:

> ```shell
> pi@raspberrypi:~ $ pip freeze
> asn1crypto==1.5.1
> attrs==21.4.0
> bcrypt==3.2.0
> beautifulsoup4==4.11.1
> cached-property==1.5.2
> certifi==2021.10.8
> cffi==1.15.0
> chardet==4.0.0
> charset-normalizer==2.0.12
> click==8.1.2
> ....
> ```

## LISTAR LOS PAQUETES OBSOLETOS O SIN ACTUALIZAR MEDIANTE EL GESTOR DE PAQUETES PIP

Para simplemente ver los paquetes que disponen de alguna actualización, y que por lo tanto están obsoletos, hay que ejecutar el siguiente comando en la terminal.

> ```shell
> pip list --outdated
> ```

En mi caso los paquetes obsoletos son los siguientes:

> ```shell
> pi@raspberrypi:~ $ pip list --outdated
> Package             Version Latest      Type
> ------------------- ------- ----------- -----
> beautifulsoup4      4.10.0  4.11.1      wheel
> click               8.0.4   8.1.2       wheel
> hachoir             3.1.2   3.1.3       wheel
> importlib-resources 5.6.0   5.7.1       wheel
> jeepney             0.7.1   0.8.0       wheel
> prompt-toolkit      3.0.28  3.0.29      wheel
> PyGObject           3.42.0  3.42.1      sdist
> SecretStorage       3.3.1   3.3.2       wheel
> ...
> ```

## VER SI LOS PAQUETES INSTALADOS PUEDEN TENER PROBLEMAS DE DEPENDENCIAS EN LA ACTUALIZACIÓN

Para ver si tenemos problemas de dependencias podemos usar el comando `pip check`. En el caso de tener problemas de dependencias obtendréis un resultados similar al siguiente:

> ```shell
> pi@raspberrypi:~ $ pip check
> docker-compose 1.29.2 has requirement jsonschema<4,>=2.5.1, but you have jsonschema 4.4.0.
> docker-compose 1.29.2 has requirement PyYAML<6,>=3.10, but you have pyyaml 6.0.
> docker-compose 1.29.2 has requirement websocket-client<1,>=0.32.0, but you have websocket-client 1.3.2.
> ```

En caso que no hubieran problemas de dependencias el resultado habría sido el siguiente:

> ```shell
> No broken requirements found.
> ```

## ACTUALIZAR UN PAQUETE A LA ÚLTIMA VERSIÓN

Anteriormente hemos visto que el paquete `beautifulsoup4` no está a la última versión.

> ```shell
> pi@raspberrypi:~ $ pip list --outdated
> Package             Version Latest      Type
> ------------------- ------- ----------- -----
> beautifulsoup4      4.10.0  4.11.1      wheel
> ```

Para actualizar este paquete a la última versión tan solo tendremos que ejecutar el siguiente comando:

> ```shell
> pip install --upgrade beautifulsoup4
> ```

Acto seguido se actualizará el paquete.

## ACTUALIZAR LA TOTALIDAD DE PAQUETES MEDIANTE EL GESTOR DE PAQUETES PIP

Para actualizar la totalidad de paquetes que están obsoletos tan solo tienen que ejecutar el siguiente comando en la terminal:

> ```shell
> pip freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
> ```

Acto seguido empezará la actualización de los paquetes:

> ```shell
> pi@raspberrypi:~ $ pip freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
> Defaulting to user installation because normal site-packages is not writeable
> Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
> Requirement already satisfied: beautifulsoup4 in ./.local/lib/python3.7/site-packages (4.10.0)
> Collecting beautifulsoup4
>   Downloading https://www.piwheels.org/simple/beautifulsoup4/beautifulsoup4-4.11.1-py3-none-any.whl (130 kB)
>      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 130.1/130.1 KB 54.3 kB/s eta 0:00:00
> Requirement already satisfied: soupsieve>1.2 in ./.local/lib/python3.7/site-packages (from beautifulsoup4) (2.3.1)
> Installing collected packages: beautifulsoup4
>   Attempting uninstall: beautifulsoup4
>     Found existing installation: beautifulsoup4 4.10.0
>     Uninstalling beautifulsoup4-4.10.0:
>       Successfully uninstalled beautifulsoup4-4.10.0
> Successfully installed beautifulsoup4-4.11.1
> Defaulting to user installation because normal site-packages is not writeable
> ...
> ```

## CREAR ENTORNOS VIRTUALES DE PYTHON

Si ahora lo que pretenden es crear entornos virtuales de desarrollo en Python les recomiendo que visiten el siguiente enlace:

[https://geeklandlinux.github.io/posts/crear-entornos-virtuales-de-desarrollo-python-en-linux/]({{< relref "/posts/crear-entornos-virtuales-de-desarrollo-python-en-linux" >}})

### Fuente

[https://www.activestate.com/resources/quick-reads/how-to-update-all-python-packages/](https://www.activestate.com/resources/quick-reads/how-to-update-all-python-packages/)
