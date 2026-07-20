---
title: "Buenas prácticas en la gestión y uso de contraseñas"
date: 2018-05-13
categories: 
  - "seguridad-informatica"
tags: 
  - "contrasenas"
  - "seguridad"
cover:
  image: "images/buenas-practicas-gestion-de-contraseñas.webp"
  relative: true
---

En el siguiente artículos veremos algunas de las pautas que deberíamos seguir para gestionar de forma adecuada nuestras contraseñas. No obstante antes de empezar es recomendable detallar la utilidad que tienen las contraseñas.<!--more-->

## ¿PARA QUE SIRVEN LAS CONTRASEÑAS?

Su principal uso es que solamente las personas que estén autorizadas puedan acceder a una determinada información o servicio.

Para que este fin se cumpla obviamente se tienen que cumplir una serie de premisas que detallaremos a continuación.

## BUENAS PRÁCTICAS EN LA GESTIÓN Y USO DE CONTRASEÑAS

Todo el mundo que gestione o utilice contraseñas debe tener en cuenta los consejos que se citan a continuación.

### Las contraseñas son privadas y no se deben difundir

Las contraseñas son confidenciales. Por lo tanto tengan en cuenta los siguientes aspectos:

1. **No se deben apuntar** en un papel, en un tablón, debajo del teclado o en un post-it.
2. **Se deben difundir o comunicar a través de un medio inseguro**. Además las tenemos que retener en nuestra cabeza o en un gestor de contraseñas.
3. **Cuando nos proporcionan una contraseña es únicamente para nosotros**. Si una tercera persona nos pregunta por una contraseña le tenemos que responder que se la proporcione el administrador del servicio al que quiere acceder.

### No debemos usar las contraseñas por defecto

Nunca tendríamos que usar las contraseñas por defecto o que nos haya proporcionado una tercera persona o institución. Tampoco es recomendable usar las contraseñas por defecto que traen los dispositivos electrónicos como por ejemplo routers, cámaras IP, etc.

Usar las contraseñas por defecto es un error grave por los siguientes motivos:

1. Una contraseña únicamente la tenemos que saber nosotros. Por lo tanto, en el momento que una persona nos proporciona una contraseña lo primero que tenemos que realizar es cambiarla.
2. Existen páginas web, como por ejemplo pastebin.com, que publican listados de contraseñas por defecto de dispositivos electrónicos. También existen páginas web en que se publican listados de contraseñas robadas o filtradas.

### Las contraseñas que usemos tienen que ser robustas, seguras impersonales y no las tenemos que reutilizar

Obviamente no es seguro usar un contraseña como por ejemplo 1234. Las contraseñas que usemos tienen que tener las siguientes características:

1. La longitud mínima recomendable de una contraseña es de **8 caracteres**.
2. Estos 8 caracteres tienen que incluir **mayúsculas, minúsculas, números y caracteres especiales** como por ejemplo \* ( # @, etc.
3. Deben ser **impersonales**. No debemos usar nunca contraseñas como el cumpleaños de una persona, el nombre de nuestro perro, el nombre de nuestra madre, números de teléfono, el número de nuestro DNI, etc.
4. **No** tenemos que **repetir la misma contraseña para distintos servicios**. En cada uno de los servicios tenemos que usar contraseñas distintas. Si un atacante se hace con la contraseña de un servicio, lo primero que hará es usar esta contraseña para intentar acceder al resto de nuestro servicios.

### Consejos para crear una contraseña segura

Para construir una contraseña que sea fácil de recordar y cumpla con los requisitos del apartado anterior recomiendo lo siguiente:

1. Seleccionamos una frase larga, de por ejemplo un poema, que sea fácil de recordar para nosotros.
2. A partir de esta frase larga podemos sustituir unos caracteres por otros. Podemos definir que la última letra de todas las palabras sea en mayúscula. Además podemos sustituir todas las letras a de la frase por el caracteres especiales como **(**, **;**, etc. Los caracteres especiales que usemos no deben guardar similitud con las letras del abecedario, por lo tanto reemplazar la letra **a** por el símbolo **@** no es lo más aconsejable.
3. Una vez definida la frase podemos crear variaciones no predecibles de esta frase. Estás variaciones servirán para asignar contraseñas para cada uno de nuestros servicios

### Las contraseñas se tienen que cambiar periódicamente

Existen filtraciones, vulnerabilidades y multitud de situaciones en que nuestra contraseña puede quedar expuesta. Por lo tanto es recomendable que periódicamente vayamos modificando las contraseñas de nuestros servicios.

En el momento de cambiar la contraseña es altamente recomendable no reutilizar viejas contraseñas.

### Usar contraseñas seguras no significa que no podamos ser víctimas de un ataque

Aunque sigamos la totalidad de consejos mencionados en el artículo no estaremos totalmente seguros.

La totalidad de contraseñas de un servicio se almacenan en un sistema de control de acceso. Si alguien consigue acceder al sistema de control de acceso puede llegar a descargase su base de datos y por lo tanto obtener las contraseñas de miles de usuarios.

Por lo tanto es extremadamente importante que el sistema de control de acceso sea seguro. Algunas de las prácticas que se pueden implementar en los sistemas de control de acceso son:

1. Los administradores del sistema de control de acceso deben **cifrar las contraseñas de los usuarios mediante funciones hash**. De este modo, si un atacante consigue robar la base de datos que contiene las claves únicamente obtendrá un hash.
2. **Proteger los sistemas de control de acceso contra ataques por fuerza bruta**. Hay que bloquear a los usuarios o IP que cometan demasiados errores en la introducción de una contraseña.
3. Es recomendable que los **sistemas de control de acceso implementen sistemas de doble autenticación**. Por otra parte los usuarios que se loguean tienen que usar los sistemas de doble autenticación. De esta forma, aunque un atacante consiga nuestra contraseña no podrá acceder a nuestro servicio y/o información.
4. Etc.

Aparte de securizar el control de accesos también podemos ser víctimas de otro tipo de ataques. Por ejemplo ataques de phishing mediante ingeniería social. Por lo tanto siempre tenemos que estar atentos de no estar en una página fraudulenta en el momento de introducir nuestra contraseña.

### Concienciar a las personas que hacen uso de un servicio

En el ámbito empresarial y personal es importante formar y concienciar a los trabajadores y personas sobre la importancia de la buena gestión de las contraseñas.

La gente y los trabajadores tienen que ser conscientes que una mala gestión de las contraseñas puede traer consecuencias negativas como por ejemplo:

1. Robo de información de una determinada empresa.
2. Pérdida de información personal o confidencial como por ejemplo fotos, contratos, etc.
3. Filtración de información confidencial sobre las actividades y estrategia de una determinada empresa.
4. Que una tercera personas cometa actividades delictivas utilizando nuestras cuentas.
5. Que un compañero de trabajo acceda a nuestras cuentas para perjudicar nuestro rendimiento laboral.
6. Etc.

Por lo tanto es extremadamente importante concienciar y formar a nuestros empleados y personas más cercanas.

### Usar gestores de contraseñas para proporcionar comodidad a los usuarios

Los gestores de contraseñas son programas que guardan nuestras contraseñas y nombres de usuario en un archivo cifrado. La ubicación de este archivo puede ser nuestro propio ordenador o un servidor externo gestionado por un tercero.

Su función principal es proporcionar comodidad a los usuarios. Proporcionan comodidad por los siguientes motivos:

1. Son capaces de generar contraseñas aleatorias y seguras.
2. Algunas soluciones son capaces de sincronizar las contraseñas en la totalidad de nuestros dispositivos. Además son capaces de auditar si las contraseñas que estamos usando en nuestros servicios son seguras, etc.
3. Si nos falla la memoria en el momento de acceder a un servicio podemos acceder y consultar al gestor de contraseñas. De este modo podremos acceder al servicio de forma cómoda y rápida.

Por lo tanto los gestores de contraseñas proporcionan comodidad al usuario, pero tenemos que tener en cuenta lo siguiente:

1. La contraseña que usemos para cifrar el archivo que contiene la totalidad de nuestras contraseñas tiene que ser extremadamente fuerte. Si un atacante se hace con nuestro archivo y consigue averiguar la contraseña, tendrá acceso a la totalidad de nuestros servicios.
2. Si el archivo que almacena nuestras contraseñas está ubicado en un servidor de terceros o en la nube hay que tener precaución. En este caso tendremos que tener confianza que el servicio que almacena nuestras contraseñas es seguro y fiable.

Algunos gestores de contraseñas que pueden probar son Kepass, LastPass, Revelation o OnePassword. Si investigan un poco encontrarán información sobre estos u otros gestores.

### Hay servicios en que es extremadamente importante usar una contraseña segura

Hay que ser consciente que hay servicios en que disponer de una buena contraseña es muy importante.

Una contraseña muy importante es la de nuestro correo. Normalmente usamos nuestro correo para registrarnos a la totalidad de servicios que usamos. En el caso que uno de estos servicios sea comprometido siempre podremos usar el correo para recuperar el control del servicio. Por lo tanto en el correo es extremadamente importante seleccionar una contraseña robusta, activar la autenticación en 2 pasos, etc.

Otro ejemplo de contraseña crucial es nuestra cuenta de Google. Si alguien nos roba nuestra cuenta de Google perderemos información valiosa como por ejemplo fotos, emails, datos almacenados en Google Drive, etc.

Por lo tanto en este tipo de servicios críticos es extremadamente importante seguir la totalidad de consejos mencionados en este artículo.

### Usar equipos de confianza para acceder a servicios críticos

Hay que usar un equipo seguro para acceder a nuestras cuentas bancarias o a un servicio que sea crítico. Eviten usar  equipos públicos que pueden encontrar en bares, bibliotecas, hoteles, etc.

El riesgo que un equipo público o que no controlamos esté comprometido es mucho más elevado que en un equipo que es de nuestra confianza. A modo de ejemplo, un atacante podría infectar la totalidad de equipos de un cibercafé con un keylogger.

### Implementar un procedimiento para una correcta gestión de las contraseñas

En el ámbito empresarial es importante que existan procedimientos que definan claramente los siguientes aspectos:

1. Disponer de un listado de la totalidad de equipos y servicios que requieren de una clave de acceso.
2. Forzar que los sistemas de autenticación solo acepten claves seguras y que se tengan que ir cambiando periódica y regularmente.
3. Que las claves de acceso tengan definido un periodo de validez máximo.
4. Disponer de un sistema y metodologia para distribuir y almacenar las claves de acceso.
5. Etc.

## FUENTES USADAS PARA ELABORAR EL ARTÍCULO

Gran parte del contenido de este artículo ha sido extraído del podcast número 25 de [bitácora de ciberseguridad](https://avpodcast.net/ciberseguridad/). Les ánimo a escuchar este podcast.
