---
title: "Atajos de teclado para emuladores de terminal en Linux"
date: 2022-07-24
categories: 
  - "linux-2"
tags: 
  - "atajos-de-teclado"
  - "terminal"
  - "tips"
coverImage: "atajos-de-teclado-para-emuladores-de-terminal-en-linux.webp"
---

Conocer los atajos de teclado de la terminal les proporcionará velocidad y también les permitirá tener acceso a una serie de funciones adicionales. Los atajos de teclado típicos que acostumbran a tener la gran mayoría de emuladores de terminal son los que verán a continuación.<!--more-->

## Atajos de teclado para mover el cursor de posición

Los atajos de teclado que podemos usar para mover la posición en que se encuentra el cursor de la terminal son:

| Atajo de Teclado | Acción |
| --- | --- |
| `ctrl+a` | Mover el cursor al inicio de la línea. |
| `ctrl+e` | Mover el cursor al final de la línea. |
| `ctrl+b` o `←` | Hacer que el cursor retroceda un carácter. |
| `alt+b` o `Ctrl+ ←` | Hacer que el cursor retroceda una palabra. |
| `ctrl+f` | Hacer que el cursor avance un carácter. |
| `alt+f` o `Ctrl+ →` | Mover el cursor al inicio de la siguiente palabra. |

## Atajos para copiar, cortar y pegar texto

Si queremos cortar, copiar, pegar y manipular texto en la terminal le serán de utilidad los siguientes atajos de teclado:

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+Shift+c` | Copiar el texto seleccionado en el portapapeles. |
| `Ctrl+Shift+v` | Pegar el texto copiado en el portapapeles. |
| `Ctrl+u` | Cortar todo el contenido desde la posición actual del cursor hasta el inició de la línea. |
| `Ctrl+k` | Cortar todo el contenido desde la posición actual del cursor hasta el final de la línea. |
| `Ctrl+w` | Para cortar la palabra que está justo antes del cursor. |
| `Alt+d` o `Esc+d` | Cortar la palabra justo después del cursor. |
| `Ctrl+y` | Permite pegar el último texto que se ha cortado con los atajos `Ctrl+u`, `Ctrl+k`, `Alt+d` o `Ctrl+u`. |
| `Alt+y` | Permite pegar el penúltimo texto que se ha cortado con los atajos `Ctrl+u`, `Ctrl+k`, `Alt+d` o `Ctrl+u`. |

## Atajos para borrar contenido en la terminal

En el caso que tengamos que borrar texto en la terminal es importante tener en mente los siguientes atajos de teclado:

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+h` o `Retroceso` | Borrar un solo carácter. Equivalente a la tecla retroceso. |
| `Ctrl+d` o `Supr` | Borrar el carácter sobre el que está el cursor |
| `Ctrl+w` | Borrar la palabra que está justo antes del cursor. |
| `Alt+d` o `Esc+d` | Borrar la palabra justo después del cursor. |
| `Ctrl+u` | Borra todo el contenido desde la posición actual del cursor hasta el inició de la línea. |
| `Ctrl+k` | Borrar todo el contenido desde la posición actual del cursor hasta el final de la línea. |
| `Ctrl+l` | Limpia la pantalla de la terminal. Este atajo de teclado es equivalente a usar el comando `clear`. |
| `Ctrl+d` | Sirve para cerrar la sesión actual de terminal. Es equivalente al comando `exit`. |

## Atajos de teclado relacionados con la edición de texto en la terminal

En el caso que necesitemos modificar el texto escrito en la terminal les pueden llegar a ser útiles los siguientes atajos de teclado:

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+t` | Intercambia la posición de los 2 caracteres antes del cursor. |
| `Esc+t` o `Alt+t` | Intercambia la posición de las 2 palabras antes del cursor. |
| `Esc+u` | Cambiar de minúsculas a mayúsculas desde la posición del cursor hasta el final de la palabra. |
| `Alt+u` | Cambiar de minúsculas a mayúsculas desde la posición del cursor hasta el siguiente espacio. |
| `Esc+l` | Cambiar de mayúsculas a minúsculas desde la posición del cursor hasta el final de la palabra. |
| `Alt+l` | Cambiar de mayúsculas a minísculas desde la posición del cursor hasta el siguiente espacio. |
| `Alt+c` | Cambia de minúsculas a mayúsculas la letra sobre la que está el cursor y el cursor se posiciona al final de la palabra. |
| `TAB` o `Alt+i` | Permite autocompletar rutas y comandos. Es sumamente útil para incrementar la velocidad cuando usamos la terminal. Si estamos en la mitad de escritura de un comando o ruta aprieten la tecla `TAB` para autocompletar el comando o la ruta. Si no se autocompleta es porque hay más de una opción disponible. Para ver todas las opciones disponibles presionen la tecla `TAB` 2 veces. |

## Ejecutar comandos que se han ejecutado con anterioridad

Linux almacena la totalidad de comandos ejecutados en la terminal en el fichero `~/.bash_history`. Gracias a este archivo podremos recuperar de forma rápida y sencilla comandos que hemos ejecutado en el pasado. De esta forma podremos ejecutar comandos largos y complejos de forma rápida y sencilla. Los atajos de teclado a usar para recuperar los comandos almacenados en el historial de comandos de la terminal son los siguientes:

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+p` o `↑` | Imprime el último comando ejecutado en la terminal. Si lo apretamos 2 veces muestra el penúltimo comando y así sucesivamente. |
| `Ctrl+n` o `↓` | Imprime el siguiente comando hacia adelante en caso que antes hayamos usado los atajos de teclado `Ctrl+p` o `↑` |
| `Alt+.` | Mostrar en pantalla la última palabra del último comando ejecutado. |
| `Alt+<` | Imprime el primer comando del historial de comandos. |
| `Alt+>` | Imprime el último comando del historial de comandos. |
| `Ctrl+r` | Permite buscar comandos almacenados en el historial de comandos. Justo después de presionar la combinación de teclas `Ctrl+r` hay que teclear parte del comando que estamos buscando y la terminal nos propondrá la ejecución de un comando similar que esté almacenado en el fichero `~/.bash_history`. Si la propuesta no es la que estamos buscando podemos volver a presionar a combinación de teclas `Ctrl+r` sucesivamente para ir obteniendo nuevas propuestas. |
| `Alt+p` | Similar al atajo de teclado `Ctrl+r`. La diferencia es que aquí la búsqueda de comandos en el historial no es incremental. |
| `Enter` o `Ctrl+o` | Para confirmar la ejecución de un comando encontrado con la opción `Ctrl+r`. |
| `Ctrl+j` | Para escribir en pantalla el comando que nos propone la opción de búsqueda de `Ctrl+r`. El comando se escribe en pantalla pero no se ejecuta. |
| `Ctrl+g` | Para salir de la búsqueda que estamos realizando. |

## Atajos de teclado para gestionar los procesos que se ejecutan en la terminal

Linux permite [gestionar distintos procesos y trabajos en la terminal]({{< relref "/posts/gestionar-trabajos-en-linux-terminal" >}}). Para ello hay una serie de atajos de teclado que les serán de gran utilidad.

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+c` | Matar un proceso o cancelar la ejecución de un programa que se está ejecutando en primer plano. |
| `Ctrl+z` | Para llevar a segundo plano un trabajo que estamos ejecutando. Podemos tener varios trabajos corriendo en segundo plano. Para acceder y gestionar los trabajos corriendo en segundo plano deberán usar los comandos detallados en el siguiente [enlace]({{< relref "/posts/gestionar-trabajos-en-linux-terminal" >}}). |
| `Ctrl+d` | Sirve para cerrar la sesión actual de terminal. Es equivalente al comando `exit`. |

## Atajos de teclado para mejorar la accesibilidad y funcionalidad de la terminal

Si el texto que aparece en la terminal es demasiado pequeño o demasiado grande lo podemos incrementar o disminuir de forma fácil. También podemos congelar la salida de un proceso o bloquear la terminal . Para realizar lo que acabamos de citar, además de otras acciones, pueden usar los siguientes atajos de teclado.

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+mayúsc++` | Incrementar el tamaño de fuente de la terminal. |
| `Ctrl+mayúsc+-` | Disminuir el tamaño de fuente de la terminal. |
| `Ctrl+mayús+Inicio` | Hacer scroll hasta el inicio de la sesión de terminal. |
| `Ctrl+mayús+Repág` | Hacer scroll una página hacia atrás en la terminal. De esta forma podremos ver que ha pasado en la sesión actual del emulador de terminal. |
| `Ctrl+mayús+↑` | Hacer scroll una línea hacia arriba en la terminal. |
| `Ctrl+mayús+Fin` | Hacer scroll hasta el final de la sesión de terminal. |
| `Ctrl+mayús+Avpág` | Hacer scroll una página hacia adelante. |
| `Ctrl+mayús+↓` | Hacer scroll una línea hacia adelante. |
| `Ctrl+mayús+t` | Abrir una pestaña en la terminal. |
| `Ctrl+mayús+→` y `Ctrl+ mayús+←` | Moverse entre las distintas pestaña abiertas. |
| `Ctrl+mayús+w` | Cerrar una pestaña que tengamos abierta. |
| `F11` | Trabajar con el emulador de terminal a pantalla completa. |
| `Ctrl+s` | Para bloquear la terminal. Una vez bloqueada nadie podrá escribir absolutamente nada en la terminal. La salida de resultados de un programa en la terminal también se congelará. |
| `Ctrl+q` | Para desbloquear la terminal que previamente se ha bloqueado con `Ctrl+s` |

## Abrir o cerrar la terminal

Puede que en determinados casos no les funcionen los atajos que cito a continuación, pero por norma general podremos abrir y cerrar la terminal mediante los siguientes atajos de teclado:

| Atajo de Teclado | Acción |
| --- | --- |
| `Ctrl+t` | Para abrir la terminal. |
| `Ctrl+d` | Sirve para cerrar la sesión actual de terminal. Es equivalente al comando `exit` |

Si conocen algún otro atajo de teclado útil y que no esté en el artículo les pido que lo comportan en los comentarios de este artículo.

### Fuentes

[https://www.makeuseof.com/linux-bash-terminal-shortcuts/](https://www.makeuseof.com/linux-bash-terminal-shortcuts/)
