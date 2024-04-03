---
title: Archivo de notas de la versión para ece-tools
description: Obtenga información sobre las mejoras archivadas para ece-tools.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 96663d2f-ef00-4490-ad2e-06e8041b228e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# Archivo de notas de la versión para ece-tools

>[!NOTE]
>
>Estas notas de la versión proporcionan información y actualizaciones para `ece-tools` v2002.0.22 y posterior. Consulte [Notas de la versión de Cloud Tools Suite](cloud-tools-suite.md) para obtener las últimas actualizaciones de `ece-tools` y otros paquetes de Cloud.

## v2002.0.22

El `ece-tools` La versión 2002.0.22 cambia la estructura del `ece-tools` paquete para desvincular la liberación de `Adobe Commerce on cloud infrastructure` parches de la versión ECE-Tools. A partir de esta versión, los parches y las correcciones críticas se entregarán mediante el [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) paquete, que es una nueva dependencia para el `ece-tools` paquete. Hemos realizado estos cambios para reducir la complejidad a la hora de programar actualizaciones de versiones y trabajar con contribuciones de la comunidad.

- ![nuevo icono](../../assets/new.svg) **Cambios en el paquete ECE-Tools**

   - ![nuevo icono](../../assets/new.svg) Se han movido los parches de Adobe Commerce desde el `ece-tools` empaquetar en un nuevo [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) paquete de compositor.

   - ![nuevo icono](../../assets/new.svg) Se ha actualizado el `composer.json` archivo para `ece-tools` paquete para añadir una dependencia para `magento/magento-cloud-patches` paquete v1.0.0.

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que `ece-tools` proceso de aplicación de parches que se interrumpe al aplicar conjuntos de parches sobre versiones de solo seguridad, a partir de la versión 2.3.2-p2 y posterior. Este problema se introdujo mediante el nuevo esquema de versiones adoptado para [parches de solo seguridad](https://devdocs.magento.com/guides/v2.3/release-notes/bk-release-notes.html#security-only-patches).<!--MAGECLOUD-4661-->

- ![icono de corrección](../../assets/fix.svg) **Parches y correcciones críticas**-Actualice los entornos de nube con `ece-tools` versión 2002.0.22 para aplicar los siguientes parches y correcciones críticas. Estos parches se incluyen en la `magento/magento-cloud-patches` paquete v1.0.0.

   - ![icono de corrección](../../assets/fix.svg) **Revisiones de seguridad de Page Builder para las versiones 2.3.1.x y 2.3.2.x**-Corrige un problema en la vista previa de Page Builder que permite a los usuarios no autenticados acceder a algunos métodos de creación de plantillas que se pueden utilizar para almacenar en déclencheur la ejecución de código arbitrario a través de la red (RCE), lo que da como resultado fugas de información global. Este problema se puede producir cuando se utilizan versiones no compatibles de Page Builder con Adobe Commerce 2.3.1 y 2.3.2.<!--MAGECLOUD-4649-->

   - ![icono de corrección](../../assets/fix.svg) **Revisiones MSI**-Soluciona problemas que causaban errores de indexación y problemas de rendimiento al usar la configuración de inventario predeterminada para administrar existencias.<!--MAGECLOUD-4428-->

   - ![icono de corrección](../../assets/fix.svg) **Compatibilidad con versiones anteriores de nuevas interfaces de correo**-Corrige un problema de incompatibilidad con versiones anteriores causado por el `Magento\Framework\Mail\EmailMessageInterface` Interfaz de PHP incluida en Adobe Commerce v2.3.3. En el ámbito de este parche, la nueva `EmailMessageInterface` hereda del antiguo `MessageInterface`, y los módulos principales de Adobe Commerce se revierten para depender de `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![icono de corrección](../../assets/fix.svg) **La paginación del catálogo no funciona en Elasticsearch 6.x**-Corrige un problema crítico con la paginación de resultados de búsqueda que afecta a los clientes que utilizan el Elasticsearch 6.x como motor de búsqueda de catálogos.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) **Nuevas imágenes Docker**: compatible con las versiones 2.3.3 y posteriores<!-- MAGECLOUD-3345 -->

      - PHP versión 7.3.<!-- MAGECLOUD-4017 -->

      - Caché de barniz 6.2.0<!-- MAGECLOUD-4017 -->

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad para aplicar la configuración de enlace personalizada especificada en `.magento.app.yaml` en el entorno de Docker. Anteriormente, el entorno de Docker solo admitía la configuración de enlace predeterminada.<!-- MAGECLOUD-3505-->

   - ![nuevo icono](../../assets/new.svg) Los archivos ENV de Docker ya no se generan durante la generación de Docker y el `docker:config:convert` El comando está obsoleto. Los datos correspondientes ahora se almacenan en `docker-compose.yml` archivo.<!-- MAGECLOUD-3816-->

   - ![nuevo icono](../../assets/new.svg) **Imagen PHP actualizada**-Se ha agregado Node.js a la imagen Docker de PHP para admitir las capacidades node, npm y grunt-cli.<!-- MAGECLOUD-3953 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**-

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **LOCK_PROVIDER** implemente la variable para configurar el proveedor de bloqueos que impide el inicio de trabajos cron y grupos cron duplicados. Consulte la descripción de la variable en la [implementar variables](../environment/variables-deploy.md#lock_provider) tema.<!-- MAGECLOUD-4052 -->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **CONSUMERS_WAIT_FOR_MAX_MESSAGES** variable de entorno para configurar cómo procesan los consumidores los mensajes de la cola de mensajes al utilizar `CRON_CONSUMERS_RUNNER` variable de entorno para administrar trabajos de cron. Consulte la descripción de la variable en la [implementar variables](../environment/variables-deploy.md#consumers_wait_for_max_messages) tema.<!-- MAGECLOUD-4071 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que puede provocar errores de interbloqueo de la base de datos cuando la variable `consumers_runner` el trabajo cron inicia varias instancias del mismo consumidor en diferentes nodos. Ahora, si ha habilitado la variable [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) implementar la variable en su entorno, la variable `consumers_runner` El trabajo utiliza el `single-thread` para iniciar una instancia de cada consumidor en un solo nodo.<!-- MAGECLOUD-3913 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que afectaba a [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) que utiliza una URL de tienda predeterminada. Ahora, si la variable `config:show:default-url` El comando no puede recuperar una dirección URL base y se utiliza la dirección URL de la variable MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

- ![nuevo icono](../../assets/new.svg) Se ha actualizado la información de registro devuelta por el `module:refresh` comando. Ahora puede ver una lista detallada de los módulos habilitados en la `cloud.log` archivo.<!-- MAGECLOUD-2514 -->

- ![nuevo icono](../../assets/new.svg) Se ha mejorado la validación de la compatibilidad de versiones y se han enviado notificaciones de advertencia sobre problemas de compatibilidad entre la versión de Adobe Commerce y los servicios instalados, como Elasticsearch, [!DNL RabbitMQ], Redis y DB.<!-- MAGECLOUD-3535 -->

- ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con la versión 3.8 de RabitMQ.<!-- MAGECLOUD-4674-->

- ![nuevo icono](../../assets/new.svg) Se han actualizado las validaciones interactivas de la compatibilidad del servicio para reflejar las versiones compatibles con las nuevas versiones de Adobe Commerce 2.3.3 y 2.2.10. Consulte [Requisitos del sistema](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html) en el _Guía de instalación_ para versiones recomendadas.<!-- MAGECLOUD-4018 -->

- ![icono de corrección](../../assets/fix.svg) Se ha mejorado el mensaje de registro devuelto cuando el proceso de administración de trabajos de cron en la fase de implementación intenta detener un trabajo de cron que ya ha finalizado para aclarar que este problema no es un error. Se ha cambiado el nivel de registro de `INFO` hasta `DEBUG`.<!-- MAGECLOUD-3653-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema al ejecutar el `setup:upgrade` que no interrumpió el proceso de implementación cuando se produjo un error durante el `app:config:import` tarea.<!-- MAGECLOUD-3806 -->

- ![nuevo icono](../../assets/new.svg) Se ha cambiado el nivel de registro predeterminado del controlador de archivos a `debug` para reducir la cantidad de detalles en el &quot;log&quot; mostrado en [!DNL Cloud Console], sin dejar de proporcionar información detallada para la depuración.<!-- MAGECLOUD-3871 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error con la implementación de contenido estático durante la compilación. Después de una instalación y `ece-tools` volcado de configuración, se produjo un error si no se especificó ninguna configuración regional para el usuario administrador en el `config.php` archivo. Ahora, hay una configuración regional predeterminada para el usuario administrador en `config.php` archivo.<!-- MAGECLOUD-3957 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error `Undefined index error` que se produce cuando `magento-cloud` El comando CLI falla en un entorno que no está configurado con una dirección URL segura (https). Ahora, el paquete ECE-Tools utiliza la URL base (http) si la URL segura no está disponible.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) Ahora puede realizar pruebas funcionales utilizando `ece-tools` en el entorno de Docker. Consulte [pruebas de aplicación](https://devdocs.magento.com/cloud/docker/docker-test-magecloud-pkg-code.html).<!-- MAGECLOUD-3129/3684 -->

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad para configurar módulos PHP usando el `.magento.app.yaml` archivo. Cualquiera [Extensiones PHP especificadas en la variable `.magento.app.yaml` archivo](https://devdocs.magento.com/cloud/project/magento-app-php-application.html#php-extensions) están disponibles en los contenedores Docker PHP.<!-- MAGECLOUD-3357 -->

   - ![nuevo icono](../../assets/new.svg) Hay nuevos comandos disponibles para mejorar la experiencia de la línea de comandos de Docker. Consulte la [`bin/magento-docker` de la referencia Docker](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la capacidad de utilizar Mutagen.io para sincronizar archivos durante el desarrollo entre el host local y Docker.<!-- MAGECLOUD-3559 -->

   - ![icono de corrección](../../assets/fix.svg) Se corrigió la ruta predeterminada al utilizar el entorno Docker. Ahora, cuando utiliza SSH para iniciar sesión en el contenedor Docker, se encuentra en la raíz del proyecto en el `/app` , según lo esperado.<!-- MAGECLOUD-3582 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha actualizado la biblioteca de Sodio de la versión 1.0.11 a la versión 1.0.18, y se ha actualizado la extensión PHP de Sodio.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Los clientes de Adobe Commerce en cloud Infrastructure deben [Enviar un ticket de asistencia de Adobe Commerce](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) para actualizar el paquete libsodio en los entornos Pro Production y Staging antes de actualizar a Adobe Commerce 2.3.2. Actualmente, no se pueden actualizar los entornos de Inicio a Adobe Commerce 2.3.2.

   - ![icono de corrección](../../assets/fix.svg) Se ha añadido la `analysis-icu` y el `analysis-phonetic` Complementos de Elasticsearch para todas las imágenes de Docker.<!-- MAGECLOUD-3446 -->

   - ![icono de corrección](../../assets/fix.svg) Validaciones mejoradas: al utilizar opciones para `docker:build` , debe proporcionar un valor al utilizar una opción. Además, se ha agregado validación para la versión del nodo al utilizar la variable `docker:build run` comando.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con los prefijos de tabla de base de datos mediante [Variable de entorno DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **FORCE_UPDATE_URLS** implemente la variable para actualizar las direcciones URL base al implementar en los entornos de producción y ensayo de Pro y Starter. Consulte la definición en la [implementar variables](../environment/variables-deploy.md#force_update_urls) contenido.<!-- MAGECLOUD-3602 -->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **TTFB_TESTED_PAGES** variable posterior a la implementación para configurar _Tiempo hasta el primer byte_ pruebas de página para comprobar el rendimiento de la aplicación en sitios implementados en la infraestructura de Cloud. Consulte la descripción de la variable en [variables posteriores a la implementación](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el SCD multiproceso, que provocaba errores aleatorios en la implementación de contenido estático. La solución consiste en configurar la variable **SCD_THREADS** variable a `1`. Ahora puede aumentar el recuento según sea necesario. Consulte las definiciones en la [implementar variables](../environment/variables-deploy.md#scd_threads) y el [variables de compilación](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![icono de corrección](../../assets/fix.svg) Puede configurar las variables **WARM_UP_PAGES** variable de entorno para almacenar en caché páginas únicas, varios dominios y varias páginas. Consulte la definición expandida en la [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages) contenido.<!-- MAGECLOUD-3258 -->

- ![icono de corrección](../../assets/fix.svg) Se ha añadido la `pub/static/.htaccess` a la lista de exclusiones. [Corrección presentada por Björn Kraus de PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error cuando todos los mensajes de validación se mostraban como `Critical` si al menos un validador de nivel crítico devolvió un error.<!-- MAGECLOUD-3178 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error de implementación si la dirección URL base no existía en la base de datos.<!-- MAGECLOUD-3075 -->

- ![nuevo icono](../../assets/new.svg) Se ha añadido una nueva **`env:config:show`mando** a la `ece-tools` que muestra servicios de entorno, rutas o variables. Consulte [Servicios, rutas y variables](https://devdocs.magento.com/cloud/reference/ece-tools-reference.html#services-routes-and-variables). [Característica enviada por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error crítico al intentar instalar Adobe Commerce 2.2.6 o anterior con `ece-tools` se desarrollan después de la refactorización del shell.<!-- MAGECLOUD-3665 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que las instalaciones de Adobe Commerce 2.1.x y 2.2.x fallaran con una advertencia sobre el uso de una versión obsoleta de Carbon.<!-- MAGECLOUD-3704 -->

- ![icono de corrección](../../assets/fix.svg) Se ha reducido el `cloud.log` nivel de registro para salida de shell desde `info` hasta `debug`.<!-- MAGECLOUD-3277 -->

- ![icono de corrección](../../assets/fix.svg) Se ha añadido la `--remove-definers (-d)` a la opción `ece-tools db-dump` para eliminar los definidores del archivo de volcado.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que sobrescribía el `env.php` durante una implementación, lo que provoca la pérdida de configuraciones personalizadas. Esta actualización garantiza que Adobe Commerce en la infraestructura en la nube actualice el `env.php` con cada implementación, conservando al mismo tiempo las configuraciones personalizadas.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - ![nuevo icono](../../assets/new.svg) Ahora, el entorno Docker admite la configuración de cron definida en la variable [propiedad crons del archivo .magento.app.yaml](https://devdocs.magento.com/cloud/project/magento-app-properties.html#crons).<!-- MAGECLOUD-3150 -->

   - ![nuevo icono](../../assets/new.svg) **Nuevo contenedor de Docker**—Se ha añadido un [Contenedor de proxy de terminación TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) para facilitar la terminación SSL de Varnish a través de HTTPS.<!-- MAGECLOUD-2890 -->

   - ![nuevo icono](../../assets/new.svg) **Nueva imagen de Docker**—Se ha agregado una imagen de Node.js para admitir Gulp y otras capacidades, como Pruebas unitarias de Jasmine JS.<!-- MAGECLOUD-3345 -->

   - ![nuevo icono](../../assets/new.svg) **Modos de generación de Docker**: ahora puede elegir iniciar el entorno Docker en [Modo de producción o modo de desarrollador](https://devdocs.magento.com/cloud/docker/docker-launch.html#set-the-launch-mode). El modo de desarrollador admite el desarrollo activo con permisos de sistema de archivos completos y grabables.<!-- MAGECLOUD-3152/3511 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que la implementación de Docker fallara con un `Name or service not known` error si la caché está configurada para un servicio que no está disponible. Ahora, puede quitar un servicio de [`.magento/services.yaml` archivo](https://devdocs.magento.com/cloud/project/services.html). El generador de configuración de Docker actualiza el servicio en la `docker/config.php.dist` Archivo automáticamente.<!-- MAGECLOUD-3369 -->

   - ![nuevo icono](../../assets/new.svg) Se han añadido validaciones interactivas para la compatibilidad del servicio. Ahora, si un servicio solicitado es incompatible con la versión de Adobe Commerce u otros servicios, la variable _modo interactivo_ pide al usuario un mensaje y una opción para continuar. Consulte la [Versiones de servicio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers) disponible para Docker. Utilice el `-n` para omitir la interactividad con fines de CI/CD.<!-- MAGECLOUD-3251 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con la composición de Docker `db-dump` que borró los volcados existentes.<!-- MAGECLOUD-3366 -->

   - ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que asignaba a Redis `session`, `default`, y `page_cache` almacenamiento en caché con el mismo ID de base de datos.<!-- MAGECLOUD-3172 -->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de variables de entorno**—

   - ![nuevo icono](../../assets/new.svg) El nuevo **ELASTICSUITE\_CONFIGURATION** retiene la configuración de servicio personalizada entre implementaciones. Consulte la definición en la [implementar variables](../environment/variables-deploy.md#elasticsuite_configuration) contenido.<!-- MAGECLOUD-3205 -->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la **SCD_MAX_EXECUTION_TIMEOUT** variable de entorno para que pueda aumentar el tiempo para completar la implementación del contenido estático desde el `.magento.env.yaml` archivo. Consulte la definición en la [implementar variables](../environment/variables-deploy.md#scd_max_execution_time), el [variables de compilación](../environment/variables-build.md#scd_max_execution_time), y el [variables globales](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![nuevo icono](../../assets/new.svg) Se ha añadido la **MAGENTO_CLOUD_LOCKS_DIR** variable de entorno para configurar la ruta al punto de montaje del proveedor de bloqueos en la infraestructura de la nube. El proveedor de bloqueo evita el inicio de trabajos cron y grupos cron duplicados. Esta variable es compatible con la versión 2.2.5 y posteriores de Adobe Commerce y se configura automáticamente. Consulte la definición en [Variables de nube](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![icono de corrección](../../assets/fix.svg) Se ha cambiado el **SCD_THREADS** valores predeterminados de variables de entorno para determinar automáticamente el valor óptimo en función del recuento de subprocesos de CPU detectado. Consulte las definiciones actualizadas en la [implementar variables](../environment/variables-deploy.md#scd_threads) y el [variables de compilación](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con un parche para el Mecanismo de aislamiento de BD que provocaba un error al actualizar a Adobe Commerce en la versión 2002.0.16 de la infraestructura de nube.<!-- MAGECLOUD-3383 -->

- ![icono de corrección](../../assets/fix.svg) Se ha añadido un parche que reemplaza a _Gráficos de imagen de Google_ con _Image-Charts_. Consulte el artículo de DevBlog [Desaprobación y actualización de los gráficos de imágenes de Google para M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![icono de corrección](../../assets/fix.svg) Se ha añadido la validación para [SEARCH_CONFIGURATION, variable](../environment/variables-deploy.md#search_configuration). La implementación falla cuando la opción &quot;motor&quot; no está establecida y `_merge` no es obligatorio.<!-- MAGECLOUD-3470 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que exponía datos confidenciales después de que se produjera una excepción. Ahora la información confidencial está enmascarada correctamente.<!-- MAGECLOUD-3525 -->

- ![icono de corrección](../../assets/fix.svg) Se ha mejorado la configuración de tolerancia a errores del paquete de Magento Open Source. En caso de que Adobe Commerce no pueda leer los datos de Redis `slave` Por ejemplo, se realiza una lectura desde el Redis `master` ejemplo. Consulte [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>El `ece-tools` La versión 2002.0.17 de incluye un parche de seguridad importante. Consulte [Recursos técnicos: Parches de Magento Open Source](https://magento.com/tech-resources/download#download2288).

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de servicio**: Compatible con las siguientes versiones de Adobe Commerce: 2.2.8 y posteriores 2.2.x, 2.3.1 y posteriores 2.3.x

   - Se ha agregado compatibilidad con la versión 6.x de Elasticsearch.<!-- MAGECLOUD-3196 -->

   - Se ha agregado compatibilidad con la versión 5.0 de Redis.

- ![nuevo icono](../../assets/new.svg) **Nuevas imágenes de Docker**—Se agregaron los siguientes servicios a la compilación de Docker:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno**: anteriormente, había un tiempo de espera codificado para la compresión SCD. Ahora puede configurar el tiempo de espera de compresión de SCD usando el **SCD_COMPRESSION_TIMEOUT** variable de entorno. Consulte las definiciones en la [variables de compilación](../environment/variables-build.md#scd_compression_timeout) y el [implementar variables](../environment/variables-deploy.md#scd_compression_timeout) contenido.<!-- MAGECLOUD-2870 -->

- ![icono de corrección](../../assets/fix.svg) Se ha añadido la `--use-rewrites` al comando install para que utilice reescrituras de servidores web para vínculos generados en la tienda y acceso de administrador para mejorar la seguridad y la experiencia del cliente.<!-- MAGECLOUD-3246 -->

- ![icono de corrección](../../assets/fix.svg) Se han añadido marcas de hora a `var/log/install_upgrade.log` para que muestre las fechas de los eventos de instalación y actualización.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - Ahora, la configuración de servicio predeterminada generada en el entorno de Docker es la misma que la configuración predeterminada en la plantilla en la nube.<!-- MAGECLOUD-3025 -->

   - Puede enviar correo desde el entorno de Docker mediante el `sendmail` servicio.<!-- MAGECLOUD-2907 -->

   - Se ha añadido la capacidad de [configurar Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) para depurar en el entorno de Cloud Docker.<!-- MAGECLOUD-2891 -->

   - Se ha corregido un problema con los permisos del servicio web al generar el `docker-compose.yml` archivo.<!-- MAGECLOUD-2883 -->

- ![nuevo icono](../../assets/new.svg) **Mejora de la actualización**: se ha añadido la validación para confirmar que la variable `autoload` propiedad en el `composer.json` contiene los cambios de configuración necesarios antes de actualizar a Adobe Commerce v2.3. Consulte [Versión de actualización](https://devdocs.magento.com/cloud/project/project-upgrade.html).<!-- MAGECLOUD-2392 -->

- ![nuevo icono](../../assets/new.svg) El proceso de compresión al implementar contenido estático ahora incluye todos los recursos (generados de forma nativa o personalizados) y se produce durante la fase de compilación al principio del [`build:transfer` sección](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks). Anteriormente, el proceso de compresión se producía antes de aplicar la minificación y el agrupamiento personalizados de los recursos estáticos. [Corrección enviada por Rafael Garcia Lepper de Trytens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![icono de corrección](../../assets/fix.svg) Se corrigió un error de conexión a base de datos que se producía durante la implementación inmediatamente después de configurar una base de datos y relación de servicio adicionales. Además, esta corrección soluciona un problema que se producía durante el proceso de configuración de Commerce Reporting for Starter. Para empezar, esta actualización es un elemento &quot;obligatorio&quot; para utilizar la Creación de informes de Commerce.<!-- MAGECLOUD-3035 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de validación con la configuración de la base de datos que provocaba que fallara el proceso de implementación.<!-- MAGECLOUD-3003 -->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado la restricción con la versión adecuada de `symfony/yaml` paquete para usar con [Constantes de PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants). El análisis constante no funciona cuando se utiliza un `symfony/yaml` versión del paquete anterior a 3.2. [Corrección enviada por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![nuevo icono](../../assets/new.svg) **Comprobación de configuración del entorno**—Se ha añadido validación para comprobar la versión de PHP y advertir a los usuarios si no están utilizando la última versión recomendada.<!--MAGECLOUD-2903-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el procesamiento de variables JSON con formato incorrecto. Ahora, si una variable JSON provoca un error de sintaxis, aparece una advertencia en la variable `cloud.log` el archivo y la implementación siguen utilizando la variable predeterminada.<!-- MAGECLOUD-2851 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de conexión que se producía durante la implementación inmediatamente después de deshabilitar el servicio Redis.<!-- MAGECLOUD-2747 -->

- ![nuevo icono](../../assets/new.svg) **Registrando cambios**: se ha actualizado el [nivel de registro](../environment/log-handlers.md#log-levels) de `Info` hasta `Notice` para los siguientes eventos de proceso de compilación e implementación:<!--MAGECLOUD-2925-->

   - Inicio y final del proceso de reconciliación de módulos instalados en `composer.json` con valores de configuración compartidos en `app/etc/config.php` archivo

   - Inicio y final del proceso de validación de la configuración

   - Inicio y final del `setup:di:compile` proceso de generación de clases

- ![nuevo icono](../../assets/new.svg) **Nuevas variables de entorno**—

   - **[Variable de implementación RESOURCE_CONFIGURATION](../environment/variables-deploy.md#resource_configuration)**: utilice esta variable para asignar un nombre de recurso a una conexión de base de datos.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[Variable global X_FRAME_CONFIGURATION](../environment/variables-global.md#x_frame_configuration)**: utilice esta variable para cambiar el `X-Frame-Options` configuración del encabezado para procesar una página de Adobe Commerce en una `<frame>`, `<iframe>`, o `<object>`.<!-- MAGECLOUD-3048 -->

- ![icono de corrección](../../assets/fix.svg) **Actualizaciones de variables de entorno**: se han cambiado las siguientes variables de entorno:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)**: se ha añadido la capacidad de precargar la caché para páginas especificadas en todos los dominios definidos para un almacén de Adobe Commerce. Anteriormente, si el sitio estaba configurado con varios dominios, el proceso posterior a la implementación no podía cargar previamente la caché para las páginas especificadas en dominios no predeterminados y devolvió el siguiente error en el registro posterior a la implementación: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL**: se ha actualizado la documentación y la muestra `.magento.env.yaml` con los valores predeterminados correctos para el nivel de compresión SCD. Consulte las definiciones en la [variables de compilación](../environment/variables-build.md#scd_compression_level) y el [implementar variables](../environment/variables-deploy.md#scd_compression_level) contenido.<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES**: esta variable de entorno está obsoleta. Utilice el [SCD_MATRIX](../environment/variables-build.md#scd_matrix) para controlar la configuración del tema.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**: se ha corregido el proceso de validación para evitar un problema que se producía cuando SCD_MATRIX ignoraba un valor de tema que contenía casos de caracteres diferentes. Consulte las definiciones en la [variables de compilación](../environment/variables-build.md#scd_matrix) y el [implementar variables](../environment/variables-deploy.md#scd_matrix) contenido.<!-- MAGECLOUD-2904 -->

   - **Variables de ADMINISTRACIÓN**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - Se ha mejorado la seguridad al administrar credenciales para el usuario administrador mediante variables de entorno. Ya no puede utilizar las variables de entorno ADMIN_EMAIL, ADMIN_USERNAME y ADMIN_PASSWORD para anular las credenciales de administrador durante las actualizaciones. Si no puede acceder al panel de administración, utilice el _Contraseña olvidada_ para la función `admin:user:create` Comando CLI para crear un nuevo usuario administrador. Consulte [Acceso a su panel de administración](https://devdocs.magento.com/cloud/onboarding/onboarding-tasks.html#admin).

      - ADMIN_EMAIL ya no es necesario al actualizar o aplicar parches.

## v2002.0.15

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de Docker**—

   - Ahora el generador Docker utiliza los servicios especificados en la variable `.magento.app.yaml` y `.magento/services.yaml` archivos de configuración al [creación del entorno de Docker](https://devdocs.magento.com/cloud/docker/docker-config.html). Puede elegir una versión de servicio diferente mediante parámetros de compilación.<!-- MAGECLOUD-2888 -->

   - Imagen de PHP 7.2 añadida: compatibilidad añadida con PHP 7.2 en Cloud Docker; se ha actualizado la [Configuración de Launch Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) para incluir el `docker:build --php` para especificar la versión de PHP compatible con su versión de Adobe Commerce.<!-- MAGECLOUD-2799 -->

   - Se ha añadido un [Contenedor de Cron](https://devdocs.magento.com/cloud/docker/docker-containers-cli.html#cron-container) basado en la imagen PHP-CLI.<!-- MAGECLOUD-2565 -->

   - Se agregaron los siguientes servicios a la versión de Docker:

      - [!DNL RabbitMQ] 3.5 y 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 y 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 y 4.0<!-- MAGECLOUD-2886 -->

- ![nuevo icono](../../assets/new.svg) **Configurar con constantes de PHP**—Se ha añadido compatibilidad con [Constantes de PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants) en el `.magento.env.yaml` archivo de configuración.<!-- MAGECLOUD- 2575 -->

- ![nuevo icono](../../assets/new.svg) **Nueva variable de entorno**: de forma predeterminada, solo el entorno de producción tiene Google Analytics activados. Puede habilitar a los Google Analytics en los entornos de ensayo e integración mediante el  [Variable de entorno ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que eliminaba las configuraciones personalizadas de cron de `env.php` después de una reimplementación. Ahora, las configuraciones personalizadas de cron permanecen de forma segura en `env.php` archivo.<!-- MAGECLOUD-2923 -->

- ![icono de corrección](../../assets/fix.svg) Se han corregido incoherencias en los mensajes y [niveles de registro](../environment/log-handlers.md#log-levels) para las fases de compilación, implementación y posterior a la implementación. Niveles de mensaje de registro inicial y final aumentados desde **información** hasta **aviso** para todas las fases y subfases. Se han añadido mensajes de registro de inicio y final, cuando corresponde.<!-- MAGECLOUD-2919 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que involucraba procesos cron y que impedía el inicio de la fase posterior a la implementación, cuando se configuraba. Ahora, si tiene habilitado el vínculo posterior a la implementación, los procesos cron vuelven a habilitarse al principio de la fase posterior a la implementación.<!-- MAGECLOUD-2862 -->

- ![icono de corrección](../../assets/fix.svg) Se ha resuelto un problema que impedía una instalación correcta de Adobe Commerce al especificar una configuración de base de datos personalizada. Anteriormente, el proceso de instalación utilizaba la configuración de base de datos del [Variable Magento_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) incluso si ha designado información de conexión personalizada en [Variable de entorno DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido el `config:dump` para que incluya cada configuración regional del sitio web en la variable `system` de la sección `config.php` archivo.<!--MAGECLOUD-2740-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba _calentamiento_ errores durante la fase posterior a la implementación al corregir la referencia de la URL base de origen.<!--MAGECLOUD-2797-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que generaba archivos incorrectamente durante la `setup:di:compile` proceso, que afectaba al módulo Amazon Pay.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![nuevo icono](../../assets/new.svg) **Verificar estado ideal**: la `ideal-state` El asistente ahora verifica la configuración actual durante cada implementación y proporciona instrucciones claras para actualizar la configuración a fin de lograr una implementación más rápida y sin tiempo de inactividad.<!--MAGECLOUD-2372-->

- ![icono de corrección](../../assets/fix.svg) **Conformidad con PCI**—Se han actualizado los protocolos de mensajería para Adobe Commerce en la infraestructura en la nube para requerir Transport Layer Security (TLS) versión 1.2 al conectarse con servicios de mensajería de terceros. Si utiliza un servicio de mensajes que no es compatible con TLS versión 1.2, debe actualizar el servicio. De lo contrario, se muestra el siguiente mensaje de error cuando la aplicación de Adobe Commerce intenta conectarse al servidor de mensajes para enviar un correo electrónico: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![nuevo icono](../../assets/new.svg) **Mejora de la implementación**: se ha añadido la validación para advertir a los clientes si un entorno de ensayo o producción tiene `dev`, `debug`, o `debug_logging` opciones habilitadas para evitar problemas de rendimiento causados por una actividad de registro excesiva.<!--MAGECLOUD-2517-->

- ![icono de corrección](../../assets/fix.svg) **Correcciones de implementación**—

   - Ahora el modo de mantenimiento está habilitado al principio de la fase de implementación y deshabilitado al final. Si la implementación falla, el sitio permanece en modo de mantenimiento hasta que se resuelvan los problemas de implementación. Anteriormente, el sitio volvía al modo de producción incluso si la implementación fallaba.<!--MAGECLOUD-2603-->

      - Se han modificado las comprobaciones de validación de la fase de implementación para reducir el nivel de error en los siguientes problemas de implementación de `CRITICAL` hasta `WARNING` para que la implementación finalice. Anteriormente, estos problemas provocaban que la implementación fallara.

      - La configuración del entorno contiene valores incorrectos para las variables de implementación o nube.

   - La versión del Elasticsearch en la infraestructura de la nube no es compatible con la versión del módulo elasticsearch/elasticsearch compatible con Adobe Commerce en la infraestructura de la nube. Consulte la [artículo de solución de problemas del Elasticsearch](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) en la base de conocimiento de asistencia de Adobe Commerce.<!--MAGECLOUD-2600-->

   - Se ha corregido un problema con la configuración compartida en `app/etc/config.php` archivo que provocó `recursion detected` errores durante la implementación.<!--MAGECLOUD-2173-->

- ![icono de corrección](../../assets/fix.svg) **Correcciones relacionadas con Cron**—

   - Se ha corregido un problema de programación cron que impedía que los trabajos se ejecutaran si especificaba una frecuencia cron distinta de la predeterminada (1 minuto).<!--MAGECLOUD-2602-->

   - Se ha corregido un problema en la fase de implementación que permitía que los trabajos cron siguieran ejecutándose durante la implementación, lo que podía provocar bloqueos de la base de datos y otros problemas críticos. Ahora, todos los trabajos cron se detienen antes de que comience la fase de implementación y se reinician después de que finalice la implementación.&lt;!—MAGECLOUD—2537—>

   - Se ha corregido el flujo de trabajo de cron en las versiones 2.2.x para desbloquear los trabajos de cron congelados de modo que se puedan detener antes de iniciar la implementación. Anteriormente, un trabajo cron bloqueado provocaba que la implementación se detuviera.<!--MAGECLOUD-2501-->

- ![icono de corrección](../../assets/fix.svg) Se ha cambiado el formato del `config.php` archivo generado por el `vendor/bin/ece-tools config:dump` para utilizar sintaxis de matriz corta y sangría de 4 espacios para cumplir con los estándares de codificación de Adobe Commerce.<!--MAGECLOUD-2527-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de implementación que se producía cuando la variable `.magento.env.yaml` contains `{{ base_url }}` y `{{ unsecure_base_url }}` marcadores de posición para configuraciones web en lugar de la configuración de URL predeterminada para un proyecto de Adobe Commerce en la nube./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![nuevo icono](../../assets/new.svg) **Habilitar implementación sin tiempo de inactividad**: ahora Adobe Commerce en la infraestructura de la nube pone en cola las solicitudes con los cambios de base de datos necesarios durante la implementación y aplica los cambios en cuanto se completa la implementación. Las solicitudes se pueden retener durante un máximo de 5 minutos para garantizar que no se pierdan sesiones. Consulte [Opciones de implementación de contenido estático para reducir el tiempo de inactividad de la implementación en la nube](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![nuevo icono](../../assets/new.svg) **Docker Compose para Cloud**: se han realizado las siguientes mejoras en el [Configuración y configuración de Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) proceso:

   - Se ha añadido un comando—`docker:config:convert` para convertir archivos de configuración de PHP al formato Docker ENV para simplificar la configuración del entorno. Ahora, usted copia los archivos de configuración de PHP en el directorio Docker y los convierte a los archivos ENV Docker. Consulte [Iniciar Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).<!--MAGECLOUD-2359-->

   - El proceso de instalación de Adobe Commerce en la nube ahora admite la implementación en sistemas de archivos de solo lectura y de lectura-escritura para emular más estrechamente el sistema de archivos en la nube. Consulte [Configurar Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).&lt;!—MAGECLOUD—2357—>

   - **Compatibilidad con el servicio Redis**: se ha añadido una imagen de Redis, que se implementa en un contenedor de Docker y se configura automáticamente para que funcione con la instalación de Docker.&lt;!—MAGECLOUD—2442—>

   - Ahora tiene la capacidad de volcado de la base de datos al utilizar Cloud Docker [contenedor de base de datos](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#database-container). Además, puede [compartir archivos](https://devdocs.magento.com/cloud/docker/docker-containers.html#sharing-data-between-host-machine-and-container) entre un equipo host y un contenedor mediante `docker/mnt` directorio.<!-- MAGECLOUD-2577 -->

   - **Soporte de servicio de barniz**— Se ha añadido una imagen de barniz, que se implementa automáticamente en un contenedor de Docker. Después de la implementación, puede configurar manualmente Varnish siguiendo las prácticas recomendadas de Adobe Commerce. Consulte [Configuración y uso de Barniz](https://devdocs.magento.com/guides/v2.3/config-guide/varnish/config-varnish.html).&lt;!—MAGECLOUD—2358—>

   - Acceso seguro al sitio: se ha añadido la compatibilidad con SSL para acceder a la tienda de Adobe Commerce y al panel de administración.&lt;!—MAGECLOUD—2360—>

- ![icono de corrección](../../assets/fix.svg) **Compatibilidad mejorada con Adobe Commerce en la extensión de la infraestructura en la nube**: se ha bajado de categoría el requisito de versión mínima para el paquete guzzlehttp/guzzle en Adobe Commerce en la infraestructura en la nube. [archivo composer.json](https://devdocs.magento.com/cloud/reference/cloud-composer.html) a la versión 6.2 para que el `ece-tools` es compatible con más extensiones.<!--MAGECLOUD-2205-->

- ![nuevo icono](../../assets/new.svg) **Aplicar cambios personalizados a la aplicación de Adobe Commerce durante la fase de compilación**: dividimos la fase de compilación en dos procesos independientes para que pueda utilizar vínculos para aplicar cambios personalizados al contenido estático generado antes de empaquetar la aplicación para su implementación. El _generar:generar_ Este proceso genera código, aplica parches y genera contenido estático. El _generar:transferir_ El proceso transfiere el código generado y el contenido estático al destino final. Consulte [Enlaces de aplicaciones](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks).<!--MAGECLOUD-2363-->

- ![icono de corrección](../../assets/fix.svg) **Comprobaciones de configuración del entorno**: validación mejorada de la configuración del entorno para advertir a los clientes sobre las incompatibilidades de la versión y los errores de configuración antes de crear e implementar Adobe Commerce en la infraestructura en la nube.

   - Se ha añadido una validación específica de la versión para identificar variables y valores de entorno no admitidos o obsoletos.<!--MAGECLOUD-2183-->

   - Se ha añadido una comprobación de compatibilidad de Elasticsearch para advertir a los usuarios sobre los problemas de configuración de Elasticsearch. Ahora, la implementación falla si la versión del servicio Elasticsearch en el servidor no es compatible con Adobe Commerce. Anteriormente, la implementación se realizaba correctamente incluso si la versión del Elasticsearch era incompatible, lo que provocaba problemas con el catálogo de productos después de la implementación del sitio.<!--MAGECLOUD-2389-->

     Para resolver la incompatibilidad, haga lo siguiente [envío de un ticket de asistencia](https://devdocs.magento.com/cloud/trouble/trouble.html) para actualizar Elasticsearch a una versión compatible o cambiar la configuración de Adobe Commerce para especificar una versión compatible del cliente PHP de Elasticsearch.

      - Para la versión de Adobe Commerce 2.1.x a 2.2.2, actualice al Elasticsearch a la versión 2.4.

      - Para la versión 2.2.3 y posteriores de Adobe Commerce, actualice Elasticsearch a la versión 5.2.

      - Si tiene Elasticsearch 1.x o 2.x y no desea actualizar, actualice el requisito de la versión del cliente PHP del Elasticsearch de Adobe Commerce en composer.json a `"elasticsearch/elasticsearch": "~2.0"`.

   - Se ha mejorado la validación de las variables de entorno para identificar las opciones de configuración que pueden provocar conflictos durante las fases de generación, implementación y posterior a la implementación. Por ejemplo, se muestra un mensaje de advertencia durante el proceso de instalación y actualización si la configuración global para la implementación de contenido estático entra en conflicto con la configuración en la fase de compilación o implementación.<!--MAGECLOUD-2156-->

- ![icono de corrección](../../assets/fix.svg) **Actualizaciones de variables de entorno**: se han cambiado las siguientes variables de entorno:

   - **[Variable global SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)**: se ha cambiado el valor por defecto a `true` para habilitar la minificación de contenido HTML bajo demanda, que minimiza el tiempo de inactividad al implementar en entornos de Ensayo y Producción. Esta configuración es necesaria para implementaciones sin tiempo de inactividad.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES implementar variable](../environment/variables-deploy.md#clean_static_files)**: se ha añadido la capacidad de administrar el procesamiento de archivos estáticos limpios para el contenido estático generado durante la fase de compilación en función de la configuración de la variable de entorno CLEAN_STATIC_FILES. Anteriormente, los archivos de contenido estático generados durante la fase de compilación siempre se limpiaban.<!--MAGECLOUD-1506-->

- ![icono de corrección](../../assets/fix.svg) **Registro**: se han realizado los siguientes cambios para mejorar los mensajes de registro y reducir el tamaño del registro:

   - Las entradas del registro de errores de implementación ahora incluyen el resultado del comando de las operaciones que provocan los errores aunque la configuración del entorno no especifique el registro de nivel de depuración. Consulte [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - Se ha agregado el registro de los errores de implementación que se producen cuando las fábricas generadas requeridas por algunas extensiones no se pueden generar correctamente porque el sistema de archivos está en estado de solo lectura.<!--MAGECLOUD-2209-->

   - Se ha reducido el tamaño del registro de implementación y se han corregido los problemas de formato causados por los comandos de configuración que utilizan la barra de progreso interactiva.<!--MAGECLOUD-2402-->

   - Se ha eliminado la verbosidad innecesaria y se han actualizado los niveles de prioridad de algunas sentencias de registro.<!--MAGECLOUD-2227-->

- ![icono de corrección](../../assets/fix.svg) **Correcciones específicas de Cron**—

   - Se han cambiado los ajustes de configuración predeterminados del trabajo cron para la duración del historial de 3d (4320 min) a 1h (60 min) para evitar problemas de rendimiento y errores de implementación que pueden producirse cuando la cola cron se llena demasiado rápido.<!--MAGECLOUD-2427-->

   - Se ha mejorado el proceso de administración de trabajos de cron durante la fase de implementación para evitar bloqueos de bases de datos y otros problemas críticos. Ahora, todos los trabajos cron se detienen durante la fase de implementación y se reinician una vez finalizada la implementación.<!--MAGECLOUD-2445-->

   - Se ha corregido un problema con el mecanismo de bloqueo para programar consumidores iniciado por los trabajos cron en las versiones 2.2.0 y posteriores de Adobe Commerce para evitar que los trabajos cron lancen consumidores duplicados.<!--MAGECLOUD-2464-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con [proceso de compresión de contenido estático](../environment/variables-intro.md) (`gzip`) que causó `not overwritten` y `no such file or directory` errores al hacer referencia al archivo comprimido durante el proceso de implementación.<!-- MAGECLOUD-2182-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que impedía que `php ./vendor/bin/ece-tools config:dump` comando para eliminar secciones redundantes de la `config.php` durante el proceso de volcado si no se especifica la configuración regional del almacén. Ahora puede mover fácilmente los archivos de configuración entre entornos. Después de actualizar a `ece-tools` v2002.0.13, regenerar anterior `config.php` archivos con el mejorado `config:dump` comando. Consulte [Administración de configuración para ajustes de tienda](https://devdocs.magento.com/cloud/live/sens-data-over.html).<!--MAGECLOUD-2444-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un error durante la fase de implementación si la configuración de ruta en `.magento/routes.yaml` redirecciones de archivos desde un [ápice](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) dominio a `www` dominio.<!--MAGECLOUD-2556-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con `_merge` para la opción [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) que producía resultados de combinación incorrectos si no se incluye la variable `engine` en el campo actualizado `.magento.env.yaml` archivo de configuración. Ahora, la operación de combinación sobrescribe correctamente solo los valores especificados en el `.magento.env.yaml` sin que sea necesario configurar el `engine` parámetro.<!--MAGECLOUD-2520-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración de Redis que habilitaba incorrectamente el bloqueo de sesiones para Adobe Commerce en las versiones 2.2.1 y posteriores de la infraestructura en la nube, lo que puede provocar un rendimiento lento y tiempos de espera. Ahora, el bloqueo de sesión está deshabilitado de forma predeterminada. El problema se debía a un cambio en el comportamiento predeterminado del `disable_locking` parámetro introducido en la versión 1.3.4 del paquete del controlador de sesión de Redis. Consulte [paquete colinmollenhour/php-redis-session-abstract](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![nuevo icono](../../assets/new.svg) **Docker Compose para Cloud**—Se ha añadido un comando—`docker:build`: para generar un [Docker Compose](https://devdocs.magento.com/cloud/docker/docker-config.html) desde la nube `ece-tools` repositorio.<!-- MAGECLOUD-2250 -->

- ![nuevo icono](../../assets/new.svg) **Cambiar configuraciones regionales**: ahora puede cambiar la configuración regional del almacén sin el proceso de exportación e importación de la configuración. Mientras la aplicación se encuentra en Producción y el SCD_ON_DEMAND está habilitado, las opciones de configuración regional del almacén y del administrador están disponibles.<!-- MAGECLOUD-2019 -->

- ![nuevo icono](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Mapa del sitio y robots**: se ha creado un [workflow](../store/robots-sitemap.md) para agregar un `robots.txt` y generar un `sitemap.xml` para una configuración de dominio único sin requerir un cambio en la infraestructura.

- ![nuevo icono](../../assets/new.svg) **Asistentes**—Se agregaron dos [magos](../deploy/smart-wizards.md) para ayudarle con la configuración de la nube:<!-- MAGECLOUD-1910 -->

   - `ideal-state`: configure el estado ideal para un tiempo de inactividad mínimo en la implementación.

   - `master-slave`—configure el equilibrio de carga para la base de datos y Redis

- ![nuevo icono](../../assets/new.svg) **Actualización de módulo**—Se ha añadido un comando de nube—`module:refresh`: para habilitar módulos que estaban desactivados o no activados explícitamente, de forma similar a como se hace automáticamente durante una compilación.<!-- MAGECLOUD-1521 -->

- ![nuevo icono](../../assets/new.svg) Se ha añadido la capacidad de elegir combinar o sobrescribir la configuración de los servicios de mediante `_merge` opción en [CACHÉ](../environment/variables-deploy.md#cache_configuration), [SESIÓN](../environment/variables-deploy.md#session_configuration), [COLA](../environment/variables-deploy.md#queue_configuration), y [BUSCAR](../environment/variables-deploy.md#search_configuration) configuraciones.<!-- MAGECLOUD-2105 -->

- ![nuevo icono](../../assets/new.svg) **Archivo de muestra de configuración del entorno**—Hemos añadido un `.magento.env.yaml` archivo de muestra al paquete ECE-Tools que incluye una descripción detallada y los valores posibles para cada variable de entorno.<!-- MAGECLOUD-1908 -->

   - También hemos añadido una validación profunda para `.magento.env.yaml` que evita errores en el proceso de implementación causados por valores inesperados. Cuando se produce un error, ahora recibe un mensaje de error detallado que comienza con lo siguiente: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![nuevo icono](../../assets/new.svg) Se ha añadido lo siguiente [**Variables de entorno**](../environment/variables-intro.md):

   - Ahora puede definir varias configuraciones regionales para cada tema con el nuevo [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) variable de entorno, lo que reduce la cantidad de archivos de tema que se van a implementar.<!-- MAGECLOUD-1501 -->

   - Se ha añadido la [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) variable de entorno para personalizar las conexiones de base de datos para la implementación.<!-- MAGECLOUD-2047 -->

   - El nuevo [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) anula el nivel de registro mínimo de todas las secuencias de salida sin realizar cambios en el código.<!-- MAGECLOUD-2129 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba un tiempo de inactividad entre la fase de implementación y la posterior a la implementación. Ahora comienza la fase posterior a la implementación _inmediatamente_ una vez finalizada la fase de implementación.

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que no limpiaba los trabajos de cron correctos, los que tenían `status = success`, en la programación.<!-- MAGECLOUD-2268 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con `post_deploy` gancho que borró la caché en la fase de implementación en lugar de en la fase posterior a la implementación del proyecto.<!-- MAGECLOUD-2113 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que se producía al usar SCD con varias configuraciones regionales, que generaban lo mismo `js-translation.json` para cada configuración regional.<!-- MAGECLOUD-2034 -->

- ![icono de corrección](../../assets/fix.svg) Optimizado para `db:dump` comando en la `ece-tools` paquete para evitar el bloqueo de tablas y aumentar la velocidad.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>La versión 2002.0.11 de ECE-Tools es necesaria para la compatibilidad con la versión 2.2.4.

- ![nuevo icono](../../assets/new.svg) **Configurar conexiones de solo lectura a nodos no maestros**: esta versión añade la capacidad de configurar una conexión de solo lectura a un nodo no maestro para recibir tráfico de solo lectura (para  [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) y para <!--MAGECLOUD-143 -->

- ![nuevo icono](../../assets/new.svg) **Asistente de configuración**: se ha añadido un asistente para comprobar la configuración de la implementación de contenido estático. Consulte [Asistentes inteligentes](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con Symfony Console**—Se ha agregado compatibilidad con Symfony Console 4 con Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![icono de corrección](../../assets/fix.svg) **Optimizaciones de programación de Cron**: se ha mejorado la administración de colas y el registro para ayudar a depurar los problemas relacionados con cron.<!-- MAGECLOUD-1607 -->

- ![icono de corrección](../../assets/fix.svg) La validación de implementación falla si `ADMIN_EMAIL` o `ADMIN_USERNAME` El valor es el mismo que el de una cuenta de administrador existente.<!-- MAGECLOUD-1221 -->

- ![icono de corrección](../../assets/fix.svg) Se ha eliminado la compatibilidad con SOLR para versiones 2.2.x. Las versiones 2.1.x conservan la capacidad de habilitar SOLR.<!-- MAGECLOUD-1282 -->

- ![icono de corrección](../../assets/fix.svg) La primera instalación de los entornos de ensayo y producción de un proyecto PRO ahora incluye diferentes prefijos de índice para el Elasticsearch a fin de evitar posibles conflictos e identificar registros que pertenecen a cada entorno.<!-- MAGECLOUD-1489 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que interrumpía la fase de compilación de la arquitectura heredada durante la implementación del contenido estático.<!-- MAGECLOUD-2021 -->

- ![icono de corrección](../../assets/fix.svg) **Mejoras específicas de Cron**—Se ha vuelto a trabajar la implementación de cron:<!-- MAGECLOUD-1607 -->

   - Se ha corregido un problema que provocaba que la cola cron se llenara rápidamente. Ahora borra los trabajos obsoletos de cron de una manera más confiable.

   - Se ha reorganizado la secuencia de trabajos cron para que todos los trabajos de subprocesos independientes se inicien antes del grupo general.

   - Se ha mejorado el registro para ayudar mejor a depurar los problemas de cron.

   - **NOTA**—Esta versión aborda muchos problemas relacionados con cron. Si está utilizando algunos parches crónicos en _m2-hotfixes_, elimínelos.

- ![icono de corrección](../../assets/fix.svg) **Mejoras específicas de SCD**—

   - Puede usar el complemento `VERBOSE_COMMANDS` y el `SCD_COMPRESSION_LEVEL` variables de entorno durante ambas _generar_ y de_ploy.<!-- MAGECLOUD-1819 -->

   - Se ha corregido un problema que provocaba que la implementación fallara con un error aleatorio al encontrar un valor inesperado para `SCD_COMPRESSION_LEVEL` variable de entorno. Se ha mejorado la validación de la configuración para proporcionar notificaciones significativas. Consulte [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) para valores aceptables.<!-- MAGECLOUD-2043 -->

   - Se ha corregido el comportamiento del `SCD_COMPRESSION_LEVEL` La configuración de la variable de entorno fluye para que las anulaciones funcionen según lo esperado.<!-- MAGECLOUD-2044 -->

   - Se ha corregido un problema que impedía la configuración del `SCD_THREADS` variable de entorno en `.magento.env.yaml` archivo _implementar_ escenario.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![nuevo icono](../../assets/new.svg) **Implementación de contenido estático (SCD)**: existe un nuevo proceso de implementación alternativo para generar contenido estático cuando se solicita (bajo demanda). Esto reduce el tiempo de inactividad y mejora la administración de la caché al generar los activos más críticos.<!-- MAGECLOUD-1285 -->

   - **Nueva variable de entorno**—Añadido el `SCD_ON_DEMAND` variable de entorno global para generar contenido estático cuando se solicita.<!-- MAGECLOUD-1738 -->

   - **Gancho posterior a la implementación**—Se ha añadido un `post_deploy` gancho para el `.magento.app.yaml` que borra la caché y la precarga (calienta) _después_ el contenedor empieza a aceptar conexiones. Solo está disponible para proyectos Pro que contienen entornos de ensayo y producción en la [!DNL Cloud Console] y para Proyectos iniciales. Aunque no es obligatorio, esto funciona junto con el `SCD_ON_DEMAND` variable de entorno.<!-- MAGECLOUD-1788 -->

- ![nuevo icono](../../assets/new.svg) **Optimización**: optimizado el movimiento o la copia de archivos durante la implementación para mejorar la velocidad de implementación y reducir las cargas en el sistema de archivos.<!-- MAGECLOUD-1842 -->

- ![nuevo icono](../../assets/new.svg) **Registro de implementación**—Se ha añadido la capacidad de habilitar controladores de formato de registro extendido (GELF) Syslog y Graylog para la salida de registros durante el proceso de implementación. Consulte [Controladores de registro](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![nuevo icono](../../assets/new.svg) Se ha añadido lo siguiente [**Variables de entorno**](../environment/variables-intro.md):

   - `CRYPT_KEY`: permite proporcionar una clave criptográfica a otro entorno al mover una base de datos.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Global_ que omite la copia de los archivos de vista estática en la `var/view_preprocessed` y genera un HTML minificado cuando se solicita.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Global_ variable de entorno para generar contenido estático cuando se solicita.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES`: se pueden enumerar las páginas que se deben utilizar para cargar previamente la caché. Disponible en el nuevo [Variables posteriores a la implementación](../environment/variables-post-deploy.md).

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que implicaba que un parche aplicado localmente rompía la implementación en una instancia de. Ahora, ECE-Tools puede detectar que se ha aplicado un parche.<!-- MAGECLOUD-982 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un conflicto entre el agrupamiento de JavaScript y la funcionalidad GZIP. Ahora, estas funciones funcionan correctamente juntas.<!-- MAGECLOUD-1735 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que fallaran los comandos CLI de ECE-Tools al usar versiones anteriores de PHP 7.0.x.<!-- MAGECLOUD-1744 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que impedía la implementación de contenido estático con la estrategia compacta en varios subprocesos.<!-- MAGECLOUD-1822 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema con el bloqueo de sesión de Redis que provocaba un retraso en el inicio de sesión del administrador. Además, la corrección está disponible para 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Usted debe [actualización del metapaquete de Adobe Commerce en la infraestructura en la nube](../dev-tools/install-package.md#update-the-metapackage) para obtener esta y todas las actualizaciones futuras.

- ![nuevo icono](../../assets/new.svg) **ece-tools**: la `ece-tools` ahora es compatible con Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![nuevo icono](../../assets/new.svg) **Configuración de Redis**: ahora puede [configurar Redis](../environment/variables-deploy.md#cache_configuration) página y caché predeterminada, y almacenamiento de sesión de Redis mediante una variable de entorno.<!-- MAGECLOUD-1552 -->

- ![nuevo icono](../../assets/new.svg) **Mejoras en los servicios de Search, AMQP y Redis**: hemos unificado el flujo de configuración del servicio para que ahora se comporte del mismo modo para todos los servicios. Edición manual de `env.php` ya no se admite el archivo para configurar servicios. Debe utilizar variables de entorno para la variable `.magento.env.yaml` en su lugar.<!-- MAGECLOUD-1437 -->

- ![icono de corrección](../../assets/fix.svg) **Variables de entorno**—

   - El uso de `env:STATIC_CONTENT_THREADS` estaba obsoleto y se eliminará en una versión futura. Utilice el [SCD_THREADS](../environment/variables-deploy.md#scd_threads) en su lugar.<!-- MAGECLOUD-1507 -->

   - El `STATIC_CONTENT_EXCLUDE_THEMES` la variable de entorno estaba obsoleta. Debe utilizar el `SCD_EXCLUDE_THEMES` en su lugar.<!-- MAGECLOUD-1640 -->

- ![icono de corrección](../../assets/fix.svg) **Registro**: simplificamos el registro en torno a las operaciones de aplicación de parches integradas.<!-- MAGECLOUD-1674 -->

- ![icono de corrección](../../assets/fix.svg) Hemos eliminado `developer` compatibilidad con el modo y `APPLICATION_MODE` variable de entorno porque estaban causando un comportamiento inesperado.<!-- MAGECLOUD-1615 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que ocasionaba errores de implementación de contenido estático relacionados con Redis. Ahora, la implementación de contenido estático de subprocesos múltiples se ejecuta según lo diseñado.<!-- MAGECLOUD-1630 -->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que impedía a los usuarios guardar las modificaciones en los campos de configuración del administrador, que se marcan como confidenciales después de ejecutar el `app:config:dump` comando.<!-- MAGECLOUD-1175 -->

- ![icono de corrección](../../assets/fix.svg) Se ha agregado compatibilidad con una versión anterior de `symfony/yaml` para solucionar conflictos con algunos paquetes, que aún no son compatibles con la última versión.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>Nos fusionamos `vendor/magento/ece-patches` con `vendor/magento/ece-tools` en esta versión. Ya no es necesario actualizar el `vendor/magento/ece-patches` empaquetar por separado.

**Nuevas funciones:**

- **Registro mejorado**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Hemos mejorado la mensajería de registro para proporcionar mejores explicaciones cuando el proceso de generación o implementación anula una variable de entorno.
   - Ahora puede ver el progreso de la instalación y actualización en tiempo real. Siga el `install_update.log` para ver el progreso. Por ejemplo,

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Nuevo comando cron**—Ahora puede desbloquear trabajos cron atascados específicos en lugar de detener y volver a iniciar todos ellos con el [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) comando. No disponible en 2.1.<!-- MAGECLOUD-1367 -->

- **Archivo de configuración unificado**: ahora puede configurar las fases de compilación e implementación mediante una [`.magento.env.yaml`](https://devdocs.magento.com/cloud/project/magento-env-yaml.html) archivo.<!-- MAGECLOUD-1369 -->

- **Archivos de configuración de copia**: el proceso de despliegue ahora crea automáticamente una copia de seguridad del `app/etc/env.php` y `app/etc/config.php` archivos de configuración después de la implementación. También hemos añadido una [nuevo comando CLI](https://support.magento.com/hc/en-us/articles/360033182871) para restaurar estos archivos de configuración desde una copia de seguridad.<!-- MAGECLOUD-1372 -->

- **Solución de problemas de validación**: se ha cambiado el comando que debe utilizarse para resolver los errores de validación cuando `config.php` no contiene datos suficientes para la implementación de contenido estático. Anteriormente, el mensaje de error le indicaba que ejecutara `bin/magento app:config:dump`. Ahora, debes correr `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Nuevas variables de entorno**: ahora puede utilizar variables de entorno para conectar elementos personalizados [búsqueda](../environment/variables-deploy.md#search_configuration) y [Basado en AMQP](../environment/variables-deploy.md#queue_configuration) servicios a su sitio.<!-- MAGECLOUD-1410 -->

- Hemos implementado parches inteligentes. Ahora el paquete aplica parches basados no en Adobe Commerce en la versión de infraestructura en la nube, sino en la versión de paquete parcheada.<!--MAGECLOUD-1090-->

**Problemas resueltos:**

- Se ha corregido un problema de registro que causaba errores de compilación.<!-- MAGECLOUD-1162 -->

- Se ha corregido un problema que ocasionaba excepciones de tiempo de espera al ejecutar implementaciones en modo interactivo.<!-- MAGECLOUD-1389 -->

- Se ha corregido un problema que ocasionaba errores al utilizar la estrategia compacta para la generación de contenido estático. No disponible en 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- Se ha corregido un problema que impedía que el script de implementación identificara correctamente los entornos de ensayo y producción.<!-- MAGECLOUD-1493 -->

- Se ha corregido un problema que provocaba que los problemas de red interrumpieran las conexiones de base de datos y causaran errores durante el proceso de instalación y actualización.<!-- MAGECLOUD-1520 -->

- Se ha corregido un problema que impedía exportar los archivos de configuración mediante `app:config:dump` más de una vez. No disponible en 2.1.<!--  MAGECLOUD-1567  -->

- Hemos arreglado una sesión de Redis _bloqueo_ problema que causó un error _Administrador_ retraso de inicio de sesión. No disponible en 2.1.<!--  MAGECLOUD-1582  -->

- Se ha corregido un problema de implementación relacionado con el control de versiones que provocaba un conflicto con otros módulos de aplicación de parches basados en el Compositor.<!-- MAGECLOUD-1450 -->

- Se ha corregido un problema que causaba problemas de memoria PHP durante la importación.<!-- MAGECLOUD-1310 -->

- Se ha eliminado el parche; corrigiendo error en `colinmollenhour/credis` v1.6 para habilitar la compatibilidad con Adobe Commerce en la infraestructura en la nube 2.2.1. No disponible en 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Problemas resueltos:**

- Hemos eliminado `var/view_preprocessed` enlaces simbólicos para solucionar un problema que causaba conflictos de minificación de JavaScript.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Problemas resueltos:**

- Se ha corregido un problema que causaba `gzip` errores cuando un nombre de archivo o directorio contiene espacios.<!-- MAGECLOUD-1413 -->

- Se ha corregido un problema que impedía que los scripts de implementación reconocieran y habilitaran correctamente las dependencias de módulo.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Nuevas funciones:**

- **Configuración de un consumidor de cron con una variable de entorno**: ahora puede configurar consumidores de cron con el nuevo `CRON_CONSUMERS_RUNNER` variable de entorno.

- **Análisis de configuración**: ahora analizamos componentes críticos durante el proceso de creación/implementación y detenemos el proceso si falla el análisis, lo que evita un tiempo de inactividad innecesario debido a que el sitio está en modo de mantenimiento.

- **Generar/implementar notificaciones**: se ha añadido un fichero de configuración que se puede utilizar para [configurar notificaciones de Slack o por correo electrónico](../environment/set-up-notifications.md) para generar e implementar acciones en todos sus entornos.

- **Compresión de contenido estático**: ahora comprimimos contenido estático mediante [gzip](https://www.gnu.org/software/gzip/) durante las fases de compilación e implementación. Esta compresión, junto con la compresión de Fastly, ayuda a reducir el tamaño de su tienda y aumentar la velocidad de implementación. Si es necesario, puede deshabilitar la compresión mediante un [opción de compilación](../environment/variables-build.md) o [implementar variable](../environment/variables-deploy.md). Consulte los siguientes temas para obtener más información:

   - [Variables de entorno de aplicación](../application/variables-property.md)

   - [Rendimiento de implementación de contenido estático](../deploy/static-content.md)

   - [Proceso de implementación](https://devdocs.magento.com/cloud/reference/discover-deploy.html)

- **Administración de configuración**: ahora generamos automáticamente un `app/etc/config.php` en el repositorio de Git durante la fase de compilación si aún no existe. El archivo generado automáticamente solo incluye una lista de módulos y extensiones. Si el archivo ya existe, la fase de compilación continúa con normalidad. Si sigue [Administración de configuración](../store/store-settings.md) posteriormente, los comandos actualizan el archivo sin necesidad de realizar pasos adicionales. Consulte [Proceso de implementación](https://devdocs.magento.com/cloud/reference/discover-deploy.html) para obtener más información.

- **Volcados de base de datos**—Hemos añadido un `magento/ece-tools` Comando CLI para crear volcados de base de datos en todos los entornos. Para entornos de producción de planificación profesional, este comando solo volca desde uno de los tres nodos de alta disponibilidad, por lo que es posible que no se copien los datos de producción escritos en un nodo diferente durante el volcado. Se recomienda poner la aplicación en modo de mantenimiento antes de hacer un volcado de la base de datos en entornos de producción. Consulte [Administración de backup](https://devdocs.magento.com/cloud/project/project-webint-snap.html#db-dump) para obtener más información.

- **Limitaciones del intervalo Cron eliminadas**: el intervalo cron predeterminado para todos los entornos aprovisionados en las regiones us-3, eu-3 y ap-3 es de 1 minuto. El intervalo cron predeterminado en todas las demás regiones es de 5 minutos para entornos de integración profesional y 1 minuto para entornos de ensayo y producción profesionales. Para modificar los trabajos de cron existentes, edite la configuración en `.magento.app.yaml` o cree un vale de soporte para los entornos de Producción/Ensayo. Consulte [Configuración de trabajos cron](../application/crons-property.md#set-up-cron-jobs) para obtener más información.

**Problemas resueltos:**

- Se ha corregido un problema que causaba tiempos de implementación largos debido a que el proceso de implementación invocaba el `cache-clean` antes de la implementación de contenido estático.<!-- MAGECLOUD-1327 -->

- Se ha corregido un problema que provocaba errores durante el paso de implementación de generación de contenido estático en entornos de producción.<!-- MAGECLOUD-1322 -->

- Hemos corregido un problema que impedía que algunos `magento/ece-tools` comandos desde el registro de salida a `stderr`.<!-- MAGECLOUD-1264 -->

- Se ha corregido un problema que impedía los valores de URL base en `env.php` de actualizarse en ramas ramificadas.<!-- MAGECLOUD-1242 -->

- Se ha corregido un problema que provocaba lo siguiente `magento setup:install` para agregar un prefijo no seguro (`http://`) para proteger las URL de base.<!-- MAGECLOUD-1171 -->

- Se ha corregido un problema que impedía que los errores de parche causaran errores de implementación.<!-- MAGECLOUD-1170 -->

- Se ha corregido un problema que impedía `ece-tools` de detener la ejecución y lanzar una excepción si no se pueden aplicar parches.<!-- MAGECLOUD-1152 -->

- Se ha corregido un problema que provocaba errores al cargar la tienda después de habilitar la minificación del HTML en el administrador.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Problemas resueltos:**

- Ahora puede [restablecer manualmente los trabajos cron atascados](https://support.magento.com/hc/en-us/articles/360033099451) uso de un comando CLI en todos los entornos a través del acceso SSH. El proceso de implementación restablece automáticamente los trabajos cron.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Problemas resueltos:**

- Se ha corregido un problema que provocaba que las páginas agotaran el tiempo de espera porque Redis tardaba demasiado en leer y escribir. Ahora puede utilizar la variable `disable_locking` en las configuraciones de Redis para evitar este problema.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Problemas resueltos:**

- El [!DNL RabbitMQ] El proceso de configuración ahora obtiene todos los parámetros necesarios automáticamente.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Nuevas funciones:**

- Adobe Commerce en la infraestructura en la nube ahora admite ámbitos y [estrategias de implementación de contenido estático](https://devdocs.magento.com/guides/v2.3/config-guide/cli/config-cli-subcommands-static-deploy-strategies.html). Hemos añadido la `–s` con una configuración predeterminada de `quick` para la estrategia de implementación de contenido estático. Puede utilizar la variable de entorno [SCD_STRATEGY](../environment/variables-deploy.md) para personalizar y utilizar estas estrategias con las acciones de compilación e implementación. Esta variable admite las opciones `standard`, `quick`, o `compact`. Si selecciona `compact`, anulamos el `STATIC_CONTENT_THREADS` valor con `1`, que puede ralentizar la implementación, especialmente en entornos de producción. No disponible en 2.1.<!--- MAGECLOUD-1057 -->

- Hemos creado un archivo de registro en entornos para capturar y compilar acciones de compilación e implementación. El `var/log/cloud.log` está en el directorio raíz de la aplicación.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Problemas resueltos:**

- Se refactorizó el `ece-tools` para que sea compatible con Adobe Commerce en la infraestructura en la nube 2.2.0 y superior.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- Se ha corregido un problema que impedía `ece-tools` de detener la ejecución y lanzar una excepción si no se pueden aplicar parches.<!-- MAGECLOUD-1186 -->

- Se ha corregido un problema que provocaba que se generaran excepciones cuando la compilación de inyección de dependencia (id) se omitía durante las compilaciones.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- Se ha corregido un problema que hacía que el proceso de implementación sobrescribiera las configuraciones de Redis personalizadas en `env.php` archivo.<!-- MAGECLOUD-1019 -->

- Se ha corregido un problema que provocaba bucles de redireccionamiento debido a deshabilitado por defecto por el administrador seguro.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Este paquete ya no es compatible con otras versiones de Adobe Commerce en infraestructuras en la nube y **no debe** se utilizará.

### Versión inicial

Versión inicial de `ece-tools` para Adobe Commerce en cloud Infrastructure 2.2.0.
