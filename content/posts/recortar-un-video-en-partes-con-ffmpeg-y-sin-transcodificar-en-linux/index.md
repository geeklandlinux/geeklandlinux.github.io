---
title: "Recortar un vídeo en partes con ffmpeg y sin transcodificar en Linux"
date: 2021-06-05
categories: 
  - "linux-2"
tags: 
  - "bash"
  - "ffmpeg"
cover:
  image: "images/Recortar-un-video-en-partes-con-ffmpeg.jpg"
  relative: true
---

A continuación veremos como podemos recortar un vídeo en varias partes sin necesidad de realizar ningún tipo de transcodificación y preservando todas las pistas y calidad del vídeo original. Para ello tan solo usaremos una terminal y el software ffmpeg. Haciéndolo de esta forma obtendremos las siguientes ventajas:<!--more-->

1. El recorte del vídeo será extremadamente rápido porque no hay ningún tipo de transcodificación.
2. Los recortes preservarán la calidad original.
3. El vídeo recortado tendrá exactamente las mismas pistas de vídeo, audio y subtítulos que el original.
4. El consumo de recursos será mínimo. Cabe recordar que las tareas de transcodificación de audio y vídeo son exigentes para la CPU de nuestro ordenador.

La forma de realizar lo que acabo de plantear es la siguiente.

## RECORTAR UN VÍDEO EN VARIAS PARTES SIN TRANSCODIFICAR CON FFMPEG Y LINUX

Imaginemos que tenemos un vídeo con el nombre **geekland.mkv** que tiene una duración de 2 horas y queremos crear un nuevo archivo de vídeo que solo contenga la última hora del vídeo. Para ello abriremos una terminal y ejecutaremos el siguiente comando:

> ```shell
> ffmpeg -i "geekland.mkv" -map 0 -default_mode infer_no_subs -ss 01:00:00 -to 02:00:00 -c copy "geekland_cortado.mkv"
> ```

El significado de todos y cada uno de los parámetros del comando es:

- `ffmpeg`: Para llamar al software que nos ayudará a recortar el vídeo.
- `-i "geekland.mkv"` : Mediante la opción `-i` indicamos la ruta del fichero de vídeo que queremos procesar que en nuestro caso es `geekland.mkv`.
- `-map 0`: Para seleccionar todos los streams de vídeo, audio y subtítulos del vídeo `geekland.mkv`. Si el vídeo tiene 3 pistas de audio y 3 de subtítulos entonces el vídeo recortado tendrá 3 pistas de audio y 3 de subtítulos.
- `-default_mode infer_no_subs`: Configuramos que no hay ninguna pista de subtítulos activada de forma predeterminada en el vídeo recortado.
- `-ss 01:00:00`: Para indicar el punto donde quieres iniciar a recortar el vídeo. En nuestro caso empezaremos a recortar a partir de la primera hora. Existen diversos formatos para indicar el tiempo. En nuestro caso hemos usado el formato de horas:minutos:segundos `01:00:00`. Podríamos haber usado otros formatos de entrada como por ejemplo indicar una hora en milisegundos `-ss 3600000ms`.
- `-to 02:00:00`: Para indicar el punto donde quieres finalizar el recorte del vídeo. En nuestro caso finalizamos el recorte a la hora 2.
- `-c copy`: Para que se copien todas las pistas de audio, vídeo y subtítulos sin transcodificar absolutamente nada.
- `"geekland_cortado.mkv"`: Definimos el nombre que tendrá el archivo recortado.

Después de ejecutar el comando se habrá generado un nuevo fichero de vídeo con el nombre **geekland\_recortado.mkv**. Este fichero contendrá la última hora de vídeo del fichero **geekland.mkv**. Por lo tanto con un simple comando hemos conseguido nuestro objetivo.

## SCRIPT PARA AUTOMATIZAR EL PROCESO DE RECORTAR PARTES DE UN VÍDEO

Si tenemos que recortar diversas partes de un mismo vídeo les recomiendo que usen el script que encontrarán a continuación. Con él podrán:

- Cortar tantas partes como quieran de un mismo vídeo indicando la hora, minuto y segundo en que quieren iniciar y finalizar el recorte.
- Tal y como comentamos en el primer apartado no se realizará ningún tipo de transcodificación y no se perderá calidad de audio y de vídeo. El proceso será muy rápido.

Para asegurar que podrán ejecutar el script sin ningún tipo de problema aseguren que tienen instalados los paquetes ffmpeg y bc en su equipo ejecutando el siguiente comando en la terminal:

> ```shell
> sudo apt install ffmpeg bc
> ```

A continuación, para crear el script ejecutan el siguiente comando en la terminal:

> ```shell
> sudo nano cutvideo.sh
> ```

Una vez se abra el editor de textos nano pegan el siguiente código:

> ```shell
> #!/bin/bash
> #Para que funcione el script hay que tener los paquetes ffmpeg y bc
> #Para ejecutar el script hay que ejecutar el comando cutvideo.sh seguido de la ruta completa del fichero de vídeo 
> 
> #Para determinar la duración del vídeo
> duracion=$(ffprobe -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$1" 2>/dev/null)
> 
> #Variable que almacena la extensión del fichero de vídeo
> extension=$(echo "$1" | sed 's/.*\.//')
> 
> #Para traspasar la duración del vídeo original a horas minutos y segundos
> horas=$(bc <<< "scale=0; $duracion / 3600")
> minutos=$(bc <<< "scale=0; $duracion % 3600 / 60")
> segundos=$(bc <<< "scale=0; $duracion % 60" | awk 'gsub(/\./,",")||1' | awk '{print int($0)}') 
> 
> #Para imprimir la duración del vídeo en horas minutos y segundos
> echo -e "\nLa duración del video es:"
> printf '%02dh:%02dm:%02ds\n' $horas $minutos $segundos
> 
> #Variable para con valor s para hacer que se pueda repetir la ejecución del script. Cuando toma valor no no se repite
> repetirscript="s"
> 
> while true; do
> 
>   case $repetirscript in
> 
>     s)
>         while true; do
> 
>             echo -e "\nIntroduzca la hora minuto y segundo quieres empezar a recortar con el formato 00:14:15?"
> 
>             while true; do
>                 read inicio
> 
>                 case $inicio in
> 
>                     [0-9][0-9]:[0-5][0-9]:[0-5][0-9])
>                     IFS=': ' read -r -a array <<< "$inicio"
>                     horaseg=`echo "var=${array[0]};var*3600" | bc`
>                     minseg=`echo "var=${array[1]};var*60" | bc`
>                     seg=`echo "var=${array[2]};var*1" | bc`
>                     duracioninicorte=`echo "$horaseg+$minseg+$seg" | bc`
> 
>                     if (( $(echo "$duracioninicorte < $duracion" | bc) ))
>                     then
>                       echo "Empezará a cortarse el vídeo a partir del tiempo $inicio"
>                       break
>                     else
>                       echo -e "El tiempo introducido es incorrecto. Por favor introduzca un valor inferior a la duración del vídeo original. Introduzca de nuevo el tiempo."
>                     fi
>                     ;;
> 
>                     *) echo "El tiempo introducido es incorrecto. Por favor introduzca un valor con el siguiente formato 01:40:30"
>                     ;;
> 
>                 esac
> 
>             done
> 
>             break
> 
>         done
> 
>         while true; do
> 
>             echo -e "\nHora minuto y segundo en que quieres finalizar el recorte del vídeo?"
> 
>             while true; do
> 
>                 read final
> 
>                 case $final in
> 
>                     [0-9][0-9]:[0-5][0-9]:[0-5][0-9])
>                     IFS=': ' read -r -a array <<< "$final"
>                     horaseg=`echo "var=${array[0]};var*3600" | bc`
>                     minseg=`echo "var=${array[1]};var*60" | bc`
>                     seg=`echo "var=${array[2]};var*1" | bc`
>                     duracionfincorte=`echo "$horaseg+$minseg+$seg" | bc`
> 
>                     if (( $(echo "$duracionfincorte <= $duracion && $duracionfincorte > $duracioninicorte" | bc) ))
>                     then
>                       echo "Recortarás el vídeo hasta $final"
>                       break
>                     else
>                       echo -e "El tiempo introducido es incorrecto. Tiene que estar entre donde inicias el corte y la duración del vídeo. Introduzca de nuevo el tiempo."
>                     fi
>                     ;;
> 
>                     *) echo "El tiempo introducido es incorrecto. Por favor introduzca un valor con el siguiente formato 02:40:30"
>                     ;;
> 
>                 esac
> 
>             done
> 
>             break
> 
>         done
> 
>         echo -e "\nIntroduzca el nombre que quieres que tenga el fichero cortado (sin su extensión)?"
>         read nombre
> 
>         ffmpeg -i "$1" -map 0 -default_mode infer_no_subs -loglevel warning -ss $inicio -to $final -c copy "$nombre".$extension
> 
>         echo -e "\nVídeo cortado. ¿Quieres cortar otro fragmento del mismo vídeo?"
>         read repetirscript
>     ;;
> 
>     n) echo -e "\nLa tarea ha finalizado"
>        break
>     ;;
> 
>     *) echo -e "\nPor favor introduzca la letra s (Sí) o n (No)"
>        read repetirscript
>     ;;
> 
>   esac
> 
> done
> ```

Una vez pegado el código guardan los cambios y cierran el fichero. Ahora haremos ejecutable el script mediante el siguiente comando:

> **`chmod + cutvideo.sh`**

### Realizar un script para facilitar la tarea

A estas alturas ya podemos cortar un vídeo con el script que acabamos de crear. Para ejecutarlo tan solo tentemos que teclear `./` seguido del nombre del fichero que almacena el script y finalmente la ruta del fichero de vídeo que queremos recortar. Una vez ejecutado el comando tendremos que introducir los siguientes parámetros para definir el corte que queremos realizar.

> **`joan@gk55:~$ ./cutvideo.sh /media/DATOS/geekland.mkv   La duración del video es: 00h:48m:03s  Introduzca la hora minuto y segundo quieres empezar a recortar con el formato 00:14:15? 00:14:10 Empezará a cortarse el vídeo a partir del tiempo 00:14:10  Hora minuto y segundo en que quieres finalizar el recorte del vídeo? 00:15:32 Recortarás el vídeo hasta 00:15:32  Introduzca el nombre que quieres que tenga el fichero cortado (sin su extensión)? cut  Vídeo cortado. ¿Quieres cortar otro fragmento del mismo vídeo? n  La tarea ha finalizado`**

De esta forma podrán extraer fragmentos de un determinado vídeo de forma rápida y cómoda.
