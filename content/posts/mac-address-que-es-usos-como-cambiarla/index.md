---
title: "Mac Address, ¿Qué es? Usos que tiene y motivos para cambiarla"
date: 2014-08-10
categories: 
  - "redes"
tags: 
  - "hardware"
  - "mac-address"
  - "privacidad"
cover:
  image: "images/Mac-Address-¿Qué-es-Qué-usos-tiene-y-motivos-para-cambiarla.jpg"
  relative: true
---

## ¿QUÉ ES LA MAC ADDRESS O LA DIRECCIÓN MAC?

La Mac Address o dirección Mac **es una identificador único de 48 bits para identificar la totalidad de dispositivos de red** como por ejemplo tarjetas de red Ethernet, tarjetas de red wifi o inalambricas, Switch de red, Routers, impresoras, etc.<!--more-->

Las direcciones MAC **son identificadores únicos a nivel mundial para cada dispositivo y por lo tanto es imposible encontrar 2 tarjetas de red o 2 dispositivos de red que tengan la misma Mac Address**.

**La totalidad de fabricantes en el momento de fabricar el hardware**, como por ejemplo una tarjeta de red wifi, **graban la Mac Address en formato binario en una [memoria ROM](https://es.wikipedia.org/wiki/Memoria_de_solo_lectura "Explicación de lo que es una memoria ROM")** del dispositivo que están fabricando. Como la memoria ROM es solo de lectura es totalmente imposible modificarla y **por lo tanto** esto implica que **la Mac address o identificador de un dispositivo nunca lo podremos modificar**. **No obstante** en futuros post veremos que **es posible hacer creer a otras personas o a integrantes de la red que nuestra MAC Address es otra diferente a la real (Termino conocido como [Mac spoofing](https://es.wikipedia.org/wiki/MAC_spoofing "Explicación de lo que es Mac Spoofing"))**.

El motivo por el cual es posible modificar la MAC de la tarjeta de red de nuestro ordenador es simple. Cuando se arranca nuestro ordenador la tarjeta de red copia la dirección MAC a nuestra memoria RAM. Una vez copiada la Mac Address a la memoria RAM la totalidad de veces que se requiere de la Mac Address se usará la Mac Address almacenada en la memoria RAM. Por lo tanto si queremos cambiar nuestra Mac Address tan solo tenemos que modificar la Mac Address almacenada en nuestra memoria RAM y esto si que es posible.

###### Nota: La gran mayoría sabe que en una red informática un equipo se puede identificar por la dirección IP. Pero la realidad es que los equipos de una red informática también se pueden identificar por su Mac Address.

## ESTRUCTURA DE LA DIRECCIÓN MAC O MAC ADDRESS

Obviamente para que estos identificadores, o las Mac Address, sean únicas para cada dispositivo tiene que existir una entidad que regule unas normas de marcado. **La entidad que se encarga de definir como se deben definir las Mac Address es la** [IEEE](https://es.wikipedia.org/wiki/IEEE "Información sobre quien es la IEEE") (Instituto de Ingeniería eléctrica y electrónica)

**Para entender la estructura y la información que contiene la Mac Address usaremos esta Mac Address de muestra**:

**00:17:4F:08:5F:69**

**La Mac Address que acabamos de escribir tiene una longitud de 48 bits representada en 6 bloques hexadecimales**. **Estos bloques hexadecimales se pueden dividir en dos partes**:

******La primera parte****, que es la de color azul (**00:17:4F**)**, consta de 3 bloques hexadecimales **e identifica quien es el fabricante del hardware**. Así por lo tanto si **00:17:4F** correspondiera al código de fabricante de Wistron infocom Manufacturing, podríamos estar seguros que si encontramos otra tarjeta de red con el mismo código el fabricante tendría que ser de Wistron infocom Manufacturing.

**La segunda parte, que es la de color rojo (08:5F:69), también consta de 3 bloques hexadecimales**. **Esta parte del código es el número de serie que identifica el dispositivo fabricado**. El fabricante puede usar el número de serie que quiera pero con la condición que únicamente puede usar el mismo número de serie una vez. El próximo dispositivo que fabrique deberá tener un número de serie diferente a 08:5F:69

Siguiendo estas simples normas la IEEE asegura que cada una de las Mac Address que usan los fabricantes sean únicas a nivel mundial.

## QUE USOS TIENE LA MAC ADDRESS O LA DIRECCIÓN MAC

Una vez conocemos lo que es la Mac Address, ahora citaremos **algunas de las utilidades que puede tener la Mac Address de una tarjeta de red o de cualquier otro dispositivo.**

1. La Mac Address de una tarjeta de red la podemos utilizar **para que nuestro Router asigne una IP estática a nuestro ordenador**. En otras palabras podemos configurar nuestro Router para que en el momento que detecta la presencia de una Mac Address determinada, le asigne la IP que nosotros hayamos definido previamente.
2. La Mac Address puede ser útil **para restringir el acceso a una red informática**. Así por lo tanto podemos entrar en la configuración de nuestro Router y configurarlo de forma que solo se puedan conectar a la red los equipos que tengan una Mac Address determinada. Como mecanismo de seguridad este punto no es muy efectivo ya que en futuros artículos podremos ver lo fácil que es clonar y cambiar una Mac Address de un dispositivo de red.
3. Como hemos dicho anteriormente la dirección Mac puede servir **para identificar a un equipo o usuario dentro de una red informática**. Usando Nmap u otros programas, podemos obtener fácilmente las IP y las Mac Address de los equipos conectados a la red.
4. la Mac Address de un router o de un dispositivo móvil puede servir **para geoposicionar personas y para espiar nuestros movimientos y** **hábitos** con una precisión mucho más alta que un geoposicionamiento por IP. Para quien dude de que esto sea posible aquí dejo este [enlace](http://www.bbc.com/news/technology-23665490 "Noticia BBC en que papeleras registran Mac Address de los vianantes") en el podéis leer que en Londres se instalaron papeleras capaces de registrar la Mac Address de **los dispositivos móviles** de la gente que estaban a sus alrededores. Además **cuando pasa el coche de** **Google por la calles realizando fotografías para generar su maravilloso Street View también están trackeando la totalidad de [BSSID](https://es.wikipedia.org/wiki/BSSID "Explicación de lo que es la BSSID") (Mac Address de los routers) que encuentran a su paso**, **y por si no fuera suficiente cada vez que nos acercamos a un Router y tengamos el wifi del teléfono móvil encendido, estaremos dejando el registro de nuestra Mac Address en el router que nos hayamos acercado, y si además tenemos los servicios de localización del teléfono activados enviaremos la BSSID (Mac Address del punto de acceso) a Google, Microsoft, Apple, etc**. Frente a esta situación y para solucionar algunos de los problemas citados, parece que el sistema operativo iOS 8 incluirá un sistema que generará direcciones Mac falsas de forma aleatoria, pero esto no quiere decir que Apple sea mejor que el resto, ya que aunque hagan esto la totalidad de compañías en el momento de activar los servicios de localización de nuestro teléfono nos tienen mas que controlados.
5. Hay proveedores de Internet que requieren Autenticación MAC para proporcionar internet a sus clientes. Por lo tanto la MAC Address en este caso también sirve **como herramienta de filtro para proporcionar un servicio a los clientes de una compañía**.

Si alguien quiere ampliar este apartado le ruego que comente en los comentarios de este post su observación será introducida introducida en el artículo.

###### Nota: Con todo lo comentado en este apartado seguro que más de una persona se preguntará y reflexionará sobre la privacidad que nos ofrecen los dispositivos móviles. Un dispositivo móvil es el dispositivo perfecto para tenernos a todos perfectamente controlados y espiados.

## ¿MOTIVOS PARA CAMBIAR NUESTRA MAC ADDRESS?

Después de leer el apartado anterior fácilmente podemos deducir que cambiar nuestra MAC Address puede ser útil en multitud de ocasiones. **Algunas de las situaciones en que puede ser útil ocultar o cambiar nuestra Mac Address pueden ser**:

1. **Cuando nos conectamos en sitios públicos**. Si no tomamos precauciones un Hacker podría fácilmente clonar nuestra Mac Address y cometer algún delito, del cual podrían acusarnos ya que habría sido realizado con nuestra Mac Address.
2. Como hemos visto anteriormente es posible geoposicionar dispositivos móviles, ordenadores o routers mediante la Mac Address. Además existen herramientas especialidades de hacking para realizar esta función. Por lo tanto **por motivos de privacidad** también es interesante y recomendable cambiar nuestra Mac Address. Tenemos que recordar que **la Mac Address es un identificador único y hay compañías que tienen extensas bases de datos de Mac Address asociadas con sus correspondientes coordenadas GPS**.
3. Anteriormente hemos visto como **algunos proveedores de Internet solo dan servicio dispositivos de red que hayan sido previamente registrados por su Mac Address**. Por lo tanto **si se nos estropea la tarjeta de red de nuestro ordenador nos quedaremos sin conexión a** **Internet** hasta que nuestro proveedor de Internet registre la dirección Mac de nuestra nueva tarjeta de red. **Como solución provisional podemos cambiar la Mac Address de nuestra nueva tarjeta de red hasta que el ISP registre nuestra nueva Mac Address**.
4. En el caso que alguien decida **realizar algún ataque o simplemente realizar alguna acción sin dejar rastro alguno**, entre muchas otras cosas deberá cambiar la Mac de su tarjeta de Red.
5. **Algunos aeropuertos y sitios públicos suelen ofrecer Internet gratis a sus clientes durante tiempo limitado**. La forma de controlar quien está conectado y el tiempo que está conectado es mediante la Mac Address del dispositivo que se conecta a Internet. Por lo tanto **cuando se termina este tiempo podemos cambiar nuestra MAC Address de nuestro ordenador y volver a disfrutar de Internet Gratis**. En el caso que acabamos de citar también es posible suplantar la Mac Address e IP de alguno de los clientes que haya pagado por el servicio.

Estas son algunas de las razones principales para cambiar la Mac Address de un dispostivo de red. En el caso que alguien tenga motivaciones diferentes a las mencionadas en este apartado le invito a dejar un comentario.

## ¿CÓMO PODEMOS CAMBIAR O OCULTAR NUESTRA DIRECCIÓN MAC?

A continuación detallaré varias opciones disponibles para ocultar o camuflar la MAC Address.

En el caso que sean usuarios de Linux, en el siguiente enlace encontrarán como se puede [cambiar la Mac Address usando NetworkManager]({{< relref "/posts/cambiar-la-mac-address-networkmanager" >}}).

Para terminar solo comentar que en las próximos meses redactaré varios artículos detallando como podemos cambiar o camuflar la Mac Address en la totalidad de sistemas operativos que acostumbro a usar.
