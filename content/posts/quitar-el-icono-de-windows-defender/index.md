---
title: "Quitar el icono de Windows Defender de la barra de tareas de Windows"
date: 2016-11-27
categories: 
  - "windows"
tags: 
  - "antivirus"
  - "configuracion"
  - "windows-defender"
coverImage: "Quitar-el-icono-de-Windows-Defender.png"
---

Quienes dispongan de Windows 10 observarán que en la barra de tareas les aparece el icono de Windows Defender. En el caso que les moleste, en este post veremos los pasos a seguir para quitar el icono de Windows Defender de la barra de tareas de Windows.<!--more-->

Antes de empezar es importante remarcar que eliminar el icono de Windows Defender no equivale a desactivar Windows Defender. Si únicamente eliminamos el icono, Windows Defender seguirá ejecutándose en segundo plano realizando su función.

###### Nota: Quien precise eliminar o desactivar Windows Defender debe seguir las instrucciones que se mencionan en el siguiente [enlace]({{< relref "/posts/desactivar-windows-defender-permanente" >}}).

## QUITAR EL ICONO DE WINDOWS DEFENDER DE LA BARRA DE TAREAS

Para quitar el icono de Windows Defender de la barra de Windows tenemos que acceder al administrador de tareas.

Para ello posicionamos el puntero del mouse encima de la barra de Windows, presionamos el botón derecho del mouse y cuando aparezca el menú desplegable clicamos encima de la opción **Administrador de tareas**.

[![Acceder al administrador de tareas](images/Acceder-al-administrador-de-tareas-256x300.png "Acceder al administrador de tareas")](images/Acceder-al-administrador-de-tareas.png)

En el administrador de tareas clicamos en la pestaña **Inicio**, buscamos y seleccionamos el proceso **Windows Defender notification icon**, presionamos el botón derecho del mouse y cuando aparezca el menú contextual clicamos encima de la opción **Deshabilitar**.

[![Deshabilitar Windows Defender notification icon](images/Eliminar-el-icono-de-Windows-defender.png "Deshabilitar Windows Defender notification icon")](images/Eliminar-el-icono-de-Windows-defender.png)

Una vez finalizado el proceso reiniciamos el ordenador y ya no nos aparecerá el icono de Windows Defender en la barra de tareas de Windows.

[![Barra de tareas sin Windows Defender](images/panel-sin-Windows-defender-300x128.png "Barra de tareas sin Windows Defender")](images/panel-sin-Windows-defender.png)

## HACER QUE VUELVA APARECER EL ICONO DE WINDOWS DEFENDER EN EL PANEL

Si algún día queremos volver a disponer del icono de Windows Defender tan solo tenemos que revertir los pasos realizados.

Para ello accedemos al administrador de tareas de Windows. Una vez dentro del administrador clicamos encima de la pestaña **Inicio**, buscamos y seleccionamos el proceso **Windows Defender notification icon**, presionamos el botón derecho del mouse y cuando aparezca el menú contextual clicamos encima de la opción **Habilitar**.

[![Reactivar el icono de Windows Defender](images/Reactivar-el-icono-de-Windows-Defender.png "Reactivar el icono de Windows Defender")](images/Reactivar-el-icono-de-Windows-Defender.png)

Después de habilitar el botón tan solo tenemos que reiniciar el ordenador. En el próximo arranque volveremos a disponer del icono de Windows Defender en la barra de tareas de Windows
