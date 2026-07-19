---
title: "Cómo instalar y usar Nala como gestor de paquetes para reemplazar apt"
date: 2023-06-18
categories: 
  - "linux-2"
tags: 
  - "apt"
  - "gestor-de-paquetes"
  - "nala"
cover:
  image: "images/Como-instalar-y-usar-el-gestor-de-paquetes-Nala-en-Linux-para-reemplazar-apt.png"
  relative: true
---

La gran mayoría de distribuciones que usan paquetes .deb usan apt como gestor de paquetes de alto nivel. Este gestor de paquetes funciona bien, no obstante también es cierto que presenta algunas limitaciones. Por este motivo en el siguiente artículo veremos como usar un gestor de paquetes alternativo que se llama Nala. Antes de iniciar la explicación de como instalar y usar Nala veremos los principales inconvenientes del gestor de paquetes apt.<!--more-->

## INCONVENIENTES DEL GESTOR DE PAQUETES APT

Los principales inconvenientes del gestor de paquetes apt son los siguientes:

1. Por defecto no permite descargas paralelas y por este motivo, entre otros, es más lento que otros gestores de paquetes como por ejemplo Pacman. En el caso que se quisieran acelerar las descargas podrían usar apt-fast, pero esto requiere de instalación de paquetes adicionales y un proceso de configuración.
2. Los mirrors configurados por defecto no acostumbran a ser los más rápidos en función de nuestra ubicación geográfica y carga de los mirrors. Para solucionar este tema Debian usa [repositorios Redirector]({{< relref "/posts/repositorios-redirector-en-debian" >}}) o también se pueden usar otras herramientas como [apt-spy o netselect]({{< relref "/posts/elegir-el-mejor-repositorio-en-debian" >}}).
3. No es fácil consultar el historial de paquetes instalados. Cabe decir que es posible, pero para ello tienes que consultar los logs del sistema operativo.
4. La lectura del contenido mostrado por la terminal no es visualmente atractivo ni proporciona información fácil de leer.

Para solucionar la totalidad de los problemas citados podemos usar el gestor de paquetes nala. Nala nos proporcionará las siguientes funcionalidades y ventajas.

## VENTAJAS DE NALA RESPECTO AL GESTOR DE PAQUETES APT

Las funcionalidades adicionales que nos proporcionará son las siguientes:

1. Permite **descargas paralelas**. Permite descargar 3 paquetes de un repositorio de forma simultánea, mientras que apt solo puede descargar un paquete y cuando se termina la descarga se inicia la descarga del siguiente paquete.
2. **Podemos configurar varios mirrors de forma simultánea** y nala alternará entre ellos para intentar disponer de la mayor velocidad de descarga.
3. Dispone de una herramienta incorporada para **detectar y usar los mirrors más rápidos**. Para obtener el listado de los mirrors más rápidos tan solo tendrán que ejecutar el comando `sudo nala fetch`.
4. Mediante el comando `nala history` podemos consultar la totalidad de acciones realizadas con el gestor de paquetes nala. Cada una de las acciones realizadas tendrá asignada un determinado ID. Mediante el ID y los comando `sudo nala undo ID` y `sudo nala redo ID` podremos deshacer y rehacer acciones realizadas en el pasado.
5. Los resultados que se muestran en la pantalla serán mucho más coloridos y fáciles de leer que los del gestor de paquetes apt.

Una vez citadas las ventajas, podéis instalar Nala siguiendo estos pasos.

## INSTALAR EL GESTOR DE PAQUETES NALA

Nala acostumbra a estar en los repositorios de la mayoría de distribuciones que usan el gestor de paquetes apt. Por lo tanto para su instalación tan solo tienen que ejecutar el siguiente comando:

> ```shell
> sudo apt install nala
> ```

En el caso que no esté en el repositorios les sugiero que usen algunos de los métodos detallados en el siguiente enlace.

[https://gitlab.com/volian/nla/-/wikis/Installation](https://gitlab.com/volian/nala/-/wikis/Installation)

El uso de este gestor de paquetes es muy similar al de apt. Para que puedan usar Nala de forma sencilla, les dejo la siguiente tabla de equivalencias entre el gestor de paquetes apt y Nala.

## TABLA DE EQUIVALENCIAS ENTRE EL GESTOR DE PAQUETES NALA Y APT

En la siguiente tabla encontrarán la totalidad de comandos que pueden ejecutar en nala con su correspondiente equivalente en apt. Si os fijáis, los comandos son prácticamente equivalentes y tan solo tienen que reemplazar apt por nala.

| Comando nala | Comando apt | Explicación del comando Nala |
| --- | --- | --- |
| `nala install paquete` | `apt install paquete` | Instalar un paquete |
| `nala remove paquete` | `apt remove paquete` | Desinstalar una paquete concreto |
| `nala autoremove` | `apt autoremove` | Desinstalar los paquetes que no se necesitan |
| `nala purge paquete` | `apt purge paquete` | Eliminar paquetes y los ficheros de configuración que especificamos |
| `nala autopurge` | `apt autoremove --purge` | Eliminar paquetes y los ficheros de configuración que no se necesitan |
| `nala clean` | `apt clean` | Borrar el repositorio local de paquetes .deb descargados |
| `nala update` | `apt update` | Refrescar los repositorios |
| `nala upgrade` | `-` | Refrescar los repositorios y actualizar el sistema operativo |
| `nala upgrade --full` | `apt dist-upgrade` | Actualizar el sistema operativo instalando nuevas paquetes y dependencias en case necesario |
| `nala upgrade --no-update` | `apt upgrade` | Para actualizar el sistema sin refrescar los repositorios |
| `nala upgrade --exclude paquete` | `apt-mark hold paquete` | Actualizar todos los paquetes excepto los que queremos excluir |
| `nala list --upgradable` | `apt list --upgradable` | Listar los paquetes que se pueden actualizar |
| `nala list` | `apt list` | Mostrar un listado de la totalidad de paquetes instalados en el sistema operativo |
| `nala list -i` | `apt list -i` | Mostrar un listado con información detallada de la totalidad de paquetes instalados en el sistema operativo |
| `nala list -N` | `-` | Obtener un listado de la totalidad de paquetes instalados únicamente con nala |
| `nala fetch` | `-` | Ver los mirrors más rápidos para acelerar las descargas |
| `nala history` | `-` | Mostrar la totalidad de operaciones realizadas con nala |
| `nala history undo ID` | `-` | Para deshacer una operación realizada en el pasado |
| `nala history redo ID` | `-` | Para repetir una operación realizada en el pasado |
| `nala search texto` | `apt search texto` | Consulta paquetes que en su nombre o descripción tengan un determinado texto |
| `nala show paquete` | `apt show paquete` | Consultar información detallada sobre un paquete específico |

Si quieren obtener más información sobre los comando disponibles tan solo tienen que consultar las [páginas man]({{< relref "/posts/poner-paginas-man-en-espanol-en-linux" >}}) de su distribución.

## ALGUNOS EJEMPLOS SIMPLES DEL USO DE NALA

Algunos ejemplos simples que muestran el uso de este gestor de paquetes son los siguientes.

### Instalar un paquete con Nala

Para instalar un paquete tan solo hay que ejecutar un comando del siguiente tipo.

> ```shell
> sudo nala install nombre_del_paquete
> ```

Por lo tanto para instalar htop tan solo tendrán que ejecutar el siguiente comando:

> ```shell
> sudo nala install htop
> ```

### Ver el historial de acciones realizadas con el gestor de paquetes Nala

Si ahora queremos consultar la totalidad de operaciones realizadas con el gestor de paquetes Nala tan solo hay que ejecutar el siguiente comando:

```shell
joan@debian:~$ nala history
  ID    Command         Date and Time               Altered    Requested-By  
  1     install htop    2023-06-04 12:37:19 CEST          1    joan (1000)  
```

Como pueden ver en la salida del comando muestra que a la 12:37 instalé el monitor htop en el sistema operativo.

### Deshacer una acción realizada con nada usando el Historial

Si os fijáis en el apartado anterior veréis que cada una de las acciones realizadas con Nala tiene un ID. La acción que muestra la instalación de htop tiene el ID 1. Por lo tanto si quiero deshacer la acción 1 y por lo tanto desinstalar htop deberé ejecutar el siguiente comando:

```shell
joan@debian:~$ sudo nala history undo 1
================================================================================================================================
Desinstalando         
....
Finalizado exitosamente
```

Si quieren realizar otras acciones, les recomiendo que miren en la tabla de equivalencias de este artículo. Allí podrán ver de forma esquemática cómo realizar lo que necesitan. Espero que esto te ayude.

Para finalizar el artículo os animo a darle una oportunidad a Nala. Si lo usan verán que solucionará parte de los inconvenientes que tiene el gestor de paquetes apt. En el caso que no les convenza siempre podrán desinstalarlo con el siguiente comando:

> ```shell
> sudo apt remove --purge nala
> ```

### Fuentes

[https://gitlab.com/volian/nal](https://gitlab.com/volian/nala)

[https://gitlab.com/volian/nal/-/wikis/Installation](https://gitlab.com/volian/nala/-/wikis/Installation)
