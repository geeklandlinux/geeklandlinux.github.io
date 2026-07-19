---
title: "Ventajas e inconvenientes del formato ogg respecto al mp3"
date: 2017-01-26
categories: 
  - "varios"
tags: 
  - "audio"
  - "mp3"
  - "ogg"
  - "software-libre"
cover:
  image: "images/Ventajas-del-formato-ogg-respecto-el-mp3.jpg"
  relative: true
---

Entre los usuarios del software libre son muchas las ocasiones en que se habla del formato ogg. Además una pregunta bastante recurrente es:

¿Qué formato de audio es mejor? ¿mp3 o ogg?<!--more-->

Después de investigar un poco y realizar pruebas he llegado a la conclusión que el formato ogg es técnicamente superior al mp3.

Además los archivos .ogg son capaces de proporcionar una calidad de audio superior con un tamaño menor.

No obstante ambos formatos de archivo presentan ventajas e inconvenientes que detallaremos a continuación.

## VENTAJAS TÉCNICAS DEL FORMATO OGG RESPECTO AL MP3

Algunas de las ventajas del formato ogg respecto al mp3 son las siguientes:

### Permite almacenar más canales de audio

Los archivos de audio mp3 solo permiten almacenar 2 canales de audio. Como contrapartida los archivos ogg permiten usar hasta 255 canales.

Así por lo tanto el formato .mp3 no permite almacenar sonido envolvente del tipo Stereo 5.1 (6 canales), mientras que un archivo .ogg sí.

### El contenedor .ogg permite almacenar audio y vídeo

El formato de contenedor .ogg es capaz de almacenar tanto audio como vídeo. En cambio el contenedor .mp3 únicamente puede almacenar audio.

No obstante en 2007 la Xiph.org creo el contenedor .ogv y el códec Theora, ambos específicos para vídeo. Desde entonces los contenedores .ogg únicamente se recomiendan para almacenar audio.

### A igualdad de tamaño de archivo la calidad del formato ogg es superior

El formato de compresión Ogg Vorbis es mejor que el mp3 porque es capaz de obtener calidades de audio iguales o superiores a un mp3, con un peso de archivo menor.

Algunos de los motivos que justifican lo que acabo de comentar son los siguientes:

1. Ogg Vorbis es un formato de compresión de audio más actual que mp3.
2. Ogg vorbis es libre y abierto para que la comunidad pueda seguir mejorándolo ininterrumpidamente.

### La calidad de un archivo .ogg puede ser muy superior a la de un mp3

En el caso que nos importe mucho la calidad del sonido es mejor utilizar el formato ogg:

Después de realizar varias pruebas he llegado a las siguientes conclusiones:

1. Un audio comprimido a mp3 con lame a máxima calidad ofrece una calidad similar a un audio comprimido con oggenc a la calidad 7 u 8.
2. La máxima calidad del formato ogg es la 10. La calidad 10 proporciona una calidad de sonido excelente con una tasa de muestreo media de 520 kbps.
3. La calidad máxima de un archivo .mp3 corresponde a un archivo con una tasa de muestreo media aproximada de 260 kbps.

### Ogg Vorbis es abierto y libre de patentes

El contenedor .ogg y el códec vorbis son completamente abiertos, libres y sin patentes.

En cambio el formato de compresión .mp3 es propietario. Si alguien decide codificar archivos de audio en mp3 para por ejemplo incluirlos en un software, deberá pagar royalties a la Fraunhofer-Gesellschaft.

### El sistema de etiquetado es mejor en ogg

El sistema de etiquetado ogg Vorbis es mejor y más flexible que el sistema de etiquetado de los mp3.

Así por ejemplo el sistema de etiquetado de Vorbis nos permite lo siguiente:

1. Editar las etiquetas al mismo tiempo que comprimimos los archivos de audio.
2. Podemos duplicar etiquetas. De este modo en el caso que tengamos más de un artista podremos disponer de 2 etiquetas de artista.
3. Al igual que ID3v2 podemos crear etiquetas personalizadas. Por lo tanto permiten añadir prácticamente toda la información que queramos.
4. Usar un número prácticamente ilimitado de etiquetas.
5. Podemos usar un número prácticamente ilimitado de caracteres para describir cada una de las etiquetas.

## INCONVENIENTES DEL FORMATO DE AUDIO OGG

Técnicamente los archivos de audio .ogg son mejores que los .mp3, pero en la actualidad presentan los siguientes problemas:

### Existen reproductores de audio que no reproducen .ogg

Hay reproductores de audio que no pueden reproducir audio en el formato ogg. A modo de ejemplo podemos encontrar los siguientes casos:

1. Los [iPod](https://support.apple.com/en-us/HT1709 "Detalle de los formatos de audio soportados por el iPod") aún no son capaces de reproducir archivos en formato ogg.
2. Windows 10 tampoco es capaz de reproducir audio .ogg de forma predeterminada. Si queremos reproducir audio ogg deberemos instalar códecs adicionales o descargar algún software que sea capaz de reproducirlos.
3. En Mac OS la situación es similar a la de Windows. El reproductor iTunes no es capaz de reproducir el formato de archivo .ogg.
4. Hay muchos reproductores de vídeo que no son capaces de reproducir los formatos de archivo .ogg, .ogv, .ogm, etc.
5. El CD de audio de mi coche no es capaz de reproducir archivos .ogg. En cambio si puede reproducir archivos .mp3.

A pesar de esto, en muchos casos podemos encontrar una solución como por ejemplo instalar códecs o un software capaz de reproducir archivos .ogg.

En en siguiente enlace encontrarán información de [software y códecs para reproducir y codificar archivos .ogg](http://www.vorbis.com/setup/ "Software para reproducir y codificar audio en ogg") sin ningún tipo de problema.

### El consumo de batería es superior

Aunque no he realizado ninguna comprobación, los comentarios de varios usuarios en foros apuntan que el consumo de batería de nuestros reproductores de audio es mayor cuando reproducimos archivos .ogg.

Los procesadores de nuestros dispositivos a priori están más optimizados para decodificar archivos .mp3 debido a que los archivos .mp3 son un estándar en la actualidad.

## DEMOSTRACIÓN QUE LOS ARCHIVOS .OGG OFRECEN UNA CALIDAD SUPERIOR A LOS .MP3

A estas alturas todo el mundo debería tener claras las ventajas teóricas que proporciona el audio .ogg sobre el .mp3.

A quien le interese ver una demostración práctica de lo citado hasta el momento puede leer el artículo en que analizo de forma práctica [las diferencias entre los audios comprimidos en mp3 y en ogg vorbis]({{< relref "/posts/comparacion-mp3-vs-ogg" >}}).
