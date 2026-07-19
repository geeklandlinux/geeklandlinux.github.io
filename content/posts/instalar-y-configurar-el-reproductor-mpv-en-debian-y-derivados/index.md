---
title: "Instalar y configurar el reproductor MPV en Debian y derivados"
date: 2021-04-03
categories: 
  - "linux-2"
tags: 
  - "mpv"
  - "multimedia"
  - "software-libre"
coverImage: "Instalar-y-configurar-el-reproductor-MPV-en-Debian-y-derivados.png"
---

En numerosas ocasiones he escuchado gente hablando maravillas del reproductor de vídeo mpv. No obstante en mi caso lo instalaba en Debian y el rendimiento era pésimo. El consumo de CPU al reproducir los vídeos era extremadamente elevado y el motivo era porque la aceleración gráfica por hardware está deshabilitada de forma predeterminada. Por lo tanto si pretenden instalar, configurar y usar de forma satisfactoria mpv en Debian les recomiendo que sigan las siguientes instrucciones.<!--more-->

## INSTALAR Y CONFIGURAR EL REPRODUCTOR DE VÍDEO MPV EN DEBIAN

El procedimiento para instalar mpv en Debian es sumamente sencillo. Tan solo tienen que abrir una terminal y ejecutar el siguiente comando:

> **`joan@gk55:~$ sudo apt install mpv`**

La versión empaquetada por Debian permite usar los plugin/scripts escritos en lua, pero no los que están en Javascript. Si pretenden usar algún script elaborado en Javascript deberán descargar e instalar uno de los paquetes binarios de la siguiente URL:

[https://non-gnu.uvt.nl/debian/bullseye/mpv/](https://non-gnu.uvt.nl/debian/bullseye/mpv/ "Ubicación para descargar paquetes binarios de MPV")

En mi caso para descargar e instalar la última versión del reproductor de vídeo mpv he usado los siguientes comandos:

> ```shell
> joan@gk55:~$ wget https://non-gnu.uvt.nl/debian/bullseye/mpv/mpv-git_2021.02.16+c766e47+wsl.1_amd64.deb
> joan@gk55:~$ sudo gdebi mpv-git_2021.02.16+c766e47+wsl.1_amd64.deb
> joan@gk55:~$ rm mpv-git_2021.02.16+c766e47+wsl.1_amd64.deb
> ```

Aplicando cualquiera de las 2 soluciones servirá para instalar mpv en nuestro ordenador. Pero una vez finalizada la instalación tendremos que configurar el programa para que funcione de forma adecuada.

## CONFIGURACIÓN Y USO DEL REPRODUCTOR DE VÍDEO MPV

Para empezar a usar mpv de forma satisfactoria deberán tener en cuenta los siguientes puntos.

### Activar la aceleración gráfica por hardware en Xorg de forma predeterminada

La aceleración gráfica por hardware viene deshabilitada por defecto en Debian. Para habilitarla debemos crear un archivo de configuración ejecutando el siguiente comando en la terminal:

> ```shell
> joan@gk55:~$ nano ~/.config/mpv/mpv.conf
> ```

**Nota**: En caso que no exista la ruta o el fichero de configuración tan solo tenéis que crearlos.

Una vez se abra el editor de textos nano añadiremos el siguiente código en el fichero y guardaremos los cambios:

> ```shell
> hwdec
> profile=gpu-hq
> ```

A partir de estos momentos ya podemos abrir cualquier vídeo y mpv usará la aceleración gráfica por hardware. Por lo tanto el consumo de CPU será bajo y la reproducción de vídeo y/o audio será mucho más fluida.

Si por algún motivo no quieren activar la aceleración gráfica por hardware de forma predeterminada pueden ejecutar vídeos con aceleración gráfica de forma puntual ejecutando un comando del siguiente tipo en la terminal:

> ```shell
> mpv --hwdec video_o_URL_a_reproducir
> ```

Por lo tanto para reproducir un vídeo de Youtube ejecutaría el siguiente comando en la terminal:

> ```shell
> mpv --hwdec https://www.youtube.com/watch?v=IAW7-v-QkJA
> ```

Si están usando la aceleración gráfica por hardware verán un mensaje del siguiente tipo:

> ```shell
> joan@gk55:~$ mpv --hwdec https://www.youtube.com/watch?v=IAW7-v-QkJA
>  (+) Video --vid=1 (*) (vp9 1280x720 60.000fps)
>  (+) Audio --aid=1 --alang=eng (*) (opus 2ch 48000Hz)
> AO: [pulse] 48000Hz stereo 2ch float
> Using hardware decoding (vaapi).
> VO: [gpu] 1280x720 vaapi[nv12]
> ```

### Opciones adicionales que pueden añadir en el archivo de configuración

Podemos añadir más opciones en el fichero de configuración que acabamos de crear. Por ejemplo en mi caso el fichero de configuración tiene el siguiente aspecto:

> ```shell
> [default]
> hwdec
> profile=gpu-hq
> save-position-on-quit
> sub-font="Roboto"
> sub-font-size=55
> osd-font="Roboto"
> osd-font-size=22
> sid="1"
> fullscreen=yes
> ```

El significado de todos y cada uno de los parámetros que uso es el siguiente:

| Parámetro de configuración | Función |
| --- | --- |
| `[default]` | Default es el nombre que uso para crear mi perfil predeterminado. |
| `hwdec` | Habilita la aceleración por hardware usando la configuración predeterminada. En el caso que no funcione consulten la documentación. |
| `profile=gpu-hq` | Renderiza el vídeo con la configuración predeterminada de alta calidad. Esta opción puede generar problemas de rendimiento. Si los tenéis borradla. |
| `save-position-on-quit` | Para que se recuerde la posición en que hemos dejado de ver cada uno de los vídeos. |
| `sub-font="Roboto"` | Especificamos que queremos visualizar los subtítulos con la fuente Roboto. |
| `sub-font-size=50` | Para especificar el tamaño de los subtítulos. |
| `osd-font="Roboto"` | Establecemos que la fuente que usará mpv es la roboto |
| `osd-font-size=22` | El tamaño de la fuente que mostrará la aplicación mpv será 22. |
| `sid="1"` | En el caso que hayan subtítulos muestra en pantalla la pista 1 de forma predeterminada. |
| `fullscreen=yes` | Iniciar mpv en modo pantalla completa. Si quieren pueden cambiar el valor a no. |

**Nota**: Para opciones adicionales en el fichero de configuración pueden consular el siguiente [enlace](https://mpv.io/manual/master/ "Opciones de configuración adicionales para MPV"). El fichero de configuración también permite crear perfiles de configuración. La posibilidades de personalización de mpv son enormes.

### Atajos de teclado para usar el reproductor de Video MPV

MPV prácticamente no tiene interfaz gráfica. Para gestionar la reproducción de contenido multimedia deberán usar atajos de teclado. Los atajos de teclado más comunes y que vale la pena conocer son los siguientes:

| Teclas | Función |
| --- | --- |
| `Cursor derecha - izquierda` | Adelanta o retrasa la reproducción 5 segundos. |
| `Shift + Cursor derecha - izquierda` | Adelantar o retrasar la reproducción 1 segundo. |
| `Cursor arriba - abajo` | Adelanta o retrasa la reproducción 1 minuto. |
| `Shift + Cursor arriba - abajo` | Adelantar o retrasar la reproducción 5 segundos. |
| `RePág` | Saltar al capítulo siguiente de la reproducción de vídeo. |
| `AvPág` | Saltar al capítulo anterior de la reproducción de vídeo. |
| `[` y `]` | Incrementar - disminuir la velocidad de reproducción un 10%. |
| `{` y `}` | Disminuir a la mitad o doblar la velocidad de reproducción. |
| `Retroceso` | Para obtener la velocidad de reproducción predeterminada. |
| `Espacio` | Pausar o reiniciar al reproducción. |
| `f` | Entrar - salir de la reproducción en pantalla completa. |
| `q` | Salir del reproductor. |
| `Q` | Salir del reproductor recordando la posición de reproducción. |
| `0` y `9` | Incrementar y disminuir el volumen. |
| `1` - `2` | Subir y bajar el contraste de la imagen. |
| `3` - `4` | Modificar brillo de la imagen. |
| `5` - `6` | Ajustar gamma de la imagen. |
| `7` - `8` | Ajustar la situación de la imagen. |
| `m` | Mutear - desmutear el reproductor de vídeo-audio. |
| `.` | Se pausa el vídeo y cada vez que presionemos `.` se avanzará un frame la reproducción del vídeo. |
| `,` | Se pausa el vídeo y cada vez que presionemos `,` se retrocederá un frame la reproducción del vídeo. |
| `s` | Hacer una captura de pantalla. |
| `S` | Hacer una captura de pantalla capturando los subtítulos. |
| `Ctrl` + `s` | Captura de pantalla capturando todo lo visible en pantalla. |
| `v` | Activar / desactivar los subtítulos. |
| `j` y `J` | Ir cambiando entre los distintos subtítulos disponibles. |
| `z` y `Z` | retrasar o avanzar la aparición de los subtítulos en +/- 0.1 segundos. |
| `r` y `R` | Mover los subtítulos más arriba o abajo de la pantalla. |
| `#` | Para ir cambiando entre las distintas pistas de audio disponible. |
| `F9` | Muestra las pistas de audio, vídeo y subitutilos disponibles en pantalla. |
| `F8` | Muestra la lista de reproducción. |
| `alt` + `cursores` | Desplazar el cuadro de reproducción. |
| `alt` `+` / `-` | Aplicar Zoom al vídeo. |
| `w` y `e` | Hacer zoom hacia delante y detrás para suprimir las franjas negras superiores e inferiores del vídeo. |
| `ctrl`+`h` | Para activar y desactivar la aceleración por hardware. |

**Nota:** En caso que no seáis amantes de los atajos de teclado existen [numerosas interfaces gráficas](https://github.com/mpv-player/mpv/wiki/Applications-using-mpv) que hacen uso de mpv.

### Modificar-Añadir atajos de teclado en el reproductor de vídeo mpv con input.conf

Acabamos de ver los atajos de teclado predeterminados. No obstante mpv permite configurar nuestros propios atajos de teclado mediante el fichero de configuración `input.conf`. Para crear el fichero ejecutaremos el siguiente comando:

> ```shell
> nano ~/.config/mpv/input.conf
> ```

Una vez se abra el editor de texto nano copian el siguiente código:

> ```shell
> # mpv keybindings
> #
> # Location of user-defined bindings: ~/.config/mpv/input.conf
> #
> # Lines starting with # are comments. Use SHARP to assign the # key.
> # Copy this file and uncomment and edit the bindings you want to change.
> #
> # List of commands and further details: DOCS/man/input.rst
> # List of special keys: --input-keylist
> # Keybindings testing mode: mpv --input-test --force-window --idle
> #
> # Use 'ignore' to unbind a key fully (e.g. 'ctrl+a ignore').
> #
> # Strings need to be quoted and escaped:
> #   KEY show-text "This is a single backslash: \\ and a quote: \" !"
> #
> # You can use modifier-key combinations like Shift+Left or Ctrl+Alt+x with
> # the modifiers Shift, Ctrl, Alt and Meta (may not work on the terminal).
> #
> # The default keybindings are hardcoded into the mpv binary.
> # You can disable them completely with: --no-input-default-bindings
> 
> # Developer note:
> # On compilation, this file is baked into the mpv binary, and all lines are
> # uncommented (unless '#' is followed by a space) - thus this file defines the
> # default key bindings.
> 
> # If this is enabled, treat all the following bindings as default.
> #default-bindings start
> 
> #MBTN_LEFT     ignore              # don't do anything
> #MBTN_LEFT_DBL cycle fullscreen    # toggle fullscreen on/off
> #MBTN_RIGHT    cycle pause         # toggle pause on/off
> #MBTN_BACK     playlist-prev
> #MBTN_FORWARD  playlist-next
> 
> # Mouse wheels, touchpad or other input devices that have axes
> # if the input devices supports precise scrolling it will also scale the
> # numeric value accordingly
> #WHEEL_UP      seek 10
> #WHEEL_DOWN    seek -10
> #WHEEL_LEFT    add volume -2
> #WHEEL_RIGHT   add volume 2
> 
> ## Seek units are in seconds, but note that these are limited by keyframes
> #RIGHT seek  5
> #LEFT  seek -5
> #UP    seek  60
> #DOWN  seek -60
> # Do smaller, always exact (non-keyframe-limited), seeks with shift.
> # Don't show them on the OSD (no-osd).
> #Shift+RIGHT no-osd seek  1 exact
> #Shift+LEFT  no-osd seek -1 exact
> #Shift+UP    no-osd seek  5 exact
> #Shift+DOWN  no-osd seek -5 exact
> # Skip to previous/next subtitle (subject to some restrictions; see manpage)
> #Ctrl+LEFT   no-osd sub-seek -1
> #Ctrl+RIGHT  no-osd sub-seek  1
> # Adjust timing to previous/next subtitle
> #Ctrl+Shift+LEFT sub-step -1
> #Ctrl+Shift+RIGHT sub-step 1
> # Move video rectangle
> #Alt+left  add video-pan-x  0.1
> #Alt+right add video-pan-x -0.1
> #Alt+up    add video-pan-y  0.1
> #Alt+down  add video-pan-y -0.1
> # Zoom/unzoom video
> #Alt++     add video-zoom   0.1
> #Alt+- add video-zoom  -0.1
> # Reset video zoom/pan settings
> #Alt+BS set video-zoom 0 ; set video-pan-x 0 ; set video-pan-y 0
> #PGUP add chapter 1                     # skip to next chapter
> #PGDWN add chapter -1                   # skip to previous chapter
> #Shift+PGUP seek 600
> #Shift+PGDWN seek -600
> #[ multiply speed 1/1.1                 # scale playback speed
> #] multiply speed 1.1
> #{ multiply speed 0.5
> #} multiply speed 2.0
> #BS set speed 1.0                       # reset speed to normal
> #Shift+BS revert-seek                   # undo previous (or marked) seek
> #Shift+Ctrl+BS revert-seek mark         # mark position for revert-seek
> #q quit
> #Q quit-watch-later
> #q {encode} quit 4
> #ESC set fullscreen no
> #ESC {encode} quit 4
> #p cycle pause                          # toggle pause/playback mode
> #. frame-step                           # advance one frame and pause
> #, frame-back-step                      # go back by one frame and pause
> #SPACE cycle pause
> #> playlist-next                        # skip to next file
> #ENTER playlist-next                    # skip to next file
> #< playlist-prev                        # skip to previous file
> #O no-osd cycle-values osd-level 3 1    # cycle through OSD mode
> #o show-progress
> #P show-progress
> #i script-binding stats/display-stats
> #I script-binding stats/display-stats-toggle
> #` script-binding console/enable
> #z add sub-delay -0.1                   # subtract 100 ms delay from subs
> #Z add sub-delay +0.1                   # add
> #x add sub-delay +0.1                   # same as previous binding (discouraged)
> #ctrl++ add audio-delay 0.100           # this changes audio/video sync
> #ctrl+- add audio-delay -0.100
> #Shift+g add sub-scale +0.1                  # increase subtitle font size
> #Shift+f add sub-scale -0.1                  # decrease subtitle font size
> #9 add volume -2
> #/ add volume -2
> #0 add volume 2
> #* add volume 2
> #m cycle mute
> #1 add contrast -1
> #2 add contrast 1
> #3 add brightness -1
> #4 add brightness 1
> #5 add gamma -1
> #6 add gamma 1
> #7 add saturation -1
> #8 add saturation 1
> #Alt+0 set window-scale 0.5
> #Alt+1 set window-scale 1.0
> #Alt+2 set window-scale 2.0
> # toggle deinterlacer (automatically inserts or removes required filter)
> #d cycle deinterlace
> #r add sub-pos -1                       # move subtitles up
> #R add sub-pos +1                       #                down
> #t add sub-pos +1                       # same as previous binding (discouraged)
> #v cycle sub-visibility
> # stretch SSA/ASS subtitles with anamorphic videos to match historical
> #V cycle sub-ass-vsfilter-aspect-compat
> # switch between applying no style overrides to SSA/ASS subtitles, and
> # overriding them almost completely with the normal subtitle style
> #u cycle-values sub-ass-override "force" "no"
> #j cycle sub                            # cycle through subtitles
> #J cycle sub down                       # ...backwards
> #SHARP cycle audio                      # switch audio streams
> #_ cycle video
> #T cycle ontop                          # toggle video window ontop of other windows
> #f cycle fullscreen                     # toggle fullscreen
> #s screenshot                           # take a screenshot
> #S screenshot video                     # ...without subtitles
> #Ctrl+s screenshot window               # ...with subtitles and OSD, and scaled
> #Alt+s screenshot each-frame            # automatically screenshot every frame
> #w add panscan -0.1                     # zoom out with -panscan 0 -fs
> #W add panscan +0.1                     #      in
> #e add panscan +0.1                     # same as previous binding (discouraged)
> # cycle video aspect ratios; "-1" is the container aspect
> #A cycle-values video-aspect-override "16:9" "4:3" "2.35:1" "-1"
> #POWER quit
> #PLAY cycle pause
> #PAUSE cycle pause
> #PLAYPAUSE cycle pause
> #PLAYONLY set pause no
> #PAUSEONLY set pause yes
> #STOP quit
> #FORWARD seek 60
> #REWIND seek -60
> #NEXT playlist-next
> #PREV playlist-prev
> #VOLUME_UP add volume 2
> #VOLUME_DOWN add volume -2
> #MUTE cycle mute
> #CLOSE_WIN quit
> #CLOSE_WIN {encode} quit 4
> #ctrl+w quit
> #E cycle edition                        # next edition
> #l ab-loop                              # Set/clear A-B loop points
> #L cycle-values loop-file "inf" "no"    # toggle infinite looping
> #ctrl+c quit 4
> #DEL script-binding osc/visibility      # cycle OSC display
> #ctrl+h cycle-values hwdec "auto" "no"  # cycle hardware decoding
> #F8 show_text ${playlist}               # show playlist
> #F9 show_text ${track-list}             # show list of audio/sub streams
> 
> #
> # Legacy bindings (may or may not be removed in the future)
> #
> #! add chapter -1                       # skip to previous chapter
> #@ add chapter 1                        #         next
> 
> #
> # Not assigned by default
> # (not an exhaustive list of unbound commands)
> #
> 
> # ? cycle angle                         # switch DVD/Bluray angle
> # ? cycle sub-forced-only               # toggle DVD forced subs
> # ? cycle program                       # cycle transport stream programs
> # ? stop                                # stop playback (quit or enter idle mode)
> ```

Una vez copiado el código guardan los cambios y cierran el fichero. A partir de ahora si quieren añadir/modificar un atajo de teclado deberán descomentar y modificar ciertas partes del fichero `input.conf`.

En mi caso los atajos de teclado estándar `Repág` y `AvPág` no me parecen lógicos. En mi caso pienso que deberían estar invertidos. Por lo tanto localizaremos la siguientes líneas en el fichero `input.conf`:

> ```shell
> #PGUP add chapter 1                     # skip to next chapter
> #PGDWN add chapter -1                   # skip to previous chapter
> ```

Una vez localizadas las descomentamos y las modificamos para que queden del siguiente modo:

> ```shell
> PGDWN add chapter 1                   # skip to previous chapter
> PGUP add chapter -1                   # skip to next chapter
> ```

Si guardamos los cambios los atajos de teclado predeterminados se invertirán. A partir de esto momento cuando presione la tecla `AvPág` la reproducción saltará automáticamente al siguiente capítulo. De esta forma podremos modificar la totalidad de atajos de teclado existentes.

### Añadir funcionalidades al reproductor de vídeo MPV

Mediante scripts podemos añadir funcionalidades adicionales al reproductor de audio y vídeo MPV. En la siguiente ubicación encontrarán un amplio surtido de scripts en lua y javascript para poder usarlos en el reproductor de vídeo mpv.

[https://github.com/mpv-player/mpv/wiki/User-Scripts](https://github.com/mpv-player/mpv/wiki/User-Scripts "URL para descargar scripts")

Para instalar cualquiera de los scripts tan solo deben seguir las instrucciones que encontrarán en el enlace que les he dejado. En mi caso detallaré como instalar los scripts Blackbox, Colorbox y autoload.lua. La función de cada uno de los scripts es la siguiente:

| Script | Función |
| --- | --- |
| **Blackbox** | Muestra la lista de reproducción. También permite tener un gestor de archivos integrado en mpv. |
| **Colorbox** | Aplica perfiles de corrección de color a nuestros vídeos e imágines. |
| **autoload** | Al añadir/reproducir un fichero de un directorio se carga todo el contenido multimedia del directorio en la lista de reproducción. |

Para instalar los scripts tan solo tenemos que descargarlos en el directorio `~/.config/mpv/scripts`. Por lo tanto para instalar los scripts Blackbox y Colorbox procederemos del siguiente modo:

Inicialmente accedemos al directorio que almacena la configuración de mpv ejecutando el siguiente comando en la terminal:

> ```shell
> joan@gk55:~$ cd ~/.config/mpv
> ```

A continuación clonaremos el repositorio mpv-tools ejecutando el siguiente comando en la terminal:

> ```shell
> joan@gk55:~/.config/mpv$ git clone https://github.com/VideoPlayerCode/mpv-tools.git
> ```

Seguidamente copiaremos el directorio `scripts` y `script-settings` en la raíz del fichero de configuración ejecutando los siguientes comandos:

> ```shell
> joan@gk55:~/.config/mpv$ cp -r mpv-tools/scripts ~/.config/mpv
> joan@gk55:~/.config/mpv$ cp -r mpv-tools/script-settings/ ~/.config/mpv
> ```

El siguiente paso consistirá en borrar el directorio que contiene los ficheros del repositorio clonado. Para ello usaremos el siguiente comando.

> ```shell
> joan@gk55:~/.config/mpv$ sudo rm -r mpv-tools
> ```

Ahora tendremos que generar los ficheros de configuración para los plugin Colorbox y Blackbox ejecutando los siguientes comandos:

> ```shell
> joan@gk55:~/.config/mpv$ cd script-settings/
> joan@gk55:~/.config/mpv/script-settings$ cp Blackbox.conf.example Blackbox.conf
> joan@gk55:~/.config/mpv/script-settings$ cp Colorbox.conf.example Colorbox.conf
> ```

Para finalizar la instalación de BlackBox y Colorbox definiremos los atajos de teclado para poder usarlos. Para ello ejecutaremos los siguientes comandos:

> ```shell
> joan@gk55:~/.config/mpv/script-settings$ cd ..
> joan@gk55:~/.config/mpv$ nano input.conf
> ```

Una vez se abra el editor de texto nano pegaremos el siguiente código al final del fichero. De este modo definiremos las teclas para usar BlackBox y Colorbox.

> ```shell
> #Atajos de teclado BlackBox y Colorbox 
> z script-binding Blackbox_Playlist
> Z script-binding Blackbox
> c script-binding Colorbox
> ```

Para instalar autoload.lua el procedimiento será mucho más sencillo. Accedemos dentro del directorio scripts mediante el siguiente comando:

> ```shell
> joan@gk55:~/.config/mpv$ cd scripts/
> ```

A continuación descargamos el script autoload.lua ejecutando el siguiente comando en la terminal:

> ```shell
> joan@gk55:~/.config/mpv/scripts$ wget https://raw.githubusercontent.com/mpv-player/mpv/master/TOOLS/lua/autoload.lua
> ```

## OPINIÓN SOBRE EL REPRODUCTOR DE VÍDEO MPV

Es un reproductor de vídeo excelente, minimalista, configurable y ligero. Comparándolo con el archiconocido VLC mis conclusiones son las siguientes:

1. El reproductor Mpv es más ligero y minimalista. Ambos software tienen infinidad de opciones de configuración.
2. Mpv permite la reproducción de vídeos online de infinidad de plataformas como por ejemplo Youtube y Twitch sin necesidad de realizar nada. De este modo mediante la extensión de Firefox [ff2mpv](https://addons.mozilla.org/es/firefox/addon/ff2mpv/ "URL para descargar la extensión ff2mpv") utilizo mpv para reproducir gran parte de los vídeos que encuentro en las webs que visito.
3. Mpv se maneja mediante atajos de teclado, mientras que VLC está pensado para usarse mediante su interfaz gráfica. Si son amantes del minimalismo y los atajos de teclado mpv es su reproductor.
4. El reproductor Mpv también puede ser usado como un visor de imágenes.

Como contrapartida considero que el reproductor VLC es superior en los siguientes aspectos:

1. VLC es más eficiente a la hora de reproducir vídeo porque su consumo de CPU al reproducir un vídeo es ligeramente menor. En VLC la aceleración gráfica por hardware es ligeramente mejor que en mpv.
2. VLC es capaz de reproducir el 100% de mis vídeos con aceleración gráfica por hardware. En mpv he encontrado algún vídeo que me ha dado problemas y el consumo de CPU se dispara.
3. Mpv requiere de un proceso de configuración. En cambio si usamos VLC es cuestión de instalar y usar.

Mi recomendación es que si les gusta el minimalismo, la terminal y los atajos de teclado usen Mpv. En cambio si no quieren problemas de ningún tipo ni quieren invertir tiempo en configuraciones entonces usen VLC.

#### Fuentes

[https://github.com/mpv-player/mpv/wiki](https://github.com/mpv-player/mpv/wiki) [https://wiki.debian.org/HardwareVideoAcceleration#mpv](https://wiki.debian.org/HardwareVideoAcceleration#mpv)
