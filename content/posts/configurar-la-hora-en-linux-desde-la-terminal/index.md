---
title: "Configurar la hora en Linux desde la terminal mediante NTP"
date: 2020-11-22
categories: 
  - "linux-2"
tags: 
  - "configuracion"
  - "hora"
  - "ntp"
cover:
  image: "images/configurar-la-hora-en-linux-desde-la-terminal.png"
  relative: true
---

Es posible que en el primer momento que creamos una instancia en un servidor VPS no tenga la hora configurada de forma correcta. Este puede ser un problema en determinadas situaciones como por ejemplo cuando queremos programar la ejecución de una tarea a una determinada hora. Para solucionar este problema y configurar la hora en Linux de forma correcta procederemos del siguiente modo.<!--more-->

## CONSULTAR LA HORA QUE TENEMOS CONFIGURADA EN NUESTRO EQUIPO

Para averiguar si la hora está configurada correctamente ejecutaremos el comando `timedatectl`. El resultado obtenido en mi caso es el siguiente:

> **`ubuntu@netherlands:~$ timedatectl                Local time: sáb 2020-11-14 20:02:48 UTC            Universal time: sáb 2020-11-14 20:02:48 UTC                  RTC time: sáb 2020-11-14 20:02:49                     Time zone: Etc/UTC (UTC, +0000)        System clock synchronized: yes                                       NTP service: active                                RTC in local TZ: no`** 

Si os fijáis en la salida del comando el servicio [NTP](https://es.wikipedia.org/wiki/Network_Time_Protocol "Explicación del protocolo NTP") está instalado y está sincronizando la hora. No obstante hay un problema porque la hora local y la hora universal coinciden. Mi hora local son las 21:02 horas y en la salida del comando me está mostrando que son las 20:02. Además también observamos que la zona horaria está mal establecida.

## COMPROBAR QUE EL SERVICIO NTP ESTÁ INSTALADO Y ESTÁ ACTIVO

Para solucionar el problema que acabamos de ver, o para solucionar cualquier otro problema relacionado con la configuración de la hora, lo primero que hay que realizar es asegurar que el servicio NTP (Net Time Protocol) está instalado. Para ello ejecutaremos el siguiente comando en la terminal:

> ```shell
> ubuntu@netherlands:~$ sudo apt install ntp
> ```

Acto seguido aseguramos que el servicio NTP está activado ejecutando el siguiente comando:

> ```shell
> ubuntu@netherlands:~$ sudo timedatectl set-ntp on
> ```

**Nota:** El servicio NTP se está ejecutando constantemente y es capaz de controlar la hora del sistema con una precisión de nanosegundos. Su función es conectarse a un servidor externo de tiempo y sincronizar la hora del servidor con la de nuestro equipos.

## CONFIGURAR LA HORA EN LINUX Y LA ZONA HORARIA

Una vez estamos seguros que el servidor NTP está instalado y está activo configuraremos la zona horaria ejecutando el siguiente comando:

> ```shell
> sudo dpkg-reconfigure tzdata
> ```

Justo después de ejecutar el comando tendremos que seleccionar nuestra zona geográfica y la ciudad o región donde vivimos. En mi caso como zona geográfica seleccionaré `Europa` y como ciudad de residencia elegiré la más cercana a donde vivo que en mi caso es `Madrid`.

> ```shell
> ubuntu@netherlands:~$ sudo dpkg-reconfigure tzdata
> debconf: no se pudo inicializar la interfaz: Dialog
> debconf: (No hay ningún programa tipo dialog instalado, así que no se puede usar la interfaz basada en «dialog». at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 76.)
> debconf: probando ahora la interfaz: Readline
> Configurando tzdata
> -------------------
> 
> Por favor elija el área geográfica donde reside. Las siguientes preguntas de la
> configuración se ajustarán a ésta presentando una lista de ciudades, representando
> las zonas horarias en las que están localizadas.
> 
>   1. África     4. Australia  7. Atlántico   10. Pacífico  13. Etc
>   2. América    5. Ártico     8. Europa      11. SystemV
>   3. Antártida  6. Asia       9. India       12. EEUU
> Área geográfica: 8
> 
> Por favor, elija la ciudad o región correspondiente a su zona horaria.
> 
>   1. Ámsterdam   14. Copenhague    27. Londres     40. Riga        53. Ulyanovsk
>   2. Andorra     15. Dublín        28. Luxemburgo  41. Roma        54. Uzhgorod
>   3. Astrakhan   16. Gibraltar     29. Madrid      42. Samara      55. Vaduz
>   4. Atenas      17. Guernsey      30. Malta       43. San Marino  56. Vaticano
>   5. Belfast     18. Helsinki      31. Mariehamn   44. Sarajevo    57. Viena
>   6. Belgrado    19. Isla de Man   32. Minsk       45. Saratov     58. Vilna
>   7. Berlín      20. Estambul      33. Mónaco      46. Simferopol  59. Volgogrado
>   8. Bratislava  21. Jersey        34. Moscú       47. Skopie      60. Varsovia
>   9. Bruselas    22. Kaliningrado  35. Nicosia     48. Sofía       61. Zagreb
>   10. Bucarest   23. Kiev          36. Oslo        49. Estocolmo   62. Zaporozhye
>   11. Budapest   24. Kirov         37. París       50. Tallin      63. Zúrich
>   12. Büsingen   25. Lisboa        38. Podgorica   51. Tirana
>   13. Chisinau   26. Liubliana     39. Praga       52. Tiraspol
> Zona horaria: 29
> 
> 
> Current default time zone: 'Europe/Madrid'
> Local time is now:      sáb 14 nov 2020 21:15:40 CET.
> Universal Time is now:  Sat Nov 14 20:15:40 UTC 2020.
> ```

## COMPROBACIÓN FINAL QUE LA HORA ESTÁ BIEN CONFIGURADA

Una vez hayamos seguido los pasos descritos volveremos a usar el comando `timedatectl` para comprobar que la configuración de la hora es correcta. En mi caso ahora los resultados son los siguientes:

> ```shell
> ubuntu@netherlands:~$ timedatectl
>                Local time: sáb 2020-11-14 21:16:19 CET  
>            Universal time: sáb 2020-11-14 20:16:19 UTC  
>                  RTC time: sáb 2020-11-14 20:16:20      
>                 Time zone: Europe/Madrid (CET, +0100)
> System clock synchronized: yes                          
>               NTP service: active                       
>           RTC in local TZ: no
> ```

1. La hora Local, Universal y RTC son correctas.
2. La zona horaria es Europa/Madrid. Por lo tanto es correcta.
3. El servicio NTP está activado y se indica que la hora del sistema está sincronizada.la hora sincronizada.

Por lo tanto la configuración es correcta y todo está funcionando a la perfección. Por lo tanto han visto que configurar la hora en Linux desde de la terminal es relativamente fácil.
