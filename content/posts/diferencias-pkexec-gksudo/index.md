---
title: "Diferencias entre pkexec y gksudo y porque se abandona gksudo"
date: 2018-10-28
categories: 
  - "linux-2"
tags: 
  - "gksudo"
  - "pkexec"
  - "sudo"
coverImage: "diferencias-pkexec-gksudo.png"
---

Hoy en día hay bastantes usuarios que intentan iniciar una aplicación gráfica como usuario root y se encuentran con la sorpresa que gksudo no está presente en su sistema operativo. Si se encuentran con está situación y usan el servidor gráfico X.org les recomiendo usar su reemplazo que es pkexec.<!--more-->

## ¿POR QUÉ SE HA ABANDONADO EL USO DE GKSUDO?

Muchas distribuciones, como por ejemplo Ubuntu a partir de su versión 18.04, han abandonado gksudo para ejecutar aplicaciones gráficas como administrador. Si usamos el servidor gráfico X.org el reemplazo de gksu o gksudo es pkexec.

**El motivo por el que se quiere abandonar gksudo es la seguridad**. Pkexec permite un control mucho más exhaustivo de los permisos que tiene cada usuario para iniciar una aplicación como si fuera otro usuario. Por lo tanto pkexec ejecutará los programas en un entorno de ejecución mucho más seguro que gksudo.

Tengan en cuenta que la mayoría de programas no han sido diseñados para ejecutarse y usarse con el usuario root. Por lo tanto, en el momento que ejecutamos una aplicación con el usuario root estamos ejecutando multitud de líneas de código que no han sido auditadas conveniente para usarse con privilegios de administrador. Esto puede generar problemas de seguridad o que simplemente el software deje de funcionar porque por ejemplo se reescriben las archivos de configuración de nuestra partición home.

## DIFERENCIAS ENTRE PKEXEC Y GKSUDO

La funcionalidad de pkexec y gksudo es similar, pero con ciertas diferencias. A continuación se muestra una explicación de lo que puede realizar cada una de las 2 herramientas:

**pkexec** es una herramienta que forma parte de PolicyKit y permite que **un usuario autorizado pueda ejecutar un único programa como si fuera otro usuario**. Con pkexec tenemos un amplio abanico de opciones para definir que puede y que no puede hacer un usuario.

**gksudo** proporciona permisos para que **un usuario autorizado** **pueda ejecutar cualquier programa o script como usuario root**. Las opciones para restringir las operaciones que puede realizar un usuario autorizado son pobres.

En resumen las principales diferencies entre gksudo y pkexec se muestran en la siguiente tabla:

| **gksudo** | **pkexec** |
| :-: | :-: |
| Un usuario autorizado puede iniciar **cualquier programa** en modo gráfico. | Un usuario autorizado puede iniciar **un único programa** en modo gráfico. |
| Únicamente permite **ejecutar programas** y scripts como si fuéramos el **usuario root**. | Permite ejecutar programas y scripts como si fuéramos **cualquiera de los usuarios** existentes en nuestro sistema operativo. |
| Las **opciones para restringir** lo que puede hacer un usuario son **menores**. | Permite un **control mucho más exhaustivo** de lo que puede hacer un usuario. |
| **Por defecto permite** iniciar aplicaciones gráficas como usuario root. | Las variables de entorno DISPLAY y XAUTHORITY no están definidas. Por lo tanto,  **por defecto no permite** iniciar aplicaciones gráficas como si fuéramos otro usuario. Antes hay que definir acciones y/o reglas. |
| Gksudo esta estrechamente ligado con **sudo**. | Pkexec forma parte de **PolicyKit**. |

## PARÁMETROS QUE SE PUEDEN CONTROLAR CON PKEXEC Y NO SE PUEDEN CONTROLAR CON GKSUDO

Como hemos detallado con anterioridad, pkexec permite un control mucho mas exhaustivo de los permisos que tiene cada usuario para iniciar una aplicación como si fuera otro usuario.

La forma que tenemos para delimitar que puede hacer un usuario es mediante las acciones y reglas de PolicyKit. Las opciones que podemos controlar y configurar con las acciones y reglas de PolicyKit son las siguientes:

1. Que **un usuario autorizado tan solo pueda iniciar la aplicación que nosotros queramos** como usuario root, o como otro usuario. Si usamos gksudo, un usuario autorizado puede iniciar todos los programas que quiera con permisos de administración.
2. **Definir el password que tiene que introducir un usuario** para iniciar una aplicación como si fuera otro usuario, como por ejemplo el root. Si lo precisamos podemos establecer que no tenga que introducir contraseña. También podemos establecer el tiempo que queremos que se guarde la contraseña, etc.
3. Que todos los **usuarios pertenecientes a un determinado grupo sean capaces de iniciar una aplicación** con permisos de administración o como si fueran otro usuario.
4. Pkexec siempre **informa del archivo binario que estamos ejecutando**. De este modo podemos incrementar la seguridad en la ejecución de programas y scripts.
5. Al contrario que gksudo, **pkexec no permite correr aplicaciones gráficas por defecto**. Para arrancar aplicaciones en modo gráfico deberemos crear una acción y configurarla para ello.

## ABRIR PROGRAMAS COMO SI FUÉRAMOS OTRO USUARIO USANDO PKEXEC

En futuros artículos veremos el proceso para crear acciones y reglas con PolicyKit. De esta forma con pkexec podremos ejecutar programas como si fuéramos otro usuario, como por ejemplo el usuario root.
