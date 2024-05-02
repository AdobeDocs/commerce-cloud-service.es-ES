---
title: Visualización y administración de registros
description: Comprenda los tipos de archivos de registro disponibles en la infraestructura de la nube y dónde encontrarlos.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: d7f63dab-23bf-4b95-b58c-3ef9b46979d4
source-git-commit: 86af69eed16e8fe464de93bd0f33cfbfd4ed8f49
workflow-type: tm+mt
source-wordcount: '1056'
ht-degree: 0%

---

# Visualización y administración de registros

Los registros de Adobe Commerce en proyectos de infraestructura en la nube son útiles para solucionar problemas relacionados con [creación e implementación de vínculos](../application/hooks-property.md), Cloud Services y la aplicación de Adobe Commerce.

Puede ver los registros desde el sistema de archivos, el [!DNL Cloud Console], y el `magento-cloud` CLI.

- **Sistema de archivos**: la `/var/log` el directorio del sistema contiene registros para todos los entornos. El `var/log/` El directorio contiene registros específicos de la aplicación exclusivos de un entorno determinado. Estos directorios no se comparten entre los nodos de un clúster. En los entornos de ensayo y producción de Pro, debe comprobar los registros de cada nodo.

- **[!DNL Cloud Console]**: puede ver la información de registro de la generación, la implementación y la implementación posterior en el entorno. _messages_ lista.

- **CLI de nube**: puede ver los registros de entorno local mediante el `magento-cloud log` registros de entorno remotos o de comandos utilizando `magento-cloud ssh` comando.

## Ubicaciones de registro

Los registros del sistema se almacenan en las siguientes ubicaciones:

- Integración: `/var/log/<log-name>.log`
- Ensayo profesional: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Producción profesional: `/var/log/platform/<project-ID>/<log-name>.log`

El valor de `<project-ID>` depende del proyecto y de si el entorno es Ensayo o Producción. Por ejemplo, con un ID de proyecto de `yw1unoukjcawe`, el usuario del entorno de ensayo es `yw1unoukjcawe_stg` y el usuario del entorno de producción es `yw1unoukjcawe`.

Con ese ejemplo, el registro de implementación es: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Ver registros de entorno remoto

La mayoría de los registros incluyen eventos que se producen en el entorno remoto. Para Pro, hay varios nodos y cada nodo tiene registros únicos. Utilice lo siguiente para ver una lista de todos los hosts:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Respuesta de ejemplo:

```terminal
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Para ver una lista de registros de entornos remotos**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Ejemplo de Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Para ver un registro remoto**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Ejemplo de Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>En los entornos de ensayo y producción de Pro, la rotación, compresión y eliminación automáticas del registro están habilitadas para los archivos de registro con un nombre de archivo fijo. Cada tipo de archivo de registro tiene un patrón giratorio y una duración. Los entornos de inicio no tienen rotación de registro. Puede encontrar todos los detalles sobre la rotación del registro del entorno y la duración de los registros comprimidos en: `/etc/logrotate.conf` y `/etc/logrotate.d/<various>`. La rotación de registros no se puede configurar en entornos de Pro Integration. Para la integración de Pro, debe implementar una solución/script personalizado y [configurar su cron](../application/crons-property.md) para ejecutar el script según sea necesario.

## Creación e implementación de registros

Después de insertar los cambios en su entorno, puede revisar el registro desde cada vínculo en la `var/log/cloud.log` archivo. El registro contiene mensajes de inicio y parada para cada vínculo. En el ejemplo siguiente, los mensajes son &quot;`Starting post-deploy.`&quot; y &quot;`Post-deploy is complete.`&quot;

Compruebe las marcas de tiempo en las entradas de registro, compruebe y busque los registros de una implementación específica. El siguiente es un ejemplo conciso de la salida de registro que puede utilizar para solucionar problemas:

```terminal
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Al configurar el entorno de Cloud, puede configurar lo siguiente [Slack basado en registros y notificaciones por correo electrónico](../environment/set-up-notifications.md) para acciones de compilación e implementación.

Los siguientes registros tienen una ubicación común para todos los proyectos en la nube:

- **Registro de implementación**: `var/log/cloud.log`
- **Registro de errores de la última implementación**: `var/log/cloud.error.log`
- **Registro de depuración**: `var/log/debug.log`
- **Registro de excepciones**: `var/log/exception.log`
- **Registro del sistema**: `var/log/system.log`
- **Registro de asistencia**: `var/log/support_report.log`
- **Informes**: `var/report/`

Aunque el `cloud.log` contiene comentarios de cada fase del proceso de implementación, los registros creados por el vínculo de implementación son únicos para cada entorno. El registro de implementación específico del entorno se encuentra en los siguientes directorios:

- **Integración de Starter y Pro**: `/var/log/deploy.log`
- **Ensayo profesional**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Producción profesional**: `/var/log/platform/<project-ID>/deploy.log`

### Implementación del registro

El registro de cada implementación se concatena al específico `deploy.log` archivo. El siguiente ejemplo imprime el registro de implementación del entorno actual en el terminal:

```bash
magento-cloud log -e <environment-ID> deploy
```

Respuesta de ejemplo:

```terminal
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Registro de errores

Los mensajes de error y advertencia generados durante el proceso de implementación se escriben tanto en la variable `var/log/cloud.log` y el `var/log/cloud.error.log` archivos. El archivo de registro de errores de Cloud solo contiene errores y advertencias de la implementación más reciente. Un archivo vacío indica una implementación correcta sin errores.

Puede ver el archivo de registro utilizando la variable [CLI de nube SSH](#view-remote-environment-logs), o puede utilizar ECE-Tools para mostrar los errores con sugerencias:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Respuesta de ejemplo:

```terminal
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

La mayoría de los mensajes de error contienen una descripción y una acción sugerida. Utilice el [Referencia de mensaje de error para ECE-Tools](../dev-tools/error-reference.md) para buscar el código de error y obtener más instrucciones. Para obtener más información, utilice el [Solucionador de problemas de implementación de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html).

## Registros de aplicaciones

De forma similar a los registros de implementación, los registros de aplicaciones son únicos para cada entorno:

| Archivo de registro | Integración de Starter y Pro | Descripción |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Implementación del registro** | `/var/log/deploy.log` | Actividad desde el [implementar gancho](../application/hooks-property.md). |
| **Registro posterior a la implementación** | `/var/log/post_deploy.log` | Actividad desde el [enlace posterior al despliegue](../application/hooks-property.md). |
| **Registro de Cron** | `/var/log/cron.log` | Salida de trabajos cron. |
| **Registro de acceso de Nginx** | `/var/log/access.log` | Al inicio de Nginx, errores HTTP para los directorios que faltan y los tipos de archivo excluidos. |
| **Registro de errores de Nginx** | `/var/log/error.log` | Mensajes de inicio útiles para depurar errores de configuración asociados a Nginx. |
| **Registro de acceso de PHP** | `/var/log/php.access.log` | Solicitudes al servicio PHP. |
| **Registro de PHP FPM** | `/var/log/app.log` | |

Para los entornos de ensayo y producción Pro, los registros de implementación, posterior a la implementación y Cron solo están disponibles en el primer nodo del clúster:

| Archivo de registro | Ensayo profesional | Producción profesional |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Implementación del registro** | Solo el primer nodo:<br>`/var/log/platform/<project-ID>_stg/deploy.log` | Solo el primer nodo:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Registro posterior a la implementación** | Solo el primer nodo:<br>`/var/log/platform/<project-ID>_stg/post_deploy.log` | Solo el primer nodo:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Registro de Cron** | Solo el primer nodo:<br>`/var/log/platform/<project-ID>_stg/cron.log` | Solo el primer nodo:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Registro de acceso de Nginx** | `/var/log/platform/<project-ID>_stg/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Registro de errores de Nginx** | `/var/log/platform/<project-ID>_stg/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **Registro de acceso de PHP** | `/var/log/platform/<project-ID>_stg/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **Registro de PHP FPM** | `/var/log/platform/<project-ID>_stg/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Archivos de registro archivados

Los registros de la aplicación se comprimen y archivan una vez al día y se conservan durante un año. Los registros comprimidos reciben un nombre mediante un ID único que corresponde a la variable `Number of Days Ago + 1`. Por ejemplo, en entornos de producción Pro, se almacena un registro de acceso PHP de 21 días en el pasado y se le asigna el siguiente nombre:

```terminal
/var/log/platform/<project-ID>/php.access.log.22.gz
```

Los archivos de registro archivados siempre se almacenan en el directorio en el que se encontraba el archivo original antes de la compresión.

>[!NOTE]
>
>**Implementar** y **Posterior a la implementación** Los archivos de registro no se giran ni se archivan. Todo el historial de implementación se escribe dentro de esos archivos de registro.

## Registros de servicio

Dado que cada servicio se ejecuta en un contenedor independiente, los registros del servicio no están disponibles en el entorno de integración. Adobe Commerce en la infraestructura en la nube proporciona acceso al contenedor del servidor web solo en el entorno de integración. Las siguientes ubicaciones de registro de servicio son para los entornos de ensayo y producción de Pro:

- **Registro de Redis**: `/var/log/platform/<project-ID>_stg/redis-server-<project-ID>_stg.log`
- **registro de Elasticsearch**: `/var/log/elasticsearch/elasticsearch.log`
- **Registro de recolección de basura de Java**: `/var/log/elasticsearch/gc.log`
- **Registro de correo**: `/var/log/mail.log`
- **Registro de errores de MySQL**: `/var/log/mysql/mysql-error.log`
- **Registro lento de MySQL**: `/var/log/mysql/mysql-slow.log`
- **Registro de RabbitMQ**: `/var/log/rabbitmq/rabbit@host1.log`

Los registros de servicio se archivan y guardan durante diferentes períodos de tiempo, según el tipo de registro. Por ejemplo, los registros MySQL tienen la duración más corta: se eliminan después de siete días.

>[!TIP]
>
>Las ubicaciones de los archivos de registro en la arquitectura escalada dependen del tipo de nodo. Consulte [Ubicaciones de registro en la arquitectura a escala](../architecture/scaled-architecture.md#log-locations) tema.

## Datos de registro para Producción y ensayo profesionales

En entornos de ensayo y producción profesional, utilice [Administración de registros de New Relic](../monitor/log-management.md) integrado con el proyecto para administrar los datos de registro agregados de todos los registros asociados con el proyecto de infraestructura de Adobe Commerce en la nube.

La aplicación New Relic Logs proporciona un panel de administración de registros centralizado para solucionar problemas y supervisar Adobe Commerce en entornos de ensayo y producción de infraestructuras en la nube. El tablero también proporciona acceso a los datos de registro para los servicios Fastly CDN, Optimización de imágenes y cortafuegos de aplicaciones web (WAF). Consulte [Servicios de New Relic](../monitor/new-relic-service.md).
