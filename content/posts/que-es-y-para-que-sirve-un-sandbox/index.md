---
title: "¿Qué es y para que sirve un sandbox en la informática?"
date: 2017-08-01
categories: 
  - "seguridad-informatica"
tags: 
  - "sandbox"
cover:
  image: "images/que-es-y-para-que-sirve-un-sandbox.png"
  relative: true
---

A continuación veremos una explicación simple para que todo el mundo pueda comprender lo que es un sandbox en el mundo de la informática.<!--more-->

## ¿QUÉ ES UN SANDBOX?

Un sandbox es un **mecanismo de seguridad para disponer de un entorno aislado del resto del sistema operativo**.

Todos los programas que se ejecutan dentro de un sandbox lo hacen de forma controlada mediante los siguientes aspectos:

1. Se les asigna un espacio en disco. Estos programas no podrán acceder a ningún espacio del disco que no les haya sido asignado previamente.
2. Podemos hacer que nuestros programas se ejecuten en un sistema de archivos temporal (tmpfs) para aislarlos del resto del sistema operativo.
3. También se les asigna un espacio en memoria. Los programas no podrán acceder a otras partes de la memoria que no les hayan sido asignadas.
4. Les podemos dar o restringir la capacidad para acceder y consultar dispositivos de almacenamiento externos.
5. Les restringimos la capacidad para que puedan inspeccionar la máquina anfitrión.
6. Podemos restringir el acceso de los programas a la red, al servidor de las X, al servidor de sonido, etc.
7. Podemos limitar el ancho de banda que usa un determinado programa.
8. Etc.

Consecuentemente podemos afirmar que los programas se ejecutan en un entorno controlado y separado del sistema operativo. Por lo tanto el hecho de ejecutar un programa en un sandbox es un ejemplo específico de [virtualización]({{< relref "/posts/que-es-una-maquina-virtual-usos-y-ventajas-que-nos-proporiciona" >}}).

## ¿QUÉ UTILIDAD TIENE UN SANDBOX?

Nos podemos hacer la idea que **su principal utilidad es ejecutar programas de forma segura y sin peligro de comprometer el resto del sistema operativo**.

Algunos ejemplos de los casos en que podemos utilizar un sandbox son los siguientes:

1. Instalar y usar de forma segura programas que no son fiables y/o pueden contener virus y/o código malicioso.
2. Usar programas que tienen el potencial de comprometer la seguridad de nuestro sistema operativo como por ejemplo un navegador web, Skype, un lector de pdf, un keygen descargado de Internet, etc.
3. Crear un entorno de pruebas para ejecutar programas o un código que acabamos de crear.
4. Para descargar Torrents.
5. Etc.

Por lo tanto podemos ver que **un sandbox no es de uso exclusivo para programadores o expertos informáticos**. También es útil para todo tipo de usuarios.

Algunos de los navegadores web que usamos disponen de su propio sandbox. No obstante no es mala idea usar dos capas de protección porque los navegadores web son altamente susceptibles de sufrir problemas de seguridad.

## ¿CÓMO PODEMOS USAR UN SANDBOX?

En función del sistema operativo que usemos existen diferentes software que nos permitirán ejecutar programas dentro de un sandbox. Algunos ejemplos son los siguientes:

1. **GNU-Linux:** [Firejail]({{< relref "/posts/firejail-sandbox-para-linux" >}}), Glimpse.
2. **Windows:** Sandboxie, Shadow Defender.
