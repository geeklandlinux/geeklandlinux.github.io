---
title: "Establecer aplicaciones predeterminadas desde la terminal Linux"
date: 2023-08-05
categories: 
  - "linux-2"
tags: 
  - "aplicaciones"
  - "configuracion"
  - "terminal"
coverImage: "Establecer-aplicaciones-predeterminadas-desde-la-terminal-Linux.jpg"
---

En mi caso he usado Firefox durante una larga temporada, no obstante por temas de rendimiento he decidido migrar a Brave Browser con el pequeño inconveniente que al usar el escritorio i3 no sabia como fijar Brave como navegador predeterminado. Al final he terminado haciendo uso de la terminal del siguiente modo que verán a continuación. Además también he aprovechado para configurar el resto de aplicaciones predeterminadas del sistema desde la terminal del modo que verán a continuación.<!--more-->

**Nota:** Los comandos que verán en es artículo han sido testeados en una distribución Debian. Es posible que en otras distribuciones algunos de los comandos no funcione.

## VER APLICACIONES PREDETERMINADAS DEL SISTEMA OPERATIVO

En el caso que quieran ver las aplicaciones predeterminadas de su sistema operativo pueden consultar el fichero `~/.config/mimeapps.list`. Para ello pueden ejecutar el siguiente comando en la terminal:

> ```shell
> cat ~/.config/mimeapps.list
> ```

Una vez se abra el archivo encontrarán dos secciones:

1. **\[Default Applications\]:** Se establecen las asociaciones predeterminadas de tipos MIME con las aplicaciones que se utilizarán para abrirlos.
2. **\[Added Associations\]:** Se definen asociaciones adicionales para tipos MIME específicos, lo que permite personalizar o agregar asociaciones que no están predeterminadas.

Así por lo tanto, si quiero saber la aplicación predeterminada para abrir ficheros imágenes jpeg puedo consultar la sección `[Default Applications]` y veré lo siguiente:

> ```shell
> [Default Applications]
> x-scheme-handler/http=brave-browser.desktop
> x-scheme-handler/https=brave-browser.desktop
> text/plain=typora.desktop
> image/jpeg=viewnior.desktop
> image/png=viewnior.desktop
> video/x-matroska=vlc.desktop
> ....
> ```

Por lo tanto puedo concluir que la totalidad de imágenes png y jpeg se abrirán con el visor de imágenes viewnior. Si os fijáis en el contenido de este fichero veremos que para establecer una aplicación por defecto hay que:

1. indicar el tipo de tipo de MIME/extensión.
2. Conocer el nombre del fichero.desktop del programa que queremos configurar como predeterminado.

Por lo tanto, a continuación verán como averiguar el nombre de los ficheros.desktop y como referirse a los distintas extensiones de archivo (MIME).

## AVERIGUAR EL NOMBRE DEL FICHERO.DESKTOP DE LOS PROGRAMAS

La totalidad de ficheros .desktop se almacenan en las ubicaciones:

- ~/.local/share/applications/
- /usr/share/applications/

Por lo tanto, para averiguar el nombre de los ficheros.desktop tan solo tendremos que ejecutar los comandos `ls /usr/share/applications/` y `ls ~/.local/share/applications/` del siguiente modo:

> ```shell
> ❯ ls /usr/share/applications/
> Permissions Size User Date Modified Name
> .rw-r--r-- 14k root 13 ene 14:52  atril.desktop
> .rw-r--r-- 4,1k root 30 abr 04:10  audacious.desktop
> .rw-r--r-- 7,3k root 17 mar  2022  bleachbit-root.desktop
> .rw-r--r-- 9,3k root 17 mar  2023 brave-browser.desktop
> .rw-r--r-- 3,4k root 23 jun  2021  redshift.desktop
> .rw-r--r-- 9,1k root 11 feb 16:56  synaptic.desktop
> .rw-r--r-- 308 root  5 abr  2022  syncthing-start.desktop
> .rw-r--r-- 324 root  5 abr  2022  syncthing-ui.desktop
> .rw-r--r-- 6,1k root  8 jul 12:02  thunderbird.desktop
> .rw-r--r-- 17k root 18 mar 03:26  transmission-gtk.desktop
> .rw-rw-r-- 288 root 19 may 12:11  typora.desktop
> .rw-r--r-- 15k root 15 jul 15:05  thunar.desktop
> .rw-r--r-- 2,5k root 16 ago  2021  viewnior.desktop
> .rw-r--r-- 5,6k root  4 jul 21:02  vim.desktop
> .rw-r--r-- 15k root 15 jul 15:05  vlc.desktop
> .rw-r--r-- 259 joan  2 ago  2022  wechat.desktop
> ...
> ```
> 
> ```shell
> ❯ ls ~/.local/share/applications/
> Permissions Size User Date Modified Name
> .rw-r--r-- 192 joan  2 ago  2022  Freezer.desktop
> .rw-r--r-- 0 joan 31 mar 00:08  mimeapps.list
> .rw-r--r-- 13 joan 29 ene 20:48  mimeinfo.cache
> .rw------- 168 joan 17 sep  2022  userapp-Firefox-5OEZS1.desktop
> .rw------- 151 joan 17 sep  2022  userapp-Firefox-78R9W0.desktop
> .rw------- 154 joan 29 ene 20:48  userapp-viewnior-MXAVZ1.desktop
> .rwx--x--x   225 joan 31 jul  2022  wechat.desktop
> ...
> ```

Una vez conocemos el nombre del fichero.desktop del programa queremos como predeterminado tendremos que averiguar como referirnos a una determinada extensión de archivo.

## AVERIGUAR COMO REFERIRSE A LAS DISTINTAS EXTENSIONES DE ARCHIVO

En el mundo de la tecnología y Linux, encontramos una gran diversidad de tipos de archivos (MIME), los cuales pueden variar según la distribución que estemos utilizando. Además, con el tiempo pueden surgir nuevos formatos de archivo que debemos conocer. Si utilizan Debian o cualquier otra distribución Linux, podrán acceder a una lista de los tipos MIME registrados en el directorio `/usr/share/mime/packages/`. Para ello tendrán que ejecutar el siguiente comando en la terminal:

> ```shell
> cat /usr/share/mime/packages/*.xml | grep "type=\"[^ ]*\"" | sed 's/.*type="\(.*\)".*/\1/' | sort | uniq
> ```

Algunos de los MIME o tipos de archivo que encontrarán serán los siguientes:

| MIME | EXPLICACIÓN |
| --- | --- |
| `text/plain` | Para referirse a ficheros de texto plano |
| `text/html` | Ficheros con extensión html |
| `text/markdown` | Ficheros con extensión markdown |
| `image/jpeg` | Imágenes jpeg |
| `image/png` | Archivo de imagen con la extensión png |
| `image/webp` | Archivo de imagen con la extensión webp |
| `audio/mp3` | Sonido con la extensión .mp3 |
| `audio/ogg` | Sonido con la extensión .ogg |
| `audio/wav` | Audio con la extensión .wav |
| `audio/flac` | Sonido con la extensión .flac |
| `video/mp4` | Archivo de vídeo con la extensión .mp4 |
| `video/webm` | Vídeos con extensión .webm |
| `video/x-matroska` | Vídeos con extensión .mkv |
| `application/json` | Ficheros .json |
| `application/epub+zip` | Archivos con extensión .epub |
| `application/pdf` | Se refiere a ficheros con extensión .pdf |

Otra forma alternativa de averiguar como referirse a un determinado tipo de archivo es mediante un fichero que de muestra. Imaginemos que tenemos el fichero pdf `Factura 202209_Joan Carles.pdf`. Si a partir de este fichero queremos saber como referirnos a un archivo con extensión .pdf tendremos que ejecutar el siguiente comando:

> ```shell
> ❯ mimetype '/home/joan/Descargas/Factura 202209_Joan Carles.pdf'
> /home/joan/Descargas/Factura 202209_Joan Carles.pdf: application/pdf
> ```

Si miran la salida verán es que que el resultado es `application/pdf`

**Nota:** Un comando alternativo al último que acabamos de ver seria `xdg-mime query filetype 'Factura 202209_Joan Carles.pdf'`

## VER LA APLICACIÓN PREDETERMINADA PARA ABRIR UN DETERMINADO TIPOS DE FICHERO

Ahora que conocemos como referirse a un tipo de fichero podemos averiguar fácilmente la aplicación que tenemos predeterminada para abrir un determinado fichero. En el apartado anterior hemos visto que para referirse a un archivo .pdf tenemos que usar:

```shell
application/pdf
```

Por lo tanto para averiguar la aplicación asociada a la extensión .pdf ejecutaremos el siguiente comando en la terminal:

> ```
> ❯ xdg-mime query default application/pdf
> atril.desktop
> ```

Y veremos que en mi caso la totalidad de archivos .pdf se abrirán con la aplicación Atril.

## ESTABLECER APLICACIONES PREDETERMINADAS DESDE LA TERMINAL EN LINUX

A continuación, mostraré cómo establecer programas predeterminados en GNU/Linux a través de una serie de ejemplos prácticos.

### Definir el navegador predeterminado desde la terminal

En estos momentos tengo Firefox como navegador predeterminado:

> ```shell
> ❯ xdg-settings get default-web-browser
> firefox.desktop
> ```

Si quiero reemplazar Brave por Firefox tan solo tendré que ejecutar en comando del siguiente tipo:

> ```shell
> xdg-settings set default-web-browser <fichero.desktop>
> ```

Según lo visto en apartados anteriores el fichero.desktop de Brave es `brave-browser.desktop`. Por lo tanto el comando a ejecutar para establecer Brave como el navegador predeterminado es:

> ```shell
> xdg-settings set default-web-browser brave-browser.desktop
> ```

Si ahora vuelvo a ejecutar el comando para ver el navegador por defecto vemos que es Brave.

> ```shell
> ❯ xdg-settings get default-web-browser
> brave-browser.desktop
> ```

Otra forma alternativa de realizar lo que hemos visto en este apartado seria ejecutar los siguientes comando en la terminal:

> ```shell
> xdg-mime default brave-browser.desktop x-scheme-handler/http
> xdg-mime default brave-browser.desktop x-scheme-handler/https
> ```

### Definir el lector de PDF predeterminado desde la terminal

En apartados anteriores vimos que para referirse a un fichero .pdf tenemos que usar:

> ```shell
> application/pdf
> ```

Si queremos abrir los pdf con el software Atril tendremos que mirar el nombre de su fichero.desktop. Si seguimos las instrucciones de apartados anteriores veremos que el nombre es:

> ```shell
> atril.desktop
> ```

Por lo tanto, para que todos los ficheros .pdf se abran con Atril ejecutaré el siguiente comando en la terminal:

> ```shell
> ❯ xdg-mime default atril.desktop application/pdf
> ```

Si ahora ejecutamos el siguiente comando comprobaremos que el lector predeterminado de ficheros pdf es Atril.

> ```shell
> ❯ xdg-mime query default application/pdf
> atril.desktop
> ```

### Configurar el gestor de ficheros predeterminado desde la terminal

A forma de curiosidad miraremos el gestor de ficheros predeterminado de mi sistema operativo ejecutando el siguiente comando:

> ```shell
> ❯ xdg-mime query default inode/directory
> org.xfce.Catfish.desktop
> ```

Según la salida obtenida, el gestor de ficheros es Catfish. Obviamente esto es incorrecto y hay que corregirlo. En mi caso quiero que el gestor de ficheros predeterminado de mi sistema operativo sea Thunar. En apartados anteriores vimos que el fichero.desktop de Thunar es `thunar.desktop`. Por lo tanto el comando que usaré para que Thunar sea mi gestor de ficheros predeterminados es el siguiente:

> ```shell
> ❯ xdg-mime default thunar.desktop inode/directory
> ```

Si ahora volvemos a ejecutar el mismo comando que ejecutamos al inicio de este apartado veremos que el gestor de ficheros predeterminado es Thunar.

> ```shell
> ❯ xdg-mime query default inode/directory
> thunar.desktop
> ```

### Definir el cliente de email predeterminado desde la terminal

En mi caso quiero que el cliente de email predeterminado sea Thunderbird. Lo primero que tenemos que realizar es averiguar el fichero.desktop de Thunderbird que en mi caso es `thunderbird.desktop`. Una vez conocemos esto ejecutaremos el siguiente comando en la terminal:

> ```shell
> ❯ xdg-settings set default-url-scheme-handler mailto thunderbird.desktop
> ```

Para comprobar que el cliente predeterminado sea Thunderbird lo haremos del siguiente modo:

> ```shell
> ❯ xdg-mime query default x-scheme-handler/mailto
> thunderbird.desktop
> ```

### Configurar el cliente predeterminado para reproducir vídeo desde la terminal

Para configurar que las extensiones más populares de archivos de vídeo se reproduzcan mediante MPV procederemos del siguiente modo:

- El fichero.desktop de MPV es `mpv.desktop`
- El modo de referirnos a las extensiones de vídeo más populares es `video/x-matroska video/x-flv video/ogg video/webm video/mpeg video/mp4 video/flv video/avi`

Por lo tanto el comando que tendremos que ejecutar para conseguir nuestro propósito es:

> ```shell
> ❯ xdg-mime default mpv.desktop video/x-matroska video/x-flv video/ogg video/webm video/mpeg video/mp4 video/flv video/avi
> ```

El comando que acabamos de ejecutar asocia una serie de formatos de archivo al reproductor MPV. En vuestro caso podéis añadir más extensiones de archivo.

### Configurar el visor de imágenes predeterminado desde la terminal

Al igual que hicimos anteriormente tenemos que conocer el nombre del fichero.desktop del programa que queremos usar como predeterminado y saber como definir los diferentes formatos de fichero. En mi caso:

- El nombre del fichero.desktop es **viewnior.desktop**
- Los formatos de archivo que quiero que se abran el con visor viewnior son **image/jpeg, image/png y image/webp**

Por lo tanto el comando a ejecutar es el siguiente:

> ```shell
> ❯ xdg-mime default viewnior.desktop image/jpeg image/png image/webp
> ```

De este modo cualquier fichero de imagen que tenga la extensión jpeg, png o webp se abrirá con viewnior.

### Hacer que LibreOffice sea nuestra suite ofimática predeterminada desde la terminal

En el caso que tengáis varias aplicaciones ofimáticas es posible que os interese establecer Libreoffice para abrir los ficheros con extensión odt, ods, odp, odg y odf de forma predeterminada. Para ello tan solo tendrán que ejecutar los siguientes comandos en la terminal:

> ```shell
> xdg-mime default libreoffice-writer.desktop application/vnd.oasis.opendocument.text
> xdg-mime default libreoffice-writer.desktop application/vnd.oasis.opendocument.text-template
> xdg-mime default libreoffice-calc.desktop application/vnd.oasis.opendocument.spreadsheet
> xdg-mime default libreoffice-calc.desktop application/vnd.oasis.opendocument.spreadsheet-template
> xdg-mime default libreoffice-impress.desktop application/vnd.oasis.opendocument.presentation
> xdg-mime default libreoffice-impress.desktop application/vnd.oasis.opendocument.presentation-template
> xdg-mime default libreoffice-draw.desktop application/vnd.oasis.opendocument.graphics
> xdg-mime default libreoffice-draw.desktop application/vnd.oasis.opendocument.graphics-template
> xdg-mime default libreoffice-math.desktop application/vnd.oasis.opendocument.formula
> ```

A partir de esto momentos todos los ficheros que tengan una extensión de archivo OpenDocument se abrirán con LibreOffice.

### Establecer el emulador de terminal predeterminado

En el caso que tengamos varios emuladores de terminal disponibles es interesante establecer un emulador de terminal predeterminado. Para ello tendremos que ejecutar el siguiente comando:

> ```shell
> ❯ sudo update-alternatives --config x-terminal-emulator
> ```

Una vez ejecutado el comando saldrán todos los emuladores de terminal disponibles. En mi caso quiero que el emulador de terminal predeterminado sea Kitty, por lo tanto, tal y como se muestra a continuación escribiré `1` y presionaré la tecla Enter.

> ```shell
> ❯ sudo update-alternatives --config x-terminal-emulator
> [sudo] contraseña para joan: 
> Existen 9 opciones para la alternativa x-terminal-emulator (que provee /usr/bin/x-terminal-emulator).
> 
>   Selección   Ruta                             Prioridad  Estado
> ------------------------------------------------------------
> * 0            /usr/bin/xfce4-terminal.wrapper   40        modo automático
>   1            /usr/bin/kitty                    20        modo manual
>   2            /usr/bin/koi8rxterm               20        modo manual
>   3            /usr/bin/lxterm                   30        modo manual
>   4            /usr/bin/mlterm                   20        modo manual
>   5            /usr/bin/txiterm                  20        modo manual
>   6            /usr/bin/uxterm                   20        modo manual
>   7            /usr/bin/xfce4-terminal.wrapper   40        modo manual
>   8            /usr/bin/xiterm+thai              20        modo manual
>   9            /usr/bin/xterm                    20        modo manual
> 
> Pulse <Intro> para mantener el valor por omisión [*] o pulse un número de selección: 1
> ```

A partir de estos momento el emulador de terminal predeterminado es Kitty.

### Configurar el editor de texto predeterminado desde la terminal

Del mismo modo que configuramos el emulador de terminal podemos configurar el editor de texto predeterminado. Para ello ejecutaremos el siguiente comando en al terminal:

> ```shell
> ❯ sudo update-alternatives --config editor
> ```

Una vez ejecutado el comando saldrán todos los editores de texto disponibles. En mi caso quiero que el editor texto predeterminado sea Vim, por lo tanto, tal y como se muestra a continuación escribiré 0 y presionaré la tecla Enter.

> ```shell
> ❯ sudo update-alternatives --config editor
> Existen 5 opciones para la alternativa editor (que provee /usr/bin/editor).
> 
>   Selección   Ruta                Prioridad  Estado
> ------------------------------------------------------------
> * 0            /usr/bin/vim.gtk3    50        modo automático
>   1            /bin/nano            40        modo manual
>   2            /usr/bin/vim.basic   30        modo manual
>   3            /usr/bin/vim.gtk3    50        modo manual
>   4            /usr/bin/vim.nox     40        modo manual
>   5            /usr/bin/vim.tiny    15        modo manual
> 
> Pulse <Intro> para mantener el valor por omisión [*] o pulse un número de selección: 0
> ```

A partir de este momento el editor de texto predeterminado es Vim.

## MÉTODO ALTERNATIVO PARA CONFIGURAR LAS APLICACIONES PREDETERMINADAS DESDE LA TERMINAL

Un método alternativo a los vistos en este artículo es editar directamente el fichero `~/.config/mimeapps.list`. Si abren y editan este ficheros también podrá configurar gran parte de las aplicaciones determinadas de sus sistema operativo

## CONCLUSIONES

A lo largo de este artículo, hemos visto cómo podemos configurar las aplicaciones predeterminadas de nuestro sistema operativo desde la terminal mediante tres útiles comandos:

1. **xdg-settings:** Este comando nos permite establecer las aplicaciones predeterminadas para actividades como abrir enlaces, correos electrónicos, y más, de una manera sencilla y eficiente.
2. **xdg-mime:** Con esta herramienta, podemos gestionar las asociaciones de tipos MIME, permitiéndonos asignar qué aplicaciones deben abrir diferentes tipos de archivos de forma predeterminada.
3. **update-alternatives:** A través de este comando, podemos seleccionar y cambiar entre distintas versiones de un programa o comando, definiendo cuál será el que se utilizará por defecto en el sistema.

Configurar las aplicaciones predeterminadas desde la terminal puede ser una opción poderosa y versátil para usuarios avanzados y administradores de sistemas, brindando un mayor control sobre la experiencia del usuario y la eficiencia del sistema en general. ¡Esperamos que estas herramientas te sean de utilidad en tu día a día con GNU/Linux!

### Fuentes

[https://wiki.archlinux.org/title/XDG\_MIME\_Applications#mimeapps.list](https://wiki.archlinux.org/title/XDG_MIME_Applications#mimeapps.list)
