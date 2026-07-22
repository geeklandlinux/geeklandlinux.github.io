---
title: "Navegar en la deep web de forma segura y anónima"
date: 2016-09-03
categories: 
  - "seguridad-informatica"
tags: 
  - "deep-web"
  - "metadatos"
  - "privacidad"
  - "tails"
  - "tor"
  - "vpn"
cover:
  image: "images/Navegar-en-la-deep-web-de-forma-segura-y-anónima.png"
  relative: true
---

En el pasado escribí varios artículos sobre la [deep web]({{< relref "/posts/acceder-a-la-deep-web" >}}). En los comentarios de los artículos detecté que muchas personas preguntaban acerca de como navegar en la deep web de forma segura, por este motivo en el siguiente artículo veremos una serie de recomendaciones que todo el mundo debería seguir para navegar en la deep web.<!--more-->

## Usar Tails o una herramienta equivalente

No recomiendo el uso de Windows, Android o iOS para navegar en la deep web. No son sistemas operativos tan seguros como lo pueden ser Linux o Mac OS.

La mejor opción es usar una herramienta como el sistema operativo Tails, o en su defecto usar una herramienta equivalente como Whonix.

Para saber como usar, instalar y empezar a usar Tails pueden seguir los pasos que se detallan en el siguiente enlace:

https://geeklandlinux.github.io/posts/instalar-tails-para-ser-anonimo/

Tails es una distribución Linux basada en Debian que se ejecuta en nuestra memoria RAM sin hacer uso de nuestro disco duro.

Tails es un sistema operativo diseñado pensando en la seguridad y en la privacidad. La totalidad del sistema operativo corre sobre la memoria RAM de nuestro ordenador. Por lo tanto en el momento que apagamos el ordenador no dejamos rastro alguno de quienes somos y que hemos realizado en el ordenador.

Tails siempre se debe instalar en una memoria USB o en un DVD. No lo uséis en una máquina virtual porque no es seguro.

Si lo ejecutamos en una máquina virtual seremos mas susceptibles de ser infectados por Malware, Keyloggers, etc. Además en el momento que cerremos la máquina virtual quedarán rastros de las actividades realizadas en la Deep web en el disco duro de nuestro ordenador.

###### Nota: Hay otras alternativas buenas como por ejemplo el navegador Tor Browser. No obstante Tails es con diferencia la mejor opción para navegar en la deep web.

## Usar Software actualizado para navegar en la Deep web

Tanto si usáis Tails, como Tor Browser u otra alternativa hay que asegurarse que el software que usamos está actualizado.

Si el software que usamos no está actualizado es más probable que un atacante encuentre una vulnerabilidad para atacarnos o se pueda revelar nuestra identidad.

## No alterar la configuración estándar de nuestro navegador

En el caso que estemos usando Tails, Tor Browser o cualquier otra herramienta de confianza, no se recomienda modificar la configuración estándar de estos programas.

Este tipo de herramientas vienen configuradas para obtener la máxima protección cuando navegamos en la deep web. Cualquier modificación que hagamos puede abrir una brecha de seguridad.

Las únicas opciones de configuración que en mi caso contemplaría modificar para mejorar la seguridad son las siguientes:

1. Deshabilitar Javascript de nuestro navegador.
2. Desactivar los iframes de nuestro navegador.
3. Asegurar que los referrers están desactivados.
4. Comprobar que las extensiones NoScript y HTTPS están activadas realizando su función.

Bajo ningún concepto debemos instalar extensiones y personalizar el funcionamiento y aspecto del navegador que usamos para navegar en la deep web.

###### Nota: Hay sitios webs que para visitarlos requieren tener javascript activado. Si lo activamos no tiene porque pasar nada, pero nuestro nivel de seguridad y privacidad serán menores.

## Usar una conexión VPN

Es recomendable usar un servidor VPN que no guarde Logs y que esté ubicado en en país que tenga una legislación favorable como por ejemplo Panamá o las Islas Vírgenes Británicas. Hay que evitar usar servidores VPN ubicados en Estados Unidos.

Usando un VPN evitaremos que nuestro proveedor de Internet, o el administrador de la red del sitio desde el que nos conectamos, detecte que nos hemos conectado a la red Tor.

El uso de una red VPN también será otro mecanismo de defensa para que no se pueda revelar nuestra IP real.

Si el el alumno de Harvard de [está noticia](https://www.wnyc.org/story/harvard-bomb-threat/ "Noticia en que un alumno de Harvard hace una amenaza de bomba") hubiera usado una conexión VPN, probablemente no lo habrían identificado o a al menos no se habría podido demostrar que él fue el culpable de lo que sucedió.

Una vez conectado al servidor VPN se recomienda comprobar periódicamente que la conexión no se ha caído. Hay servicios VPN que ellos mismos son capaces de realizar esta comprobación y en el momento que se cae el servicio VPN se cae la conexión y no podemos navegar en la deep web.

En el futuro escribiré un post para que la gente tenga las herramientas para saber si un proveedor de VPN es confiable y anónimo para navegar en la Deep Web.

## Cuidado con los metadatos del contenido que subimos a la deep web

No es recomendable que usuarios inexpertos suban, fotos, documentos u otro tipo de archivos en la Deep web.

Todos los archivos que generamos tienen metadatos. Estos metadatos pueden revelar datos como por ejemplo los siguientes:

1. Nuestra nombre y nuestros apellidos.
2. El sistema operativo que usamos.
3. Nuestra ubicación geográfica de forma precisa y en coordenadas GPS.
4. Otros datos que pueden revelar nuestra identidad.

Por lo tanto no es recomendable subir información a la Deep Web y en el caso de hacerlo deberemos tener en cuenta los siguientes puntos:

1. Tenemos que borrar los metadatos.
2. Debemos analizar que el contenido del documento no de pistas de nuestra identidad y/o ubicación.
3. Si la información que subimos es sensible se recomienda que la transmisión de datos sea cifrada con el protocolo https, o que la URL donde subimos el contenido tenga el dominio **.onion**.

Para ver una muestra de la información que se puede obtener con los metadatos miren el siguiente vídeo.

https://www.youtube.com/watch?v=6SGHD2sf-to

###### Nota: Tails viene equipado con la herramienta [Metadata Anonymisation Toolkit](https://mat.boum.org/ "Web de Metadata Anonymisation Toolkit") para eliminar los metadatos de nuestros archivos.

###### Nota: Metada Anonymisation Toolkit no es infalible, por lo tanto recomiendo combinar su uso con Exiftool, exiv2, jhead y pdfparanoia.

## Nunca Navegar con la pantalla maximizada

No debemos navegar con la pantalla del navegador maximizada.

Navegar con la pantalla del navegador maximizada revela nuestra resolución de pantalla. Este dato se puede usar como parámetro para identificar un usuario.

En la deep web se trata de no ser diferente al resto de navegantes. En el momento que somos diferentes estamos dando pistas para que nos puedan identificar.

###### Nota: Tened en cuenta que una resolución de pantalla poco común puede ayudar a identificar un determinado usuario.

## No descargar documentos de la Deep web

No es una buena idea descargar documentos de la deep web.

Los ficheros descargados pueden contener código malicioso que al ejecutarlo puede revelar nuestra identidad o inyectar algún tipo de virus o malware en nuestro ordenador.

En el caso que tengamos necesidad de descargar algo lo podemos hacer, pero se recomienda abrir los archivos en un entorno controlado y sin conexión a Internet.

###### Nota: Como entorno controlado entendemos una [máquina Virtual]({{< relref "/posts/que-es-una-maquina-virtual-usos-y-ventajas-que-nos-proporiciona" >}}) o un sistema operativo como por ejemplo Tails.

## No visitar tu propia página web

Cuando navegamos usando la red Tor no se recomienda que visitemos nuestra propia web.

El nodo de salida de la red Tor registrará que hemos visitado una web. En el caso que está página web sea muy poco popular puede servir para que alguien deduzca que nosotros somos los propietarios de esta página web.

###### Nota: Con ingeniería social o con la herramienta whois es muy fácil conocer el nombre y los datos de contacto del propietario de un dominio en concreto.

## No conectarse a redes sociales ni a servicios que usamos habitualmente

Cuando usamos la red Tor para navegar en la Clearnet nunca debemos loguearnos a servicios que utilizamos habitualmente como por ejemplo:

1. Redes sociales como Facebook, Twitter, Google Plus, etc.
2. Nuestro servicio de correo electrónico.
3. Market place como por ejemplo Amazon o Ebay.
4. En nuestro banco.
5. Etc.

Los motivos para ello son simples:

1. Aunque nuestra IP sea anónima y no puedan localizar nuestra ubicación real, los servicios sabrán que somos nosotros los que nos estamos conectando. Por lo tanto en ningún momento seremos anónimos.
2. En el momento que los servicios detecten que nos conectamos a través de la red Tor, es posible que bloqueen nuestra cuenta porque pensarán que estamos recibiendo un ataque por parte de un cracker o un criminal.

## No navegar en sitios web locales

Cuando usamos la red Tor nunca debemos navegar en sitios web locales o que puedan dar pistar que somos de un determinado lugar o país.

En el momento que visitamos la web de nuestro ayuntamiento, un atacante en el último de los nodos de la red Tor se puede hacer la idea que somos del pueblo o ciudad del ayuntamiento que hemos visitado.

En el caso que tengamos que realizar una búsqueda en google siempre tenemos que usar el dominio google.com. Si usamos google.es estamos revelando que somos españoles.

###### Nota: Como veremos más adelante no es recomendable realizar busquedas con Google cuando navegamos en la Deep Web.

## Se recomienda comunicarte en un inglés correcto

Gran parte del contenido y foros de la deep web son en inglés. Por lo tanto es importante que sepamos escribir el inglés correctamente.

Cuando escribamos hay que evitar usar letras que son especificas de un lenguaje en concreto.

A modo de ejemplo, si estáis leyendo un foro en inglés y de repente veis que a alguien se le escapa la letra **ç**, podéis haceros una idea que la persona en cuestión puede ser catalana, turca, albanesa, etc.

También es aconsejable no escribir siempre del mismo modo ni cometer siempre las mismas faltas de ortografía. El modo de escribir de una persona puede dar pistas para conocer la identidad de una persona.

###### Nota: Gran parte del contenido de la deep web es en inglés. Por lo tanto prácticamente estamos forzados a comprender y escribir el inglés.

## Usar un seudónimo y no revelar ningún dato personal

En el momento de navegar en la deep web nunca debemos mencionar nuestro nombre. Lo ideal es usar un seudónimo que no de pistas de nuestra identidad.

Además no debemos mencionar datos que puedan dar pistas para revelar nuestra identidad, como por ejemplo:

1. Una dirección de email convencional como por ejemplo gmail o outlook.
2. La dirección de vuestro domicilio.
3. El lugar en donde estudiamos.
4. Si estamos casados, si tenemos hijos, si somos gordos, nuestros hobbies, etc.

## Usar medios de comunicación seguros

Si necesitáis contactar con alguien a través de la red Tor se recomienda usar servicios de mensajería e email seguros y anónimos.

Algunos de los servicios de correo que bajo mi humilde criterio son seguros los podéis encontrar en el siguiente enlace:

[Listado de proveedor de email anónimos](https://1earthunite.wordpress.com/2016/02/02/list-of-secure-dark-web-email-providers-in-2016/ "Web en que se recomiendan servicios de correo anónimos")

En el caso que uséis algún tipo de cliente de mensajería asegurad que las comunicación entre clientes sea cifrada. Para ello existen clientes de chat seguros y anónimos como por ejemplo [Torchat](https://github.com/prof7bit/TorChat "Plataforma de desarrollo de Torchat").

En las conversaciones de email y en los chats nunca debemos revelar información que pueda dar pistas sobre cual es nuestra identidad.

###### Nota: En ningún caso se os ocurra dar y/o comunicaros mediante vuestras cuentas de Facebook, Twitter, gmail, etc en la deep web.

## No se recomienda firmar todos los mensajes con claves PGP

No se recomienda firmar absolutamente todos los mensajes a través de la red Tor.

Únicamente debemos firmar los mensajes que nosotros consideramos esenciales.

En el momento que firmamos un mensaje estamos dando una garantía al receptor que el mensajes proviene de una fuente autorizada.

No obstante si un día alguien asocia nuestra firma con nuestra persona, podrá demostrar que todos los mensajes que contienen una firma en concreto los hemos escrito nosotros.

## Siempre que sea posible cifrar el contenido mediante PGP

Siempre que sea posible debemos cifrar nuestros emails y nuestros archivos mediante PGP.

A estas alturas todo el mundo sabe que las comunicaciones en texto plano las puede ver todo el mundo.

Además las claves de cifrado las debemos guardar en un lugar seguro como por ejemplo un USB.

## No uséis siempre el mismo password y el mismo usuario

En el momento de navegar en la Deep web verán muchos foros, tiendas online y servicios de email que requieren de un usuario y un password.

Por motivos obvios no uséis siempre el mismo usuario y el mismo password.

Es bueno que tengáis varias identidades cuando sea cuestión de navegar dentro de la Deep web.

El username y el password usado no deben dar detalles de vuestra identidad. Además tenemos que usar un password seguro.

## No dejar el ordenador desatendido

Si estamos navegando en la deep web y tenemos que irnos por unos instantes hay que bloquear el ordenador.

Si no lo bloqueamos corremos el peligro que una tercera persona use el ordenador en nuestra ausencia y se loguee y realice actos que puedan revelar nuestra identidad como por ejemplo conectarse a una cuenta de facebook, dar datos personales a un desconocido, etc.

## No informar a tus conocidos que te conectas a la Deep Web

En ningún momento debemos informar a nadie que nos hemos conectado o nos conectamos a la Deep web.

## Destruir completamente los ficheros descargados y cifrar nuestro disco duro

Por cuestiones de seguridad es recomendable que cifréis el contenido de vuestro disco duro. De este modo estaremos más protegidos frente a posibles accesos remotos a nuestro ordenador.

Es recomendable almacenar los archivos que descargamos de la Deep Web en un volumen cifrado. De esta forma estarán a salvo de curiosos.

En el momento que queramos eliminar un archivo tenemos que triturarlo. De está forma evitaremos que nadie pueda recuperar los archivos que hemos borrado.

Si usáis Tails no os tenéis que preocupar por este punto. Tails trae las herramientas necesarias para solventar la totalidad de puntos mencionados en este apartado.

## Navegad preferiblemente en direcciones URL que contengan las palabras .onion o https

Las direcciones web **.onion** ofrecen un cifrado de la información de extremo a extremo.

Este significa que el último nodo de la red Tor no será el encargado de descifrar el contenido.

El encargado de descifrar el contenido será el servidor Web que estamos visitando. De este modo conseguimos solucionar uno los puntos débiles de la red Tor.

Si las URL que visitamos disponen de una dirección https también podemos estar más tranquilos ya que el último nodo de la red Tor no podrá ver el contenido de nuestra petición.

###### Nota: Las claves de encriptación https deberían ser como mínimo de 2048 bits.

## No utilizar el buscador de Google para navegar en la clearnet

Cuando usamos el navegador Tor Browser para navegar en la Clearnet tenemos que evitar usar el navegador de Google.

Usad siempre buscadores que sean respetuosos con nuestra privacidad como por ejemplo los siguientes:

[https://www.startpage.com/](https://www.startpage.com/ "Buscador Startpage")

[https://duckduckgo.com/](https://duckduckgo.com/ "Buscador duckduckgo")

## Buscadores y directorios de enlaces para navegar en la deep web

La primera vez que entramos a navegar en la Deep web es difícil encontrar enlaces y páginas para visitar.

Afortunadamente hoy en día existen algunos directorios de links y buscadores especializados en indexar contenido de la Deep Web. Algunos de ellos son los siguientes:

[Onion.city](https://onion.link/ "buscador onion.city") [Onion.to](https://tor2web.org/ "Directorio de links Onion.to") [Tor search Not Evil](http://hss3uro2hsxfogfq.onion/ "Buscador Tor search") [DuckDuckgo](http://3g2upl4pq6kufc4m.onion/ "Buscador Duckduckgo") [Pastebin](http://pastebin.com/ "Directorio de links pastebin.com") [Thehiddenwiki](http://wikitjerrta4qgz4.onion/ "Directorio de links thehiddenwiki") [Tor Search](http://xmh57jrzrnw6insl.onion/) [Onion.list](http://jh32yv5zgayyyts3.onion/) [TorLinks](http://torlinkbgs6aabns.onion/) [Ahmia](https://ahmia.fi/)

## Reiniciar nuestro navegador de forma periódica

Las cookies de nuestro navegador pueden ser usadas para intentar identificarnos. Por lo tanto es recomendable borrar las cookies de nuestro navegador periódicamente.

Si usamos herramientas como el navegador Tor Browser o Tails, cada vez que cerramos el navegador se borran las cookies almacenadas. Por lo tanto de forma periódica es bueno cerrar y abrir el navegador.

## Nunca usar procesos de verificación en dos pasos

Al navegar en la deep web nunca debemos aceptar procesos de verificación en dos pasos.

En el momento que damos nuestro número de teléfono hay que tener en cuenta los siguientes puntos:

1. Nuestro teléfono será almacenado en alguna base de datos.
2. El hecho de recibir un SMS delata tu localización.
3. La tarjeta SIM del teléfono es más que posible que esté a nuestro nombre.
4. Los operadores de telefonía móvil toman logs de los número de serie de nuestra SIM card y de nuestro teléfono de forma periódica.

Por lo tanto en el momento de realizar una verificación en dos pasos estamos revelando nuestra identidad.

## No navegar en la deep web y en la web normal de forma simultánea

No se recomienda navegar de forma simultánea en la deep web y en la clearnet.

Si lo hacemos corremos el peligro de confundir un navegador por el otro y de forma tonta podemos revelar información no deseada a terceros.

## No conectarse a un servidor de forma anónima y de forma normal de forma simultánea

Tenemos que evitar conectarnos a un mismo servidor de forma anónima y de forma normal al mismo tiempo.

Si lo hacemos, en el momento que se caiga nuestra conexión de Internet se caerán de forma simultánea nuestra conexión normal y nuestra conexión anónima. Por lo tanto el administrador del servidor podrá deducir fácilmente la IP real de la conexión anónima.

## No publicitar nuestro servicio anónimo

Si disponemos de una página web o de un servicio anónimo en la deep web no debemos publicitarlo en la redes sociales o entre nuestros conocidos.

## Navegar usando redes wifi abiertas no es seguro ni anónimo

Mucha gente pensará que conectarse a través de la red wifi de un bar o de una biblioteca es anónimo y seguro.

Es posible que la red wifi abierta que usamos esté registrando la [Mac Address]({{< relref "/posts/mac-address-que-es-usos-como-cambiarla" >}}) de nuestro dispositivo conjuntamente con nuestra actividad. Por lo tanto en el caso que sea necesario investigar algún tipo de suceso, el círculo de sospechosos a investigar seria pequeño y nos podrían llegar a identificar.

Además navegando a través de una red wifi abierta estamos mostrando nuestra IP real a terceros que revela nuestra posición geográfica.

Para evitar estos problemas siempre que os conectéis desde de una red wifi abierta debéis usar Tor, y si es posible mediante Tails que de forma predeterminada enmascara nuestra Mac Address.

## Nota final

Los consejos citados en este artículo los debería seguir todo el mundo que navegue por la deep web.

En el momento que se salten algunos de los consejos no tiene porque pasar nada, pero cuantos más se salten más altas serán las posibilidades de recibir una ataque o de que nuestra identidad sea descubierta.

## Fuentes

[https://www.whonix.org/wiki/DoNot](https://www.whonix.org/wiki/DoNot)

[https://www.reddit.com/r/TOR/comments/1x2jff/deep\_web\_saftey\_rules\_for\_noobs\_help\_share\_your/](https://www.reddit.com/r/TOR/comments/1x2jff/deep_web_saftey_rules_for_noobs_help_share_your/)
