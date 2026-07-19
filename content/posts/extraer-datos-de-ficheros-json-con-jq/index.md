---
title: "Extraer datos de ficheros json con jq"
date: 2023-02-26
categories: 
  - "linux-2"
tags: 
  - "bash"
  - "jq"
  - "json"
  - "programar"
cover:
  image: "images/Extraer-datos-de-ficheros-json-con-jq.png"
  relative: true
---

Son muchas las API que utilizan ficheros JSON para enviar y recibir datos. Por ejemplo es normal que una aplicación web pueda enviar datos dentro de un fichero JSON a una aplicación móvil para que la aplicación móvil pueda mostrar los datos en una interfaz de usuario amigable. Por esto motivo resulta interesante conocer algún método simple y sencillo para poder extraer los datos que queramos de los ficheros JSON. Por lo tanto, a continuación explicaremos como podemos extraer datos de un fichero JSON con la ayuda de la herramienta jq.<!--more-->

## ¿QUÉ SON LOS ARCHIVOS CON FORMATO JSON?

JSON es un formato de intercambio de datos. Los ficheros JSON contienen datos de una forma estructurada y fácilmente legibles tanto para humanos como para computadoras. La estructura de los datos que tienen los ficheros JSON es la siguiente.

### Muestra de lo que es un objeto, un nombre y un valor en JSON

En JSON podemos definir objetos, nombres y valores de la siguiente forma:

```shell
{
    "nombre": "Joan",
    "apellido": "Geekland",
    "edad": 32
}
```

En el ejemplo que acabamos de detallar vemos lo siguiente:

1. Hay un objeto JSON. Cada uno de los objetos JSON se delimita por los símbolos `{}`.
2. El objeto JSON contiene 3 pares **nombre/valor** que en este caso describen información de una persona. Cada uno de los **nombres** y de los **valores** está entre comillas y se usa `:` para separar el **nombre** del **valor**. Por lo tanto tendríamos el **nombre** `nombre` cuyo valor sería `Joan`, el **nombre** `apellido` cuyo valor sería `Geekland` y finalmente el **nombre** `edad` cuyo **valor** sería `32`.
3. A nota informativa pueden existir **objetos** dentro de otros **objetos**.

### Muestra de lo que es una array JSON

Los ficheros JSON también pueden contener **arrays**. Si al ejemplo anterior queremos añadir una array que contenga dos objetos y que a su vez contengan dos pares de nombre/valor lo haremos del siguiente modo:

```shell
{
    "nombre": "Joan",
    "apellido": "Geekland",
    "edad": 32,
    "trabajosRealizados": [
        {
            "empresa": "Geekland SA.",
            "puesto": "CEO"
        },
        {
            "empresa": "Linux SL.",
            "puesto": "Programador"
        }
    ]
}
```

Si observan el ejemplo podrán ver que:

1. Toda Array va delimitada entre los símbolos `[]`.
2. La **Array** `trabajosRealizados` mostrará todos los trabajos realizados por una determinada persona. La **Array** `trabajosRealizados` tiene 2 objetos y cada uno de los objectos tiene 2 pares de nombre/valor que detallan información cada uno de los trabajos realizados por una determinada persona.

Los objetos y las Array puedan anidarse dentro de otros objetos y Array. Por lo tanto podemos crear una estructura de datos extensa y flexible. Si partimos del ejemplo anterior podemos añadir la **Array** `puestosTrabajo` dentro de la **Array** `trabajosRealizados` del siguiente modo:

```shell
{
    "nombre": "Joan",
    "apellido": "Geekland",
    "edad": 32,
    "trabajosRealizados": [
        {
            "empresa": "Geekland SA.",
            "puestosTrabajo": [
            	{
            		"primero": "programador",
            		"segundo": "CEO"
            	}
            ]
        },
        {
            "empresa": "Linux SL.",
            "puesto": "Programador"
        }
    ]
}
```

De este modo podemos especificar la totalidad de trabajos que una persona ha realizado dentro de una empresa.

## EXTRAER DATOS DE LOS FICHEROS JSON CON LA UTILIDAD JQ

Acabamos de ver la estructura en que JSON almacena la información. Para poder extraer datos concretos del fichero JSON podemos utilizar la herramienta jq.

**Nota:** Existen otras herramientas como [sed]({{< relref "/posts/uso-del-comando-sed-en-linux-y-unix-con-ejemplos" >}}), [awk]({{< relref "/posts/uso-del-comando-awk-en-linux-y-unix-con-ejemplos" >}}), [cut]({{< relref "/posts/uso-del-comando-cut-en-linux-y-unix-con-ejemplos" >}}), etc que permiten extraer formatos datos de un archivo. Pero en el caso de ficheros json es mucho más sencillo y efectivo usar jq.

### ¿Qué es la herramienta jq?

jq es una herramienta de terminal multiplafatorma presente en Linux, macOS y Windows. La herramienta permite filtrar, extraer, buscar o modificar los datos dentro de un fichero JSON. Algunos usos específicos que podemos dar a jq son:

- Extraer y/o consultar elementos específicos almacenados en un ficheor JSON
- Filtrar datos mediante la función `select` según los criterios que nosotros podamos definir.
- Transformar/modificar los datos existentes dentro de un fichero JSON gracias a la función `map`.
- Modificar el valor de un **nombre**. Por ejemplo con la función `gsub` podemos reemplazar todos los espacios por \_.
- Agregar objetos y elementos dentro de un fichero JSON.
- Combinar los datos de múltiples ficheros JSON en un solo fichero JSON mediante la función `input`.
- Mediante la función `add` podemos sumar los elementos númericos almacenados en una matriz.

- Ordenar datos mediante la función `sort`.
- Etc.

jq se trata de un herramienta útil y versátil que dispone de innumerables funciones para trabajar con ficheros JSON. Algunas de estas funciones son `del`, `max`, `min`, `unique`, `contain`, `join`, etc. De la multitud de utilidades de jq únicamente nos centraremos en extraer los datos contenidos en ficheros JSON.

### Instalar jq en Linux para extraer datos de ficheros json

jq se encuentra en los repositorios de prácticamente la totalidad de distribuciones Linux. Por lo tanto para instalar jq en distribuciones que usen el gestor de paquetes apt deberán ejecutar el siguiente comando :

```shell
sudo apt install jq
```

Si su distribución usa el gestor de paquetes dnf entonces deberán ejecutar el siguiente comando:

```shell
sudo dnf install jq
```

Finalmente si usan el gestor de paquetes Pacman:

```shell
sudo pacman -Sy jq
```

### Extraer el valor de un determinado nombre de un fichero JSON con jq

Para extraer el **valor** del **nombre** `apellido` del fichero JSON `samplegeekland.json` que tiene el siguiente contenido:

```json
{
    "nombre": "Joan",
    "apellido": "Geekland",
    "edad": 32
}
```

Tan solo tendremos que ejecutar el siguiente comando:

```shell
❯ jq .apellido samplegeekland.json
"Geekland"
```

Si queremos que el resultado no muestre las comillas tan solo tendremos añadir la opción `-r` del siguiente modo:

```shell
❯ jq -r .nombre samplegeekland.json
Joan
```

### Extraer el valor de varios nombres de forma simultanea de un fichero JSON

Si ahora queremos obtener el **valor** de más de un **nombre** lo haremos del siguiente modo:

```shell
❯ jq -r ".nombre, .apellido, .edad" samplegeekland.json
Joan
Geekland
32
```

### Extraer el valor de un determinado nombre especificando el número de dígitos de la salida

Para hacer que el resultado obtenido no tenga más de por ejemplo 6 podemos realizar lo siguiente. Imaginemos que el fichero `samplegeekland.json` tiene el siguiente contenido:

```json
{
  "message": "success",
  "timestamp": 1677336457,
  "iss_position": {
    "longitude": "29.6908",
    "latitude": "-48.9932"
  }
}
```

Para obtener el **valor** del **nombre** `latitude` procederemos del siguiente modo:

```shell
❯ jq -r .iss_position.latitude samplegeekland.json
-48.9932
```

Finalmente si queremos que el valor `latitude` no tenga más de 6 cifras:

```shell
❯ jq -r .iss_position.latitude[0:6] samplegeekland.json
-48.99
```

### Extraer valores de matrices de un fichero JSON con jq

Si el fichero `samplegeekland.json` tiene las matrices **trabajosRealizados** y **puestosTrabajo**:

```json
{
    "nombre": "Joan",
    "apellido": "Geekland",
    "edad": 32,
    "trabajosRealizados": [
        {
            "empresa": "Geekland SA.",
            "puestosTrabajo": [
            	{
            		"primero": "programador",
            		"segundo": "CEO"
            	}
            ]
        },
        {
            "empresa": "Linux SL.",
            "puestosTrabajo": [
            	{
            		"primero": "Merchandiser",
            		"segundo": "Mozo de Almacén"
            	}
            ]
        },
        {
            "empresa": "Arnco.",
            "puestosTrabajo": [
            	{
            		"primero": "Comercial"
            	}
            ]
        },
        {
            "empresa": "Fruteria Geekland",
            "puestosTrabajo": [
            	{
            		"primero": "Limpiador",
            		"segundo": "Vendedor"
            	}
            ]
        }        
    ]
}
```

Y queremos extraer la totalidad de empresas en las que ha trabajado el usuario `joan` tendremos que ejecutar el siguiente comando:

```shell
❯ jq -r .trabajosRealizados[].empresa samplegeekland.json
Geekland SA.
Linux SL.
Arnco.
Fruteria Geekland
```

**Nota:** jq permite obtener el mismo resultado que acabamos de obtener usando tuberías. De este modo hubiéramos podido utilizar el comando `jq -r ".trabajosRealizados[] | .empresa" samplegeekland.json` para obtener el mismo resultado.

### Extraer valores de matrices especificando índice y longitud con jq

Si queremos que solo se muestre el nombre de la primera empresa usaremos el siguiente comando:

```shell
❯ jq -r .trabajosRealizados[0].empresa samplegeekland.json
Geekland SA.
```

A continuación usaremos el siguiente comando para mostrar el nombre de la última empresa:

```shell
❯ jq -r .trabajosRealizados[-1].empresa samplegeekland.json
Fruteria Geekland
```

Si ahora queremos obtener el nombre de la penúltima empresa:

```shell
❯ jq -r .trabajosRealizados[-2].empresa samplegeekland.json
Arnco.
```

Ahora si queremos el nombre de las 3 empresas más recientes:

```shell
❯ jq -r .trabajosRealizados[0,1,2].empresa samplegeekland.json
Geekland SA.
Linux SL.
Arnco.
```

Finalmente si lo que pretendemos es obtener del segundo valor hasta el final del listado:

```shell
❯ jq -r ".trabajosRealizados[1:] | .[] | .empresa" samplegeekland.json
Linux SL.
Arnco.
Fruteria Geekland
```

**Nota:** El elemento `.[]` se usa para iterar a través de todos los elementos de una matriz.

### Otros ejemplos de extracción de información almacenada en matrices

Para obtener los **objetos** y elementos almacenados en la **array** `puestrosTrabajo` procederemos del siguiente modo:

```json
❯ jq -r .trabajosRealizados[].puestosTrabajo[] samplegeekland.json
{
  "primero": "programador",
  "segundo": "CEO"
}
{
  "primero": "Merchandiser",
  "segundo": "Mozo de Almacén"
}
{
  "primero": "Comercial"
}
{
  "primero": "Limpiador",
  "segundo": "Vendedor"
}
```

Y ahora para que solo se muestren los **valores** del **nombre** `primero`:

```shell
❯ jq -r .trabajosRealizados[].puestosTrabajo[].primero samplegeekland.json
programador
Merchandiser
Comercial
Limpiador
```

De la misma forma que lo hicimos con anterioridad, también podemos hacer que unicamente se muestre el último de los elementos del siguiente modo.

```shell
❯ jq -r .trabajosRealizados[-1].puestosTrabajo[].primero samplegeekland.json
Limpiador
```

## INFORMACIÓN ADICIONAL SOBRE JQ

Si quieren profundizar más sobre el uso de jq les recomiendo que visiten el siguiente enlace.

[https://stedolan.github.io/jq/manual/](https://stedolan.github.io/jq/manual/)
