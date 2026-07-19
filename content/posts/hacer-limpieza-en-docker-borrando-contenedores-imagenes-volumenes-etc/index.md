---
title: "Hacer limpieza en Docker borrando contenedores, imágenes, volúmenes, etc"
date: 2023-02-05
categories: 
  - "docker"
tags: 
  - "docker"
  - "optimizacion"
coverImage: "hacer-limpieza-en-docker-borrando-contenedores-imagenes.png"
---

A medida que vamos instalando y desinstalando servicios en Docker se va llenando el disco duro con contenedores, imágenes, redes, cache y volúmenes de persistencia que no usamos. Para solucionar este problema y para hacer un poco de limpieza en Docker y en el sistema operativo podemos borrar los contenedores, imágenes, redes y volúmenes que no usamos del siguiente modo.<!--more-->

## LISTAR LA TOTALIDAD DE CONTENEDORES, IMÁGENES, REDES Y VOLÚMENES EXISTENTES EN SU EQUIPO

Lo primero a realizar es tomar la decisión de los servicios que queremos mantener en nuestro equipo. Para ello listaremos la totalidad de contenedores existentes usando el comando `docker ps -a`.

```shell
ubuntu@ubuntuamd-22-04:~$ docker ps -a
CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS                      PORTS                                                                                                                 NAMES
6caee85da9be   atareao/ytpodcast:v0.2.0                  "/bin/sh /app/entryp…"   5 weeks ago    Exited (143) 5 weeks ago                                                                                                                          ytpodcast
1a0b0a2c428c   benbusby/whoogle-search:latest            "/bin/sh -c 'misc/to…"   6 weeks ago    Exited (137) 2 weeks ago                                                                                                                          whoogle-search
2e60ca157c11   heussd/fivefilters-full-text-rss:latest   "docker-php-entrypoi…"   6 weeks ago    Up 6 days                   0.0.0.0:8086->80/tcp, :::8086->80/tcp                                                                                 fivefilters-full-text-rss-docker_fullfeedrss_1
9a21f98188c3   lscr.io/linuxserver/freshrss:latest       "/init"                  6 weeks ago    Exited (0) 6 weeks ago                                                                                                                            freshrss
7d13ac4fbffa   ttrss-docker_web-nginx                    "/docker-entrypoint.…"   7 months ago   Exited (0) 7 months ago                                                                                                                           ttrss-docker_web-nginx_1
db5c5c3df11a   ttrss-docker_updater                      "/opt/tt-rss/updater…"   7 months ago   Exited (137) 7 months ago                                                                                                                         ttrss-docker_updater_1
9bd7aa3155de   ttrss-docker_app                          "/bin/sh -c ${SCRIPT…"   7 months ago   Exited (0) 7 months ago                                                                                                                           ttrss-docker_app_1
a290bcf74b13   ttrss-docker_backups                      "/opt/tt-rss/dcron.s…"   7 months ago   Exited (137) 7 months ago                                                                                                                         ttrss-docker_backups_1
c37bb8d245a4   postgres:12-alpine                        "docker-entrypoint.s…"   7 months ago   Exited (0) 7 months ago                                                                                                                           ttrss-docker_db_1
1dc4ec9850c2   534c29cb433f                              "/opt/tt-rss/updater…"   7 months ago   Created                     9000/tcp                                                                                                              ttrss_updater_1
b852f1c8649a   534c29cb433f                              "/opt/tt-rss/dcron.s…"   7 months ago   Created                     9000/tcp                                                                                                              ttrss_backups_1
03c247c8f047   104294920402                              "/opt/tt-rss/updater…"   7 months ago   Created                                                                                                                                           03c247c8f047_ttrss_updater_1
a8bcae879e73   104294920402                              "/opt/tt-rss/dcron.s…"   7 months ago   Created                                                                                                                                           a8bcae879e73_ttrss_backups_1
1f6f2f0a3db7   shaarli/shaarli:master                    "/bin/s6-svscan /etc…"   7 months ago   Up 6 days                   0.0.0.0:900->80/tcp, :::900->80/tcp                                                                                   shaarli_shaarli_1
96df86a5f8e9   traefik:latest                            "/entrypoint.sh trae…"   7 months ago   Up 6 days                   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   traefik
```

Para ver las imágenes que tenemos en el equipo lo haremos con el comando `docker images` . En mi caso tengo las siguientes imágenes:

```shell
ubuntu@ubuntuamd-22-04:~$ docker images
REPOSITORY                         TAG         IMAGE ID       CREATED         SIZE
benbusby/whoogle-search            latest      134cd16eba2f   6 weeks ago     118MB
atareao/ytpodcast                  latest      417124ad520b   7 weeks ago     174MB
atareao/ytpodcast                  v0.2.0      417124ad520b   7 weeks ago     174MB
heussd/fivefilters-full-text-rss   latest      482a8f60ef3c   7 weeks ago     370MB
lscr.io/linuxserver/freshrss       latest      47ed67d52b1d   8 weeks ago     115MB
ttrss-docker_web-nginx             latest      34bf30b46f1e   7 months ago    23.4MB
ttrss-docker_app                   latest      fb6c071600c3   7 months ago    98MB
ttrss-docker_backups               latest      fb6c071600c3   7 months ago    98MB
ttrss-docker_updater               latest      fb6c071600c3   7 months ago    98MB
postgres                           12-alpine   3496e5bc48fb   8 months ago    211MB
traefik                            latest      22a478a269ff   8 months ago    105MB
nginx                              alpine      b1c3acb28882   8 months ago    23.4MB
shaarli/shaarli                    latest      ad6af6c7baf3   10 months ago   100MB
alpine                             3.14        e04c818066af   10 months ago   5.59MB
heussd/git                         latest      c737e804560c   15 months ago   25.2MB
shaarli/shaarli                    master      cea4a66d0807   18 months ago   97.6MB
php                                5-apache    24c791995c1e   4 years ago     355MB
```

Ahora listaremos las redes mediante el comando `docker network ls`:

```shell
ubuntu@ubuntuamd-22-04:~$ docker network ls
NETWORK ID     NAME                 DRIVER    SCOPE
0882b3274c3e   bridge               bridge    local
46c682f88f61   host                 host      local
b95af41127b7   none                 null      local
783c64e58f2e   shaarli_http-proxy   bridge    local
e2b8ea1140e7   web                  bridge    local
```

Finalmente listaremos los volúmenes existentes mediante el comando `docker volume ls`

```shell
ubuntu@ubuntuamd-22-04:~$ docker volume ls
DRIVER    VOLUME NAME
local     1bc0f6795d5fd41609ddf0b434742b6e1b420ba582aa149aab12ca29b2798b95
local     76a8a0f8e4b451ca527d4b17c150f0dfbafa171482e29d8eda2acaaf9eb95a3c
local     994735b3b8a40d977aaf62ade792c87af79ac5679971b8ca3eee5d4784afee86
local     fivefilters-full-text-rss-docker_rss-cache
local     full_text_rss_rss-cache
local     shaarli_cache
local     shaarli_data
local     shaarli_shaarli-cache
local     shaarli_shaarli-data
local     shaarli_traefik-acme
local     ttrss-docker_app
local     ttrss-docker_backups
local     ttrss-docker_certs
local     ttrss-docker_db
local     ttrss_app
local     ttrss_backups
local     ttrss_certs
local     ttrss_db
```

## DEFINIR LOS SERVICIOS QUE QUERÉIS USAR Y LOS QUE QUERÉIS BORRAR COMPLETAMENTE

Si os fijáis en las salidas de los comandos aplicados en el apartado anterior vemos que tengo instalados los siguientes servicios:

1. Ytpodcast
2. Whoogle
3. Five filters full text rss
4. Tiny Tiny RSS
5. FreshRSS
6. Shaarli
7. Traefik

De la totalidad de servicios citados los únicos que utilizo y quiero utilizar en este servidor son:

1. Five filters full text rss
2. Shaarli
3. Traefik

Por lo tanto a continuación veremos como borrar todo rastro de los servicios ytpodcast, Whoogle, Tiny Tiny RSS y FreshRSS.

## PARAR LA TOTALIDAD DE CONTENEDORES DOCKER

El primer paso para iniciar el proceso de limpieza y optimización del sistema es parar la totalidad de contenedores. Para ello ejecutaremos el siguiente comando en la terminal:

```shell
ubuntu@ubuntuamd-22-04:~$ docker stop $(docker ps -a -q)
6caee85da9be
1a0b0a2c428c
2e60ca157c11
9a21f98188c3
7d13ac4fbffa
db5c5c3df11a
9bd7aa3155de
a290bcf74b13
c37bb8d245a4
1dc4ec9850c2
b852f1c8649a
03c247c8f047
a8bcae879e73
1f6f2f0a3db7
96df86a5f8e9
```

## LEVANTAR LOS SERVICIOS DOCKER QUE QUIERO USAR

Una vez parados todos los contenedores levantaremos únicamente los contenedores que queremos usar y mantener en el equipo. Para ello en mi caso ejecutaré el siguiente comando en la terminal:

```shell
ubuntu@ubuntuamd-22-04:~$ docker start traefik shaarli_shaarli_1 fivefilters-full-text-rss-docker_fullfeedrss_1 
traefik
shaarli_shaarli_1
fivefilters-full-text-rss-docker_fullfeedrss_1
```

Una vez levantados los contenedores es muy importante asegurar que los servicios levantados funcionan correctamente.

## REALIZAR LIMPIEZA EN DOCKER Y BORRAR LOS CONTENEDORES, IMÁGENES, REDES Y VOLÚMENES DE PERSISTENCIA QUE NO NECESITAMOS

Docker dispone de varios comandos para realizar limpieza de nuestro sistema operativo. Algunos de estos comandos que pueden usar son los siguientes:

| Comando | Función |
| --- | --- |
| `docker system prune -a` | Eliminar los contenedores detenidos, todas las imágenes detenidas y redes que no están siendo utilizadas. |
| `docker system prune --volumes -a` | Eliminar los contenedores detenidos, todas las imágenes detenidas, redes que ne se usan y volúmenes de persistencia que no están siendo no utilizadas. |

En mi caso no quiero dejar rastro algúno de los servicios que borraré. Por lo tanto el comando que usaré para realizar limpieza de Docker es `docker system prune --volumes -a`

**Nota:** El proceso que veremos a continuación borrará la totalidad de imágenes, contenedores, volúmenes de persistencia y redes que no están en uso o están colgadas.

El resultado obtenido después de ejecutar el comando de limpieza ha sido el siguiente:

```shell
ubuntu@ubuntuamd-22-04:~$ sudo docker system prune --volumes -a
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all volumes not used by at least one container
  - all images without at least one container associated to them
  - all build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
6caee85da9bec084c74e2552cf45a5c0ef983478bdbef33fc21aa09b4664e154
1a0b0a2c428cca8f6bb6c5fe354d31bf8f4e1a983b41af3e4e88d3b2fa986c79
9a21f98188c3e5a54328fb6399ed38a07f853c996300971ed8650553157dc834
7d13ac4fbffa661669bffc262f10a06a206c11d7392a31c77bb7ff44566017ff
db5c5c3df11ae598f88e543489f35398795de3c9003be09a33574662d1bc2535
9bd7aa3155de56e89b09d3cbea9e9874483d12d8e6bbb5b25cda6ae95c3426d6
a290bcf74b135b263fbfef1a6c3260eea204570cbddff28d578cde8265bbbb21
c37bb8d245a487d25039da371da946ca66eeac9c39c1f57212fed83dde694332
1dc4ec9850c22f3842210b823075851b872aea57605b1f5ae6ccc42ba5435813
b852f1c8649a76a8388b7dfa7a585f58598975a99ed843ddc2c4d5f1057e0144
03c247c8f0478afb9dbf680428546a3c761cbe2cc5d8e278c7b8bbd9b1891df7
a8bcae879e735afd4cb42844afddf3f26f52e2de10070202d12647718a0fbbcf

Deleted Networks:
shaarli_http-proxy

Deleted Volumes:
shaarli_shaarli-cache
ttrss-docker_app
ttrss_certs
ttrss-docker_db
ttrss_app
full_text_rss_rss-cache
shaarli_data
ttrss-docker_backups
1bc0f6795d5fd41609ddf0b434742b6e1b420ba582aa149aab12ca29b2798b95
76a8a0f8e4b451ca527d4b17c150f0dfbafa171482e29d8eda2acaaf9eb95a3c
shaarli_traefik-acme
ttrss-docker_certs
ttrss_backups
ttrss_db
994735b3b8a40d977aaf62ade792c87af79ac5679971b8ca3eee5d4784afee86
shaarli_cache
shaarli_shaarli-data

Deleted Images:
untagged: ttrss-docker_web-nginx:latest
deleted: sha256:34bf30b46f1edd91c9864ff46822df87e8ba8f8eca96dcb0d0094ef55cc57f48
deleted: sha256:97ae92fdbc1077fa635b8976a50fdf77c590ce44318b8f872b985bac4b4f91ea
deleted: sha256:d945a660a23cba42c7679fcec25675d4f703d8ff12008150320cfd78434031ac
untagged: php:5-apache
untagged: php@sha256:0a40fd273961b99d8afe69a61a68c73c04bc0caa9de384d3b2dd9e7986eec86d
deleted: sha256:24c791995c1e498255393db857040793a7a040fa0f8ddd4bbe8fb230648e37d7
untagged: ttrss-docker_app:latest
untagged: ttrss-docker_backups:latest
untagged: ttrss-docker_updater:latest
deleted: sha256:fb6c071600c3b3e1c45dc04a9f44fa9f614bad186608a21594698d8b108ec9fe
deleted: sha256:ec09d3bb5b418b5cb65b44fcd9c0a5f37278a9a6feb0662a242f3fbf22098f64
deleted: sha256:205e6156d55d202bc84e6906d3ce241c3847f0b2f182bc632921be8f44260361
deleted: sha256:4e79c7e2ce21585b50a2c27aac7b1039b877f032c685504603e35cec3f51e08a
deleted: sha256:5cab0a002e563bcd96f7cf541e43d77819a8b0be707b078f2367f4f3de4606a4
deleted: sha256:f1edb05ab0258506ce439898340b524123f7a49466e9bb98428e13b149c25068
deleted: sha256:46baed5ded025fdf32b5443d180b78183464c0a65ea7c6ed6366810d20076975
deleted: sha256:4b9136d87555675badd9a3927819ce451389e0af1302812966148971fbad50f7
deleted: sha256:3a2f708a97c886e69f32251ee7bfdf1b0033d9e2634a3b4124821bd7c61b402f
deleted: sha256:5b1cbec40d3c23f8297dfb46cca0b085450ca4faedfa811a49580710e415ccec
deleted: sha256:ae1b070e77ada7c80f5e49fc0a480a4a93b1ac8f5d71352764449d320c1a0ddc
deleted: sha256:8381005aa0dda5beb147dce7f9d08f61bd3a6398500fc45cc684944e46ff8e6d
deleted: sha256:10f4e34929cfa8aa69943eb770ed887a052ba17de88946af7ef3a46c20b6f20e
deleted: sha256:05bf1582fe24ac8bf18176aa130f8caeb7441bc3f5ebc5636db436c22e5072ae
deleted: sha256:8eb9de592eaa0450aeffe2066ac7694e265806253984a436b5a5d5e4dd6db43f
deleted: sha256:f0052eefaa751b9d003172d1da5ccf9a7e8605ed04bc854a0ee367fd7fc2e215
deleted: sha256:23c93b630f15d36cb6c8a98dab45317be3ae5a6dfb54d11498e7b0cb9517df8a
deleted: sha256:e85ebb06e09b73948292b66b9f687b6bc84b9d32a26a1aad267d833b130e40ea
deleted: sha256:95f4833cd92887212ccb3784f3257fa937efb5b698ad3bf9538e870a2abebc6d
deleted: sha256:24c942aad3094de14de814b1cd8f14600934971f9522a711653c17735fed9d28
deleted: sha256:533f7e96f2d357135c40f2a719deb4daef3ebd3cb44046bdec01cab038fffa87
deleted: sha256:a99eb632af01c452844f1cc62653f1e2c4cf1e7d146f1aeff4de42b5ced96ab8
deleted: sha256:42d08096432769df01a8bed00c7abb8a514b5ef09ca0cf7648d2c599bc033998
deleted: sha256:e1ef2fa84fe09bae27da96d2ae7f889f522f329e289526658ea5100b31e67e1e
deleted: sha256:7daba31225658871a225ccb40f025f0b0b1966deb3be70f680e6e24ae563630a
deleted: sha256:626282ff5adc69f03f357b61bcde2bbe13d9da642d1874d7c71f44469b89b101
deleted: sha256:5e8a2ab155510ce403449e80fbfa0dba3a71c5f1419f4296845c2577fd6867bf
deleted: sha256:f4f0fdddfa46e77c554a2da2a89ac870ad0a112ccb1c74e152e403c7c1b1af33
deleted: sha256:10a14fd63ed72e2d3883a56a6493105b19a6185ba9994c68256104967ca3d719
deleted: sha256:8f673d8d2960235bdc842ae5e88a809f3df04a5a6b4f8f85ac8c988a6643ad52
deleted: sha256:ba2edf286f993ae92e97c5d1bebe8c98a0c327fad9bc1f81e4f92024b97f5101
deleted: sha256:aa3c5d98da2b18ad83720c87fb7ab4e43cb9eaa5dbcd0dcc5da793e69ca4dce7
deleted: sha256:8a5caf5eae7218de6470d179c81f7b19592d6b94ea521cd389037119074e2e71
deleted: sha256:be29a72def69dea15d1fd0f5e5ae1dad1feed8a6efa3c1fec6cf92e45e414bcb
deleted: sha256:fb4874070fc50b8f6ffb048c7e1fa28e15d832495a2f60e3f2a305221f462ba8
deleted: sha256:4731acb0adfe4c72e9e163d742975a44a859e8b2dc92cf5ead14a5e012a4941b
untagged: atareao/ytpodcast:latest
untagged: atareao/ytpodcast:v0.2.0
untagged: atareao/ytpodcast@sha256:65ebc0a9d50741dc3c50345b7064315d078739ef05f242d075170938cc93b128
deleted: sha256:417124ad520b2408f5ea4bf503491a8be8e5010486ed3e305dab0f8610e8bdd5
deleted: sha256:f8a54da710a2827be5fc94eeed315c55afebc9ccfc64c0e97434687b871ba7c7
deleted: sha256:1f9eb09e2faee1191aab26a39c42d24801cedaed345c4985ce57a5226b0060a6
deleted: sha256:5c50e8171f0a4337714affc4da0a41e46d6099edc05292ffcb77491bc8fde48b
deleted: sha256:7f3e6bfca2ef1b153188cb0995d005d09e42086c30d32e128da0001f2d8ae7ad
deleted: sha256:a26b542b2b46a3d779f4079b98935472a9e4e75c2cdb0cf782f3502e1387a1b9
deleted: sha256:e5e13b0c77cbb769548077189c3da2f0a764ceca06af49d8d558e759f5c232bd
untagged: shaarli/shaarli:latest
untagged: shaarli/shaarli@sha256:813a6e0acf70f8eefb70afb319acd4762716d3f839cdddac28a7283c3a95fa11
deleted: sha256:ad6af6c7baf36b9f2652842b7c6fcf30f2d38b5a3338969145d8cc4b560fa593
deleted: sha256:27fd7b0630cd62476c8ae41e6ae2389f27a70a8288f65b74900932b29757b08d
deleted: sha256:ed13260e5d9fc05f5ddfda66fac5a4e709da97d7bc949a41ae63f301aab096f4
deleted: sha256:45061d3e3c186e69b1ad7bac9cb7d851510f792b6fa6d08b4968f425ce0b4eeb
deleted: sha256:48cc47fffbf8ec8cc08d053b7f325bab445d9021c8dcbb19552e08c930031d6a
deleted: sha256:4f351e758e9c2f161ba38bc1dd7f422699b97fbe3fb2b40e8ebd1677cd511452
deleted: sha256:d4d9337f9490ccd97d7986a706394b154487c1032cc8d048aa9790cf6aa92a32
deleted: sha256:e2efd9423be749107024d89f24425cb0abb5af16bfc9b833417414836e826bc8
deleted: sha256:be8bde508a17da7a361d91a9737af22cbc4f1f992bf1a0c4e078ebd9494208d7
untagged: heussd/git:latest
untagged: heussd/git@sha256:e5dbe550343e5f80bd64ce747a8780cc88f4c6a3972aa84833f7a3e34f25ef58
deleted: sha256:c737e804560c01373e803f3f41e402377c8d97eab409e761f027987419265886
deleted: sha256:242b195a4c587cbe32262ffafa8bb6c5da517f3d664d93e00b1074ff5cec539e
deleted: sha256:47b9d22c7a3e6d4849a5a1d8d58691e41ed995a12f4d8e9430e5031b76cf5967
deleted: sha256:e2eb06d8af8218cfec8210147357a68b7e13f7c485b991c288c2d01dc228bb68
untagged: alpine:3.14
untagged: alpine@sha256:06b5d462c92fc39303e6363c65e074559f8d6b1363250027ed5053557e3398c5
deleted: sha256:e04c818066afe78a0c9379f62ec65aece28566024fd348242de92760293454b8
deleted: sha256:b541d28bf3b491aeb424c61353c8c92476ecc2cd603a6c09ee5c2708f1a4b258
untagged: nginx:alpine
untagged: nginx@sha256:a74534e76ee1121d418fa7394ca930eb67440deda413848bc67c68138535b989
deleted: sha256:b1c3acb28882519cf6d3a4d7fe2b21d0ae20bde9cfd2c08a7de057f8cfccff15
deleted: sha256:1e82c6d6bb97ec37fdaf16a3578db9f79efc2a7fa987875e259148857265b410
deleted: sha256:fb462cfb3519855804e8462cd393197394587a9783c1f705b65f6f60469fec8d
deleted: sha256:89f0dcdafc41c1feb58d632fac93739b74a6f1ae6a2ca26d3a7278707e22e98e
deleted: sha256:02f69f6c362c5b7bba5e59ade6509c10cdf2454fbe1e28867ade6e16a40704b7
deleted: sha256:922eb96b3e3938899bf43813740b2eb24410ed08e1f9bf519b3d6f74be7b033a
untagged: lscr.io/linuxserver/freshrss:latest
untagged: lscr.io/linuxserver/freshrss@sha256:fb9429a3a476e06219cd291bbdbfe06d6cc5ec1187013700e9d8df992dcf0c89
deleted: sha256:47ed67d52b1d016a1cb4c8f6cf756156695583bb8e1bb482d5fae49c8e45c4be
deleted: sha256:57c121847d64b258b7b1b58a66c2dde986a69e134167c0909f511b1829944b33
deleted: sha256:daa4a32bb8a498088d66c9e32a26ccf395f6ebe71acf90c9984a59611a3566b0
deleted: sha256:ec12b8d48ffef6dab69b9e8f6e5739dd180762269369350562eec79fcfc8b8f6
deleted: sha256:711dac113ce0a63a88d700193031fd75781362b40a48f6780ea143fec8724637
deleted: sha256:7a75295ba6eea0fc6705baf198364610352bbda9a9fd8e50ae7177cf557110e2
deleted: sha256:e5d01b7462514b5b4f06e1530e7f29acd11e6abc13f0f72b8efdf5232641ccce
deleted: sha256:c79211a3264159ea867f770ee86d3f1ed8da9a00dbbba065f2144ebf4269e6c3
untagged: postgres:12-alpine
untagged: postgres@sha256:0ebf2918e06fbc0ff9f30e144253972050f219a3652054017ebb5670f39d2564
deleted: sha256:3496e5bc48fb8ba14bcf5ff1c1f7b9ed408efb820f880e74cf4769310a7029aa
deleted: sha256:c0f7926f77fb00fe77a7c09b85cb1068ca121893c0fbb979e75051ba15749741
deleted: sha256:3a536cdc1c3979728c1230827c206bfb60c86576fae4fa034faf87269cfc526a
deleted: sha256:17438a0c156f43978bfc0a34ef2e48d9b84ad19c27f54ff98e0f9b62be1d5898
deleted: sha256:f35943dd8056f5efadd23fd371f7e4f2981eb8dc0bb505c22ea80cf87256a206
deleted: sha256:31e6c6516a21e0767cb0c8b6713aec82ca715667fe4f63dab836827c8a5496a8
deleted: sha256:e1ae6e0a84dc9d520de5f0906555ed5c0dfde1dc56012bded5a0ef8839487f1e
deleted: sha256:2600a80197e0b0abd1aab08b2e6cd9ff51c75acc6ee93a082603796e3c421e86
deleted: sha256:24302eb7d9085da80f016e7e4ae55417e412fb7e0a8021e95e3b60c67cde557d
untagged: benbusby/whoogle-search:latest
untagged: benbusby/whoogle-search@sha256:81750d8f298e7884255affd54ae18b48f6484206943d2be7f63453a40bed071e
deleted: sha256:134cd16eba2f6c9bd3d2486374892c0b7ceb79f86da014beb4cbd2d416ec4ec6
deleted: sha256:63fa635f69bf0996abf3f72dd5842701391be09ad19ebf393a2e77d264668dc5
deleted: sha256:291463f11a88e4ce4b23ab6eca7b4e0c7665ca04b74dd63996e09839a6f8549a
deleted: sha256:9093f3fb759241498a33fe8c1cf3686251253aa85ea0f088e3c78376fff24f28
deleted: sha256:5b347a27b22d00a5209b11515e10ef7735eb7c2d153ba9d69756a97bc3a3182c
deleted: sha256:48c502cc31198a6e8c224e8e150168354ec07f7c216ed4388f2a5ea03815e538
deleted: sha256:0b72660782110783d1e6cb3cb5b78d09991bd284bb23beb5cc1bc861f027231d
deleted: sha256:92357ce01a91e2bb3eded68eae135e36d133d72a4c6de32680c7014b6f430adc
deleted: sha256:a07746f8a9933908839c0c9c36551fe2ab98fb4707c35501ea737c855dbe3749
deleted: sha256:ecefce4f3730d3e57821d06e2c25c66488abc5300745f9d1b9392b5c64de1b42
deleted: sha256:0c4b0448f52a8b8838091bcb1957542b34900f84a569d213e4a17ef0bfd33322
deleted: sha256:e2e624964abe8b97565713a53d08704471b84b7ae0aa3a9ac92b3d4b999215f1
deleted: sha256:263f1981ef81d928dfee35dbb3d0178bae6caf955b46e122b38a7c313b112aa6
deleted: sha256:a869a37a01e77a76c824b90700fb3c7eaede0ba28908977e24f7b63e85a555df
deleted: sha256:873f32a32d4cbb5c3431289e81e64b74409ac8937667cd049e794940bbb3948e
deleted: sha256:51eb2cdf4ae72a420b49e2fcd567c6b5bd3ba650238029f69bd7063181d41a64
deleted: sha256:596e2616800aef6b0f3957ccef06fcd348031a11fa69501edc1a6d2730eb9e98
deleted: sha256:15e77434263d53e40c7d184dedc74f5f606cd373ca4bf627c07fc11aa7064b37
deleted: sha256:8d3ac3489996423f53d6087c81180006263b79f206d3fdec9e66f0e27ceb8759

Total reclaimed space: 893.1MB
```

Si analizamos la salida del comando veremos la totalidad de información que se ha borrado. También veréis el espacio en disco que se ha ahorrado que en mi caso ha sido 893.1MB.

## LISTAR LOS CONTENEDORES, IMÁGENES, REDES Y VOLÚMENES PARA ASEGURAR QUE SE HA REALIZADO LA LIMPIEZA

Para comprobar la efectividad de las acciones emprendidas listaremos los contenedores instalados y en mi caso veremos que únicamente tengo 3 contenedores en el equipo.

```shell
ubuntu@ubuntuamd-22-04:~$ docker ps -a
CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS          PORTS                                                                                                                 NAMES
2e60ca157c11   heussd/fivefilters-full-text-rss:latest   "docker-php-entrypoi…"   6 weeks ago    Up 24 minutes   0.0.0.0:8086->80/tcp, :::8086->80/tcp                                                                                 fivefilters-full-text-rss-docker_fullfeedrss_1
1f6f2f0a3db7   shaarli/shaarli:master                    "/bin/s6-svscan /etc…"   7 months ago   Up 24 minutes   0.0.0.0:900->80/tcp, :::900->80/tcp                                                                                   shaarli_shaarli_1
96df86a5f8e9   traefik:latest                            "/entrypoint.sh trae…"   7 months ago   Up 24 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   traefik
```

Con las imágenes pasará exactamente lo mismo. Mientras antes teníamos 17 imágines ahora solo tenemos 3.

```shell
ubuntu@ubuntuamd-22-04:~$ docker images
REPOSITORY                         TAG       IMAGE ID       CREATED         SIZE
heussd/fivefilters-full-text-rss   latest    482a8f60ef3c   7 weeks ago     370MB
traefik                            latest    22a478a269ff   8 months ago    105MB
shaarli/shaarli                    master    cea4a66d0807   18 months ago   97.6MB
```

Referente a las redes vemos que se ha eliminado la red `shaarli_http-proxy`

```shell
ubuntu@ubuntuamd-22-04:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
0882b3274c3e   bridge    bridge    local
46c682f88f61   host      host      local
b95af41127b7   none      null      local
e2b8ea1140e7   web       bridge    local
```

Finalmente vemos que únicamente nos hemos quedado con un solo volúmen de persistencia.

```shell
ubuntu@ubuntuamd-22-04:~$ docker volume ls
DRIVER    VOLUME NAME
local     fivefilters-full-text-rss-docker_rss-cacheer volume ls
DRIVER    VOLUME NAME
local     fivefilters-full-text-rss-docker_rss-cache
```

Por lo tanto objetivo cumplido. Aplicando un simple compando hemos sido capaces de borrar la totalidad de contenedors, imágenes, volúmenes y redes que no necesitamos.

### Fuentes

[https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes-es](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes-es)

[https://docs.docker.com/engine/reference/commandline/system\_prune/](https://docs.docker.com/engine/reference/commandline/system_prune/)
