---
title: "Ventajas de la terminal respecto una interfaz gráfica"
date: 2022-08-06
categories: 
  - "linux-2"
  - "mac-os-x"
  - "windows"
tags: 
  - "terminal"
cover:
  image: "images/ventajas-de-la-terminal-respecto-la-interfaz-grafica.jpeg"
  relative: true
---

Todo usuario que se inicia en el uso de la [terminal (CLI)](https://terminaldelinux.com/terminal/introduccion/que-es-terminal/) acostumbra a pensar que usar la terminal es lento y engorroso. Además la gran mayoría piensa que es mejor utilizar una [interfaz gráfica (GUI)](https://es.wikipedia.org/wiki/Interfaz_gr%C3%A1fica_de_usuario). Pero si estos usuarios hacen el esfuerzo de usar la terminal, tarde o temprano, se darán cuenta que la terminal también ofrece ventajas frente a las interfaces gráficas. Por lo tanto a lo largo de este artículo veremos las características ofrecidas por cada una de las opciones.<!--more-->

## VENTAJAS Y DESVENTAJAS DE USAR LA TERMINAL

Algunas de las ventajas y desventajas al usar la terminal son las que citaremos a continuación.

### FACILIDAD DE USO

**Aprender a usar la terminal tiene una curva de aprendizaje más alta que usar interfaz gráfica**. Usar la terminal requiere:

1. Tener la capacidad y paciencia de memorizar comandos.
2. Tener curiosidad y ganas de aprender.
3. Ser paciente para leer la documentación de las [páginas man]({{< relref "/posts/poner-paginas-man-en-espanol-en-linux" >}}).
4. A ser posible tener nociones básicas de programación.
5. Tener la suficiente paciencia para irse familiarizando con el uso de terminal.

**En contraposición las aplicaciones con interfaz gráfica (GUI) tienen una curva de aprendizaje mucho menor que las que se ejecutan en la terminal (CLI).** Esto es así por los siguientes motivos:

1. Proporcionan un **entorno visual agradable**. Si una cosa es bonita por fuera atrae a los usuarios.
2. Acostumbran a ser **intuitivas**. Las interfaces gráficas tienen iconos, colores e imágenes que normalmente hacen que la curva de aprendizaje de los usuarios sea mucho menor. En este caso es importante recordar que la mayoría de seres humanos tienden a memorizar mejor los conceptos gráficos que los que están en formato texto.

Por las razones citadas en este apartado los usuarios noveles prefieren usar interfaces gráficas antes que la terminal. No obstante es importante remarcar lo siguiente. Es verdad que tardaremos más tiempo en aprender los comandos para usar la terminal, pero una vez aprendidos podremos abrir una terminal en cualquier ordenador y trabajar de forma habitual. En cambio las GUI acostumbran a varían mucho entre distintas versiones de programas.

### AUTOMATIZACIÓN DE TAREAS MEDIANTE SCRIPTS

Por norma general **las interfaces gráficas no permiten la automatización de tareas repetitivas**. Una claro ejemplo de lo que acabo de comentar es el redimensionado de fotografías. Si usáis un programa con interfaz gráfica y tenéis que redimensionar 1000 fotografías de un directorio a un 50% es altamente probable que tengáis que repetir 1000 veces la misma operación.

En cambio si usáis la terminal lo podrán hacer en cuestión de segundos ejecutando un comando similar al siguiente:

```shell
for file in *.png; do convert $file -resize 50% resized-$file; done
```

Basándome en este último ejemplo **recomendaría el uso de la terminal a la totalidad de usuarios que tengan en mente la productividad personal**. Tengan en cuenta que **toda tarea/operación realizada en al terminal se podrá automatizar** mediante scripts. Cuantas más operaciones consigáis automatizar más tiempo ahorrareis. Por lo tanto una de las ventajas que proporciona la terminal es la automatización de tareas.

### CONSUMO DE RECURSOS Y RENDIMIENTO

**Las aplicaciones que se ejecutan en la terminal (CLI) consumen menos recursos y tienen un rendimiento superior**. Esto es así porque las aplicaciones ejecutadas en la terminal:

1. No requieren de un driver gráfico avanzado.
2. No usan iconos ni imágenes.
3. No tienen que cargar ningún tipo de fuente.
4. Tienen un consumo de memoria menor.
5. Tienen un consumo de CPU menor.
6. Requieren una capacidad de almacenamiento en disco duro menor. Las aplicaciones que se ejecutan en la terminal prácticamente no ocupan espacio en nuestro disco duro.
7. Interactúan de forma mucho más directa con el sistema operativo.
8. etc.

Por lo tanto otra de las ventajas de la terminal será que el consumo de recursos será mucho menor que si usamos una interfaz gráfica. Consecuentemente el rendimiento y la velocidad con que se ejecutarán las tareas/operaciones será mejor en la terminal.

### VELOCIDAD DE EJECUCIÓN Y USO DE LOS PROGRAMAS

Con la terminal pueden satisfacer el 100% de sus necesidades mediante el uso del teclado. En cambio con las interfaces gráficas tendrán que ir moviendo el ratón e ir clicando en las opciones pertinentes. Por lo tanto **ejecutar acciones en la terminal (CLI) será más rápido que con una interfaz gráfica (GUI)**. Esto es así porque en la mayoría de ocasiones es mucho más rápido usar el teclado que ir moviendo el ratón e ir clicando encima de los iconos y opciones pertinentes. No obstante para ejecutar tareas de forma rápida en la terminal tendremos que ser capaces de:

1. Recordar comandos.
2. Recordar los atajos de teclado para poder interactuar con las aplicaciones que ejecutamos en la terminal.
3. Acceder de forma rápida a los comandos almacenados en el historial de nuestra shell.
4. Conocer trucos y los atajos de teclado para ejecutar de forma rápida comandos que se han ejecutado en el pasado.
5. Tener conocimientos básicos de programación de scripts. Los scripts permiten ejecutar un conjunto de comandos para conseguir un fin de forma rápida y sencilla.

Por lo tanto si tienen paciencia y dominan los 5 puntos que acabo de citar les aseguro que podrán realizar las tareas de forma sensiblemente más rápida en la terminal. No obstante requiere de un tiempo de aprendizaje y de persistencia.

### EVITAR DISTRACCIONES MIENTRAS USAMOS UN PROGRAMA

**Las aplicaciones que se ejecutan en la terminal facilitan concentrarte en lo que estás haciendo**. Las interfaces de terminal acostumbran a:

1. Mostrar únicamente las opciones que son necesarias para proseguir al siguiente paso.
2. Tener un número reducido de colores e información en pantalla. La interfaz de uso acostumbra a ser límpia.

En cambio las aplicaciones que se ejecutan mediante una interfaz gráfica acostumbran a ser todo lo contrario a lo que acabo de citar. Por lo tanto las aplicaciones que se ejecutan en la terminal evitan distracciones mientras las estamos usando. También nos deberían permitir ser más productivos.

### INTERACCIÓN CON EL SISTEMA OPERATIVO

**La línea de comandos (CLI) permite interactuar de forma más directa y precisa con nuestro sistema operativo** y saber en todo momento lo que está pasando. Por ejemplo si al ejecutar un programa en la terminal se produce un error nos saldrá un mensaje de error que nos dará alguna pista de lo que está pasando. En cambio si el programa se ejecuta a través de una interfaz gráfica no podremos ver el error de forma directa.

Además cuando lanzamos una aplicación en la terminal te da a entender que estás ejecutando un fichero binario junto a una serie de opciones que nosotros podemos modificar. Esta serie de opciones nos dan un mejor control de las acciones que realizamos en todo momento.

## CONCLUSIONES SOBRE LAS VENTAJAS DE USAR LA TERMINAL EN GNU LINUX

En ningún momento voy a decir que la terminal es mejor que una interfaz gráfica o viceversa. Simplemente me limitaré a citar sus ventajas e inconvenientes y después que cada usuario elija lo que crea conveniente. No obstante de antemano también digo que la terminal no es para todos los usuarios. Mis conclusiones finales son las siguientes:

|  | Terminal (CLI) | Interfaz Gráfica (GUI) |
| --- | --- | --- |
| **Productividad** | Es superior porque funciona con el teclado, no hay distracciones en su uso, permite automatización de tareas, permite ejecutar tareas de forma más rápida, etc. | Es inferior. No acostumbra a ofrecer automatización de tareas, se usa el ratón, muchas interfaces no son límpias, es más lento ejecutar tareas, etc. |
| **Consumo de recursos** | Es claramente menor. Menor consumo de memoria, de CPU y requiere de menos espacio de almacenaje. | Es claramente superior. Mayor consumo de memoria, de CPU y requiere más espacio de almacenaje. |
| **Velocidad de ejecución** | Al consumir menos recursos las tareas se ejecutarán de forma más rápida. | Al consumir más recursos las tareas se ejecutarán de forma más lenta. |
| **Facilidad de uso** | Su uso es más complicado. Requiere de una curva de aprendizaje elevada. | Su uso es más simple e intuivo. La curva de aprendizaje es menor. |

Hoy en día la realidad es que la gran mayoría de usuarios prefieren la interfaz gráfica porque de entrada es más amigable. No obstante si eres un heavy user, un programador o un administrador de sistemas te deberías plantear iniciarte poco a poco en el uso de la terminal.

###### FUENTES
