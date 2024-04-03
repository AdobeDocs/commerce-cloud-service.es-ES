---
title: Propiedad Crons
description: Consulte ejemplos sobre cómo configurar la propiedad crons en la variable [!DNL Commerce] archivo de configuración de la aplicación.
feature: Cloud, Configuration
exl-id: 67d592c1-2933-4cdf-b4f6-d73cd44b9f59
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 0%

---

# Propiedad Crons

Adobe Commerce utiliza el `crons` propiedad para programar actividades repetitivas. Es ideal para programar una tarea específica para que se ejecute a determinadas horas del día. Solo se puede ejecutar un trabajo cron a la vez en la instancia web para Adobe Commerce en proyectos de infraestructura en la nube debido a la naturaleza de los entornos de solo lectura. Se recomienda desglosar las tareas de larga duración en tareas más pequeñas en cola. Como alternativa, puede crear un [instancia de trabajo](workers-property.md).

El Adobe recomienda ejecutar `crons` como el [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html). Hacer _no_ correr `crons` as `root` o como usuario del servidor web.

Esta configuración es diferente de las implementaciones locales de Adobe Commerce, que tienen varios trabajos cron predeterminados. Consulte [Configuración de trabajos cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) en el _Guía de configuración_.

## Configuración de trabajos cron

El `crons` La propiedad describe los procesos que se activan según una programación. Cada trabajo requiere un nombre y las siguientes opciones:

- `spec`: la expresión cron utilizada para la programación.
- `cmd`: el comando en el que se ejecutará `start` y `stop`.
- `shutdown_timeout`—(_Opcional_) Si se cancela un trabajo cron, este es el número de segundos después de los cuales se envía una señal SIGKILL para detener el trabajo o proceso. El valor predeterminado es 10 segundos.
- `timeout`—(_Opcional_) Cantidad máxima de tiempo que un trabajo cron puede ejecutarse antes del tiempo de espera. El valor predeterminado es el máximo permitido de 86400 segundos (24 horas).

De forma predeterminada, cada proyecto de Commerce Cloud tiene el siguiente valor predeterminado `crons` configuración en la `.magento.app.yaml` archivo:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Si el proyecto requiere trabajos cron personalizados, puede agregarlos al predeterminado `crons` configuración. Consulte [Creación de un trabajo cron](#build-a-cron-job).

### `crontab`

Adobe Commerce agregó una opción de configuración de autocrons solo a proyectos Pro para admitir el autoservicio `crons` en los entornos de ensayo y producción. Si esta opción está activada, puede utilizar `crontab` para revisar la configuración de cron. Esto es _no_ disponible con Proyectos iniciales.

Aunque puede utilizar `crontab` para revisar la configuración en proyectos Pro, Adobe Commerce no utiliza `crontab` para ejecutar trabajos cron para sitios implementados en la infraestructura de la nube.

**Para revisar la configuración de cron en entornos Pro**:

1. Uso [SSH](../development/secure-connections.md#use-an-ssh-command) para iniciar sesión en el entorno remoto.

1. Enumerar los procesos cron programados.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Si la variable `crontab -l` El comando devuelve un `Command not found` error, debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para habilitar la opción de configuración de autoservicio auto-crons en su proyecto Pro.

El siguiente ejemplo muestra el `crontab` para un entorno que solo tiene el valor predeterminado `crons` configuración:

```terminal
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Creación de un trabajo cron

Un trabajo cron incluye la especificación de programación y tiempo y el comando para ejecutarse a la hora programada. Para entornos de inicio y Pro `integration` entornos, el intervalo mínimo es de una vez cada cinco minutos. Para los entornos de ensayo y producción profesionales, el intervalo mínimo es de una vez por minuto. En Adobe Commerce, en la infraestructura en la nube, se agregan trabajos cron personalizados al `.magento.app.yaml` archivo en el `crons` sección. El formato general es `spec` para programación y `cmd` para especificar el comando o el script personalizado que se va a ejecutar.

### Especificación

Adobe Commerce utiliza una expresión de cinco valores para un `crons` especificación (especificación): `* * * * *`

1. Minuto (0 a 59) Para todos los entornos Starter y Pro, la frecuencia mínima admitida para los trabajos cron es de cinco minutos. Es posible que tenga que configurar los ajustes de su administrador.
2. Hora (de 0 a 23)
3. Día del mes (del 1 al 31)
4. Mes (del 1 al 12)
5. Día de la semana (0 a 6) (de domingo a sábado; 7 también es domingo en algunos sistemas)

Algunos ejemplos:

- `00 */3 * * *` funciona cada tres horas en el primer minuto (12:00 a.m., 3:00 a.m., 6:00 a.m.)
- `20 */8 * * *` funciona cada 8 horas en el minuto 20 (12:20 a.m., 8:20 a.m., 4:20 p.m.)
- `00 00 * * *` se ejecuta una vez al día a medianoche
- `00 * * * 1` corre una vez a la semana los lunes a medianoche.

>[!NOTE]
>
>El `crons` tiempo especificado en el `.magento.app.yaml` se basa en la zona horaria del servidor, no en la especificada en los valores de configuración del almacén de la base de datos.

Al determinar la programación, tenga en cuenta el tiempo que se tarda en completar la tarea. Por ejemplo, si ejecuta un trabajo cada tres horas y la tarea tarda 40 minutos en completarse, puede considerar la posibilidad de cambiar el horario programado.

### Comando

El `cmd` especifica el comando o el script personalizado que se va a ejecutar. El formato del script de comandos puede incluir lo siguiente:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Por ejemplo:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

En este ejemplo, `<path-to-php-binary>` es `/usr/bin/php`. El directorio de instalación, que incluye el ID de proyecto es `/app/abc123edf890/bin/magento`, y la acción del script es `export:start catalog_category_product`.

### Agregar trabajos cron personalizados al proyecto

En Adobe Commerce, en la plataforma de infraestructura en la nube, puede agregar personalizaciones a la `crons` de la sección [`.magento.app.yaml`](../application/configure-app-yaml.md) archivo.

>[!NOTE]
>
>Para entornos de inicio y Pro `integration` entornos, el intervalo mínimo es de una vez cada cinco minutos. Para los entornos de ensayo y producción profesionales, el intervalo mínimo es de una vez por minuto. No puede configurar intervalos más frecuentes que los mínimos predeterminados.

En proyectos de Adobe Commerce Pro, la variable [función de autocrons](#set-up-cron-jobs) debe estar habilitado en el proyecto para poder agregar trabajos cron personalizados a los entornos de ensayo y producción mediante `.magento.app.yaml` archivo. Si esta función no está habilitada, [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para habilitar los crons automáticos.

**Para agregar trabajos cron personalizados**:

1. En el entorno de desarrollo local, edite la variable `.magento.app.yaml` en el Adobe Commerce `/app` directorio.

1. En el `crons` , agregue la personalización utilizando el siguiente formato:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   En el ejemplo siguiente, la variable `productcatalog` el trabajo exporta el catálogo de productos cada ocho horas, 20 minutos después de la hora.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Actualizar trabajos cron

Para agregar, quitar o actualizar un trabajo personalizado, cambie la configuración en la `crons` de la sección `.magento.app.yaml` archivo. A continuación, pruebe las actualizaciones en el control remoto `integration` antes de insertar los cambios en los entornos de ensayo y producción.

## Deshabilitar trabajos cron

Puede deshabilitar manualmente los trabajos cron antes de completar las tareas de mantenimiento, como la reindexación o la limpieza de la caché, para evitar problemas de rendimiento. Puede usar el complemento `ece-tools` comando CLI `cron:disable` para deshabilitar todos los trabajos de cron y detener cualquier proceso de cron activo.

**Para deshabilitar los trabajos cron**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Deshabilite los trabajos cron y detenga los procesos cron activos.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Después de completar cualquier tarea de mantenimiento necesaria, asegúrese de volver a habilitar los trabajos cron.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Solución de problemas de trabajos cron

Adobe ha actualizado el paquete de Adobe Commerce sobre la infraestructura en la nube para optimizar el procesamiento cron en Adobe Commerce sobre la plataforma de infraestructura en la nube y solucionar los problemas relacionados con cron. Si tiene problemas con el procesamiento cron, asegúrese de que el proyecto esté utilizando la versión más actual de `ece-tools` paquete. Consulte [Actualizar ECE-Tools](../dev-tools/update-package.md).

Puede revisar la información de procesamiento de cron en los archivos de registro de nivel de aplicación para cada entorno. Consulte [Registros de aplicaciones](../test/log-locations.md#application-logs).

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda con la resolución de problemas relacionados con cron:

- [Las tareas cron bloquean las tareas de otros grupos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [Restablecer los trabajos cron atascados manualmente en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
