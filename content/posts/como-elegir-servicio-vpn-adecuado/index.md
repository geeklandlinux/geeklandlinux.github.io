---
title: "Como elegir un servicio VPN adecuado a nuestras necesidades"
date: 2016-09-11
categories: 
  - "seguridad-informatica"
tags: 
  - "internet"
  - "openvpn"
  - "privacidad"
  - "vpn"
cover:
  image: "images/Elegir-el-servicio-VPN-adecuado-a-nuestras-necesidades.jpg"
  relative: true
---

Cuando contratamos un servicio VPN es recomendable tener en cuenta ciertos parámetros para asegurar que nuestra elección es la correcta.

Si pensamos en términos de seguridad lo primero que les vendrá a la cabeza es que el servicio VPN no guarde logs de nuestra actividad.<!--more-->

Pero aparte de este parámetro existen otros parámetros que podemos analizar como por ejemplo los que veremos a lo largo de este artículo.

###### Nota: Contratar un servidor VPN no es algo para tomarse a la ligera. En el momento que un servidor VPN entrega nuestros datos a un tercero es equivalente a decir que hemos recibido un ataque Man in the Middle.

## ASPECTOS A CONSIDERAR PARA ELEGIR UN VPN

### ¿Guarda logs tu proveedor de VPN?

Obviamente es importante que nuestro servicio VPN publicite que no guarda logs y no registra nuestra actividad.

Pero en la mayoría de casos la realidad es muy distinta a la que nos venden porque en muchos países existen leyes de retención de datos. Así que aunque el servicio VPN diga que no guarda logs desconfiad y valorad el resto de puntos que verán en este artículo.

Si el FBI, la NSA o cualquier otra autoridad reclama datos al administrador de un servidor VPN se los dará sin pensarlo mucho por los siguientes motivos:

1. Colaborar con las autoridades para asegurar la continuidad de su negocio.
2. Dar todos los datos disponibles ya que seguro que preferirán que vosotros vayas a la cárcel antes que ellos.
3. Nadie quiere que su negocio sea usado para realizar actividades ilegales como por ejemplo ataques DDOS, transmitir contenido poco apropiado, etc.

### Métodos de pago que ofrece el servicio VPN

Obviamente cuantos más métodos de pago tengamos disponibles mejor.

No obstante hay que ser consciente que en el momento que pagamos estamos dando datos precisos acerca de nuestra identidad.

Por lo tanto si os preocupa la privacidad puede resultar interesante contratar servicios VPN que ofrezcan la posibilidad de pagar de forma anónima.

Algunas formas de pago anónimas son las siguientes:

1. Pagar con monedas criptográficas pseudoanónimas como el Bitcoin.
2. Pago mediante tarjetas prepago de regalo.
3. Pagar mediante servicios de Internet que nos garantizan pagar de forma anónima.

### Ubicación de los servidores VPN

Hay 2 factores a considerar para determinar si la ubicación de un servidor VPN es adecuada a nuestras necesidades.

El primero de los factores es la privacidad que nos ofrece la ubicación del servidor VPN.

El segundo de los factores es evaluar los países en que ofrece servicio el VPN que queremos contratar.

#### Elección de la ubicación del servidor en función de la privacidad

Sí únicamente estáis pensando en términos de privacidad mi consejo es que desconfiéis de la totalidad de servicios VPN ubicados en Estados Unidos, Canadá, Australia, Reino Unido, Nueva Zelanda, Alemania, Bélgica, Italia, Suecia, España, Noruega, Holanda, Francia, Dinamarca, Austria, Bélgica, República Checa, Grecia, Hungría, Islandia, Italia, Portugal, Korea del Sur, Turquía, Luxemburgo, Japón, Polonia, etc

Los países que acabo de citar conforman o cooperan con la alianza de inteligencia conocida como [FVEY](https://es.wikipedia.org/wiki/Five_Eyes "Explicación de la alianza estratégica de los 5 ojos") (Five Eyes). Esta alianza se preocupa especialmente de todo lo que pasa en la Word Wide Web y capturan datos como por ejemplo conversaciones telefónicas, correos electrónicos, búsquedas que realizamos en Internet, etc.

Escudándose en la alianza FVEY, estos países pueden espiarnos y acceder a la totalidad de nuestros datos independientemente de las leyes de protección de datos que pueden existir en un país.

Se recomienda elegir servicios VPN que estén ubicados en un país que tenga leyes favorables en términos de privacidad y no dispongan de leyes de retención y protección de datos.

Algunos de los países que en principio garantizaran la privacidad de sus clientes son los siguientes:

Panamá, Islas vírgenes británicas, Hong Kong, Malasia, Bulgaria, Moldavia, Seychelles, Rumanía, Taiwan, etc.

#### Elección de la ubicación del servidor en función de los países en que ofrece servicio

Si nuestro objetivo es usar un VPN para poder acceder a servicios que no están disponibles en nuestro país, entonces tenéis que buscar un servicio VPN que disponga de servidores VPN disponibles en varios países.

De este modo si somos españoles y queremos acceder a un servicio que únicamente está disponible para los ciudadanos de Estados Unidos tendremos que buscar un servicio VPN que nos proporcione una IP Americana.

La gran mayoría de servicios VPN acostumbran a ofrecer servidores VPN ubicados en distintos puntos geográficos. Cuantos más puntos geográficos podamos elegir mayores serán las restricciones geográficas que podemos saltarnos.

### Protocolo usado por los servicios VPN

Existen varios protocoles para el funcionamiento de un servicio VPN como por ejemplo el PPTP, el L2TP/IPSec, OpenPVN, SSTP, etc.

En términos de seguridad tenemos que evitar usar un VPN con el protocolo PPTP. El protocolo PPTP es un protocolo elaborado por Microsoft que tiene demasiados años y presenta vulnerabilidades de seguridad. Además el tamaño máximo de la clave de cifrado solo será de 128 bits.

Los otros dos protocolos son más seguros y modernos y son buenas opciones en términos de seguridad y privacidad. Por lo tanto podéis usar tanto L2TP/IPSec como OpenVPN.

En mi caso que quedo con OpenVPN por los siguientes motivos:

1. Es un protocolo OpenSource.
2. Es compatible con multitud de sistemas operativos.
3. Aunque técnicamente es más lento que L2TP/IPSec, ofrece una velocidad aceptable y es un protocolo ofrece gran estabilidad.
4. Existen rumores no confirmados y sospechas de personajes como Edward Snowden, que el protocolo L2TP/IPSec es vulnerable por parte de la NSA.
5. Es un protocolo altamente configurable y flexible.
6. Soporta un gran número de algoritmos de cifrado.

### Algoritmos y claves de cifrados del servidor VPN

Los algoritmos de cifrado que acostumbran a usar la mayoría de servicios VPN son el AES y el Blowfish con claves de cifrado de 128 bits.

###### Nota: El protocolo Blowfish se considera seguro. No obstante se conocen ciertas debilidades que los podrían comprometer. Por este motivo es mejor evitar usar este protocolo.

En mi caso recomendaría usar algoritmos de cifrado seguros como por ejemplo el AES, y el Twofish con claves de cifrado de como mínimo 256 bits.

Intentad evitar los VPN que ofrezcan cifrados de 128 bits porque existen sospechas que la NSA tiene los medios para descifrar o el contenido con este tipo de llaves.

Si en el proceso de contratación también os hablan del tamaño de las claves para el proceso de autenticación asegurad que la llaves ofrecidas sean como mínimo sean de 2048 bits.

### Petición de datos personales

Si os preocupa vuestra privacidad desconfiad de un servicio VPN que os pida vuestros datos personales.

No es recomendable dar datos personales, como vuestros nombre y dirección, a la hora de contratar un servicio VPN.

En el caso que os pidan un correo electrónico usad uno que sea completamente anónimo como por ejemplo Mail2Tor.

### ¿El servidor VPN es útil para lo que quiero realizar?

Hay servicios VPN que restringen ciertas actividades como por ejemplo las siguientes:

1. Realizar descargas P2P.
2. Navegar por la Deep Web.
3. Transmitir archivos mediante programas como Bittorrent.
4. Transmitir grandes cantidades de tráfico.
5. Estar muchas horas conectado al servicio VPN.
6. Realizar más de una conexión de forma simultanea.
7. Etc.

Si nuestra intención es realizar una de las 6 actividades que acabamos de mencionar tenemos que asegurar que el servicio VPN nos lo permita.

Obviamente también deberemos mirar que el servicio VPN no nos imponga ningún limite de ancho banda.

### Velocidad del servicio

La velocidad que nos pueda ofrecer una conexión VPN es un factor a tener en cuenta antes de su contratación.

A través de foros de Internet podréis encontrar información de usuarios del servicio que queréis contratar.

### Sistemas operativos en los que podemos usar el servicio VPN

Hay servicios VPN que solo pueden usarse en determinados sistemas operativos.

Al adquirir un servicio VPN tenemos que analizar si ofrecen servicio para los sistemas operativos que acostumbramos a utilizar habitualmente.

Por lo tanto en el caso que queramos contratar un servicio VPN para usarlo en Windows y en Android tendremos que asegurarnos que nos ofrecen clientes para Windows y para Android.

### Facilidad de configuración y de uso del servicio VPN

Es importante informarse si el proceso de configuración y uso del servicio VPN es fácil y amigable.

Existen servicios VPN que, aunque sean buenos, requieren conocimientos técnicos medios-elevados para poder configurarlos y usarlos.

Por lo tanto en el caso que no os guste la tecnología y/o no tengan conocimientos técnicos adecuados intenten buscar un servidor VPN que sea fácil de configurar y de usar.

### Precio del servicio VPN

Desconfiad de un servicio VPN gratuito. Si un servidor VPN es gratuito entonces el producto somos nosotros.

En el momento que un servicio VPN es gratis es posible que la totalidad de nuestros datos sean vendidos a terceros como por ejemplo gobiernos, empresas de publicidad, la industria del cibercrimen, etc.

Por lo tanto es recomendable usar un VPN de pago.

En el momento de pagar el servidor VPN tenemos que ir con cuidado. Si no lo pagamos con nuestra moneda la tarifa variará de un mes a otro por el tipo de cambio.

## SERVICIOS VPN QUE PODEMOS CONTRATAR

Finalmente para elegir un servicio VPN adecuado a sus necesidades los dejaré estás 2 direcciones URL:

[Enlace 1 para seleccionar vuestro VPN](https://thatoneprivacysite.net/ "Enlace en el que hay opciones para contratar un servicio VPN")

[Enlace 2 para seleccionar vuestro VPN](https://torrentfreak.com/which-vpn-services-take-your-anonymity-seriously-2014-edition-140315/ "Enlace en el que hay opciones para contratar un servicio VPN")

En estas URL encontrarán distintos servicios VPN con la información necesaria para que vosotros mismos podáis decidir el servicio VPN que más os convenga.

## CONSIDERACIONES FINALES

Para elegir el servicio VPN ideal para nuestra necesidades necesitamos tener claro para que lo queremos y analizar la totalidad de puntos que hemos mencionado en el post.

No obstante, aunque analicemos la totalidad de puntos y seleccionemos un buen servidor VPN, nunca tendremos la seguridad que la elección es buena.

Los servicios VPN se basan en la confianza que nosotros depositamos en un proveedor de VPN.

Si la empresa en la que nosotros depositamos nuestra confianza nos traiciona no podremos realizar nada al respecto.
