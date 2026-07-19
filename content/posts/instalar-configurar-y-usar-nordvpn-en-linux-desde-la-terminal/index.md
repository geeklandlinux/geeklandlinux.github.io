---
title: "Instalar, configurar y usar NordVPN en Linux desde la terminal"
date: 2021-01-08
categories: 
  - "linux-2"
  - "raspberry-pi"
tags: 
  - "nordvpn"
  - "raspbian"
  - "seguridad"
  - "terminal"
cover:
  image: "images/Configurar-NordVPN-en-Linux-o-en-una-Raspberry-Pi-desde-la-terminal.png"
  relative: true
---

Por la red he visto algunos tutoriales de como instalar, configurar y usar NordVPN en la terminal de Linux. La gran mayoría de ellos no era correcto o no aportaba la información necesaria. Por este motivo a continuación detallaré el procedimiento para instalar, configurar y sacar el máximo partido a NordVPN en Linux.<!--more-->

## INSTALAR EL CLIENTE DE NORDVPN EN LINUX, UNA RASPBERRY PI O CUALQUIER DISTRIBUCIÓN QUE USE PAQUETERIA .DEB

NordVPN ofrece un script de instalación. Para descargar y ejecutar el script en cualquier distribución que use paquetes .deb tan solo tenemos que ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ sh <(curl -sSf https://downloads.nordcdn.com/apps/linux/install.sh)
> /usr/bin/apt-get
> Leyendo lista de paquetes... Hecho
> Creando árbol de dependencias       
> Leyendo la información de estado... Hecho
> apt-transport-https ya está en su versión más reciente (1.8.2.2).
> 0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
> /usr/bin/wget
> deb https://repo.nordvpn.com//deb/nordvpn/debian stable main
> Obj:1 https://download.docker.com/linux/raspbian buster InRelease
> Obj:2 http://archive.raspberrypi.org/debian buster InRelease                    
> ...
> El paquete indicado a continuación se instaló de forma automática y ya no es necesario.
>   libpng-tools
> Utilice «sudo apt autoremove» para eliminarlo.
> Se instalarán los siguientes paquetes adicionales:
>   ipset libipset11 libxslt1.1 xsltproc
> Se instalarán los siguientes paquetes NUEVOS:
>   ipset libipset11 libxslt1.1 nordvpn xsltproc
> 0 actualizados, 5 nuevos se instalarán, 0 para eliminar y 3 no actualizados.
> Se necesita descargar 9.607 kB de archivos.
> Se utilizarán 34,5 MB de espacio de disco adicional después de esta operación.
> ¿Desea continuar? [S/n] S
> ....
> Configurando xsltproc (1.1.32-2.2~deb10u1) ...
> Configurando nordvpn (3.8.9) ...
> NordVPN for Linux successfully installed!
> To get started, please re-login or execute `su - $USER` in the current shell, type 'nordvpn login' and enter your NordVPN account details. Then type 'nordvpn connect' and you’re all set! To allow other users to use the application run 'usermod -aG nordvpn otheruser'. If you need help using the app, use the command 'nordvpn --help'.
> Procesando disparadores para systemd (241-7~deb10u5+rpi1) ...
> Procesando disparadores para man-db (2.8.5-2) ...
> ```

Así de simple. A partir de estos momentos NordVPN ya está instalado en nuestro equipo con Linux o en nuestra Raspberry Pi. No hay que realizar ningún invento más para que NordVPN pueda funcionar en Linux. A partir de estos momentos el usuario que haya instalado el servicio VPN podrá usarlo sin ningún tipo de problema. Si quieran dar permisos a otros usuarios deberán ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ usermod -aG nordvpn nombre_usuario
> ```

Además podéis comprobar que NordVPN está levantado ejecutando el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ systemctl status nordvpnd
> ● nordvpnd.service - NordVPN Daemon
>    Loaded: loaded (/usr/lib/systemd/system/nordvpnd.service; enabled; vendor preset: enabled)
>    Active: active (running) since Mon 2021-01-04 15:12:23 CET; 2 days ago
>  Main PID: 558 (nordvpnd)
>     Tasks: 24 (limit: 2063)
>    CGroup: /system.slice/nordvpnd.service
>            └─558 /usr/sbin/nordvpnd
> ```

## LOGUEARSE AL SERVICIO DE NORDVPN EN LINUX

Lo primero que tenemos que hacer después de finalizar la instalación es loguearnos a nuestra cuenta. Para ello ejecutaremos el comando `nordvpn login` e introduciremos nuestro usuario y contraseña.

> ```shell
> pi@raspberrypi:~ $ nordvpn login
> Please enter your login details.
> Email / Username: geekland@geekland.eu
> Password: tucontraseña
> ```

Una vez se hayan logueado ya pueden iniciar el proceso de configuración y conexión.

## CONFIGURAR NORDVPN PARA SACAR EL MÁXIMO PARTIDO EN LINUX

Antes de iniciar el proceso de configuración es altamente recomendable que lean la ayuda de NordVPN. Para ello procederemos del siguiente modo.

### Consultar la ayuda de NordVPN en Linux

Para consultar el manual de uso y configuración tan solo tienen que ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ man nordvpn
> ```

**Nota**: La ayuda proporcionada verán que es bastante buena. Además incluye ejemplos de uso.

**Nota**: Un comando alternativo al mencionado es `nordvpn --help`

Una vez hayan leído la ayuda pueden iniciar la configuración del siguiente modo.

### Definir una lista blanca de puertos o direcciones IP

En el momento que conectamos nuestro dispositivo a un servidor VPN la conexiones entrantes dejarán de estar permitidas. Por lo tanto si estoy conectado a la Raspberry Pi de forma local vía SSH por el puerto 22 y conecto la Raspberry Pi a un servidor VPN la conexión SSH se cortará. Para evitar este inconveniente tendremos que añadir el puerto 22 a la lista blanca ejecutando el siguiente comando:

> ```shell
> pi@raspberrypi:~/jdownloader $ nordvpn whitelist add port 22
> Port 22 (UDP|TCP) is whitelisted successfully.
> ```

A partir de este momento puedo iniciar una conexión SSH con mi Raspberry Pi a nivel local. En el momento que la Raspberry Pi se conecte a un servidor VPN la conexión SSH no se caerá. Para confirmar que esto es así podemos consultar las reglas que están introducidas en el firewall de nuestro equipo:

> ```shell
> pi@raspberrypi:~ $ sudo iptables -L -v -n
> Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
>  pkts bytes target     prot opt in     out     source               destination         
>     0     0 ACCEPT     udp  -- wlan0  *       0.0.0.0/0            0.0.0.0/0            udp dpt:22
>     0     0 ACCEPT     udp  -- wlan0  *       0.0.0.0/0            0.0.0.0/0            udp spt:22
>     0     0 ACCEPT     udp  -- eth0   *       0.0.0.0/0            0.0.0.0/0            udp dpt:22
>     0     0 ACCEPT     udp  -- eth0   *       0.0.0.0/0            0.0.0.0/0            udp spt:22
>    12   852 ACCEPT     tcp  -- wlan0  *       0.0.0.0/0            0.0.0.0/0            tcp dpt:22
>     0     0 ACCEPT     tcp  -- wlan0  *       0.0.0.0/0            0.0.0.0/0            tcp spt:22
>     0     0 ACCEPT     tcp  -- eth0   *       0.0.0.0/0            0.0.0.0/0            tcp dpt:22
>     0     0 ACCEPT     tcp  -- eth0   *       0.0.0.0/0            0.0.0.0/0            tcp spt:22
> ```

Si observan la salida del comando verán que las entradas a nuestro equipo a través de los puertos 22/UDP y 22/TCP están permitidas a través de las interfaces eth0 y wlan0.

Si ahora quisiéramos poner a la lista blanca una subred determinada ejecutaríamos un comando del siguiente tipo:

> ```shell
> pi@raspberrypi:~ $ nordvpn whitelist add subnet 192.168.0.1/24
> Subnet 192.168.0.1/24 is added to hte whitelist successfully.
> ```

De esta forma todos los equipos pertenecientes a la subred `192.168.1.0.1/24` podrán interactuar con la Raspberry Pi sin ningún tipo de problema.

En el caso que quisiéramos quitar de la lista blanca el puerto 22 y la subred 192.168.0.1/24 deberíamos ejecutar los siguientes comandos.

> ```shell
> pi@raspberrypi:~ $ nordvpn whitelist remove port 22
> Port 22 (UDP|TCP) is removed from the whitelist successfully.
> 
> pi@raspberrypi:~ $ nordvpn whitelist remove subnet 192.168.0.1/24
> Subnet 192.168.0.1/24 is removed from a whitelist successfully.
> ```

### Activar el modo kill Switch

El modo Kill Switch hace que en el momento que se corte de forma accidental la conexión con nuestro servidor VPN no podamos continuar usando Internet. Por lo tanto con Kill switch activado estaremos garantizando que la totalidad de nuestro tráfico irá a través del servidor VPN.

Kill Switch viene desactivado de forma predeterminada. Para habilitarlo tendréis que ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn set killswitch enable
> Kill Switch is set to 'enabled' successfully.
> ```

Si una vez habilitado queremos deshabilitarlo tenemos que ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set killswitch disable
> Kill Switch is set to 'disabled' successfully.
> ```

**Nota**: Kill Switch puede llegar a ser molesto. Si tenéis una mala conexión a Internet es posible que de forma frecuente se caiga la conexión entre su equipo y el servidor VPN. Si los cortes son frecuentes obviamente tener activado el Kill Switch puede llegar a ser una molestia. En mi caso tengo deshabilitada esta opción. Solo recomendaría usarla en el caso que realicen actividades críticas en que lo más importante sea su privacidad.

### Definir si queremos usar el protocolo TCP o UDP

Los servidores VPN pueden trabajar a través del protocolo UDP o TCP. Por temas de rendimiento NordVPN está configurado para trabajar con el protocolo UDP de forma predeterminada. No obstante si quieren pueden usar el protocolo TCP ejecutando el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set protocol tcp
> Protocol is successfully set to 'TCP'.
> ```

Si llega el día que queremos volver a usar el protocolo UDP ejecutaremos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set protocol udp
> Protocol is successfully set to 'UDP'.
> ```

**Nota**: Este apartado que acabamos de ver solo aplica si usamos OpenVPN. En caso de usar NordLynx no aplica este apartado de configuración. En caso de usar OpenVPN recomiendo el uso de UDP.

### Definir DNS personalizados para nuestro servicio VPN

NorVPN trabaja con sus propios DNS de forma predeterminada. Para ver estos DNS en Linux tan solo tienen que ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ cat /etc/resolv.conf
> # Generated by NordVPN
> nameserver 103.86.96.100
> nameserver 103.86.99.100
> ```

Si quieren usar otros DNS como por ejemplos los 1.1.1.1 y 8.8.8.8 tan solo tendrán que ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set dns 1.1.1.1 8.8.8.8
> DNS is set to '1.1.1.1, 8.8.8.8' successfully.
> ```

Para comprobar que los nuevos DNS están correctamente configurados podemos realizar la misma comprobación que hicimos al inicio de este apartado:

> ```shell
> pi@raspberrypi:~ $ cat /etc/resolv.conf
> # Generated by NordVPN
> nameserver 1.1.1.1
> nameserver 8.8.8.8
> ```

Si llega el día que quieren volver a usar los servidores DNS predeterminados de NordVPN ejecuten el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $  nordvpn set dns disable
> DNS is set to 'disabled' successfully
> ```

### Habilitar o deshabilitar CyberSec en NordVPN usando Linux desde la terminal

CyberSec es el encargado de realizar un filtrado de direcciones IP. Es como un [Pi-hole]({{< relref "/posts/instalar-configurar-pi-hole-raspberry-pi" >}}) gestionado por NordVPN. Con esta característica activada conseguiremos lo siguiente:

1. Bloquear peticiones maliciosas que podamos hacer por accidente. De esta forma se incrementará nuestra seguridad al navegar.
2. Bloquear la publicidad.

**Nota:** El NordVPN descargado de la Google Play Store de Android no bloquea la publicidad. Si en Android quieren bloquear la publicidad mediante CyberSec deberán [descargar e instalar el archivo .apk](https://nordvpn.com/es/download/android/ "URL para descargar la aplicación de NordVPN en Android") directamente desde la web de NordVPN.

Para activar CyberSec deberán ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn set cybersec enable
> CyberSec is set to 'enabled' successfully.
> ```

Si una vez activado lo quieren desactivar deberán ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set cybersec disable
> CyberSec is set to 'disabled' successfully.
> ```

**Nota**: Es opción vuestra si queréis activar o no esta función. En mi caso la uso y la encuentro útil para bloquear la publicidad.

### Habilitar Autoconnect para estar siempre conectados a un servidor VPN de forma automática

Autoconnect se encargará que cada vez que cada vez que nuestro equipo se conecte a Internet lo haga estando conectado a un servidor VPN. Por lo tanto en el momento que nuestro equipo se conecte a Internet NordVPN se conectará a un servidor VPN de forma completamente automática.

Para habilitar está característica tenemos que ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set autoconnect enable
> Auto-connect is set to 'enabled' successfully.
> ```

Si además quieren conectarse a un servidor VPN en particular como por ejemplo uno que esté ubicado en la ciudad de Dallas de Estados Unidos deberán ejecutar el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn set autoconnect enable United_States Dallas
> Auto-connect is set to 'enabled' successfully.
> ```

**Nota**: En apartados posteriores encontrarán como listar todos los países y ciudades disponibles.

Para deshabilitar Autoconnect deberán ejecutar el siguiente comando.

> ```shell
> pi@raspberrypi:~ $ nordvpn set autoconnect disable
> Auto-connect is set to 'disabled' successfully.
> ```

### Usar servidores ofuscados

En países que existe una fuerte censura, como por ejemplo China, es altamente probable que la gran mayoría de servidores VPN no funcionen. En estos países tendremos que usar servidores VPN ofuscados. Los servidores ofuscados son aquellos que están optimizados para evitar la censura que aplican algunos gobiernos.

Para habilitar los servidores ofuscados tendrán que ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn set obfuscate enable
> Obfuscation is successfully set to 'enabled'
> ```

Una vez habilitados nos podemos conectar a un servidor VPN cualquiera. Así por ejemplo si ejecutamos el comando `nordvpn connect` nos conectaremos al servidor VPN ofuscado que NordVPN considere apropiado.

> ```shell
> pi@raspberrypi:~ $ nordvpn connect
> Connecting to Germany #1014 (de1014.nordvpn.com)
> You are connected to Germany #1014 (de1014.nordvpn.com)!
> ```

Para deshabilitar los servidores ofuscados ejecutaremos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn set obfuscate disable
> Obfuscation is successfully set to 'disabled'.
> ```

**Nota**: No hace falta habilitar este tipo de servidores en países en que no exista censura. Si estáis en Francia no obtendréis ningún beneficio al usar los servidores VPN ofuscados.

**Nota**: Los servidores ofuscados solamente están disponibles para el protocolo OpenVPN.

### Elegir el tipo de VPN al que nos queremos conectar

NordVPN ofrece servidores VPN basados en protocolos OpenVPN y Wireguard (NordLynx). De forma predeterminada se usará OpenVPN, pero en mi caso prefiero y recomiendo usar NordLynx. La velocidad de respuesta es más rápida, la conexión es más estable y es un protocolo mucho más moderno que OpenVPN.

Para habilitar NordLynx tan solo tendréis que ejecutar el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn set technology NordLynx
> Technology is successfully set to 'NordLynx'.
> ```

Si ahora nos conectamos a un servicio VPN cualquiera del siguiente modo:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect
> Connecting to Spain #146 (es146.nordvpn.com)
> You are connected to Spain #146 (es146.nordvpn.com)!
> ```

Podremos constatar que efectivamente estamos usando el protocolo NordLynx.

> ```shell
> pi@raspberrypi:~ $ nordvpn status
> Status: Connected
> Current server: es146.nordvpn.com
> Country: Spain
> City: Madrid
> Your new IP: 195.181.167.166
> Current technology: NordLynx
> Transfer: 0 B received, 2.83 KiB sent
> Uptime: 8 seconds
> ```

Si por el motivo que sea queremos volver a usar OpenVPN ejecutaremos el siguiente comando en la terminal.

> ```shell
> pi@raspberrypi:~ $ nordvpn set technology OpenVPN
> Technology is successfully set to 'OpenVPN'.
> ```

## COMO CONECTARSE A UN SERVIDOR VPN DE NORDVPN EN LINUX

Una vez finalizada la configuración ya podemos empezar a usar NordVPN. Para ello iniciaremos con la forma más simple de conectarnos a un servidor VPN.

### Conectarnos a un servidor VPN de forma aleatoria

Para conectarnos a un servidor VPN aleatorio tan solo tenemos que usar el comando `nordvpn connect`:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect
> Connecting to Spain #174 (es174.nordvpn.com)
> You are connected to Spain #174 (es174.nordvpn.com)!
> ```

**Nota**: Con este comando tan simple NordVPN nos conecta al servidor VPN que piensa que nos dará mayor rendimiento.

Una vez conectados al servidor podemos ejecutar el comando `nordvpn status` para visualizar datos de nuestra conexión.

> ```shell
> pi@raspberrypi:~ $ nordvpn status
> Status: Connected
> Current server: es174.nordvpn.com
> Country: Spain
> City: Madrid
> Your new IP: 37.122.192.132
> Current technology: OpenVPN
> Current protocol: UDP
> Transfer: 6.07 GiB received, 4.85 GiB sent
> Uptime: 13 hours 13 minutes
> ```

Si además de los datos de la conexión quieren visualizar la configuración que tenemos aplicada pueden ejecutar el comando `nordvpn settings`:

> ```shell
> pi@raspberrypi:~ $ nordvpn settings
> Technology: OpenVPN
> Protocol: UDP
> Kill Switch: disabled
> CyberSec: disabled
> Obfuscate: disabled
> Notify: enabled
> Auto-connect: disabled
> DNS: disabled
> Whitelisted ports:
>        22 (UDP|TCP)
> ```

### Desconectarnos del servidos al que estamos conectados

Si ahora queremos desconectarnos del servidor VPN al que acabamos de conectarnos tan solo tenemos que usar el comando `nordvpn disconnect`.

> ```shell
> pi@raspberrypi:~ $ nordvpn disconnect
> You have been disconnected from NordVPN.
> ```

Para asegurar que realmente están desconectados pueden ejecutar el comando `nordvpn status`.

> ```shell
> pi@raspberrypi:~ $ nordvpn status
> Status: Disconnected
> ```

### Conectarnos a un servidor VPN de un determinado país y ciudad

Para ver la totalidad de países en los que NordVPN tiene servidores ejecutaremos el comando `nordvpn countries`:

> ```shell
> pi@raspberrypi:~ $ nordvpn countries
> Albania			Greece			Portugal
> Argentina		Hong_Kong		Romania
> Australia		Hungary			Serbia
> Austria			Iceland			Singapore
> Belgium			India			Slovakia
> Bosnia_And_Herzegovina	Indonesia		Slovenia
> Brazil			Ireland			South_Africa
> Bulgaria		Israel			South_Korea
> Canada			Italy			Spain
> Chile			Japan			Sweden
> Costa_Rica		Latvia			Switzerland
> Croatia			Luxembourg		Taiwan
> Cyprus			Malaysia		Thailand
> Czech_Republic		Mexico			Turkey
> Denmark			Moldova			Ukraine
> Estonia			Netherlands		United_Kingdom
> Finland			New_Zealand		United_States
> France			North_Macedonia		Vietnam
> Georgia			Norway
> Germany			Poland
> ```

Si queremos conectarnos a un servidor VPN cualquiera de Corea del Sur ejecutaremos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect South_Korea
> Connecting to South Korea #32 (kr32.nordvpn.com)
> You are connected to South Korea #32 (kr32.nordvpn.com)!
> ```

### Conectarnos a un servidor VPN de una determinada ciudad

Si ahora queremos ver las ciudades de Corea del Sur en que NordVPN tiene ubicados servidores para sus clientes ejecutamos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn cities South_Korea
> Seoul
> ```

En este caso NordVPN solo tiene servidores VPN en la ciudad de Seúl. Para especificar que nos queremos conectar a un servidor VPN cualquiera de la ciudad de Seúl en Corea del Sur ejecutaremos el siguiente comando en la terminal:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect South_Korea Seoul
> Connecting to South Korea #36 (kr36.nordvpn.com)
> You are connected to South Korea #36 (kr36.nordvpn.com)!
> ```

### Conectarse a un servidor específico para hacer descargas P2P

NordVPN clasifica los servidores por grupos. Para ver todos los grupos disponibles ejecutamos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn groups
> Africa_The_Middle_East_And_India	Onion_Over_VPN
> Asia_Pacific				P2P
> Dedicated_IP				Standard_VPN_Servers
> Double_VPN				The_Americas
> Europe
> ```

**Nota**: El significado de cada uno de los grupos está aquí los encontraréis en el siguiente [enlace](https://support.nordvpn.com/General-info/Features/1047407962/What-do-the-different-server-categories-mean.htm "Explicación de los distintos tipos de VPN ofrecidos por NordVPN").

Si ahora queremos conectarnos a un servidor VPN cualquiera para poder realizar descargas P2P ejecutaremos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect P2P
> Connecting to Spain #152 (es152.nordvpn.com)
> You are connected to Spain #152 (es152.nordvpn.com)!
> ```

Si queremos que se realicen las descargas P2P a través de un servidor ubicado por ejemplo en Suiza ejecutaremos el siguiente comando:

> ```shell
> pi@raspberrypi:~ $ nordvpn connect --group P2P Switzerland
> Connecting to Switzerland #277 (ch277.nordvpn.com)
> You are connected to Switzerland #277 (ch277.nordvpn.com)!
> ```

De este modo podremos realizar descargas P2P sin que nadies pueda conocer nuestra IP real.

## DESOLOGUEARSE COMPLETAMENTE DE LA CUENTA

Si quieren desloguearse de la cuenta de NordVPN tan solo deberán ejecutar el comando `nordvpn logout`:

> ```shell
> pi@raspberrypi:~ $ nordvpn logout
> You are logged out.
> ```

De esta forma simple e intuitiva podréis sacar el máximo partido a NordVPN desde la terminal.

#### Fuentes

[https://blog.sleeplessbeastie.eu/2019/02/04/how-to-use-nordvpn-command-line-utility/](https://blog.sleeplessbeastie.eu/2019/02/04/how-to-use-nordvpn-command-line-utility/) [https://wiki.archlinux.org/index.php/NordVPN\_(Espa%C3%B1ol)](https://wiki.archlinux.org/index.php/NordVPN_\(Espa%C3%B1ol\))
