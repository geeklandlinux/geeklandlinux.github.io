---
title: "Consejos para comprimir un audio con pérdida de forma adecuada"
date: 2017-02-09
categories: 
  - "varios"
tags: 
  - "audio"
  - "mp3"
  - "ogg"
coverImage: "consejos-comprimir-audio-de-forma-adecuada.jpg"
---

En las pasadas semanas analizamos las [calidades que nos ofrecen los archivos mp3 y ogg]({{< relref "/posts/comparacion-mp3-vs-ogg" >}}). A continuación veremos una serie de consejos a seguir para conseguir buenos resultados al comprimir un audio.<!--more-->

## ¿EN QUÉ CONSISTE COMPRIMIR UN AUDIO DIGITAL CON PÉRDIDAS?

Comprimir un audio digital con pérdida consiste en reducir el tamaño que tiene un archivo de audio.

Para reducir el tamaño de los archivos se utilizan los códecs. Algunas de las operaciones que pueden realizan los códecs para reducir el tamaño de los archivos son las siguientes:

1. Reducir la tasa de bits del sonido original para que el audio ocupe menos espacio.
2. Quitar sonidos a frecuencias que no son perceptibles por el oído humano.
3. Eliminación de redundancias de la señal de audio.
4. Pueden reducir el número de canales existentes transformando un sonido de envolvente a Stereo.
5. Reducir el número de bits por muestra.
6. Etc.

###### Nota: El proceso de compresión de un audio es extremadamente complejo. Además cada códec aplica metodologías diferentes para comprimir el tamaño de un audio.

Obviamente durante el proceso de compresión habrá una pérdida en la calidad del audio. Cuanto más alta sea la reducción de peso del archivo comprimido, más alta será la pérdida de calidad.

## UTILIDADES QUE TIENE COMPRIMIR UN AUDIO

Obviamente comprimir un audio tiene ciertas utilidades. Algunas de ellas son las que detallo a continuación:

1. El espacio necesario para almacenar las canciones en nuestro disco duro será mucho menor. Aunque los discos duros sean baratos y su capacidad de almacenamiento sea grande, no es viable y/o práctico almacenar la totalidad de nuestras canciones sin pérdida.
2. Podemos pasar los archivos de audio a terceros de una forma mucho más cómoda y rápida. Después de comprimir una canción de música podremos pasarla por email u otro medio sin ningún tipo de problema.
3. Parece que la tendencia en un futuro muy cercano será consumir el vídeo y el audio vía streaming. Por lo tanto la compresión del vídeo y del audio es muy importante. Si ofrecemos un servicio web en que proporcionamos audio en streaming, es imprescindible comprimir el audio para ahorrar ancho de banda y para que los clientes puedan reproducirlo en su casa sin problemas.

## CONSEJOS PARA COMPRIMIR UN AUDIO EN LINUX

Algunos consejos y puntos a tener en cuenta antes de comprimir un audio son los siguientes:

### Comprimir a partir de un archivo sin pérdidas

Para obtener buenos resultados es imprescindible hacer la compresión a partir de un archivo sin pérdidas y que no haya sido comprimido anteriormente.

Por lo tanto antes de comprimir un audio hay que [analizar la calidad del archivo de audio]({{< relref "/posts/como-conocer-calidad-de-un-archivo-de-audio" >}}) que queremos comprimir.

Si la calidad del audio es mala simplemente descarten la compresión. Una vez el archivo de audio ha perdido su calidad no la podremos recuperar nunca más.

### Seleccionar el formato de archivo del audio comprimido

Existen numerosos tipos de formato de archivo comprimido con pérdida. Algunos de los más populares son los siguientes:

- .mp3
- .ogg
- .wma
- .m4a
- .aac

En función de las necesidades les puede resultar útil seleccionar un formato u otro.

No obstante en mi caso recomiendo usar el formato de archivo mp3 o el ogg. Las razones son las siguientes:

1. Los archivos .mp3 no destacan por proporcionar la mejor calidad de audio. No obstante el formato .mp3 es el más extendido universalmente.
2. Cualquier reproductor de música es capaz de reproducir el formato de archivo .mp3. No se puede decir los mismo de los otros formatos de archivos.
3. Todo el mundo es capaz de reproducir un audio en formato .mp3. Incluso personas con pocos conocimientos tecnológicamente hablando.
4. Si alguien no quiere usar el formato .mp3 porque piensa que la calidad no es suficiente, o porque se trata de un formato propietario, le recomiendo usar .ogg. En el siguiente enlace podrán las [ventajas e inconvenientes de los archivos .ogg respecto los .mp3]({{< relref "/posts/ventajas-formato-ogg-respecto-mp3" >}}).

### Seleccionar el códec de compresión del audio (encoder)

Una vez seleccionado el formato de archivo debemos ser conscientes del códec que usaremos para realizar la compresión del audio.

En el caso que queramos comprimir en el formato de archivo .mp3 existen los siguientes códecs:

1. **FHC:** Es el primero códec que existió. En la actualidad este códec ha quedado obsoleto. Su tiempo de compresión es muy elevado y no soporta tasa de bits variable. Algunos de los programas que usan este códec son Audioactive Production Studio y MP3 Producer.
2. **Xing:** Es el encoder más rápido y permite obtener archivos con una tasa de bits variable (VBR). No obstante el nivel de calidad ofrecido por este tipo de encoder es inferior al Blade y al Lame. Algunos de los programas que utilizan este códec son AudioCatalyst y JukeBox.
3. **Blade:** Hasta la aparición de Lame era la mejor opción. Es ligeramente más lento que Xing, pero los niveles de calidad obtenidos son mucho mejores. Actualmente este enconder tiene un problema y es que no soporta las tasas de bits variable. Una muestra de los programas que utilizan este encoder son Easy CD-DA Extractor, Audiograbber y Cdex.
4. **Lame:** Está disponible bajo la licencia GNU y además es el mejor enconder disponible en la actualidad. Soporta tasas de bits variable, es rápido y la calidad obtenida es mejor que en el resto de encoders. Una muestra de los programas que utilizan Lame son WaveLab, CDcopy y AudioStation.

Después de ver todas las opciones, en mi caso caso intentaría asegurar que el software que usamos para comprimir el audio utilice el códec Lame.

En el caso que utilicen el formato de archivo .ogg recomiendo usar el códec oggenc que es el oficial de la fundación Xiph.

###### Nota: Asegúrense que los programas que usen para comprimir el audio dispongan de las versiones más actuales de los encoders.

### Usa siempre una tasa de bits variable (VBR)

En la gran mayoría de ocasiones es recomendable comprimir los audios usando una tasa de bits variable (VBR). El motivo es que este método es mucho más eficiente que el resto de métodos existentes.

Usando una tasa de bits variable cuando hay silencios se reduce la tasa de bits. De esta forma podemos almacenar el sonido de forma correcta ahorrando espacio.

En cambio en las partes de la canción complejas se incrementa la tasa de bits. De este modo en las partes más complejas de la canción usaremos más bits para asegurar que la calidad de este sonido sea aceptable.

De este modo, con una tasa de bits variable conseguiremos una calidad igual o superior a un archivo con tasa de bits constante (CBR) con un tamaño de archivo muy inferior.

###### Nota: En pruebas realizadas he constatado que la calidad ofrecida por un mp3 con una tasa de bits variable de 256 kbps es superior a la del mismo mp3 con una tasa de bits constante de 320 kbps.

El único caso que hoy en día justifica usar una tasa de bits constante (CBR) es cuando el audio a comprimir está destinado a escucharse vía streaming. En este caso usar una tasa de bits constante evita que un pico en la tasa de bits pueda superar los límites permitidos del servidor que realiza el streaming.

### Seleccionar la calidad de la compresión del audio

Este punto es muy personal y depende de muchos factores como por ejemplo los siguientes:

1. De lo fino que tengamos nuestro oído. Un oído fino y entrenado es capaz de detectar anomalías que no detectaría un oído normal.
2. Del uso que queremos dar al audio que queremos comprimir. No es lo mismo comprimir un audio para ofrecerlo en un servicio en streaming que comprimir un audio para escucharlo con una calidad alta.
3. Del reproductor de audio que usaremos para escuchar la música. De nada nos sirve disponer de un archivo con poca pérdida por compresión si los altavoces o auriculares que usamos son de baja calidad.

Por lo tanto mi recomendación es que realicen varias pruebas y lleguen ustedes a sus propias conclusiones.

Cuando hayan encontrado la calidad que les convenga pueden iniciar el proceso para comprimir la totalidad de sus audios.

En mi caso me gusta almacenar la música comprimida con una calidad decente que incluso las personas que aprecian la música considerarían aceptable.

Por lo tanto cuando comprimo archivos en mp3 siempre selecciono la calidad máxima. Para mi la calidad máxima se obtiene eligiendo los siguientes parámetros:

1. Usando el códec lame para realizar la compresión.
2. Con una tasa variable de bits.
3. Seleccionando la calidad máxima ofrecida por el códec Lame.

Para conseguir el propósito que acabo de citar utilizo los siguientes parámetros del encoder lame en la terminal de Linux:

> ```
> lame --quiet -vbr-new -V 0 ruta_y_nombre_del:archivo_a_comprimir
> ```

###### Nota: 0 corresponde a la máxima calidad. Si queremos obtener archivos de audio más comprimidos deberemos cambiar el 0 por un valor más alto como por ejemplo el 4. El valor máximo de calidad corresponde al 0, mientras que el valor mínimo corresponde al 9.

En el caso que decida comprimir mis audios mediante ogg vorbis como mínimo utilizo la calidad 7. Para obtener la calidad 7 el comando que acostumbro a ejecutar en la terminal es el siguiente:

> ```
> oggenc -q 7 ruta_y_nombre_del:archivo_a_comprimir
> ```

###### Nota: 7 corresponde a una muy buena calidad. Si queremos incrementar aún más la calidad podemos cambiar el numero 7 por el 9 o por el 10. El valor máximo de calidad corresponde al 10 mientras que el valor mínimo corresponde al -1.

Encontrarán sitios que les darán una clasificación de la calidad del audio obtenido en función de su tasa de bits. No considero que estas clasificaciones sean útiles por los siguientes motivos:

1. No existe unanimidad en la escala. A modo de ejemplo hay gente que dice que la calidad CD corresponde a 192 Kbps mientras que hay gente que dice que es 256 kbps.
2. La tasa de bits no determina la calidad de un audio. En determinados casos lo único que determina es el tamaño del archivo. Así por ejemplo el formato de archivo ogg es capaz de proporcionar calidades de audio superiores a un mp3 con un tasa de bits inferior.
