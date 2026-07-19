---
title: "Gestor de paquetes para Windows para instalar software"
date: 2024-06-29
categories: 
  - "windows"
tags: 
  - "chocolatey"
  - "gestor-de-paquetes"
  - "software"
coverImage: "Gestor-de-paquetes-para-Windows-para-instalar-software.png"
---

En el mundo de los sistemas operativos, la gestión eficiente de software es crucial para mantener nuestro entorno de trabajo optimizado y seguro. Para los usuarios de Windows, Chocolatey se ha establecido como el gestor de paquetes por excelencia. Si alguna vez has soñado con una manera más sencilla y automatizada de instalar, actualizar y gestionar tus aplicaciones, este es tú artículo. <!--more-->

## QUE ES CHOCOLATEY

Chocolatey es un gestor de paquetes para Windows, similar a los gestores de paquetes [apt]({{< relref "/posts/tabla-equivalencias-apt-get-y-apt" >}}), dnf, pacman, rpm, etc que se encuentran instalados en sistemas operativos basados en Unix. Por lo tanto el gestor de paquetes Chocolatey facilita la instalación, actualización y administración de software en el sistema operativo Windows mediante la línea de comandos.

## DE DONDE PROVIENE EL SOFTWARE INSTALADO MEDIANTE CHOCOLATEY

El software instalado a través de Chocolatey proviene de un repositorio central de paquetes mantenido por la comunidad y los mantenedores de Chocolatey.

Los repositorios de Chocolatey no únicamente contienen archivos ejecutables. En muchas ocasiones contienen scripts que lo hacen que es descargar e instalar el software desde sus páginas oficiales.

## ¿SON SEGUROS LOS PAQUETES DESCARGADOS CON CHOCOLATEY?

El software descargado es seguro. La comunidad de Chocolatey implementa las siguientes medidas de seguridad para asegurar la seguridad e integridad de los paquetes descargados:

- La totalidad de paquetes de Chocolatey pasan por un proceso de **revisión manual**. Se revisan la totalidad de scripts para asegurar que no contienen código malicioso y siguen la directrices de calidad de Chocolatey.
- Se utilizan **herramientas automatizadas** para garantizar que no existan problemas de seguridad ni malas prácticas.
- Algunos de los **paquetes están firmados mediante un certificado digital**. Por lo tanto antes de instalar un paquetes intenten visitar su página web y ver si el paquete está firmado digitalmente y marcado como aprovado.
- Los scripts de instalación están disponibles para ser **revisados por los usuarios**. Cualquier persona puede inspeccionar el contenido de cualquier paquete antes de instalarlo utilizando comandos como `choco install <nombre_del_paquete> -dv`

A pesar de la totalidad de mecanismos de seguridad recomiendo realizar auditorías regulares de los paquetes instalados para asegurarse que estén actualizados, libres de vulnerabilidades conocidas y aprobados por parte de la comunidad de Chocolatey.

Para finalizar este apartado comentar que cuando buscamos paquetes mediante el comando `choco search` obtendremos información sobre si el programa a instalar está verificado o no. Por ejemplo si ejecuto el comando `choco search adobe` obtendré el siguiente resultado

> ```powershell
> Chocolatey v2.3.0
> AdobeAIR 51.0.1.3 [Approved] Downloads cached for licensed users
> adobe-connect 2024.4.729 [Approved] - Possibly broken
> adobedigitaleditions 4.5.12 [Approved] Downloads cached for licensed users
> adobe-dnc 16.3.0 [Approved]
> adobereader 2024.2.20759
> ```

Como se puede ver en la gran mayoría de paquetes figura la palabra \[Approved\]. Por lo tanto estos paquetes están verificados.

## COMO INSTALAR CHOCOLATEY EN WINDOWS

Chocolatey es una herramienta de administración de paquetes que debe instalarse manualmente. Para su instalación se tiene que proceder del siguiente modo:

1. Abrir PowerShell con privilegios de administrador: Para ello presionar `Win+x` y seleccionar la opción `Windows PowerShell (Administrador)`
2. Cuando se abra Powershell ejecutar el siguiente comando para configurar la ejecución de scripts:

> ```powershell
> Set-ExecutionPolicy Bypass -Scope Process -Force
> ```

3. Seguidamente ejecutar el siguiente comando para instalar Chocolatey:

> ```powe
> [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.WebClient]::new().DownloadString('https://chocolatey.org/install.ps1') | Invoke-Expression
> ```

4. Una vez instalado el gestor de paquetes ejecutar e comando `choco --version` para asegurar que la instalación se ha realizado correctamente.

> ```powe
> choco --version
> 2.3.0
> ```

## PROGRAMAS QUE PODEMOS INSTALAR CON EL GESTOR DE PAQUETES CHOCOLATEY

Existen más de 10.000 paquetes / programas mantenidos por la comunidad. Estos paquetes se pueden encontrar y consultar en la siguiente ubicación:

[https://community.chocolatey.org/packages/](https://community.chocolatey.org/packages/)

Si quieren comprobar si existe un programa en concreto desde la consola de Windows, como por ejemplo el reproductor de vídeo `mpv`, pueden ejecutar un comando del siguiente tipo:

> ```powershell
> choco search mpv
> ```

El resultado en mi caso ha sido el siguiente:

> ```powershell
> jellyfin-media-player 1.11.1 [Approved] Downloads cached for licensed users
> lively 2.1.0.8 [Approved] Downloads cached for licensed users
> memento 1.2.2 [Approved]
> memento.install 1.2.2 [Approved]
> memento.portable 1.2.2 [Approved]
> mpc-qt 23.12.0 [Approved]
> mpv 2023.12.10 [Approved]
> mpv.install 2023.12.10 [Approved]
> mpv.net 7.1.1 [Approved]
> mpv.portable 2023.12.10 [Approved]
> mpvio 0.38.0 [Approved]
> mpvio.install 0.38.0 [Approved]
> mpvio.portable 0.38.0 [Approved]
> mpvnet.install 5.4.9.20220427 [Approved] Downloads cached for licensed users
> mpvnet.portable 7.1.1 [Approved] Downloads cached for licensed users
> ...
> ```

Por lo tanto el programa mpv existe y lo puedo instalar de la forma que verán en el siguiente apartado. Para ver más opciones para buscar paquetes pueden consultar el siguiente enlace:

[https://docs.chocolatey.org/en-us/choco/commands/search/](https://docs.chocolatey.org/en-us/choco/commands/search/)

## INSTALAR PROGRAMAS CON CHOCOLATEY

Después de consultar el listado de paquetes he decido que instalar el reproductor de vídeo `mpv` mediante chocolatey. Para ello tan solo hay que ejecutar el comando `choco install` seguido del paquete que quiero instalar, que en mi caso es `mpv`.

> ```shell
> choco install mpv -y
> ```

**Nota:** El parámetro `-y` es para confirmar por avanzado la instalación del paquete mpv. Es imprescindible ponerlo en el caso que se genere algún script para automatizar la instalación de software.

La salida de la instalación ha sido la siguiente:

> ```powershell
> C:\WINDOWS\system32>choco install mpv
> Chocolatey v2.3.0
> Installing the following packages:
> mpv
> By installing, you accept licenses for the packages.
> Downloading package from source 'https://chocolatey.org/api/v2/'
> Progress: Downloading mpv.install 2023.12.10... 100%
> 
> mpv.install v2023.12.10 [Approved]
> mpv.install package files install completed. Performing other installation steps.
> The package mpv.install wants to run 'chocolateyinstall.ps1'.
> Note: If you don't run this script, the installation will fail.
> Note: To confirm automatically next time, use '-y' or consider:
> choco feature enable -n allowGlobalConfirmation
> Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): y
> 
> Extracting 64-bit C:\ProgramData\chocolatey\lib\mpv.install\tools\mpv-0.37.0-x86_64_x64.7z to C:\ProgramData\chocolatey\lib\mpv.install\tools...
> C:\ProgramData\chocolatey\lib\mpv.install\tools
> Extracting C:\ProgramData\chocolatey\lib\mpv.install\tools\rossy.zip to C:\ProgramData\chocolatey\lib\mpv.install\tools...
> C:\ProgramData\chocolatey\lib\mpv.install\tools
> 0
> PATH environment variable does not have C:\ProgramData\chocolatey\lib\mpv.install\tools in it. Adding...
> Environment Vars (like PATH) have changed. Close/reopen your shell to
>  see the changes (or in powershell/cmd.exe just type `refreshenv`).
>  The install of mpv.install was successful.
>   Deployed to 'C:\ProgramData\chocolatey\lib\mpv.install\tools'
> Downloading package from source 'https://chocolatey.org/api/v2/'
> Progress: Downloading mpv 2023.12.10... 100%
> 
> mpv v2023.12.10 [Approved]
> mpv package files install completed. Performing other installation steps.
> The package mpv wants to run 'chocolateyinstall.ps1'.
> Note: If you don't run this script, the installation will fail.
> Note: To confirm automatically next time, use '-y' or consider:
> choco feature enable -n allowGlobalConfirmation
> Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): y
> 
>  The install of mpv was successful.
>   Software install location not explicitly set, it could be in package or
>   default install location of installer.
> 
> Chocolatey installed 2/2 packages.
>  See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
> ```

Una vez instalado el programa estará disponible y completamente integrado en nuestro sistema operativo Windows. Para ver más ejemplos pueden consultar el siguiente enlace [https://docs.chocolatey.org/en-us/choco/commands/install/](https://docs.chocolatey.org/en-us/choco/commands/install/)

## LISTAR LA TOTALIDAD DE PROGRAMAS INSTALADOS

Si queremos listar la totalidad de paquetes instalados en nuestro equipo mediante chocolatey tan solo tenemos que ejecutar el comando `choco list`. En mi caso los programas que tengo instalados son:

> ```powershell
> C:\WINDOWS\system32>choco list
> 
> chocolatey 2.3.0
> croc 10.0.7
> mpv 2023.12.10
> mpv.install 2023.12.10
> yt-dlp 2024.5.27
> ```

## LISTAR LA TOTALIDAD DE PAQUETES OBSOLETOS

Para listar la totalidad de paquetes instalados que están desactualizados tan solo tienen que ejecutar el siguiente comando:

> ```powershell
> choco outdated
> ```

En mi caso pueden ver que la totalidad de paquetes están actualizados a la última versión.

> ```powershell
> C:\WINDOWS\system32>choco outdated
> Chocolatey v2.3.0
> Outdated Packages
>  Output is package name | current version | available version | pinned?
> 
> 
> Chocolatey has determined 0 package(s) are outdated.
> ```

## ACTUALIZAR PROGRAMAS CON EL GESTOR DE PAQUETES CHOCOLATEY

Si queremos actualizar la totalidad de programas instalados mediante chocolatey tan solo tenemos que ejecutar el siguiente comando:

> ```powershell
> choco upgrade all -y
> ```

En el caso que únicamente quisiéramos actualizar un paquete en particular ejecutaríamos el comando `choco upgrade` seguido del nombre del paquete que queremos actualizar. Si en mi caso quiero actualizar `mpv` ejecutaré el siguiente comando:

> ```shell
> choco upgrade mpv
> ```

## DESINSTALAR PROGRAMAS CON EL GESTOR DE PAQUETES CHOCOLATEY

Para desinstalar un paquete tan solo tenemos que ejecutar el comando `choco uninstall` seguido del nombre del paquete que queremos desinstalar. Por lo tanto para desinstalar el paquete `mpv` tan solo tenemos que ejecutar el siguiente comando:

> ```powershell
> choco uninstall mpv
> ```

Para desinstalar un paquete y asegurar que se desinstalan la totalidad de dependencias deberán usar el siguiente comando:

> ```powershell
> choco uninstall mpv -y --remove-dependencies
> ```

## VENTAJAS QUE OFRECE INSTALAR PROGRAMAS CON CHOCOLATEY

Las ventajas de usar un gestor de paquetes como chocolatey son diversas. Algunas de ellas son las que cito a continuación:

- Las **actualizaciones son más fáciles** ya que mediante la ejecución de simples comandos puedo mantener mi software actualizado.
- Permite **automatizar la gestión de los programas** instalados en nuestro equipo. Por ejemplo nos permite automatizar la actualización de software en el equipo. Permite generar scripts para automatizar la instalación del software necesario en nuevos equipos. Además se integra con herramientas de automatización como Ansible, Puppet y Chef.
- Puedo gestionar la **actualización y desinstalación de programas de forma centralizada**.
- **Dispone de más de 10.000 programas listos** para su instalación. Por lo tanto podremos instalar software de forma muy sencilla que de otra manera no podríamos.
- Los repositorios de Chocolatey disponen de programas que de otra forma serian difíciles de localizar e instalar.

## ALTERNATIVAS AL GESTOR DE PAQUETES CHOCOLATEY

En Windows existen diversas alternativas a Chocolatey. Algunas de ellas son las siguientes:

1. **Winget:** Gestor de paquetes desarrollados por Microsoft y por lo tanto se integra bien con el sistema operativo Windows. Es una opción muy a tener en cuenta para la gran mayoría de usuarios domésticos. El catálogo de aplicaciones disponibles para su instalación es grande.
2. **Scoop:** Enfocado e ideal para los desarrolladores de software y usuarios que les guste instalar aplicaciones de línea de comandos. Este repositorio también puede ser del agrado para los desarrolladores de código abierto.
3. **Ninite:** Encontraremos utilidades y software relacionados con la productividad. La cantidad de programas que podemos instalar con este gestor de paquetes es bastante reducida.
4. **NuGet:** Enfocado a desarrolladores de proyectos .NET. Allí podrán encontrar un extenso repositorio de bibliotecas y herramientas para el desarrollo con .NET

En mi caso uso Chocolatey porque tiene una comunidad amplia, un desarrollo activo y además es el gestor de paquetes que dispone de un catálogo más amplio. Otras opciones que usuarios normales pueden considerar es Winget. Winget está desarrollado por microsoft y también tiene un catálogo amplio de aplicaciones que va creciendo día a día.

## CONCLUSIONES

Chocolatey es una herramienta esencial para cualquier usuario de Windows que busque simplificar y automatizar la gestión de software. Con su amplio catálogo de paquetes, fácil integración con herramientas de automatización y una comunidad activa, es la opción ideal para usuarios avanzados y administradores de sistemas.

Si quieren profundizar más sobre el uso de Chocolatey pueden consultar:

[https://docs.chocolatey.org/en-us/choco/commands/](https://docs.chocolatey.org/en-us/choco/commands/)

Por otra parte si quieren usar Chocolatey mediante una interfaz gráfica pueden seguir las instrucciones del siguiente enlace:

[https://docs.chocolatey.org/en-us/chocolatey-gui/setup/installation/](https://docs.chocolatey.org/en-us/chocolatey-gui/setup/installation/)
