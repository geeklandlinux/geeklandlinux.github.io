---
title: "Motivos por los que recomiendo usar la distribución Debian en Linux"
date: 2016-08-13
categories: 
  - "linux-2"
tags: 
  - "debian"
  - "distribuciones"
coverImage: "Motivos-por-los-recomiendo-usar-Debian-en-Linux.png"
---

Llevo más de 5 años usando Debian y la verdad es que estoy más que satisfecho. De momento no me planteo cambiar de distribución básicamente por los siguientes motivos:<!--more-->

## SUS REPOSITORIOS SON EXCELENTES

Debian dispone de unos repositorios de software excepcionales por los siguientes motivos:

1. La cantidad de programas que podemos encontrar en los repositorios es enorme en cantidad y en calidad.
2. Los paquetes de software de los repositorios reciben un buen mantenimiento por parte de los desarrolladores. No acostumbran a existir problemas de dependencias y en el caso que un paquete presente algún error se acostumbra a solucionar de forma rápida.
3. Los repositorios nos dan la libertad de elegir si queremos disponer de paquetes privativos. Por lo tanto Debian da la posibilidad a los que son puristas de tener un sistema operativo 100% libre.
4. El gestor de paquetes proporciona herramientas para poder [mezclar paquetes de distintas ramas]({{< relref "/posts/apt-pinning-en-debian" >}}). Esto ofrece la posibilidad de descargar multitud de paquetes y programas de un solo sitio centralizado.
5. Hay drivers libres y privativos para prácticamente la totalidad de tarjetas gráficas y el resto de Hardware de nuestro ordenador.

## LA METODOLOGIA DE DESARROLLO SEGUIDA POR DEBIAN

Me gusta la forma que sigue Debian para desarrollar y hacer evolucionar sus versiones.

Su metodología para desarrollar la distribución es la gran responsable de la estabilidad que proporciona este sistema operativo. A grandes rasgos la metodología de desarrollo de Debian es la siguiente:

- Inicialmente los paquetes más nuevos se suben a las ramas inestable y experimental.
- A medida que los paquetes de la rama inestable van siendo testeados por los desarrolladores y por los usuarios se corrigen errores y se van subiendo a la rama testing.
- En un determinado momento se decide que la rama testing de Debian entra en fase de congelación. En este momento no entrarán nuevos paquetes en testing y los desarrolladores se focalizarán exclusivamente en corregir errores y hacer que el sistema sea estable.
- Cuando todos los bugs son cerrados y el sistema se considera estable la Rama Debian testing se convierte en una nueva versión de Debian estable y en la rama testing volverán a entrar nuevos paquetes.

### Consecuencias de la metodología de desarrollo

Esta forma de desarrollar el sistema operativo consigue proporcionar una distribución Linux estable y adecuada a diversos tipos de usuarios.

Cada una de las 3 ramas de Debian es adecuada para una tipología de usuario distinto. Así de este modo:

**Debian Estable (Stable):** Es la rama adecuada para usuarios que necesitan montar y gestionar un servidor en producción. El perfil base de un usuario de Debian estable es el siguiente:

1. Usuarios que utilizan el ordenador en un entorno profesional.
2. Personas que prefieren la estabilidad y la solidez antes que disponer de las versiones más actuales de Software.
3. Usuarios que quieren un sistema operativo que les de cero problemas.

**Debian Testing:** Rama ideal para ordenadores de escritorio y para usuarios comunes que tengan el siguiente tipo de perfil:

1. Usuarios que utilizan el ordenador para trabajar o consumir contenido.
2. Las personas que son exigentes en usar versiones actuales de programas.
3. Personas a los que no les importa resolver incidencias que se puedan dar de forma muy puntual.

**Debian Inestable (Unstable):** Rama perfecta para los usuarios a los que le gusta experimentar y jugar con el sistema operativo. Los usuarios de Debian inestable acostumbran a tener el siguiente perfil:

1. Usuarios que son desarrolladores y/o con un perfil técnico.
2. Personas a los que les guste experimentar con el sistema operativo.
3. Usuarios a los que les gusta aportar su grano de arena para hacer evolucionar el sistema operativo.
4. Usuarios que no les importa resolver incidencias/problemas que se dan en el sistema operativo.

###### Nota: A lo largo de este apartado se han mencionado distintas ramas de Debian. Aparte de las ramas mencionadas existe la rama Old Stable que es la encargada de dar soporte a las versiones estables que no son la versión más actual.

## ESTABILIDAD Y SEGURIDAD

La versión estable de Debian proporciona una gran estabilidad, pero las otras ramas de Debian no se quedan atrás.

He estado durante más de 5 años usando Debian testing y puedo afirmar que no he tenido prácticamente ninguna incidencia relacionada con el sistema operativo. Además las pocas incidencias que he tenido se han solucionado de forma relativamente fácil.

Debian es un sistema seguro ya que en el caso de existir vulnerabilidades de seguridad son solucionadas de forma rápida por sus desarrolladores.

## SOPORTE EXISTENTE PARA LA DISTRIBUCIÓN

En el caso de tener problemas se pueden solucionar de forma rápida y efectiva por los siguientes motivos:

1. Es un sistema operativo universal y detrás existe una gran comunidad de personas dispuestas a ayudar a sus usuarios.
2. Existen foros en Inglés y en Español capaces de dar respuesta y guiar a los usuarios en caso que tengan dudas y/o problemas. Un foro en español que bajo mi criterio es digno de destacar por su calidad es [Exdebian](http://exdebian.org/ "Link de un foro de soporte de Debian en Español").
3. Ubuntu se basa en Debian. Por lo tanto en los foros de ayuda de Ubuntu también podremos encontrar ayuda y soporte para solucionar los problemas que se pueden dar en el día a día.

## OFRECE MÚLTIPLES POSIBILIDADES DE INSTALACIÓN Y USO

Ofrece múltiples posibilidades de instalación. Algunas de las posibilidades son las siguientes:

1. Instalación a través de una netinstall.
2. Instalación mediante una imagen ISO completa.
3. Permite ser probada en un LiveCD o en un LiveUSB
4. Existen imágenes de instalación ISO que ofrecen componentes privativos de serie para facilitar la instalación.

En definitiva el resultado de **instalar Debian a partir de una ISO** completa es una distribución que podemos empezar a usar out of the box sin prácticamente realizar nada.

###### Nota: El proceso de instalación a partir de una ISO completa es extremadamente simple ya que su instalador es simple e intuitivo.

En cambio si lo que queremos es una distro a nuestra medida con únicamente los paquetes y funcionalidades que necesitamos podemos realizar una **netinstall**.

En mi caso siempre acostumbro a instalar Debian a través de una netinstall. De esta forma voy instalando los programas y las funcionalidades a medida que las voy necesitando.

Para finalizar solo decir que Debian es compatible con multitud de arquitecturas. Esto permite instalar Debian en muchos dispositivos como por ejemplo un teléfono móvil, una tablet, un ordenador personal, en servidores, en supercomputadoras, etc.

## FILOSOFIA DE LA DISTRIBUCIÓN

Es una distribución comunitaria que no pretende explotar la distribución con fines comerciales, como por ejemplo lo hacen Red Hat y Ubuntu.

Este punto para mi es una garantía para preservar nuestra seguridad y nuestra privacidad.

Quien esté interesado en este punto le invito a leer el [contrato social](https://www.debian.org/social_contract.es.html "Consultar el contrato social de Debian") de Debian.

## LA INSTALO UNA VEZ Y ME OLVIDO PARA SIEMPRE

No tenemos que estar formateando nuestro ordenador cada 6 meses ya que las ramas testing e inestable siguen un modelo cercano al Rolling Release.

Si instalamos Debian Testing o Inestable nunca tendremos que reinstalar nuestro sistema operativo porque los repositorios de Debian se actualizan a diario con nuevo Software. De esta forma cuando actualicemos nuestro sistema operativo iremos recibiendo las actualizaciones pertinentes de forma progresiva y siempre estaremos usando versiones de software actuales.

Los usuarios que decidan usar la versión estable tendrán un soporte de 5 años y en el caso que quieran disponer de versiones de software actuales podrán usar los [repositorios backports]({{< relref "/posts/repositorio-backports-debian-estable" >}}) de Debian.

###### Nota: La intención de este post no es decir que Debian es superior al resto de distribuciones. Solo pretendo describir los aspectos que me gustan y por los cuales uso Debian.
