---
title: "Cómo configurar una IP estática en Raspberry Pi con nmcli"
date: 2024-02-24
categories: 
  - "linux-2"
  - "raspberry-pi"
  - "redes"
tags: 
  - "networkmanager"
  - "nmcli"
  - "raspberry-pi"
coverImage: "ip-estatica-nmcli-raspberry-pi.png"
---

En este artículo, aprenderás a configurar una dirección IP estática en tu Raspberry Pi OS Bookworm usando Network Manager. A partir de la versión Raspberry Pi OS 12 (bookworm) se reemplazo dhcpcd y wpa\_supplicant por Network Manager. Esto implica que el proceso de para que nuestra Raspberry Pi tenga una IP interna fija es totalmente [diferente a como lo hacíamos con anterioridad]({{< relref "/posts/configurar-ip-fija_o_estatica_ipv4" >}}). El proceso a seguir es el siguiente.<!--more-->

## CONFIGURAR UNA IP ESTÁTICA EN RASPBERRY PI OS BOOKWORM Y NETWORK MANAGER

Los pasos a seguir para conseguir una IP estática en el sistema operativo Raspberry Pi OS 12 mediante Network Manager y nmcli es el que citaremos a continuación

### DEFINIR LOS PARÁMETROS DE CONFIGURACIÓN DE NUESTRA CONEXIÓN DE RED CON IP ESTÁTICA

En mi caso defino que los parámetros de configuración de red tienen que ser los siguientes:

| Parámetros | Valores |
| --- | --- |
| Dirección IP | 192.168.1.100 |
| Puerta de enlace | 192.168.1.1 |
| Máscara de subred | /24 |
| Servidores DNS | 8.8.8.8 y 8.8.4.4 |
| Tipo de conexión | Vía Ethernet |

### IDENTIFICAR LOS NOMBRES DE LAS INTERFACES DE RED

Para identificar el nombre de nuestras interfaces de red ejecutaremos el siguiente comando en la terminal:

> **`pi@raspberrypi:~ $ sudo nmcli -p connection show`**

El resultado debería ser similar al siguiente:

> ```shell
> pi@raspberrypi:~ $ sudo nmcli -p connection show
> ============================================
>   Perfiles de conexiones de NetworkManager
> ============================================
> NAME                 UUID                                  TYPE      DEVICE          
> ----------------------------------------------------------------------------------------------------------------------------------
> Conexión cableada 1  79297c53-82ae-393c-ba8b-739c146a8a4e  ethernet  end0
> lo                   a7ba3035-f697-40a6-9502-be312b7732cd  loopback  lo
> preconfigured        80004d04-a3c2-4910-8a3f-6797e4f96a67  wifi      wlan0
> docker0              73c85449-57b4-4337-aba0-50decfbda887  bridge    docker0
> ```

Si observamos los resultados de la salida, la interfaz de red Ethernet se llama:

- `Conexión cableada 1`

**Nota:** En el hipotético caso que también quisiéramos configurar la IP de nuestra conexión de wifi vemos que su nombre es `preconfigured`

### EJECUTAR LOS COMANDOS PARA CONFIGURAR NUESTRA CONEXIÓN CON IP ESTÁTICA

Hasta el momento que hemos identificado la totalidad de parámetros necesarios:

| Parámetros | Valores |
| --- | --- |
| Dirección IP | 192.168.1.100 |
| Puerta de enlace | 192.168.1.1 |
| Mascara de subred | /24 |
| Servidores DNS | 8.8.8.8 y 8.8.4.4 |
| Nombre de la interfaz de red | Conexión cableada 1 |

Ahora ejecutaremos los siguientes comandos para configurar nuestra conexión a Internet:

#### Configurar la IP Fija IPv4 o IPv6

Si estamos usando IPv4:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv4.addresses 192.168.1.100/24 ipv4.method manual
> ```

Si en vez de una dirección IPv4 quisiéramos establecer una dirección IPv6 deberíamos ejecutar el siguiente comando:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv6.addresses tu_dirección_ipv6 ipv6.method manual
> ```

En el caso hipotético que quisiéramos crear una VLAN (Virtual Local Area Network) con las IP, 192.168.1.101 y 192.168.1.102/24 deberíamos ejecutar el comando:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv4.addresses "192.168.1.100/24, 192.168.1.101/24, 192.168.1.102/24" ipv4.method manual
> ```

#### Configurar la puerta de enlace en IPv4 o IPv6

Si estamos usando IPv4:

> **`sudo nmcli con mod "Conexión cableada 1" ipv4.gateway 192.168.1.1`**

Para configurar la puerta de entrada en el caso que estemos usando una IPv6 ejecutaríamos el siguiente comando:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv6.gateway tu_puerta_de_entrada_ipv6
> ```

#### Configurar los servidores DNS en IPv4 o IPv6

Si estamos usando IPv4:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv4.dns "8.8.8.8,8.8.4.4"
> ```

En el caso que usemos IPv6 entonces el comando se deberá modificar de la siguiente forma:

> ```shell
> sudo nmcli con mod "Conexión cableada 1" ipv6.dns "tu_servidor_dns_ipv6"
> ```

### REINICIAR EL GESTOR DE RED NETWORK MANAGER

Para que los cambios surtan efecto, reiniciaremos Network Manager ejecutando el siguiente comando:

> ```shell
> sudo nmcli con down "Conexión cableada 1" && sudo nmcli con up "Conexión cableada 1"
> ```

**Nota:** Otra opción disponible es reiniciar la Raspberry Pi.

### VERIFICAR LA CONFIGURACIÓN DE LA CONEXIÓN CON IP ESTÁTICA

Una vez finalizado el proceso podemos consultar la configuración de nuestra conexión Ethernet ejecutando el siguiente comando:

> ```shell
> nmcli -p connection show "Conexión cableada 1"
> ```

y el resultado obtenido será parecido al siguiente:

> `**pi@raspberrypi:~ $ nmcli -p connection show "Conexión cableada 1" ===============================================================================             Detalles del perfil de conexiones (Conexión cableada 1) =============================================================================== connection.id:                          Conexión cableada 1 connection.uuid:                        79297c53-82ae-393c-ba8b-739c146a8a4e connection.stable-id:                   -- connection.type:                        802-3-ethernet connection.interface-name:              end0 connection.autoconnect:                 sí connection.autoconnect-priority:        -999** ....`

## OTROS TAREAS QUE SE PUEDE REALIZAR CON NMCLI

A parte de lo mencionado a lo largo del artículo existen otras operaciones que podemos realizar con nmcli. Por ejemplo:

### ASIGNACIÓN DE IP DINÁMICA EN VEZ DE IP ESTÁTICA

Si en vez de una ip estática queremos una IPv4 dinámica ejecutaremos el siguiente comando:

> ```shell
> sudo nmcli con modify "Conexión cableada 1" ipv4.method auto
> ```

Y si queremos una dirección IPv6 dinámica:

> ```shell
> sudo nmcli con modify "Conexión cableada 1" ipv6.method auto
> ```

Una vez realizados los cambios reiniciaremos NetworkManager para que los cambios surjan efecto mediante el siguiente comando:

> ```shell
> sudo nmcli con down "Conexión cableada 1" && sudo nmcli con up "Conexión cableada 1"
> ```

### EVITAR LA CONEXIÓN AUTOMÁTICA DE ALGUNA DE LAS INTERFACES DE RED

En mi caso cada vez que arranco la Raspberry Pi se me conecta tanto la red Ethernet como a la Wifi. Para evitar que se conecte a la red wifi procederemos del siguiente modo.

En apartados anteriores vemos que la red Wifi se reconoce como `preconfigured`. Por lo tanto para evitar que la Raspberry Pi se conecte a la red Wifi de forma automática cada vez que la arranquemos ejecutaremos el siguiente comando en la terminal:

> ```shell
> sudo nmcli con modify "preconfigured" connection.autoconnect no
> ```

### Fuentes

[https://access.redhat.com/documentation/es-es/red\_hat\_enterprise\_linux/8/html/configuring\_basic\_system\_settings/configuring-a-static-ethernet-connection-using-nmcli\_configuring-and-managing-network-access](https://access.redhat.com/documentation/es-es/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/configuring-a-static-ethernet-connection-using-nmcli_configuring-and-managing-network-access)
