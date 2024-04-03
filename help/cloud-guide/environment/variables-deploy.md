---
title: Implementación de variables
description: Consulte la lista de variables de entorno que controlan las acciones en la fase de implementación de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 673880b5-830b-4837-ac0c-5fa5643ae34c
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '2185'
ht-degree: 0%

---

# Implementación de variables

Lo siguiente _implementar_ las variables controlan las acciones en la fase de implementación y pueden heredar y anular los valores de [Variables globales](variables-global.md). Inserte estas variables en la variable `deploy` fase de la `.magento.env.yaml` archivo:

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure la página Redis y el almacenamiento en caché predeterminado. Al configurar el `cm_cache_backend_redis` , debe especificar el parámetro `server`, `port`, y `database` opciones.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

El ejemplo siguiente utiliza el [Función de precarga Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature) tal como se define en la _Guía de configuración_:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Para usar un personalizado [REDIS_BACKEND](#redis_backend) (no solo desde la lista de permitidos), configure el `_custom_redis_backend` opción para `true` para habilitar la validación correcta, como en el siguiente ejemplo:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Activa o desactiva la limpieza [archivos de contenido estático](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) generadas durante la fase de compilación o implementación. Utilizar el valor predeterminado _true_ en el desarrollo como práctica recomendada.

- **`true`**: permite eliminar todo el contenido estático existente antes de implementar el contenido estático actualizado.
- **`false`**: la implementación solo sobrescribe los archivos de contenido estático existentes si el contenido generado contiene una versión más reciente.

Si modifica el contenido estático mediante un proceso independiente, establezca el valor en _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Si no se limpian los archivos de vista estática antes de la implementación, pueden producirse problemas si se implementan actualizaciones en los archivos existentes sin quitar las versiones anteriores. Debido a [reserva de archivo estático](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache) , las operaciones de reserva pueden mostrar el archivo incorrecto si el directorio contiene varias versiones del mismo archivo.

## `CRON_CONSUMERS_RUNNER`

- **Predeterminado**—`cron_run = false`, `max_messages = 1000`
- **Versión**: Adobe Commerce 2.2.0 y posterior

Utilice esta variable de entorno para confirmar que las colas de mensajes se están ejecutando después de una implementación.

- `cron_run`: un valor booleano que habilita o deshabilita la variable `consumers_runner` trabajo cron (predeterminado = `false`).
- `max_messages`: un número que especifica el número máximo de mensajes que cada consumidor debe procesar antes de finalizar (valor por defecto = `1000`). Puede establecer el valor en `0` para evitar que el consumidor finalice.
- `consumers`: una matriz de cadenas que especifica qué consumidores se van a ejecutar. Se ejecuta una matriz vacía _todo_ consumidores.

- `multiple_processes`-Un número que especifica el número de procesos que se van a generar para cada consumidor. Admitido en Commerce **2.4.4** o superior.

>[!NOTE]
>
>Para devolver una lista de colas de mensajes `consumers`, ejecute el `./bin/magento queue:consumers:list` en el entorno remoto.

Matriz de ejemplo que se ejecuta de forma específica `consumers` y el `multiple_processes` para generar para cada consumidor:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Ejemplo de una matriz vacía que ejecuta todo `consumers`:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

De forma predeterminada, el proceso de implementación sobrescribe todos los ajustes de la `env.php` archivo. Consulte [Administrar colas de mensajes](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html) en el _Guía de configuración de Commerce_ para Adobe Commerce local.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.2.0 y posterior

Configurar cómo `consumers` procesar mensajes de la cola de mensajes seleccionando una de las siguientes opciones:

- `false`—`Consumers` procesar los mensajes disponibles en la cola, cerrar la conexión TCP y finalizar. `Consumers` no espere a que otros mensajes entren en la cola, aunque el número de mensajes procesados sea menor que el `max_messages` valor especificado en `CRON_CONSUMERS_RUNNER` implementar variable.

- `true`—`Consumers` continúe procesando mensajes de la cola de mensajes hasta alcanzar el número máximo de mensajes (`max_messages`) especificado en el `CRON_CONSUMERS_RUNNER` implemente la variable antes de cerrar la conexión TCP y finalizar el proceso de consumidor. Si la cola se vacía antes de alcanzar `max_messages`, el consumidor espera a que lleguen más mensajes.

>[!WARNING]
>
>Si utiliza trabajadores para ejecutar `consumers` en lugar de usar un trabajo cron, establezca esta variable en true.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

>[!WARNING]
>
>Configure las variables `CRYPT_KEY` mediante el [!DNL Cloud Console] en lugar del `.magento.env.yaml` para evitar exponer la clave en el repositorio de código fuente de su entorno. Consulte [Establecer variables de entorno y proyecto](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment).

Cuando mueve la base de datos de un entorno a otro sin un proceso de instalación, necesita la información criptográfica correspondiente. Adobe Commerce utiliza el valor de clave de cifrado establecido en [!DNL Cloud Console] como el `crypt/key` valor en `env.php` archivo.

## `DATABASE_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Si ha definido una base de datos en la variable [propiedad de relaciones](../application/properties.md#relationships) de la `.magento.app.yaml` , puede personalizar las conexiones de base de datos para la implementación.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

Además, puede configurar un prefijo de tabla.

>[!WARNING]
>
>Si no utiliza la opción de combinación con el prefijo de tabla, debe proporcionar la configuración de conexión predeterminada o la implementación no superará la validación.

El ejemplo siguiente utiliza el `ece_` prefijo de tabla con la configuración de conexión predeterminada en lugar de utilizar la variable `_merge` opción:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Salida de ejemplo:

```terminal
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.2.0 y posterior

Conserva personalizado [!DNL Elastic Suite] la configuración del servicio entre implementaciones y lo utiliza en la sección &quot;system/default/mile_elasticsuite_core_base_settings&quot; del [!DNL Elastic Suite] configuración. Si la variable [!DNL Elastic Suite] El paquete Composer está instalado y se configura automáticamente.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**Limitaciones conocidas**:

- Cambiar el motor de búsqueda a cualquier tipo distinto de `elasticsuite` causa un error de implementación acompañado de un error de validación adecuado
- La eliminación del servicio de Elasticsearch provoca un error de implementación acompañado de un error de validación adecuado

>[!NOTE]
>
>Para obtener más información sobre el uso o la resolución de problemas de [!DNL Elastic Suite] con Adobe Commerce, consulte la [[!DNL Elastic Suite] documentación](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilita y deshabilita los Google Analytics al implementarlos en entornos de ensayo e integración. De forma predeterminada, Google Analytics solo es true para el entorno Producción. Establezca este valor en `true` para habilitar Google Analytics en los entornos de ensayo e integración.

- **`true`**: permite activar Google Analytics en entornos de ensayo e integración.
- **`false`**: desactiva los Google Analytics en los entornos de ensayo e integración.

Añada el `ENABLE_GOOGLE_ANALYTICS` variable de entorno a `deploy` fase en la `.magento.env.yaml` archivo:

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>El proceso de implementación siempre permite a los Google Analytics en entornos de producción.

## `FORCE_UPDATE_URLS`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Al implementarse en entornos de ensayo y producción Pro o Starter, esta variable reemplaza las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por el [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) variable. Utilice esta configuración para anular el comportamiento predeterminado del [UPDATE_URLS](#update_urls) implementar variable, que se ignora al implementar en entornos de Ensayo o Producción.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Predeterminado**—`file`
- **Versión**: Adobe Commerce 2.2.5 y posterior

El proveedor de bloqueo evita el inicio de trabajos cron y grupos cron duplicados. Utilice el `file` Bloquear proveedor en el entorno de producción. Los entornos iniciales y el entorno de integración de Pro no utilizan el [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md) , por lo que `ece-tools` aplica el `db` bloquear proveedor automáticamente.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Consulte [Configuración del bloqueo](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html) en el _Guía de instalación_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.1.4 y posterior

>[!TIP]
>
>El `MYSQL_USE_SLAVE_CONNECTION` La variable solo es compatible con Adobe Commerce en entornos de clúster de Cloud Infrastructure, Staging y Production Pro, y no con proyectos iniciales.

Adobe Commerce puede leer varias bases de datos de forma asincrónica. Configure como. `true` para utilizar automáticamente un _de solo lectura_ conexión a la base de datos para recibir tráfico de solo lectura en un nodo no maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Configure como. `false` para quitar cualquier matriz de conexión de solo lectura existente de `env.php` archivo.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Si la variable `MYSQL_USE_SLAVE_CONNECTION` se establece en `true`, el `synchronous_replication` El parámetro se ha establecido en `true` de forma predeterminada, en la variable `env.php` en entornos de ensayo y producción de Pro. Si la variable `MYSQL_USE_SLAVE_CONNECTION` se establece en `false`, el `synchronous_replication` el parámetro no está configurado.

## `QUEUE_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Utilice esta variable de entorno para conservar la configuración personalizada del servicio AMQP entre implementaciones. Por ejemplo, si prefiere utilizar un servicio de cola de mensajes existente en lugar de depender de la infraestructura en la nube para crearlo, utilice el `QUEUE_CONFIGURATION` para conectarla al sitio:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Predeterminado**—`Cm_Cache_Backend_Redis`
- **Versión**: Adobe Commerce 2.3.0 y posterior

Especifica la configuración del modelo backend para la caché de Redis.

La versión 2.3.0 y posteriores de Adobe Commerce incluyen los siguientes modelos backend:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

El ejemplo de cómo se establece `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Si especifica `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` como el modelo backend de Redis para habilitar [Caché L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html), `ece-tools` genera la configuración de caché automáticamente. Ver un ejemplo [archivo de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) en el _Guía de configuración de Adobe Commerce_. Para anular la configuración de caché generada, utilice el [CACHE_CONFIGURATION](#cache_configuration) implementar variable.

## `REDIS_USE_SLAVE_CONNECTION`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.1.16 y posterior

>[!WARNING]
>
>Hacer _no_ habilitar esta variable en un [arquitectura a escala](../architecture/scaled-architecture.md) proyecto. Provoca errores de conexión de Redis. Los esclavos de Redis siguen activos, pero no se utilizan para las lecturas de Redis. Como alternativa, Adobe recomienda utilizar Adobe Commerce 2.3.5 o posterior, implementar una nueva configuración de back-end de Redis e implementar el almacenamiento en caché L2 para Redis.

>[!TIP]
>
>El `REDIS_USE_SLAVE_CONNECTION` La variable solo es compatible con Adobe Commerce en entornos de clúster de Cloud Infrastructure, Staging y Production Pro, y no con proyectos iniciales.

Adobe Commerce puede leer varias instancias de Redis de forma asincrónica. Configure como. `true` para utilizar automáticamente un _de solo lectura_ conexión a una instancia de Redis para recibir tráfico de solo lectura en un nodo no maestro. Esta conexión mejora el rendimiento mediante el equilibrio de carga, ya que solo un nodo administra el tráfico de lectura-escritura. Configure como. `false` para quitar cualquier matriz de conexión de solo lectura existente de `env.php` archivo.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Debe tener un servicio de Redis configurado en `.magento.app.yaml` y en el `services.yaml` archivo.

[ECE-Tools versión 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) y posterior utiliza más configuraciones de tolerancia a errores. Si Adobe Commerce no puede leer los datos de Redis _esclavo_ y, a continuación, lee los datos de la interfaz de usuario de _principal_ ejemplo.

La conexión de solo lectura no está disponible para su uso en el entorno de integración o si utiliza el [`CACHE_CONFIGURATION` variable](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Predeterminado**—No se ha definido
- **Versión**: Adobe Commerce 2.1.4 y posterior

Asigna un nombre de recurso a una conexión de base de datos. Esta configuración corresponde a la variable `resource` de la sección `env.php` archivo.

{{merge-options}}

El siguiente ejemplo combina nuevos valores en una configuración existente:

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Predeterminado**—`4`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifica qué [gzip](https://www.gnu.org/software/gzip) nivel de compresión (`0` hasta `9`) para usar al comprimir contenido estático; `0` deshabilita la compresión.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predeterminado**—`600`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Cuando el tiempo necesario para comprimir los recursos estáticos supera el límite de tiempo de espera de compresión, interrumpe el proceso de implementación. Establezca el tiempo máximo de ejecución, en segundos, para el comando de compresión de contenido estático.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Puede configurar varias configuraciones regionales por tema. Esta personalización acelera el proceso de implementación al reducir el número de archivos de temas innecesarios. Por ejemplo, puede implementar el _magento/backend_ temática en inglés y temática personalizada en otros idiomas.

El siguiente ejemplo implementa la variable `Magento/backend` tema con tres configuraciones regionales:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Además, puede elegir lo siguiente _no_ implementar un tema:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.2.0 y posterior

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce establece el tiempo de ejecución máximo esperado en 900 segundos, pero en algunos casos puede necesitar más tiempo para completar la implementación de contenido estático para un proyecto de Cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.2 y posterior

En la fase de implementación, establezca `SCD_NO_PARENT: true` para que la generación de contenido estático para las temáticas principales no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Predeterminado**—`quick`
- **Versión**: Adobe Commerce 2.2.0 y posterior

Permite personalizar el [estrategia de implementación](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) para contenido estático. Consulte [Implementación de archivos de vista estática](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilice estas opciones _solamente_ si tiene más de una configuración regional:

- `standard`: permite desplegar todos los ficheros de vista estática para todos los paquetes.
- `quick`—(_predeterminado_) minimiza el tiempo de implementación.
- `compact`: permite ahorrar espacio en disco en el servidor. En la versión 2.2.4 y anteriores de Adobe Commerce, esta configuración anula el valor de `scd_threads` con un valor de `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predeterminado**—Automático
- **Versión**: Adobe Commerce 2.1.4 y posterior

Establece el número de subprocesos para la implementación de contenido estático. El valor predeterminado se establece en función del recuento de subprocesos de CPU detectado y no supera un valor de 4. El aumento del número de subprocesos acelera la implementación de contenido estático; reducir el número de subprocesos hace que se ralentice. Puede establecer el valor del subproceso, por ejemplo:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Para reducir aún más el tiempo de implementación, utilice [Administración de configuración](../store/store-settings.md) con el `scd-dump` para mover la implementación estática a la fase de compilación.

## `SEARCH_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Utilice esta variable de entorno para conservar la configuración personalizada del servicio de búsqueda entre implementaciones. Por ejemplo:

Configuración del Elasticsearch:

```yaml
stage:
  deploy:
   SEARCH_CONFIGURATION:
     engine: elasticsearch
     elasticsearch_server_hostname: http://elasticsearch.internal
     elasticsearch_server_port: '9200'
     elasticsearch_index_prefix: magento2
     elasticsearch_server_timeout: '15'
```

Configuración de OpenSearch (para Commerce 2.4.6 y posterior):

```yaml
stage:
  deploy:
   SEARCH_CONFIGURATION:
     engine: opensearch
     opensearch_server_hostname: 'http://opensearch.internal'
     opensearch_server_port: '9200'
     opensearch_index_prefix: 'magento2'
     opensearch_server_timeout: '15'
```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure el almacenamiento de sesión de Redis. Requiere el `save`, `redis`, `host`, `port`, y `database` opciones para la variable de almacenamiento de sesión. Por ejemplo:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

El siguiente ejemplo combina un nuevo valor con la configuración existente:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Predeterminado**— _Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure como. `true` para omitir la implementación de contenido estático durante la fase de implementación.

En la fase de implementación, establezca `SKIP_SCD: true` para que la generación de contenido estático no se produzca durante la fase de implementación. Esta configuración minimiza el tiempo de implementación y evita el tiempo de inactividad del sitio que puede producirse si la compilación de contenido estático falla durante la implementación. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Predeterminado**—`true`
- **Versión**: Adobe Commerce 2.1.4 y posterior

En la implementación, reemplace las direcciones URL base de Adobe Commerce en la base de datos por las direcciones URL del proyecto especificadas por el [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) variable. Esta configuración es útil para el desarrollo local, donde las direcciones URL base están configuradas para el entorno local. Al implementar en un entorno de Cloud, las direcciones URL se actualizan para que pueda acceder a su tienda y al administrador mediante las direcciones URL del proyecto.

Si debe actualizar las direcciones URL al implementar en entornos de ensayo y producción Pro o Starter, utilice el [`FORCE_UPDATE_URLS`](#force_update_urls) variable.

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilitar o deshabilitar el [Symfony](https://symfony.com/doc/current/console/verbosity.html) depurar nivel de detalle para `bin/magento` Comandos CLI ejecutados durante la fase de implementación.

>[!NOTE]
>
>Para utilizar la configuración VERBOSE_COMMANDS para controlar los detalles de la salida del comando tanto para los resultados correctos como para los erróneos `bin/magento` Comandos CLI, debe establecer [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Elija el nivel de detalle proporcionado en los registros:

- `-v`= salida normal
- `-vv`= salida más detallada
- `-vvv` = salida detallada ideal para depurar

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
