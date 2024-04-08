---
title: Notas de la versión de ECE-Tools
description: Vea una lista de las mejoras más recientes del paquete ECE-Tools.
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: e21f21e34f89b62842bd22c99ff5705f984898e0
workflow-type: tm+mt
source-wordcount: '2905'
ht-degree: 0%

---

# Notas de la versión de ECE-Tools

El [ece-tools](https://github.com/magento/ece-tools) es un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube. Estas notas de la versión describen las mejoras más recientes realizadas en este paquete, que forma parte de la [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Consulte [Actualizar ECE-Tools](../dev-tools/update-package.md) para obtener información sobre cómo actualizar a la última versión de `ece-tools` paquete.

El `ece-tools` El paquete utiliza la siguiente secuencia de versiones de lanzamiento: `200<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas funciones
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->


## v2002.1.18 {#latest}

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Añadido soporte para PHP 8.3.
- ![icono de corrección](../../assets/fix.svg) Validador: se ha actualizado el validador de EOL.

## v2002.1.17

Fecha de la versión: 16 de enero de 2024

- ![icono de corrección](../../assets/fix.svg) **Validador para Elasticsearch y OpenSearch**: se ha corregido el validador que generaba un mensaje engañoso para instalar un servicio de búsqueda cuando LiveSearch estaba habilitado.<!-- MCLOUD-10167 -->
- ![icono de corrección](../../assets/fix.svg) **Advertencia de implementación**: se ha corregido un problema que provocaba advertencias de implementación sobre carpetas que no estaban vacías.<!-- MCLOUD-8958 -->

## v2002.1.16

Fecha de la versión: 16 de octubre de 2023

- ![nuevo icono](../../assets/new.svg) **Variable de entorno global ENABLE_WEBHOOKS**—Añadido el [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) variable global que se utiliza con los webhooks de Commerce para conectarse a un extremo externo, como una acción de tiempo de ejecución de App Builder o un sistema de administración de inventario de terceros.

## v2002.1.15

Fecha de la versión: 31 de julio de 2023

- ![icono de corrección](../../assets/fix.svg) **Códigos de error**: esquema de código de error actualizado y generador de documentos de código de error.
- ![icono de corrección](../../assets/fix.svg) **Validador para el modelo Redis personalizado**-Se ha actualizado el validador para modelos backend Redis personalizados. [Consulte el ejemplo de configuración de caché](../environment/variables-deploy.md#cache_configuration).
- ![icono de corrección](../../assets/fix.svg) **Validador para RabbitMQ**-Se ha agregado compatibilidad con RabbitMQ 3.11
- ![icono de corrección](../../assets/fix.svg) **Se ha corregido un vínculo incorrecto**-Se ha corregido el vínculo incorrecto a la documentación de incorporación en la plantilla de correo electrónico de bienvenida.

## v2002.1.14

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **PHP**—Añadido soporte para PHP 8.2.
- ![nuevo icono](../../assets/new.svg) **Validadores para servicios**—Se han actualizado los validadores de los servicios requeridos de Commerce 2.4.6: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x y RabbitMQ 3.9.
- ![icono de corrección](../../assets/fix.svg) **ece-tools db-dump**: se ha corregido un problema que provocaba que `db-dump` operación para detener prematuramente.

## v2002.1.13

Fecha de la versión: 27 de octubre de 2022

- ![nuevo icono](../../assets/new.svg) **Se ha agregado compatibilidad con Eventos de Adobe I/O para Adobe Commerce**. Los desarrolladores de extensiones ahora pueden utilizar [Eventos de Adobe I/O](https://developer.adobe.com/events/docs/) para enviar información de evento de Commerce desde instancias de Cloud a sus aplicaciones escritas para [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). Los eventos de Adobe I/O para Adobe Commerce se encuentran en Vista previa de socios.<!-- CEXT-932 -->
- ![nuevo icono](../../assets/new.svg) **Validador para la configuración de OPcache**: se ha añadido un validador para comprobar la configuración de OPcache para las rutas excluidas.<!-- MCLOUD-9485 -->
- ![icono de corrección](../../assets/fix.svg) **Se ha corregido un problema con la configuración de caché de GraphQL**—Ahora ECE-Tools mantiene el GraphQL `id_salt` valor en `cache` configuración en la `app/etc/env.php` archivo.<!-- MCLOUD-9486 -->

## v2002.1.12

Fecha de la versión: 13 de septiembre de 2022

- ![nuevo icono](../../assets/new.svg) **Activar`synchronous_replication`**—ECE-Conjuntos de herramientas `synchronous_replication=>true` en el `app/etc/env.php` archivo cuando `MYSQL_USE_SLAVE_CONNECTION` está activada. Esta configuración solo afecta a Commerce 2.4.6+. Consulte la `MYSQL_USE_SLAVE_CONNECTION` descripción de la variable en [Implementación de variables](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nuevo icono](../../assets/new.svg) **OpenSearch**: funcionalidad añadida para configurar y definir el `opensearch` para la próxima versión 2.4.6 de Adobe Commerce. Consulte [Configuración del servicio OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Fecha de lanzamiento: 4 de agosto de 2022

- ![icono de corrección](../../assets/fix.svg) **ElasticSuite Validator y OpenSearch**: se ha corregido un problema con el validador de comprobación de integridad de ElasticSuite al instalar OpenSearch.<!-- MCLOUD-8767 -->
- ![icono de corrección](../../assets/fix.svg) **Tipos de devolución para comandos de implementación**: tipos de valor devuelto fijos para los comandos de despliegue.<!-- AC-3208 -->
- ![icono de corrección](../../assets/fix.svg) **[!DNL RabbitMQ]problema con la nueva instalación de Commerce 2.4.5**: fijo [!DNL RabbitMQ] problema de bloqueo en la nueva instalación de Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Fecha de la versión: 31 de marzo de 2022

- ![icono de corrección](../../assets/fix.svg) **Elasticsearch 7.10**: se han actualizado los validadores para que admitan la versión 7.10 de Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **OpenSearch**—Se ha agregado compatibilidad con OpenSearch para las versiones de Adobe Commerce 2.4.4, 2.4.3-p2 y 2.3.7-p3.<!-- MCLOUD-8296 -->
- ![nuevo icono](../../assets/new.svg) **PHP**—Añadido soporte para PHP 8.1.
- ![icono de corrección](../../assets/fix.svg) **symfony/process**—Se ha añadido la compatibilidad con symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![nuevo icono](../../assets/new.svg) **Consumidor con varios procesos**—Se ha añadido un `multiple_processes` para que pueda especificar el número de procesos que se van a generar para cada consumidor. Consulte la `CRON_CONSUMERS_RUNNER` descripción de la variable en [Implementación de variables](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![nuevo icono](../../assets/new.svg) **Esquema OpenSearch y ruta de host completa**: se ha añadido la capacidad de configurar un esquema de Elasticsearch y una ruta de host completa.
- ![icono de corrección](../../assets/fix.svg) **AWS S3**: se ha cambiado el método de activación de AWS S3.
- ![icono de corrección](../../assets/fix.svg) **Corregir lector driver_options**: se ha añadido la lectura de la configuración driver_options para la conexión DB desde `env.php` archivar por `ece-tools` para validadores.<!-- MCLOUD-8420 -->

## v2002.1.8

Fecha de la versión: 25 de octubre de 2021

- ![nuevo icono](../../assets/new.svg) **Ubicación de volcado alternativa**—Añadido el `--dump-directory` para que pueda elegir un directorio de destino para un volcado de la base de datos. Ahora `/app/var/dump-main` es el directorio de destino predeterminado para un volcado de la base de datos. Consulte [Administración de copias de seguridad: volcar la base de datos](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![icono de corrección](../../assets/fix.svg) **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el `monolog` empaquetar en `^2.3`.<!-- ACMP-1263 -->
- ![icono de corrección](../../assets/fix.svg) **Actualizar Symfony**: se han actualizado las dependencias de Symfony para que sean compatibles con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![icono de corrección](../../assets/fix.svg) **Función/resolución de carga automática**: se ha corregido un problema que se producía al implementar en un entorno de integración y ver el `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` error.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Fecha de la versión: 29 de julio de 2021

**Actualizaciones de configuración**—

- ![nuevo icono](../../assets/new.svg) Se ha añadido compatibilidad con Composer 2.0.<!--MCLOUD-8003-->

- ![icono de corrección](../../assets/fix.svg) **Requisitos de composición actualizados para`symphony/console`**—Se ha actualizado ECE-Tools `composer.json` Requisitos de versión para `symphony/console` para solucionar un problema que causaba la `di:compile` para fallar con el siguiente error: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![icono de corrección](../../assets/fix.svg) Se han actualizado las comprobaciones del software al final de su vida útil (`eol.yaml`) para incluir el Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Fecha de publicación: 20 de abril de 2021

- ![nuevo icono](../../assets/new.svg) **Leer credenciales de autenticación**—Se ha añadido la capacidad de leer las credenciales de autorización de Redis desde el `relationships` durante la fase de implementación.<!--MCLOUD-7694-->

- ![nuevo icono](../../assets/new.svg) **Credenciales de autorización de Elasticsearch**—Se ha añadido la capacidad de leer las credenciales de autorización de Elasticsearch de la `relationships` durante la fase de implementación.<!--MCLOUD-7695-->

- ![nuevo icono](../../assets/new.svg) **Servicio de almacenamiento de sesión dedicado**—Añadido `redis-session` como segunda opción para el almacenamiento de sesión. Puede usar el complemento `redis-session` servicio para almacenar información de sesión y utilizar `redis` servicio para caché a fin de proporcionar un mejor rendimiento.<!--MCLOUD-7698-->

- ![nuevo icono](../../assets/new.svg) **Mensajes obsoletos de SPLIT_DB**: se han añadido mensajes críticos y de advertencia del validador para los obsoletos. `SPLIT_DB` para Adobe Commerce 2.4.2 y su eliminación en Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![icono de corrección](../../assets/fix.svg) **Versión de Elasticsearch de relaciones**: validador de servicio fijo para recuperar la versión correcta del Elasticsearch desde el `relationships` propiedades en Cloud Docker y entornos de integración.<!--MCLOUD-7572-->

- ![icono de corrección](../../assets/fix.svg) **Validación flexible de informes de Redis**—Redis ahora puede validar el puerto en una conexión de caché personalizada desde el `server` URL. Por ejemplo, puede agregar el número de puerto a la dirección URL del servidor de la siguiente manera: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Esto ayuda a evitar errores de validación en los que la variable `port` falta la opción o es incorrecta.<!--MCLOUD-7722-->

- ![icono de corrección](../../assets/fix.svg) **Actualización a Adobe Commerce 2.4.2**—Se ha corregido el problema que requería que los usuarios se ejecutaran manualmente `bin/magento setup:upgrade` para hacer que sus sitios estén operativos después de actualizar a Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Fecha de publicación: 1 de febrero de 2021

- ![nuevo icono](../../assets/new.svg) **Almacenamiento remoto**—Añadido el `REMOTE_STORAGE` variable de entorno para habilitar Proyectos en la nube para el almacenamiento remoto de archivos multimedia mediante un servicio de almacenamiento, como AWS S3. Esta opción de configuración forma parte del paquete ECE-Tools, pero no es compatible con Adobe Commerce en la infraestructura en la nube.<!--MCLOUD-7153-->

- ![nuevo icono](../../assets/new.svg) **Nuevo `cloud:config:validate` mando**—Comando añadido `php vendor/bin/ece-tools cloud:config:validate` para validar el `.magento.env.yaml` antes de insertar cambios en el entorno remoto de la nube.<!--MCLOUD-7120-->

- ![nuevo icono](../../assets/new.svg) **Vaciar la memoria opcache**—Se ha añadido compatibilidad con el `opcache.enable_cli` Opción de PHP para vaciar la caché de OP antes de ejecutar el gancho de implementación. Esta configuración restablece la configuración de la caché para garantizar que se aplican los ajustes de configuración actuales en cada implementación.<!--MCLOUD-7015-->

- ![nuevo icono](../../assets/new.svg) **Validación de Aurora DB**: se ha actualizado la validación del servicio de base de datos para que sea compatible con la base de datos Aurora.<!--MCLOUD-7269-->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno SCD_NO_PARENT**—Añadido el `SCD_NO_PARENT` variable de entorno (para Adobe Commerce >=2.4.2) para administrar la generación de contenido estático para temáticas principales.<!--MCLOUD-7284-->

- ![icono de corrección](../../assets/fix.svg) **Límites de memoria y comandos**—Se ha corregido un problema en el que `php vendor/bin/ece-tools` Los comandos de no funcionarían si el tamaño del `cloud.log` El archivo ha superado el límite de memoria de PHP. En lugar de leer el `cloud.log` en la memoria, ahora solo leemos un subconjunto más pequeño de datos del archivo de registro.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icono de corrección](../../assets/fix.svg) **Conexiones de base de datos personalizadas**: se ha corregido un `.magento.env.yaml` problema de configuración en el que se definieron las conexiones de base de datos personalizadas para `DATABASE_CONFIGURATION` no se han utilizado. No se agregaba la configuración de conexión a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icono de corrección](../../assets/fix.svg) **Registros de error vacíos**: se ha corregido un problema que provocaba que fallaran las implementaciones si la variable `cloud.error.log` estaba vacío.<!--MCLOUD-7296-->

- ![icono de corrección](../../assets/fix.svg) **Validación de MariaDB 10.3**: se ha corregido la validación de MariaDB 10.3 para Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![icono de corrección](../../assets/fix.svg) **Caché:registro de vaciado**: entradas de registro mejoradas para indicar el inicio y el final de la `cache:flush` paso.<!--MCLOUD-7503-->

## v2002.1.4

Fecha de la versión: 19 de noviembre de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error de implementación cuando el motor de búsqueda especificado en la variable `SEARCH_CONFIGURATION` variable de entorno es un valor distinto de `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Fecha de la versión: 9 de noviembre de 2020

**Actualizaciones de infraestructura**—

- ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con ECE-Tools para solo lectura `pub/static` cuando el contenido estático está configurado para implementarse en la fase de compilación.<!--MC-37699-->

- ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con Elasticsearch 7.9 y Redis 6 para la compatibilidad con próximas versiones de Adobe Commerce.<!--MCLOUD-7191-->

- ![icono de corrección](../../assets/fix.svg) Actualizado el ECE-Tools `composer.json` para añadir una dependencia necesaria para la herramienta Parches de calidad. Esto corrige una dependencia circular que existía entre los paquetes ECE-Tools y magento-cloud-patch.<!--MCLOUD-6910-->

**Mejoras de validación y registro**—

- ![nuevo icono](../../assets/new.svg) Se ha añadido la validación del motor de búsqueda para garantizar que `elasticsearch` se configura para Adobe Commerce en la infraestructura en la nube 2.4 y posterior. Si la validación falla, la implementación se detiene con un mensaje de error crítico que sugiere correcciones para el problema. Consulte [Errores críticos, fase de implementación](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nuevo icono](../../assets/new.svg) Se ha añadido la validación del Elasticsearch para comprobar la compatibilidad entre la versión del servicio del Elasticsearch y la versión de Adobe Commerce.<!--MCLOUD-7193-->

- ![nuevo icono](../../assets/new.svg) Se ha actualizado el mensaje de error de compatibilidad del Elasticsearch para mostrar las versiones de Elasticsearch compatibles con el módulo de Elasticsearch de Adobe Commerce. El mensaje de error ahora proporciona las versiones específicas del Elasticsearch que se van a instalar en la infraestructura de Cloud para que sea compatible con el módulo del Elasticsearch utilizado por la versión de Adobe Commerce. Consulte [Errores de advertencia, fase de implementación](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nuevo icono](../../assets/new.svg) Errores de advertencia añadidos `2026` y `2027` para no válido `MAGE_MODE` configuración de variables de entorno. El único valor válido es `production`. Antes de esta corrección, `MAGE_MODE` se puede establecer en `developer` sin errores de implementación, solo para provocar errores más adelante al intentar escribir en archivos de solo lectura. Consulte [Errores de advertencia](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido la validación de los servicios de Redis, RabbitMQ y MySQL para garantizar que estas versiones sean compatibles con la versión de Adobe Commerce. Las versiones válidas de estos servicios ahora se escriben en `cloud.log`.<!--MCLOUD-7098-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado el `cloud.log` para incluir el límite de solicitudes simultáneas para enviar solicitudes durante el calentamiento de la caché. Este valor se configura en [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) variable posterior a la implementación.<!--MCLOUD-5563-->

**Actualizaciones de comandos de CLI**—

- ![nuevo icono](../../assets/new.svg) Comandos CLI añadidos (`cloud:config:create` y `cloud:config:update`) para crear y actualizar el `.magento.env.yaml` con una configuración que puede incluir una o más variables de compilación, implementación y posteriores a la implementación. Consulte [Crear archivo de configuración desde CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Actualizaciones de variables de entorno**—

- ![nuevo icono](../../assets/new.svg) Se ha añadido la [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) variable de compilación. Configuración de la variable en `true` impide que la aplicación ejecute el `composer dump-autoload` durante una instalación de Cloud Docker para Commerce. La variable solo es relevante para los contenedores de Cloud Docker para Commerce con sistemas de archivos editables (creados para pruebas y desarrollo mediante `./vendor/bin/ece-docker build:compose --with-test`). Con tales instalaciones, omitiendo la `composer dump-autoload` evita errores cuando se ejecutan otros comandos que intentan obtener acceso a archivos desde un archivo eliminado `generated` directorio.<!--MCLOUD-6939-->

## v2002.1.2

Fecha de lanzamiento: 5 de agosto de 2020

**Mejoras de validación y registro**—

- ![nuevo icono](../../assets/new.svg) Se ha añadido la `schema.error.yaml` que incluye todas las notificaciones de error y advertencia que se pueden producir durante el proceso de generación, implementación y posterior a la implementación, junto con sugerencias para resolver los errores. La información de este archivo también está disponible en la _Guía de Cloud para Commerce_. Consulte [Referencia de mensaje de error para ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![nuevo icono](../../assets/new.svg) Se ha cambiado el registro de errores de nube (`/var/log/cloud.error.log`) entradas en formato JSON para facilitar el análisis del registro mediante programación.<!--MCLOUD-5879-->

- ![nuevo icono](../../assets/new.svg) Se han añadido comprobaciones de error adicionales para el procesamiento de la generación, la implementación y la implementación posterior, así como comprobaciones existentes mejoradas:

   - Código de error 2026: error al restaurar algunos datos generados durante la fase de compilación en los directorios montados

   - Código de error 3004: no se pueden crear archivos de copia de seguridad

   - Código de error 102: se han añadido comprobaciones adicionales para detectar problemas que se producen cuando la variable `env.php` el archivo no se puede escribir <!--MCLOUD-6221-->

- ![nuevo icono](../../assets/new.svg) Se ha añadido la **QUALITY_PATCH** variable de entorno para especificar uno o más parches de calidad que aplicar durante el proceso de implementación. Consulte [Variables de compilación](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de infraestructura**—

   - ![nuevo icono](../../assets/new.svg) **Mejoras de registro**: se ha mejorado la capacidad de seguimiento de registros al asignar códigos de salida a errores de implementación críticos y al exponer los códigos de salida en notificaciones de mensajes de error y eventos de registro. Consulte [Referencia de mensaje de error para ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nuevo icono](../../assets/new.svg) Se ha mejorado el proceso de volcados de base de datos (`vendor/bin/ece-tools db-dump`) y se actualizaron los mensajes de registro para aclarar que la operación de volcado de la base de datos cambia la aplicación al modo de mantenimiento, detiene los procesos de cola de los consumidores y deshabilita los trabajos cron antes de que comience el volcado.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema para garantizar que la dirección URL del proyecto se actualice correctamente al implementarla en los entornos de ensayo y producción. Ahora, `ece-tools` utiliza la dirección URL para la ruta con `primary:true` establecido en la configuración de ruta del proyecto. Consulte [Implementación de variables](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![icono de corrección](../../assets/fix.svg) Se ha actualizado el `generate.xml` flujo de trabajo del escenario de compilación para aplicar parches. Los parches deben aplicarse antes para actualizar Adobe Commerce y solucionar los problemas que puedan provocar la `di:compile` y `module:refresh` pasos para fallar.<!--MCLOUD-5941-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema en el proceso de instalación que devolvía incorrectamente la variable `Crypt key missing` error. El `crypt/key` El valor de se genera automáticamente durante la instalación.<!--MCLOUD-6120-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**—

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con PHP 7.4 y MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **SCD_USE_BALER** para habilitar el módulo Baler para el agrupamiento de JavaScript durante el proceso de compilación de la infraestructura de Adobe Commerce en la nube. Consulte la descripción de la variable en la [variables de compilación](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **REDIS_BACKEND** variable de entorno para configurar el modelo de back-end de Redis para la caché de Redis para Adobe Commerce 2.3.5 o posterior. Consulte la descripción de la variable en la [implementar variables](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de comandos de CLI**—

   - ![nuevo icono](../../assets/new.svg) Se han actualizado los siguientes comandos CLI con una opción para un registro más detallado:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     El nivel de registro de cada llamada viene determinado por la configuración del [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) en la variable `.magento.env.yaml` archivo.<!--MCLOUD-3503-->

- ![nuevo icono](../../assets/new.svg) **Mejoras de validación**—

   - ![nuevo icono](../../assets/new.svg) **Comprobaciones de compatibilidad de Elasticsearch 7.x**: se ha actualizado la validación del Elasticsearch para las comprobaciones de compatibilidad del software Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![nuevo icono](../../assets/new.svg) **Comprobaciones de validación de versión de servicio y EOL actualizadas**: se ha actualizado la validación para comprobar que las versiones del servicio instaladas cumplen los requisitos de Adobe Commerce 2.4.<!--MCLOUD-6144-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de validación por el que el siguiente mensaje de advertencia posterior a la implementación solo se mostraba si la variable `post-deploy` falta la configuración de enlace en el `.magento.app.yaml` archivo:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![nuevo icono](../../assets/new.svg) **Validación añadida para las dependencias de Zend Framework**—Se ha añadido la validación de la dependencia del compositor para Zend Framework que ha migrado al proyecto Laminas. Si faltan las dependencias necesarias, aparece el siguiente mensaje de error durante el proceso de generación.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Consulte [Verificar dependencias de Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![nuevo icono](../../assets/new.svg) **Validación añadida para `env.php` archivo y datos**: se han añadido comprobaciones para `env.php` archivos y datos durante el proceso de instalación y actualización.<!--MCLOUD-5991-->

      - Si la variable `env.php` falta en la instalación, y el archivo `crypt/key` no se ha especificado el valor en `.magento.app.yaml` , la implementación falla con la siguiente notificación:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Si la instalación no incluye el `env.php` o la configuración solo contiene un tipo de caché, el `cron:enable` El comando se ejecuta durante el proceso de actualización para restaurar el archivo con todas las `cache_types`. Se agrega la siguiente notificación al registro:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Fecha de publicación: 6 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de infraestructura**—

   - ![nuevo icono](../../assets/new.svg) **Se ha agregado un paquete independiente para Cloud Docker para Commerce**: permite separar el paquete Docker del `ece-tools` para mantener la calidad del código y proporcionar versiones independientes. Actualizaciones y correcciones relacionadas con `ece-tools` se administran desde el [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) Repositorio de GitHub.<!--MAGECLOUD-2927-->

   - ![nuevo icono](../../assets/new.svg) **Funciones de parches actualizadas**: permite trasladar la funcionalidad de aplicación de parches del paquete ECE-Tools a otro [magento-cloud-patch](https://github.com/magento/magento-cloud-patches) paquete. Durante la implementación, `ece-tools` utiliza el nuevo paquete para aplicar parches. Consulte [Notas de la versión de parches de Cloud](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![nuevo icono](../../assets/new.svg) **Dependencias actualizadas del compositor**: se ha actualizado el `composer.json` archivo para Adobe Commerce en la infraestructura de la nube con una dependencia para `magento/magento-cloud-docker` paquete. Ahora, `ece-tools` incluye dependencias para todos los paquetes en [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Estos paquetes se instalan y actualizan automáticamente al instalar o actualizar `ece-tools`.

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con implementaciones basadas en escenarios**—<!--MAGECLOUD-4101-->

   - ![nuevo icono](../../assets/new.svg) Ahora puede personalizar los procesos de generación, implementación y posterior implementación mediante archivos de configuración XML para anular o personalizar la configuración predeterminada.

   - ![nuevo icono](../../assets/new.svg) **Se ha cambiado el `hooks` configuración en`.magento.app.yaml`**—Hemos actualizado la `hooks` formato de configuración para admitir implementaciones basadas en escenarios. El formato heredado de versiones anteriores de ECE-Tools 2002.0.x todavía es compatible. Sin embargo, debe actualizar al nuevo formato para utilizar la función de implementación basada en escenarios. Consulte [Implementaciones basadas en escenarios](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Antes de actualizar a ECE-Tools versión 2002.1.0, revise la [cambios incompatibles con versiones anteriores](backward-incompatible-changes.md) para obtener más información sobre los cambios que podrían requerir la actualización de Adobe Commerce en la configuración o los procesos de proyectos de infraestructura en la nube.

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**—

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con PHP 7.3.<!--MAGECLOUD-4022-->

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la validación para comprobar las versiones del servicio instaladas con la fecha límite para cada servicio. Ahora, los clientes reciben una notificación si una versión de servicio se encuentra en los tres meses siguientes a la fecha límite, y una advertencia si la fecha límite se encuentra en el pasado.<!--MAGECLOUD-4076-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración del Elasticsearch para garantizar que se establezca la configuración de Elasticsearch correcta en todos los entornos.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulte [Versiones de servicio](../services/services-yaml.md#service-versions) para obtener una lista de los servicios utilizados en Adobe Commerce en la infraestructura en la nube y su compatibilidad con la versión de la plantilla en la nube.

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) Se ha ampliado la funcionalidad del `WARM_UP_PAGES` variable de entorno para admitir la precarga de caché para páginas de producto específicas. Consulte la definición expandida en la [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages) tema.<!--MAGECLOUD-4444-->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la `ERROR_REPORT_DIR_NESTING_LEVEL` variable de entorno para simplificar la administración de datos del informe de errores en `<magento_root>/var/report/` directorio. Consulte la descripción de la variable en la [variables de compilación](../environment/variables-build.md#error_report_dir_nesting_level) tema.

   - ![icono de corrección](../../assets/fix.svg) Se ha eliminado el `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT`, y `STATIC_CONTENT_SYMLINK` variables de entorno. Consulte [Cambios incompatibles con versiones anteriores](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema en el proceso de configuración de grupo elástico para que la configuración predeterminada se sobrescriba según lo esperado al configurar el `ELASTICSUITE_CONFIGURATION` implementar variable sin la variable `_merge` opción.<!--MAGECLOUD-4388-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de comandos de CLI**—

   - ![nuevo icono](../../assets/new.svg) **Nuevo comando cron**: ahora puede administrar manualmente el procesamiento cron en su entorno de Adobe Commerce en la infraestructura en la nube mediante el `cron:disable` y `cron:enable` comandos. Utilice el comando disable para todos los procesos cron activos y deshabilitar todos los trabajos cron. Utilice el comando enable para volver a habilitar los trabajos cron cuando esté listo. Consulte [Deshabilitar trabajos cron](../application/crons-property.md#disable-cron-jobs).

   - ![nuevo icono](../../assets/new.svg) **Informes de errores mejorados**—Se ha agregado un mejor registro para los errores de comando de CLI que ocurren durante el procesamiento de ECE-Tools.<!--MAGECLOUD-4849-->

   - ![nuevo icono](../../assets/new.svg) **Quitar comandos de generación obsoletos**— Se han eliminado los siguientes comandos de compilación: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump`y con el nombre cambiado `ece-tools docker` comandos para `ece-docker`. Consulte [Cambios incompatibles con versiones anteriores](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nuevo icono](../../assets/new.svg) Se eliminaron los obsoletos `build_options.ini` y se agregó la validación para dar error a la compilación si el archivo existe. Utilice el [.magento.env.yaml](../environment/configure-env-yaml.md) para configurar las opciones de generación.

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que hacía que el proceso de generación fallara cuando la variable `config.php` el archivo está vacío.<!--MAGECLOUD-4127-->

## 2002.0.23

Fecha de publicación: 27 de febrero de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de compatibilidad con `ece-tools` Versiones 2002.0.x que evitaban que la generación de contenido estático bajo demanda finalizara correctamente en el modo de producción.

## Versiones anteriores

Consulte la [archivo de notas de versión](cloud-release-archive.md) para la versión 2002.0.22 y anteriores de.
