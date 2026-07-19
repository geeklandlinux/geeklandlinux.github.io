---
title: "Datos que nuestro navegador filtra o entrega al visitar una web"
date: 2018-07-22
categories: 
  - "seguridad-informatica"
tags: 
  - "navegadores"
  - "privacidad"
cover:
  image: "images/datos-navegador-da-terceros.jpg"
  relative: true
---

La privacidad en la red es una batalla prácticamente pérdida, pero existen formas de minimizar la información proporcionamos a terceros. También es útil y saludable ser consciente de la información que estamos proporcionando en todo momento. Por este motivo en el siguiente artículo veremos la información que nuestro navegador está entregando a terceros cuando estamos navegando.<!--more-->

###### Nota: La información del artículo hace referencia a la información entregada por nuestro navegador. La información del artículo no contempla la información que pueden entregar las cookies y cross-site cookies.

## INFORMACIÓN SOBRE NUESTRA UBICACIÓN GEOGRÁFICA

La totalidad de navegadores web disponen de una API que revela **nuestra ubicación** a las web que visitamos. Las utilidades de esta API son varias. Algunas de ellas son las siguientes:

1. Bloquear contenidos de internet en una determinada región o país.
2. Proporcionar una versión de la web adaptada a las necesidades de cada país. De este modo si accedemos a una web desde Estados Unidos se mostrará en Inglés, pero si accedemos desde España se mostrará en Español.

Obviamente el navegador no mostrará nuestra ubicación exacta porqué la API del navegador nos geoposiciona a través de nuestra IP pública. Las IP públicas solo dan una idea aproximada del país, región o ciudad desde que nos conectamos. En muchos casos los errores del servicio de geolocalización son importantes por el siguiente motivo:

1. Los servicios de geolocalización solo son capaces de mostrar la posición que nuestro proveedor de Internet ha asignado a una determinada IP. Normalmente, la dirección asignada a una IP es la central del ISP más próxima a nuestra ubicación real.

Por lo tanto, si vivimos en poblaciones pequeñas alejadas de la sede de nuestro proveedor de Internet se pueden producir errores de localización importantes.

Si nos conectamos desde una empresa es posible que nuestra IP revelé nuestra ubicación real. El motivo es que las empresas acostumbran a disponer de servicios de IP pública fija que están registrados a nombre de la compañía.

Por lo tanto, en la mayoría de casos una IP no revela nuestra posición real. Para que alguien pueda identificarnos y conocer nuestra ubicación real a través de la IP tendría que llamar a nuestro proveedor de Internet y preguntarle a que cliente corresponde una determinada IP.

## DATOS DE NUESTRO SISTEMA OPERATIVO

Los navegadores web proporcionan diversos datos sobre nuestro equipo. Algunos ejemplos de la información proporcionada son los siguientes:

1. El navegador web que estamos usando.
2. La versión de navegador que estamos usando. Si usamos una versión de navegador obsoleta, un atacante puede intentar aprovechar algún agujero de seguridad existente.
3. Los plugins que está usando nuestro navegador.
4. La marca y el modelo de la tarjeta gráfica que estamos usando.
5. La resolución de nuestro monitor.
6. El número de núcleos de nuestro procesador.
7. La arquitectura de nuestro procesador.
8. Saber si nuestro equipo dispone de una cámara web.
9. Etc.

###### Nota: Gran parte de estos datos son proporcionados por WebGL, Canvas, Silverlight, Flashplayer, la forma en que se renderizan las fuentes, etc.

Todos los datos mencionados en este apartado, conjuntamente con los datos mencionados en el resto de apartados, pueden ser utilizados para realizar un perfil e intentar identificar un usuario.

## DATOS SOBRE LA BATERÍA DE NUESTRO DISPOSITIVO

Los grandes navegadores como Firefox, Opera y Chrome tienen la capacidad de informar sobre los siguientes aspectos de nuestra batería:

1. El nivel de carga actual de nuestra batería.
2. Información si la batería se está cargando o descargando.
3. El tiempo que tardará en cargarse o descargarse completamente.

Esta característica puede ser utilizada para mejorar la experiencia de los lectores que visitan una web. Existen web que si detectan que tienes un nivel de carga bajo son capaces de mostrarte una versión reducida para minimizar el consumo de batería.

No obstante estos datos también pueden ser usados para otros propósitos como por ejemplo invadir nuestra privacidad. Por ejemplo:

1. El nivel de carga actual conjuntamente con el tiempo que la batería tardará en cargarse o agotarse es un dato muy útil a la hora de crear perfiles de usuario.
2. Determinados servicios online, como por ejemplo pedir un taxi, hacer el booking de un hotel, comprar un billete de avión, etc. pueden aprovecharse y pedirnos más dinero si ven que nuestro nivel de carga es bajo.
3. Etc.

## INFORMACIÓN SOBRE NUESTRA RED LOCAL

La gran mayoría de navegadores traen habilitado WebRTC (Web Real-Time Communication). WebRTC facilita las llamadas de voz, chats de vídeo y transferencias de archivos, pero también trae inconvenientes.

WebRTC proporciona nuestra IP local y pública a los administradores de las web que visitamos. Esto sin duda hace que nuestra red local sea menos segura ya que si un atacante conoce nuestra IP pública y local puede atacarnos.

En este apartado es importante remarcar que da igual que usemos un proxy o una conexión VPN. Aunque usemos estás tecnologías, WebRTC es capaz de filtrar nuestras direcciones IP.

## VELOCIDAD DE DESCARGA DE NUESTRA CONEXIÓN DE INTERNET

El navegador es capaz de proporcionar información sobre la velocidad de descarga de nuestra conexión a Internet.

Esta información puede ser útil a webs que proporcionan vídeos en streaming. De este modo pueden adaptar la calidad de los vídeos en función de nuestra velocidad de descarga. No obstante es otro dato que se puede usar para crear un identificador de usuario.

## ESCANEAR DISPOSITIVOS CONECTADOS A NUESTRA RED LOCAL

Toda web que visitamos tiene la capacidad de escanear nuestra red local en busca de dispositivos sin pedir permiso. Esto sin duda vulnera nuestra privacidad y puede ser un agujero de seguridad a explotar por un atacante.

La forma de conocer los dispositivos y servicios de nuestra red local es simple. La web a la que nos conectamos puede ir realizando peticiones a posibles recursos dentro de nuestra red local. Si no se obtiene ninguna respuesta significa que el recurso no existe, mientras que si se obtiene una respuesta obviamente existe el recurso.

Este tipo de información puede ser aprovechada para realizar ataques del tipo [DNS rebinding](https://security.stackexchange.com/questions/145336/how-can-a-webpage-scan-my-local-internal-network-from-the-internet "Breve explicación de como el navegador filtra datos de los dispositivos de nuestra red local "). Con este tipo de ataques, un atacante puede ser capaz de robar información que tenemos almacenada dentro de nuestra red local.

## ACCESO A LOS DATOS DEL GIROSCOPIO Y ACELERÓMETRO DE NUESTRO DISPOSITIVO MÓVIL

Este apartado afecta mayoritariamente a dispositivos móviles y algunos ordenadores portátiles.

El navegador es capaz de proporcionar información sobre aceleraciones y orientación de nuestro dispositivo móvil. Esto es útil para que los servicios web puedan proporcionar un servicio adaptado a las necesidades del cliente. Pero también puede ser usado para otros fines como por ejemplo intentar identificar las actividades que estamos realizando. Mediante los datos proporcionados por el acelerómetro y el giroscopio podemos averiguar si alguien está realizando ciertas actividades como por ejemplo:

1. Andar.
2. Correr a pie o en bicicleta.
3. Etc.

En si no son datos críticos que vulneren de forma flagrante nuestra privacidad, pero un pequeño detalle unido con otros pequeños detalles ayudan a construir un identificador de usuario.

## CONOCER LAS REDES SOCIALES EN QUE ESTAMOS LOGUEADOS

Bajo ciertas circunstancias nuestro navegador es capaz de proporcionar información sobre si estamos conectados a una determinada red social como por ejemplo Google, Flickr, Twitch, etc.

Un administrador web con buenas intenciones usará esta información para los siguientes propósitos:

1. Saber las redes sociales en que tiene que participar más.
2. Definir los botones de compartir que introduce en su web.
3. Decidir si es apropiado invertir en Facebook Ads.
4. Tomar decisiones sobre acciones de marketing a implementar.
5. Etc.

Un administrador web o un servidor web comprometido usará estos datos para construir un identificador de usuario.

## DATOS PERSONALES FILTRADOS POR LA OPCIÓN AUTORELLENAR

Cuando se usa la función autorellenar hay que ir con cuidado. En el momento de usar esta función es posible que nuestro navegador también rellene información que está oculta de nuestros ojos. Esta información oculta a nuestros ojos puede ser información importante como por ejemplo

1. Nuestro puesto de trabajo.
2. Un número de teléfono.
3. Un email.
4. El número de nuestra tarjeta de crédito.
5. Cualquier otra información que hayamos introducido en un formulario online.

Por este motivos tenemos que ser especialmente cuidadosos a la hora de usar la función autorellenar que traen la mayoría de navegadores. También es altamente recomendable que usen navegadores web actualizados.

No se tomen en broma este apartado. En mi caso el navegador Chrome de mi Android es vulnerable a este tipo de prácticas. Pueden consultar la siguiente URL para comprobar que no sean vulnerables a este tipo de ataque:

[https://robinlinus.github.io/autofill-phishing/](https://robinlinus.github.io/autofill-phishing/ "Ver si somos vulnerables por ataques de autofill-phising")

###### Nota: La función autorellenar también puede suministrar información sensible en el caso que prestemos un ordenador a un tercero.

## DATOS PERSONALES OBTENIDOS MEDIANTE LA EJECUCIÓN DE CÓDIGO MALICIOSO

Si nuestro navegador es vulnerable y/o está mal configurado podemos ser víctimas de ataques Clickjacking.

Un ataque de Clickjacking consiste en que de forma inocente una web maliciosa o comprometida nos haga presionar sobre un link o un botón. En el momento de clicar en un link o botón malicioso puede suceder lo siguiente:

1. Ejecutar un script o un código embedido que nos robe información o tome el control de nuestro equipo.
2. Ser redireccionado a una web falsa para robarnos información y/o mostrar publicidad.
3. Etc.

De forma inocente y sin querer podemos dar permisos para que un atacante ejecute código remoto en nuestro equipo. Por lo tanto cuando naveguéis hay que ser extremadamente cuidadosos.

## COMPROBAR LA INFORMACIÓN QUE PROPORCIONA NUESTRO NAVEGADOR

Existen varios servicios que nos advierten de la información que está proporcionando nuestro navegador. Algunos de los servicios que recomiendo que consulten son los siguientes:

[https://browserleaks.com/](https://browserleaks.com/)

[http://webkay.robinlinus.com/](http://webkay.robinlinus.com/)

[https://panopticlick.eff.org/](https://panopticlick.eff.org/)

Después de visitar estos tres servicios os podréis hacer una idea más clara de la totalidad de información que nuestro navegador está entregando.

## MEDIDAS PARA MINIMIZAR LA INFORMACIÓN QUE PROPORCIONA NUESTRO NAVEGADOR

Existen opciones para minimizar la información que nuestro navegador entrega a los servidores web que se conecta. Algunas de estas opciones son las siguientes:

1. **Usar el sentido común** mientras estamos navegando. Las personas que se preocupan por su privacidad y tienen conocimientos informáticos proporcionarán menos información que el resto de usuarios.
2. **Usar extensiones como por ejemplo NoScript**. Con extensiones como NoScript bloquearemos la ejecución de scripts de java. Usando esta extensión prácticamente bloquearemos la totalidad de información proporcionada a terceros, pero a cambio algunas funciones de las webs que visitamos dejaran de funcionar.
3. **Contratar y usar un servicio VPN** que sea de calidad y de confianza. De este modo evitaremos dar información sobre cual puede ser nuestra ubicación real.
4. **Utilizar un bloqueador de publicidad** como [uBlock Origin]({{< relref "/posts/instalar-ublock-origin-chrome-firefox" >}}). Ublock Origin no únicamente bloquea la publicidad. Aparte de bloquear la publicidad bloquea las fugas de WebRTC, bloquea webs que intentan minar bitcon, etc.
5. **Deshabilitar WebRTC**. Los navegador permiten deshabilitar WebRTC, pero tengan en cuenta que en el momento de hacerlo ciertos servicios web, como por ejemplo Spotify web, dejaran de funcionar. También podemos instalar extensiones en nuestro navegador web para bloquear WebRTC.
6. **Usar el modo incógnito** ayuda a minimizar la fuga de datos. Si navegamos en modo incógnito evitaremos revelar las redes sociales en que estamos logueados, evitaremos los peligros de la función autorellenar, etc.
7. **Desactivar la función autorellenar** de nuestro navegador. En el artículo se ha explicado de forma clara los riesgos que tiene usar esta función del navegador.
8. **Desinstalar Flashplayer**, Silverlight y otros complementos en el caso que no los necesiten.
9. **Minimizar el uso de extensiones del navegador**. Las extensiones del navegador son vectores de ataque y pueden filtrar datos a terceros. Por lo tanto usen las mínimas e imprescindibles.
10. **Usar navegadores como Tor browser**. Ciertos navegadores, como por ejemplo Tor browser, están configurados para preservar la privacidad de sus usuarios.

Algunas de las medidas que se proponen en este aparado pueden romper la funcionalidad de las web que visitamos. Existen muchas partes de las web que necesitan Javascript, WebRTC, etc para funcionar.

## CONCLUSIONES

Después de leer el artículo es fácil llegar a la conclusión que ninguno de los navegadores modernos populares respeta la privacidad de sus usuarios.

Si unimos todos los datos que filtra o puede filtrar un navegador moderno corremos los siguientes riesgos:

1. **Un tercero puede construir un perfil de usuario único**. El perfil de usuario único puede ser vendido para obtener un beneficio económico. También puede ser usado para identificar a un usuario.
2. **Ser víctimas de un ataque informático**. Mediante los datos que proporciona el navegador un atacante puede robarnos información, comprometer nuestro equipo e incluso puede llegar a comprometer nuestra red local.

En el caso que se intenten deshabilitar los mecanismos de proporcionan las fugas de datos nos encontraremos con dificultades como por ejemplo las siguientes:

1. Ciertas **partes de las web dejarán de funcionar**. Así de este modo no podremos realizar comentarios en la mayoría de los blogs.
2. **Algunas web no se visualizarán de forma correcta**. Incluso habrán web que no se podrán ni visualizar.
3. **Ciertos servicios web simplemente no funcionarán**. Como hemos dicho anteriormente, si deshabilitamos WebRTC no funcionará Spotify web.

Por lo tanto, la privacidad estará reñida con las funcionalidades que ofrecen las nuevas tecnologías y algunos servicios web.
