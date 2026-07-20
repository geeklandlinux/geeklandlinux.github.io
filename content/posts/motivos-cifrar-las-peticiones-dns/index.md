---
title: "Motivos para cifrar las peticiones DNS"
date: 2016-07-02
categories: 
  - "seguridad-informatica"
tags: 
  - "cifrado"
  - "dns"
  - "dnscrypt"
  - "privacidad"
cover:
  image: "images/motivos-para-cifrar-nuestras-peticiones-dns.webp"
  relative: true
---

Con el fin de preservar nuestra privacidad en este post detallaré los principales motivos para cifrar las peticiones DNS. Este post forma parte de una serie de 3 post en los que explicaré detalladamente los siguientes aspectos:<!--more-->

1. **Los motivos existentes para cifrar las peticiones DNS**
2. [El procedimiento a seguir para instalar DNSCrypt en Linux y en Windows]({{< relref "/posts/instalar-dnscrypt-linux-ubuntu-windows" >}}).
3. [Pasos a seguir para configurar, usar y optimizar DNSCrypt en Linux]({{< relref "/posts/configurar-dnscrypt-en-gnu-linux" >}}).
4. [Configurar y usar DNSCrypt en Windows.]({{< relref "/posts/configurar-dnscrypt-en-windows" >}})
5. [Usar DNSmasq para mejorar el rendimiento de DNSCrypt en Linux]({{< relref "/posts/usar-dnsmasq-mejorar-rendimiento-dnscrypt" >}})

Como he detallado anteriormente, en el primero de los post comprenderemos los motivos que hacen que sea importante cifrar las peticiones DNS, pero antes de entrar en materia tenemos que entender lo que son los servidores DNS y que es una petición DNS.

## FUNCIONAMIENTO DE UN SERVIDOR DNS

En artículos anteriores ya explicamos detalladamente la función que tienen los servidores DNS. Por lo tanto quien quiera consultar detalladamente las funciones de un servidor DNS puede consultar el siguiente [enlace]({{< relref "/posts/elegir-el-mejor-servidor-dns" >}}).

Para quien no quiera profundizar en exceso, o ya sepa la función de un servidor DNS, solo decirle que **un servidor DNS es el encargado de traducir las direcciones URL de las páginas que visitamos a IP y viceversa**. Así por lo tanto cuando queremos visitar la página web https://geeklandlinux.github.io/, nuestro navegador contactará con el servidor DNS para averiguar la IP de la dirección URL [https://geeklandlinux.github.io/](https://geeklandlinux.github.io/ "Web Geekland"). Una vez conocida la IP nos podremos conectar a la página web que queremos visitar.

**Una petición DNS** no **es** más que **la solicitud que enviamos al servidor DNS para que traduzca la dirección URL de la página web que queremos visitar a una dirección IP**.

## MOTIVOS PARA CIFRAR LAS PETICIONES DNS

**La totalidad de peticiones DNS que hacemos son en texto plano y sin cifrar**. Este hecho debería preocuparnos ya que la información que se puede obtener de las peticiones DNS que hacemos es importante. **Algunos ejemplos de la utilidad que tiene cifrar las peticiones DNS son las siguientes**:

### Preservar nuestra privacidad

Nuestros proveedores de internet acostumbran ofrecernos de forma predeterminada sus propios servidores DNS. De esta forma asocian nuestra IP con nuestras peticiones DNS y les resulta sumamente sencillo **saber los sitios web que hemos visitado vulnerando de esta forma nuestra privacidad**. En el momento que nosotros cifremos las peticiones DNS nuestro proveedor de internet tendrá más dificultades en saber las páginas que visitamos.

### Para evitar tener una falsa sensación de seguridad

Varios usuarios intentan proteger su privacidad **mediante el uso de servicios VPN, de servidores proxy, conexiones https**, etc. Lo que no saben estos usuarios es que usando un servidor proxy, conexiones https o un servidor VPN mal configurado, **están realizando la totalidad de sus peticiones DNS sin cifrar**, por lo tanto nuestro ISP o un atacante puede llegar a conocer sin problema las páginas web que hemos visitado y ser víctimas de ataques por parte de hackers o personas mal intencionadas.

### Para evitar ataques informáticos y el robo de información

Mediante nuestras peticiones DNS un atacante puede llegar a realizar un ataque del tipo man in the middle como por ejemplo los siguientes:

**Man in the middle para interceptar nuestro tráfico**: El atacante es capaz de interceptar y capturar la totalidad de nuestro tráfico llegando a conseguir nuestras contraseñas, nuestras cookies, etc.

**Man in the middle para DNS spoofing**: El atacante dirige a las víctimas a páginas web falsas con el fin de robarles información como por ejemplo la contraseña de cuentas bancarias, de nuestro correo electrónico, etc.

### Para evitar la censura de algunos gobiernos

Algunos gobiernos e ISP restringen y controlan el tráfico de Internet mediante las peticiones que sus usuarios realizan a los servidores DNS. Por lo tanto en ciertos casos **cifrando nuestras peticiones DNS es posible evitar la censura que aplican los ISP de determinados países** como por ejemplo Venezuela, Turquía, etc.

## SOLUCIONES DISPONIBLES PARA CIFRAR LAS PETICIONES DNS

Para intentar solucionar los problemas descritos en el apartado anterior **podemos usar DNSCrypt**. DNSCrypt es la única solución de confianza existente en la actualidad que conozco.

## FUNCIONAMIENTO DE DNSCRYPT

Entender el funcionamiento de DNScrypt es simple. **DNScrypt se encarga de cifrar las peticiones DNS desde nuestro ordenador al servidor DNS**. De esta forma podremos evitar ataques Man in the middle, DNS spoofing y que nuestro ISP o un atacante pueda espiar las páginas web que visitamos, ya que aunque intercepten nuestra petición no podrán averiguar su contenido.

Por lo tanto de la misma forma que ciframos las peticiones y tráfico web con https, con DNSCrypt también cifraremos las peticiones que realizamos a los servidores DNS. **El mecanismo de cifrado utilizado por DNSCrypt es muy similar al mecanismo utilizado por https**.

Para finalizar decir que los servidores que usará DNSCrypt para resolver nuestras peticiones DNS son los OpenDNS. Si preferimos usar otros servidores, en futuros post veremos como podemos modificar este aspecto.

## ¿QUIÉN ESTA DETRÁS DE DNSCRYPT?

**DNScrypt es un proyecto desarrollado por** [OpenDNS](https://www.opendns.com/ "Web de OpenDNS"), y que recientemente ha sido adquirido por Cisco. Por lo tanto **bajo mi punto de vista es un proyecto serio y una solución fiable** para solucionar los problemas de privacidad y de seguridad descritos en el post.

No obstante cabe decir que en este punto habrán opiniones diferentes a la mía ya que nuestras peticiones DNS dependerán del servicio de un tercero. No obstante más adelante verán que DNSCrypt permite seleccionar múltiples servidores DNS y algunos de ellos incluso garantizan que no se guardaran logs de nuestras peticiones DNS.

## PLATAFORMAS EN LAS QUE PUEDO USAR DNSCRYPT

DNSCrypt está **disponible para prácticamente la totalidad de sistemas operativos más usados en la actualidad**:

1. Windows
2. Android
3. Linux
4. Mac OS
5. iOS

En la serie de post que realizaré durante las próximas semanas detallaré la instalación y el funcionamiento de DNSCrypt en Linux y en Windows.
