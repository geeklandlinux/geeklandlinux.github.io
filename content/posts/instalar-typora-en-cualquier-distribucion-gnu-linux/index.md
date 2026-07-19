---
title: "Instalar Typora en cualquier distribución GNU Linux"
date: 2021-02-28
categories: 
  - "linux-2"
tags: 
  - "ofimatica"
  - "software"
coverImage: "Instalar-typora-en-Linux.jpg"
---

En mi caso uso el editor de textos Typora para redactar la totalidad de contenido del blog. Es el método que me resulta más rápido, fácil y práctico. Por este motivo a continuación veremos como podemos instalar Typora y mantenerlo actualizado en cualquier distribución GNU Linux.<!--more-->

## ¿POR QUÉ OS RECOMIENDO EL USO DE TYPORA?

Los motivos por los que uso Typora son los que detallo a continuación:

1. Me permite leer y editar ficheros en formato Markdown de forma sencilla cómoda, rápida y sin distracciones.
2. Proporciona una interfaz limpia sin ningún tipo de distracción. Me permite aprovechar el 100% de la pantalla.
3. Puedo escribir texto en Markdown y de forma inmediata se previsualiza el código tecleado. Por lo tanto, aunque escribamos Markdown en pantalla no veremos el código Markdown. Para ver el código Markdown deberemos activar el modo código fuente.
4. Prácticamente la totalidad de las acciones se pueden realizar mediante [atajos de teclado](https://support.typora.io/Shortcut-Keys/). Por lo tanto obtendremos mayor productividad después de una leve curva de aprendizaje.
5. Dispone de [corrector ortográfico](https://support.typora.io/Spellcheck/) para prácticamente la totalidad de idiomas.
6. Una vez tengo el texto en Markdown lo puedo exportar a multitud de formatos como por ejemplo .html, .pdf, .docx, .odt, .rtf, .epub, .LaTex, etc.
7. Una vez escrito el texto en Markdown lo puedo copiar como .html y pegarlo en mi blog de Wordpress. De esta forma ahorro tiempo.
8. Puedo realizar tablas e insertar bloques de código de forma extremadamente rápida y sencilla. Estas tablas y bloques de código se pueden exportar a html y el código se visualizará perfecto en cualquier página web.
9. Existen diversos temas CSS para que nos podamos sentir cómodos en el momento de crear contenido.
10. Es multiplataforma. Por lo tanto lo puedo usar en Linux, MacOS o Windows. Desafortunadamente no tiene versión para iOS o Android.
11. En mi caso no uso esta opción porque no está disponible ni en Linux ni en Windows. Pero en MacOS Typora ofrece un control de versiones sobre los documentos que editamos.
12. Permite insertar fórmulas matemáticas, insertar diagramas, etc. En resumen me permite realizar todo lo que en mi caso realizaría con un editor de textos convencional, pero de una forma más rápida.

Obviamente para usar Typora se recomienda saber Markdown. Mardown es un lenguaje de etiquetado extremadamente sencillo que [podéis aprender tranquilamente en media hora]({{< relref "/posts/aprender-markdown-en-minutos" >}}) de dedicación.

**Nota** : Typora tiene licencia propietaria y su uso es gratuito. Es previsible que cuando el software salga del estado Beta sea de pago.

## INSTALAR EL EDITOR DE TEXTOS TYPORA EN DEBIAN, UBUNTU Y LINUX MINT

Instalaremos Typora mediante un repositorio. Para no tener ningún tipo de problema al agregar el repositorio de Typora instalaremos el paquete `software-properties-common` ejecutando el siguiente comando:

> ```shell
> sudo apt-get install software-properties-common
> ```

### Importar la clave pública del repositorio de Typora

Para importar la clave pública del repositorio de typora ejecutamos el siguiente comando en la terminal:

> ```shell
> wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
> ```

**Nota:** Otra forma alternativa de importar la clave pública es mediante el comando `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE`

### Agregar el repositorio de Typora en Debian y Ubuntu

Para agregar el repositorio de typora en Debian y/o Ubuntu ejecutaremos el siguiente comando en la terminal:

> ```shell
> sudo add-apt-repository 'deb https://typora.io/linux ./'
> ```

### Agregar el repositorio de Typora en Linux Mint

El método para agregar el repositorio en Linux Mint es distinto a Debian y Ubuntu. El comando que deberemos usar es el siguiente:

> ```shell
> echo -e "\ndeb https://typora.io/linux ./" | sudo tee -a /etc/apt/sources.list
> ```

### Instalar el editor de textos Typora

Finalmente para instalar Typora tan solo tendremos que ejecutar los siguientes comando en la terminal:

> ```shell
> sudo apt update
> 
> sudo apt install typora
> ```

## INSTALAR EL EDITOR DE TEXTOS TYPORA EN OTRAS DISTRIBUCIONES

Typora dispone de paquetes binarios y paquetes Snap. Para instalarlo mediante sus ficheros binarios deberán proceder del siguiente modo.

### Instalar Typora mediante ficheros binarios

Para descargar el paquete binario de Typora en una distribución con arquitectura 64 bits ejecuten el siguiente comando en la la terminal:

> ```shell
> wget https://typora.io/linux/Typora-linux-x64.tar.gz
> ```

**Nota**: En caso que su distribución sea de 32 bits deberán descargar el paquete binario usando el comando `wget https://typora.io/linux/Typora-linux-ia32.tar.gz`

Una vez descargado el fichero binario comprimido lo descomprimiremos mediante el siguiente comando:

> ```shell
> tar xvf Typora-linux-x64.tar.gz
> ```

**Nota**: En el caso que usen una distro de 32 bits el comando será `tar xvf Typora-linux-ia32.tar.gz`

Acto seguido accedemos a la ubicación donde se halla el fichero binario ejecutando el siguiente comando en la terminal:

> ```shell
> cd Typora-linux-x64
> ```

**Nota**: En el caso que usen una distro de 32 bits el comando será `cd Typora-linux-ia32`

Finalmente ejecutaremos Typora ejecutando el siguiente comando en la terminal:

> ```shell
> ./Typora
> ```

### Instalar Typora mediante paquetería SNAP

Instalar Typora mediante snap es sumamente sencillo. Para ello tan solo tendrán que ejecutar el siguiente comando en la terminal:

> ```shell
> sudo snap install typora
> ```

**Nota**: Si conocen una solución igual de buena que Typora y que además sea Software Libre u Open Source déjenla en los comentarios del artículo.

#### Fuentes

[https://support.typora.io/](https://support.typora.io/)
