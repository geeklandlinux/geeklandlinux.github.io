---
title: "Cómo generar y verificar el Hash de archivos en linux"
date: 2023-10-22
categories: 
  - "linux-2"
  - "seguridad-informatica"
tags: 
  - "hash"
cover:
  image: "images/Como-generar-y-comprobar-el-Hash-de-archivos-en-linux.png"
  relative: true
---

La integridad de los archivos es de suma importancia. A menudo, descargamos archivos desde Internet, ya sean programas, imágenes ISO, documentos, o cualquier otro tipo de datos. Pero, ¿Cómo podemos estar seguros de que los archivos que descargamos no se han corrompido durante su descarga?, ¿Cómo podemos estar seguros que los ficheros descargados no han sido comprometidos por un atacante? Aquí es donde entran en juego los hash. En este artículo, exploraremos qué son los hash, por qué son útiles y finalmente veremos como podemos crear y verificar un hash.<!--more-->

## QUÉ ES UN HASH

Un hash es una cadena de caracteres generada por un algoritmo a partir de un conjunto de datos como por ejemplo un fichero o un mensaje. Esta cadena es única para cada conjunto de datos y tiene una longitud fija.

Por lo tanto:

1. Si generamos el hash de un fichero nos dará una cadena de caracteres como por ejemplo la siguiente: `a435f6f393dda581172490eda9f683c32e495158a780b5a1de422ee77d98e908`
2. Si intentamos crear otro hash y el fichero no ha sido modificado obtendremos exactamente el mismo valor que antes porque el fichero es exactamente el mismo que antes.
3. Si ahora modificamos el fichero y volvemos a generar el hash veremos que ahora es diferente. Cualquier cambio en los datos de entrada generará un hash completamente diferente.

Por lo tanto un hash es como una "huella digital" única para un conjunto de datos. Si dos archivos tienen el mismo hash, quiere decir que son son idénticos. Sin embargo, si sus hashes son diferentes, entonces los archivos son diferentes.

## UTILIDADES QUE TIENE VERIFICAR EL HASH DE UN FICHERO

Los hashes tienen diversas utilidades en el ámbito de la informática, especialmente en la seguridad y la integridad de los datos. A continuación, se presentan algunos ejemplos de cómo se utilizan los hash en diferentes contextos:

1. **Verificación de integridad de archivos**: Cuando descargas un archivo de Internet, es posible que desees asegurarte de que no ha sido modificado o dañado durante la transferencia. Al comparar el hash del archivo descargado con el hash proporcionado por la fuente, puedes verificar si el archivo es idéntico al original.
2. **Almacenamiento seguro de contraseñas**: En lugar de almacenar contraseñas en texto plano, que sería vulnerable a ataques, los sistemas de autenticación suelen almacenar hashes de contraseñas. Cuando un usuario ingresa su contraseña, el sistema calcula el hash y lo compara con el hash almacenado. Si coinciden, se concede acceso. Esto protege las contraseñas en caso de que la base de datos sea comprometida, ya que los hashes son difíciles de revertir a su forma original.
3. **Detección de malware**: Los programas antivirus y de seguridad informática utilizan hashes para identificar y detectar malware. Al comparar el hash de un archivo sospechoso con una base de datos de hashes de malware conocido, el software de seguridad puede determinar si el archivo es malicioso o no.
4. **Firmas digitales y autenticación de mensajes**: Los hashes se utilizan en combinación con la criptografía de clave pública para crear firmas digitales. Estas firmas permiten verificar la autenticidad e integridad de un mensaje o documento, asegurando que proviene de la fuente esperada y que no ha sido modificado.
5. **Tecnología blockchain y criptomonedas**: Las funciones hash son fundamentales en el funcionamiento de la tecnología blockchain y las criptomonedas como Bitcoin. Se utilizan para crear direcciones de billetera, en el proceso de minería y en la gestión de contratos inteligentes.

En resumen, los hashes son útiles en diversos contextos dentro de la informática, especialmente en la seguridad y la integridad de los datos. Algunas de sus aplicaciones incluyen la verificación de integridad de archivos, el almacenamiento seguro de contraseñas, la detección de malware, la autenticación de mensajes y la tecnología blockchain.

## ALGORITMOS DISPONIBLES PARA GENERAR UN HASH VENTAJAS E INCONVENIETES

Existen diversos algoritmos para generar hash. Los más comunes y sus características son las siguientes.

1. [**MD5**](https://es.wikipedia.org/wiki/MD5): Desarrollado en 1991, MD5 es un algoritmo de hash que produce un valor de hash de 128 bits. Aunque fue ampliamente utilizado en el pasado, actualmente se considera inseguro debido a vulnerabilidades.
2. **SHA-1**: Diseñado por la Agencia de Seguridad Nacional de EE. UU. (NSA), SHA-1 es un algoritmo de hash que genera un valor de hash de 160 bits. Al igual que MD5, SHA-1 ha sido considerado inseguro debido a debilidades criptográficas y ya no se recomienda para la mayoría de los usos criptográficos desde 2010.
3. [**SHA-2**](https://es.wikipedia.org/wiki/SHA-2): La familia SHA-2 incluye varias funciones de hash, como SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224 y SHA-512/256. Estos algoritmos, también diseñados por la NSA, son más seguros que SHA-1 y MD5. SHA-256 y SHA-512 son los más utilizados en la familia SHA-2 y emplean tamaños de palabra de 32 y 64 bits, respectivamente.
4. [**SHA-3**](https://es.wikipedia.org/wiki/SHA-3): Anteriormente conocido como Keccak, SHA-3 fue seleccionado en 2012 después de un concurso público entre diseñadores no pertenecientes a la NSA. Aunque es parte de la familia SHA, su estructura interna difiere significativamente de SHA-1 y SHA-2. SHA-3 admite las mismas longitudes de hash que SHA-2.
5. [**BLAKE2**](https://en.wikipedia.org/wiki/BLAKE_\(hash_function\)): BLAKE2 es un algoritmo de hash criptográfico diseñado como una alternativa más rápida y segura a SHA-2 y SHA-3. BLAKE2 tiene dos variantes principales: BLAKE2b, que es optimizada para sistemas de 64 bits, y BLAKE2s, que es optimizada para sistemas de 8 a 32 bits.

Los algoritmos de hash más utilizados actualmente son SHA-2 y SHA-3, debido a su mayor seguridad en comparación con MD5 y SHA-1. La elección de un algoritmo de hash específico depende de las necesidades de seguridad y rendimiento de la aplicación en la que se utilizará. En general, se recomienda utilizar algoritmos de hash más seguros y modernos, como SHA-2 o SHA-3, para garantizar la integridad y autenticidad de los datos.

## PAQUETES NECESARIOS PARA LA GENERAR Y VERIFICAR EL HASH DE UN FICHERO EN LINUX

Para verificar y generar un hash tendremos que tener los siguientes paquetes instalados en nuestro sistema operativo.

| Algoritmo hash | Paquete | Comando de instalación en Debian y derivadas |
| --- | --- | --- |
| SHA-2, MD5, SHA-1, BLAKE2 | coreutils | `sudo apt install coreutils` |
| SHA-3 | libdigest-sha3-perl | `sudo apt install libdigest-sha3-perl` |

## COMO PODEMOS GENERAR Y VERIFICAR EL HASH PARA UN FICHERO O ARCHIVO EN LINUX

Imaginemos que tenemos el fichero `geekland.txt`. Para generar y comprobar un hash con los algoritmos más habituales procederemos del siguiente modo.

### CREACIÓN Y VERIFICACIÓN DE UN HASH USANDO EL ALGORITMO MD5

El comando para generar un hash con el algoritmo MD5 del fichero geekland.txt es el siguiente:

```shell
❯ md5sum geekland.txt
d41d8cd98f00b204e9800998ecf8427e  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `d41d8cd98f00b204e9800998ecf8427e`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `d41d8cd98f00b204e9800998ecf8427e` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "d41d8cd98f00b204e9800998ecf8427e  geekland.txt" | md5sum --check
geekland.txt: La suma coincide
```

Si el resultado obtenido es `La suma coincide` significa que el hash del archivo `geekland.txt` es `d41d8cd98f00b204e9800998ecf8427e`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante o tercero.

**Nota:** A día de hoy el algoritmo de Hash MD5 es considerado obsoleto y vulnerable.

### CREACIÓN Y VERIFICACIÓN DE UN HASH CON EL ALGORITMO SHA-1

El comando para generar un hash SHA-1 del fichero `geekland.txt` es el siguiente:

```shell
❯ shasum -a 1 geekland.txt
da39a3ee5e6b4b0d3255bfef95601890afd80709  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `da39a3ee5e6b4b0d3255bfef95601890afd80709`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `da39a3ee5e6b4b0d3255bfef95601890afd80709` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "da39a3ee5e6b4b0d3255bfef95601890afd80709  geekland.txt" | shasum -a 1 --check
geekland.txt: OK
```

Si el resultado obtenido es `OK` significa que el hash del archivo `geekland.txt` es `da39a3ee5e6b4b0d3255bfef95601890afd80709`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante.

**Nota:** A día de hoy el algoritmo de Hash SHA-1 es considerado obsoleto y vulnerable.

### CREACIÓN Y COMPROBACIÓN DE UN HASH CON EL ALGORITMO SHA-256

El comando para generar un hash SHA-256 del fichero `geekland.txt` es el siguiente:

```shell
❯ shasum -a 256 geekland.txt
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855  geekland.txt" | shasum -a 256 --check
geekland.txt: OK
```

Si el resultado obtenido es `OK` significa que el hash del archivo `geekland.txt` es `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante.

**Nota:** SHA-256 fue creado por la NSA. Por lo tanto existen personas que desconfiarán a la hora de usar un HASH del tipo SHA-2.

### CREACIÓN Y COMPROBACIÓN DE UN HASH CON EL ALGORITMO SHA-512224

El comando para generar un hash SHA-512224 del fichero `geekland.txt` es el siguiente:

```shell
❯ shasum -a 512224 geekland.txt
6ed0dd02806fa89e25de060c19d3ac86cabb87d6a0ddd05c333b84f4  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `6ed0dd02806fa89e25de060c19d3ac86cabb87d6a0ddd05c333b84f4`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `6ed0dd02806fa89e25de060c19d3ac86cabb87d6a0ddd05c333b84f4` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "6ed0dd02806fa89e25de060c19d3ac86cabb87d6a0ddd05c333b84f4  geekland.txt" | shasum -a 512224 --check
geekland.txt: OK
```

Si el resultado obtenido es `OK` significa que el hash del archivo `geekland.txt` es `6ed0dd02806fa89e25de060c19d3ac86cabb87d6a0ddd05c333b84f4`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante.

**Nota:** SHA-51224 fue creado por la NSA. Por lo tanto existen personas que seguramente desconfiarán a la hora de usar un HASH del tipo SHA-2.

### CREAR Y VERIFICAR UN HASH CON EL ALGORITMO SHA-3 Y LA FUNCIÓN SHA-512

El comando para generar un hash con el algoritmo SHA-3 y la función SHA-512 del fichero `geekland.txt` es el siguiente:

```shell
❯ sha3sum -a 512 geekland.txt
a69f73cca23a9ac5c8b567dc185a756e97c982164fe25859e0d1dcc1475c80a615b2123af1f5f94c11e3e9402c3ac558f500199d95b6d3e301758586281dcd26  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `a69f73cca23a9ac5c8b567dc185a756e97c982164fe25859e0d1dcc1475c80a615b2123af1f5f94c11e3e9402c3ac558f500199d95b6d3e301758586281dcd26`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `a69f73cca23a9ac5c8b567dc185a756e97c982164fe25859e0d1dcc1475c80a615b2123af1f5f94c11e3e9402c3ac558f500199d95b6d3e301758586281dcd26` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "a69f73cca23a9ac5c8b567dc185a756e97c982164fe25859e0d1dcc1475c80a615b2123af1f5f94c11e3e9402c3ac558f500199d95b6d3e301758586281dcd26  geekland.txt" | sha3sum -a 512 --check
geekland.txt: OK
```

Si el resultado obtenido es `OK` significa que el hash del archivo `geekland.txt` es `a69f73cca23a9ac5c8b567dc185a756e97c982164fe25859e0d1dcc1475c80a615b2123af1f5f94c11e3e9402c3ac558f500199d95b6d3e301758586281dcd26`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante.

### CREAR Y VERIFICAR UN HASH CON EL ALGORITMO BLAKE2

El comando para generar un hash con el algoritmo BLAKE2 del fichero `geekland.txt` es el siguiente:

```shell
❯ b2sum geekland.txt
786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce  geekland.txt
```

Por lo tanto el hash del fichero `geekland.txt` es `786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce`

Si ahora enviamos el archivo `geeklant.txt` a una tercera persona y le decimos que el hash del fichero es `786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce` podrá comprobar la integridad del fichero mediante el siguiente comando:

```shell
❯ echo "786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce  geekland.txt" | b2sum --check
geekland.txt: La suma coincide
```

Si el resultado obtenido es `La suma coincide` significa que el hash del archivo `geekland.txt` es `786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce`. Por lo tanto el fichero `geekland.txt` no está corrompido ni ha sido modificado por un atacante.

## COMO VERIFICAR EL HASH DE UN FICHERO DESCARGADO EN LINUX

Imaginemos que queremos descargar una ISO De ubuntu. Esta ISO:

- Tiene el hash: `a435f6f393dda581172490eda9f683c32e495158a780b5a1de422ee77d98e909`
- El Nombre de la ISO es `ubuntu-22.04.3-desktop-amd64.iso`
- El algoritmo usado para crear el hash `a435f6f393dda581172490eda9f683c32e495158a780b5a1de422ee77d98e909` es SHA256

Por lo tanto para comprobar la integridad de este fichero .iso ejecutaremos el siguiente comando en la terminal:

```
echo "a435f6f393dda581172490eda9f683c32e495158a780b5a1de422ee77d98e909 ubuntu-22.04.3-desktop-amd64.iso" | shasum -a 256 --check`
`ubuntu-22.04.3-desktop-amd64.iso: OK
```

Como el resultado es `OK` podemos estar tranquilos que el archivo descargado no está corrompido.

## HERRAMIENTAS CON INTERFAZ GRÁFICA PARA VERIFICAR EL HASH DE UN FICHERO

En mi caso recomiendo usar la terminal para comprobar y generar los hash. No obstante también existen algunas herramientas de interfaz gráfica para Linux que son interesantes para comprobar y generar hash de un archivo. Algunas de ellas son las siguientes:

1. **QuickHash GUI**: QuickHash GUI es una herramienta de código abierto para Linux, Windows y Apple Mac OSX con interfaz gráfica de usuario (GUI). Los algoritmos hash disponibles actualmente incluyen MD5, SHA1, SHA-3 (256 bits), SHA256, SHA512, xxHash, Blake2B (256 bits) y Blake3MultiHasher.
2. **GTKHash:** Es una herramienta de escritorio para calcular resúmenes de mensajes o checksums en sistemas Linux. Está diseñada para ser una alternativa fácil de usar y gráfica a programas de línea de comandos como md5sum. Entre las funciones de hash compatibles se incluyen MD5, SHA1, SHA2 (SHA256/SHA512), SHA3 y BLAKE2. Adicionalmente podemos instalar los paquetes `caja-gtkhash`, `nemo-gtkhash` y `thunar-gtkhash` que harán que la utilidad GTKHash se integre en los gestores de archivos Caja, Nemo o Thunar.

En el caso que quieran recomendar otra aplicación lo pueden hacer en los comentarios de este artículo.

## CONCLUSIONES

El hash de un archivo es una herramienta importante para verificar la integridad de un archivo descargado. Si bien puede llevar un poco más de tiempo verificar el hash de un archivo, es un esfuerzo que vale la pena para asegurarse de que el archivo no se haya corrompido durante la descarga o que no haya sido manipulado por un actor malintencionado. En conclusión, verificar el hash de un archivo es una práctica importante para garantizar la seguridad y la integridad de los archivos descargados. Si la línea de comandos resulta incómoda para verificar el hash de un archivo, existen herramientas con interfaz gráfica que pueden ser utilizadas en su lugar.
