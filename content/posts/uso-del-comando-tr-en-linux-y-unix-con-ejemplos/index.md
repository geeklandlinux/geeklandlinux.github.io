---
title: "Uso del comando tr en Linux y UNIX con ejemplos"
date: 2022-01-08
categories: 
  - "linux-2"
tags: 
  - "bash"
  - "terminal"
  - "tr"
cover:
  image: "images/uso-del-comando-tr-en-linux.webp"
  relative: true
---

En esta ocasión veremos otra de las utilidades que nos servirán para manipular texto cuando estemos programando scripts o usando la terminal Linux o Unix. En esta ocasión veremos una serie de ejemplos que nos ayudarán a comprender el uso de la utilidad o comando tr<!--more-->

## ¿CUÁL ES LA PRINCIPAL FUNCIÓN DE LA UTILIDAD TR?

`tr` no es más que una abreviación de `translate`. Esto ya nos puede dar la pista que las principales utilidades de `tr` son las siguientes:

1. Cambiar caracteres, palabras y frases de mayúscula a minúscula o viceversa.
2. Buscar palabras y reemplazarlas por otras.
3. Borrar caracteres, palabras o frases.
4. Eliminar caracteres repetidos de forma innecesaria.
5. Cambiar letras o palabras de mayúscula a minúscula.
6. Eliminar la totalidad de caracteres que son ilegibles de un texto.
7. Añadir una nueva línea cada vez que encontremos un delimitador que podemos definir.
8. Eliminar la totalidad de caracteres de puntuación de una frase.
9. Etc.

`tr` solo puede operar y transformar texto desde la entrada estándar hasta la salida estándar. Por lo tanto no podremos actuar de forma directa sobre un fichero de texto.

## SINTAXIS BÁSICA DEL COMANDO TR

La sintaxis para usar el comando `tr` tiene la siguiente estructura:

```shell
tr Opciones conjunto_caracteres_1 conjunto_caracteres_2
```

Donde:

`tr`: Parte del comando que hace que se ejecute la utilidad tr.

`Opciones`: Se puede dejar en blanco o usar las opciones `-c`, `-d`, `-s`, y `-t`. En los ejemplos del siguiente apartado verán como pueden usar todas y cada una de de las opciones.

`conjunto_caracteres_1`: Conjunto de caracteres que se buscaran en un texto.

`conjunto_caracteres_2`: Conjunto de caracteres que se usarán para modificar un texto.

## EJEMPLOS PARA APRENDER EL USO EL COMANDO O TR

A continuación comprenderemos mejor el uso de `tr` mediante la siguiente serie de ejemplos.

### Cambiar una letra de minúscula a mayúscula y viceversa con el comando TR

En el caso que tengamos un texto y queramos transformar la totalidad de `a` a `A` lo haremos del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr 'a' 'A'
Esto es unA ejemplo de tr pArA el blog geeklAnd
```

En el caso que queramos reemplazar todas las vocales de una frase de minúscula a mayúscula lo podemos hacer del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr 'aeiou' 'AEIOU'
EstO Es UnA EjEmplO dE tr pArA El blOg gEEklAnd
```

### Cambiar toda una línea de minúscula a mayúscula

En el caso que queramos transformar la totalidad de letras mayúsculas a minúsculas lo podemos hacer del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr 'a-z' 'A-Z'
ESTO ES UNA EJEMPLO DE TR PARA EL BLOG GEEKLAND
```

Otro procedimiento alternativo para realizar lo que acabo de realizar sería el siguiente:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr '[:lower:]' '[:upper:]'
ESTO ES UNA EJEMPLO DE TR PARA EL BLOG GEEKLAND
```

Si queremos trasladar el resultado de la salida estándar a un fichero de texto cuyo nombre sea `geekland.txt` lo haremos del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr 'a-z' 'A-Z' > geekland.txt
joan@gk55:~$ cat geekland.txt 
ESTO ES UNA EJEMPLO DE TR PARA EL BLOG GEEKLAND
```

Si queremos también podemos añadir una línea nueva al fichero `geekland.txt` que sea igual que la primera pero en minúscula siguiendo el siguiente procedimiento:

`joan@gk55:~$ cat geekland.txt | tr 'A-Z' 'a-z' >> geekland.txt  joan@gk55:~$ cat geekland.txt  ESTO ES UNA EJEMPLO DE TR PARA EL BLOG GEEKLAND esto es una ejemplo de tr para el blog geekland`

### Reemplazar caracteres o cambiar de minúscula a mayúscula mediante la opción `-t` truncate

Si ejecutamos el siguiente comando:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog geekland" | tr 'a-z' 'AB'
EBBB BB BB BBBBBBB BB BB BABA BB BBBB BBBBBABB
```

Vemos que ha ocurrido lo siguiente:

La letra `a` se reemplaza por la `A`. La `b` por la `B` y a partir de aquí la `c`, `e`, `f`... hasta llegar la `z` se reemplazarán por la `B`.

Si queremos evitar este comportamiento podemos usar la opción `-t` o truncate. Si ejecutamos el mismo comando añadiendo la opción `-t`:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog geekland" | tr -t 'a-z' 'AB'
Esto es un ejemplo de tr pArA el Blog geeklAnd
```

Vemos que ha ocurrido lo siguiente:

La letra `a` se reemplaza por la `A`. La `b` por la `B` y a partir de aquí no se reemplazará ninguna otra letra. El comportamiento ahora será de este modo gracias a la opción `-t`. Por lo tanto la opción `-t` hace que la longitud del primero de los argumentos introducidos en el comando `tr` sea igual que la del segundo argumento.

**Nota**: Con la opción `-t` el primero de los argumentos introducidos del comando `tr` tiene que tener una longitud igual o superior al segundo de los argumentos.

### Reemplazar caracteres con la utilidad TR

De la misma forma que cambiábamos letras de mayúsculas a minúsculas o viceversa también podemos reemplazar caracteres. Por lo tanto si queremos reemplazar todas las `a` por `u` y las `u` por `a` lo haremos del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es una ejemplo de tr para el blog geekland" | tr 'au' 'ua'
Esto es anu ejemplo de tr puru el blog geeklund
```

### Borrar caracteres con el comando TR

Si queremos borrar todas las vocales mayúsculas y minúsculas de una frase y que sean reemplazadas por un espacio en blanco lo haremos mediante la opción `tr -d`. Un ejemplo de lo que acabo de citar es el siguiente:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog geekland" | tr -d 'aeiou' | tr -d 'AEIOU'
st s n jmpl d tr pr l blg gklnd
```

Si quisiéramos que las vocales borradas fueran reemplazadas por espacios en blanco deberíamos haber usado el siguiente comando:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog geekland" | tr 'aeiou' ' '
Est   s  n  j mpl  d  tr p r   l bl g g  kl nd
```

### Borrar todo el contenido de una frase excepto las vocales aeiou

Para conseguir el propósito del título tendremos que usar la opción `-c`. La opción `-c` o complemento vendría a actuar como lo contrario a la orden que introducimos en el comando. Por lo tanto para borrar todas las letras que no sea vocales de una frase lo haremos del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog geekland" | tr -cd 'aeiou'
oeueeoeaaeoeea
```

**Nota:** El significado de `-cd 'aeiou'` vendría a ser borra todo aquello que no sean vocales a, e, i, o, u.

### Borrar todos los caracteres que no sean legíbles

La salida del comando `head /dev/urandom` es la siguiente:

``joan@gk55:~$ head /dev/urandom ��d�q)0�K����^���^�M���$���n���rp�y��J�ӠY�J��p�T~�"�`�W�\�W�&C�{�ӒL��hk~�Nu�Za�ٍ]�                                                                                  �N?ӳw�"��D�鳶:��U��A�첣�*�s-px�/A'�#v`�.�����t�ݽ��ßb���mx����8�aX�i�Ĉnϗ� �n�籛�(�<?"pȉ����fȊ�ϓ���������5�U���(<�<�DZ����P;����?���\:�(�M�������~<ڧ�UQ��׽�+���<=�D��]=�Um���M�c 2m��5X�DJB� ...``

Como han podido ver hay una gran parte de caracteres que son completamente ilegibles. Para eliminar la totalidad de caracteres ilegibles lo pueden hacer mediante la opción `[:print:]` del siguiente modo:

```shell
joan@gk55:~$ head /dev/urandom | tr -cd '[:print:]\n'
NB
Oz.R$Kv/6-J$gTU2AAg=y{{9{n"W|eD8)ZzO96{h"kDGX="3g_R}p#,t[lbp;ci+an"w5*r, L8b<QC}?8/49M$TA#uK;M>nXCEeE7t~7,ShzeJUVO<AjN;vuuy1xOG1FLS[e}{SC_8N,b^9v=4nNez23'\.,7dv|tHR.78yK&"gM,dZH)<_2l~+13}8*xz7mu>)=nf5bB@J<h|Sl}LRI~jKP*r~U)T"8^0])\1K1l+,+%eZRi[Q>$Xtf] "hoIL,c[gZ(V(ju(#l^z SH}vzToTn&%P94IQR3PWXH\uLs-.y#[6^T!:PGedzUoL;u3kA~A{7=_]AB< t:k:xPI]oFT`Oob:",)W9X9CA
J0M!Uo(>~QHuZnjj>u5%+V}7PKy?@S'z}~9c1E-':3gYM87myPZ?{0^EuX^O0l+E~,?(I-DbPNh1t@apeXFY9\n\W0{~'fjy*u

.....
```

**Nota:** Mediante la opción `-cd '[:print:]'` estamos diciendo que se borre todo carácter que no se imprimible/legíble. Además la opción `\n` hará que al finalizar la tarea el cursos de la terminal quede ubicado en una nueva línea.

### Borrar todo lo que no sea un dígito o número mediante la utilidad TR

Para conseguir nuestro objetivo imaginemos que tenemos la siguiente frase:

```shell
Esto es un ejemplo de tr para el blog gee1kland 1234
```

Si ahora queremos eliminar absolutamente todos los caracteres que no sean numéricos lo haremos del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog gee1kland 1234" | tr -cd '[:digit:]\n'
11234
```

Si por lo contrario quieren borrar la totalidad de números que aparecen a la frase tan solo tendrían que eliminar la opción `-c` complemento y obtendrían el siguiente resultado:

```shell
joan@gk55:~$ echo "Esto es un ejemplo de tr para el blog gee1kland 1234" | tr -d '[:digit:]\n'
```

```shell
Esto es un ejemplo de tr para el blog geekland
```

### Borrar 2 caracteres iguales que hemos escrito de forma consecutiva mediante la opción Squeeze

En ocasiones escribimos rápido y es posible que por error escribamos 2 caracteres repetidos de forma consecutiva. Un ejemplo de lo que acabo de citar es el siguiente:

```shell
Esto es un ejeeemplo de tr para el blog.
```

En el ejemplo vemos que a la palabra ejemplo le sobran dos e. Para eliminarlas lo podemos hacer con la opción `-s` Squeeze del siguiente modo:

```shell
joan@gk55:~$ echo "Esto es un ejeeemplo de tr para el blog geekland" | tr -s 'e'
Esto es un ejemplo de tr para el blog.
```

Si tuviéramos una frase con exceso de espacios, como por ejemplo la siguiente:

```shell
El comando    tr  es       útil   en determinados casos
```

Y quisiéramos eliminar los espacios sobrantes lo haríamos del siguiente modo:

```shell
joan@gk55:~$ echo "El comando    tr  es       útil   en determinados casos" | tr -s [:space:]
El comando tr es útil en determinados casos
```

### Transformar los espacios en tabulaciones horizontales o verticales con TR

En el ejemplo que propondremos a continuación tenemos 3 palabras separadas por un espacio:

```shell
joan carles geekland
```

Si queremos transformar este espacio por un tabulación lo haremos usando la opción `[:space:]` y `\t` del siguiente modo:

```shell
joan@gk55:~$ echo "joan carles geekland" | tr [:space:] '\t'
joan	carles	geekland
```

La opción `\t` añade una tabulación horizontal. Si quisiéramos añadir una tabulación vertical lo haríamos con la opción `\v` del siguiente modo obteniendo un resultado parecido al siguiente:

```shell
joan@gk55:~$ echo "joan carles geekland" | tr [:space:] '\v'
joan
    carles
          geekland
```

### Generar una nueva línea cada vez que encontremos un determinado carácter delimitador

Si tenemos una frase, que obviamente está compuesta por palabras separadas por espacio, podemos separar cada una de las palabras en una nueva línea usando la siguiente sintaxis:

```shell
joan@gk55:~$ echo "joan carles geekland" | tr [:space:] '\n'
joan
carles
geekland
```

### Otras utilidades del comando TR

Obviamente este artículo únicamente detalla algunas de las opciones del comando `tr`. Si quieren profundizar más en las opciones les recomiendo encarecidamente que abran una terminal y ejecuten el siguiente comando:

```shell
joan@gk55:~$ man tr
```

Allí podrán obtener información Para más obtener más información para poder incrementar la funcionalidad de la utilidad `tr`.

Si te ha interesado esté articulo es posible que también te interese:

[Uso del comando cut en Linux y UNIX con ejemplos]({{< relref "/posts/uso-del-comando-cut-en-linux-y-unix-con-ejemplos" >}}) [Uso del comando AWK en Linux y UNIX con ejemplos]({{< relref "/posts/uso-del-comando-awk-en-linux-y-unix-con-ejemplos" >}}) [Uso del comando SED en Linux y UNIX con ejemplos]({{< relref "/posts/uso-del-comando-sed-en-linux-y-unix-con-ejemplos" >}})

#### Fuentes

[https://man7.org/linux/man-pages/man1/tr.1.html](https://man7.org/linux/man-pages/man1/tr.1.html)
