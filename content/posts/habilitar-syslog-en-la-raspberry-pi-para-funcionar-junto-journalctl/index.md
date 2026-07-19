---
title: "Habilitar Syslog en la Raspberry Pi para funcionar junto journalctl"
date: 2024-05-26
categories: 
  - "linux-2"
  - "raspberry-pi"
tags: 
  - "logs"
  - "syslog"
coverImage: "Habilitar-Syslog-en-la-Raspberry-Pi.webp"
---

Después de realizar una instalación mínima de Raspbian OS en mi Raspberry Pi, observé que el [sistema de logs]({{< relref "/posts/logs-en-linux" >}}) `syslog` estaba desactivado. Desde la versión Raspbian Buster, solo viene activado `journalctl`, ya que se considera un sistema más moderno y eficiente que ayuda a evitar el consumo de recursos. Si estás buscando habilitar syslog en la Raspberry Pi, este artículo te mostrará cómo hacerlo paso a paso.

<!--more-->

## ¿Por qué habilitar Syslog en Raspberry Pi?

A pesar de las ventajas de `journalctl`, muchas aplicaciones aún requieren `syslog` para funcionar correctamente. Un ejemplo es la herramienta `fail2ban`, que de forma predeterminada no puede operar sin `syslog`. Esto puede generar varios inconvenientes si no está habilitado.

## Pasos para habilitar Syslog en Raspbian OS

Si te has encontrado en una situación similar y necesitas habilitar `syslog`, solo tienes que instalar el paquete `inetutils-syslogd`. Sigue estos sencillos pasos:

1. Abre la terminal en tu Raspberry Pi.
2. Ejecuta el siguiente comando para instalar el paquete `inetutils-syslogd`:

> ```shell
> sudo apt install inetutils-syslogd
> ```

Después de instalar el paquete, el registro de logs volverá a funcionar del mismo modo que en versiones anteriores de Raspbian.

Con esta instalación, tendrás `journalctl` y `syslog` funcionando de forma simultánea, lo que te permitirá aprovechar las ventajas de ambos sistemas de registro.

## Beneficios de habilitar Syslog

Habilitar `syslog` en tu Raspberry Pi no solo garantiza la compatibilidad con aplicaciones más antiguas, sino que también te permite:

- **Mejorar la seguridad**: Herramientas como `fail2ban` dependen de `syslog` para bloquear IPs maliciosas basándose en los registros de intento de acceso.
- **Compatibilidad**: Muchas aplicaciones y scripts están diseñados para trabajar con `syslog`, por lo que habilitarlo asegura su correcto funcionamiento.
- **Monitoreo**: Con `syslog` habilitado, puedes enviar registros a servidores externos para un monitoreo centralizado.

## Conclusión

Aunque `journalctl` es una herramienta moderna y eficiente, habilitar `syslog` en tu Raspberry Pi puede ser útil para asegurar la compatibilidad y el correcto funcionamiento de diversas aplicaciones. La instalación del paquete `inetutils-syslogd` es sencilla y rápida, y te permitirá disfrutar de lo mejor de ambos mundos en términos de registro y monitoreo de logs.

Si tienes alguna duda o quieres compartir tu experiencia habilitando `syslog` en tu Raspberry Pi, deja un comentario a continuación.
