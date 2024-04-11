---
title: Mensajes de error para el paquete ECE-Tools
description: Consulte la lista de códigos de error y mensajes que pueden producirse durante los procesos de creación, implementación y posterior implementación de la infraestructura en la nube de Adobe Commerce.
recommendations: noDisplay
role: Developer
exl-id: d8cc8d49-32da-43cf-a105-aa56b5334000
source-git-commit: 9dda6fe7f6a9d6064436820a3c8426ec982b5230
workflow-type: tm+mt
source-wordcount: '2763'
ht-degree: 4%

---

# Mensajes de error para ECE-Tools

Esta referencia de mensaje de error proporciona información para solucionar errores que pueden producirse durante los procesos de creación, implementación y posterior a la implementación de Adobe Commerce en la infraestructura en la nube.

Todos los mensajes de error críticos y de advertencia que se producen durante la implementación se escriben tanto en el `var/log/cloud.log` y `/var/log/cloud.error.log` archivos. El archivo de registro de errores de la nube solo contiene errores de la implementación más reciente. Un archivo vacío indica una implementación correcta sin errores.

En el `cloud.error.log` , cada entrada tiene el formato de una cadena JSON para facilitar el análisis:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

Los mensajes de error se clasifican por una de las etapas de implementación: generación, implementación y posterior a la implementación. Cada sección proporciona una lista de errores asociados con la siguiente información para cada error:

- **Código de error**: Identificador asignado por Adobe Commerce al mensaje de error
- **Fase**: indica si el error se produjo durante la fase de compilación, implementación o posterior a la implementación
- **Etapa**: indica el paso en el escenario de implementación que puede devolver el error. Si la variable _Etapa_ está en blanco, se trata de un error general que se puede devolver mediante varios pasos o durante las operaciones de preprocesamiento. Consulte [Implementación basada en escenarios](../deploy/scenario-based.md) para obtener más información sobre los pasos de compilación, implementación y posterior a la implementación.
- **Sugerencia**: Proporciona instrucciones para solucionar y resolver el error
- **Título (descripción del error)**: Una descripción que resume la causa del error
- **Tipo**: indica si el error es un error crítico o una advertencia

<!-- Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository. -->

## Errores críticos

Los errores críticos indican un problema con la configuración del proyecto de Commerce en la infraestructura en la nube que provoca un error de implementación, por ejemplo, una configuración incorrecta, no admitida o faltante para la configuración requerida. Antes de poder implementar, debe actualizar la configuración para resolver estos errores.

### Fase de compilación

| Código de error | Paso Generar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 2 |  | No se puede escribir en `./app/etc/env.php` archivo | El script de implementación no puede realizar los cambios necesarios en `/app/etc/env.php` archivo. Compruebe los permisos del sistema de archivos. |
| 3 |  | La configuración no está definida en `schema.yaml` archivo | La configuración no está definida en `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre de la variable de configuración sea correcto y esté definido. |
| 4 |  | Error al analizar el `.magento.env.yaml` archivo | El `./.magento.env.yaml` formato de archivo no válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 5 |  | No se puede leer el `.magento.env.yaml` archivo | No se puede leer el `./.magento.env.yaml` archivo. Compruebe los permisos del archivo. |
| 6 |  | No se puede leer el `.schema.yaml` archivo | No se puede leer el `./vendor/magento/ece-tools/config/magento.env.yaml` archivo. Compruebe los permisos de archivos y vuelva a implementar (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | No se puede escribir en `./app/etc/config.php` archivo | El script de implementación no puede realizar los cambios necesarios en `/app/etc/config.php` archivo. Compruebe los permisos del sistema de archivos. |
| 8 | validate-config | No se puede leer el `composer.json` archivo | No se puede leer el `./composer.json` archivo. Compruebe los permisos del archivo. |
| 9 | validate-config | El `composer.json` falta la sección de carga automática necesaria del archivo | Requerido `autoload` falta en la sección `composer.json` archivo. Comparar la sección de carga automática con la `composer.json` en la plantilla de Cloud y agregue la configuración que falta. |
| 10 | validate-config | El `.magento.env.yaml` el archivo contiene una opción que no está declarada en el esquema o una opción configurada con un valor o fase no válidos | El `./.magento.env.yaml` el archivo contiene una configuración no válida. Consulte el registro de errores para obtener información detallada. |
| 11 | refresh-modules | Error del comando: `/bin/magento module:enable --all` | Intente ejecutar `composer update` localmente. A continuación, confirme y publique el `composer.lock` archivo. Compruebe también la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 12 | apply-patch | Error al aplicar el parche |  |
| 13 | set-report-dir-nesting-level | No se puede escribir en el archivo `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Error al copiar los archivos de datos de ejemplo |  |
| 15 | compile-id | Error del comando: `/bin/magento setup:di:compile` | Compruebe la `cloud.log` para obtener más información. Añadir `VERBOSE_COMMANDS: '-vvv'` en `.magento.env.yaml` para obtener resultados de comandos más detallados. |
| 16 | dump-autoload | Error del comando: `composer dump-autoload` | El `composer dump-autoload` error del comando. Compruebe la `cloud.log` para obtener más información. |
| 17 | empacador de carreras | El comando que se va a ejecutar `Baler` error al empaquetar para Javascript | Compruebe la `SCD_USE_BALER` variable de entorno para comprobar que el módulo Baler está configurado y habilitado para el agrupamiento JS. Si no necesita el módulo Baler, establezca `SCD_USE_BALER: false`. |
| 18 | compress-static-content | No se ha encontrado la utilidad requerida (timeout, bash) |  |
| 19 | deploy-static-content | Comando `/bin/magento setup:static-content:deploy` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 20 | compress-static-content | Error de compresión de contenido estático | Compruebe la `cloud.log` para obtener más información. |
| 21 | backup-data: static-content | Error al copiar contenido estático en `init` directorio | Compruebe la `cloud.log` para obtener más información. |
| 22 | backup-data: writable-dirs | No se pudieron copiar algunos directorios editables en el `init` directorio | No se pudieron copiar los directorios grabables en el `./init` carpeta. Compruebe los permisos del sistema de archivos. |
| 23 |  | No se puede crear un objeto de registro |  |
| 24 | backup-data: static-content | Error al limpiar el `./init/pub/static/` directorio | Error al limpiar `./init/pub/static` carpeta. Compruebe los permisos del sistema de archivos. |
| 25 |  | No se encuentra el paquete Composer | Si ha instalado la versión de la aplicación de Adobe Commerce directamente desde el repositorio de GitHub, compruebe que la variable `DEPLOYED_MAGENTO_VERSION_FROM_GIT` variable de entorno está configurada. |
| 26 | validate-config | Elimine la configuración del módulo Braintree del Magento que ya no es compatible con Adobe Commerce y Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye en Magento 2.4.0 y versiones posteriores. Elimine la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la sección de variables de `.magento.app.yaml` archivo. Para la compatibilidad con pagos de Braintree, utilice una extensión oficial del Commerce Marketplace en su lugar. |

### Implementar fase

| Código de error | Paso Implementar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 101 | implementación previa: caché | Configuración de caché incorrecta (falta puerto o host) | Faltan parámetros obligatorios en la configuración de caché `server` o `port`. Compruebe la `cloud.log` para obtener más información. |
| 102 |  | No se puede escribir en `./app/etc/env.php` archivo | El script de implementación no puede realizar los cambios necesarios en `/app/etc/env.php` archivo. Compruebe los permisos del sistema de archivos. |
| 103 |  | La configuración no está definida en `schema.yaml` archivo | La configuración no está definida en `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 104 |  | Error al analizar el `.magento.env.yaml` archivo | La configuración no está definida en `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 105 |  | No se puede leer el `.magento.env.yaml` archivo | No se puede leer el `./.magento.env.yaml` archivo. Compruebe los permisos del archivo. |
| 106 |  | No se puede leer el `.schema.yaml` archivo |  |
| 107 | implementación previa: clean-redis-cache | Error al limpiar la caché de Redis | Error al limpiar la caché de Redis. Compruebe que la configuración de la caché de Redis sea correcta y que el servicio Redis esté disponible. Consulte [Configurar el servicio Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html). |
| 108 | implementación previa: establecer modo de producción | Comando `/bin/magento maintenance:enable` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 109 | validate-config | Configuración de base de datos incorrecta | Compruebe que la variable `DATABASE_CONFIGURATION` La variable de entorno está configurada correctamente. |
| 110 | validate-config | Configuración de sesión incorrecta | Compruebe que la variable `SESSION_CONFIGURATION` La variable de entorno está configurada correctamente. La configuración debe contener al menos el `save` parámetro. |
| 111 | validate-config | Configuración de búsqueda incorrecta | Compruebe que la variable `SEARCH_CONFIGURATION` La variable de entorno está configurada correctamente. La configuración debe contener al menos el `engine` parámetro. |
| 112 | validate-config | Configuración de recurso incorrecta | Compruebe que la variable `RESOURCE_CONFIGURATION` La variable de entorno está configurada correctamente. La configuración debe contener al menos `connection` parámetro. |
| 113 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero el servicio de Elasticsearch no está disponible | Compruebe que la variable `SEARCH_CONFIGURATION` La variable de entorno está configurada correctamente y compruebe que el servicio Elasticsearch está disponible. |
| 114 | validate-config:elasticsuite-integration | ElasticSuite está instalado, pero se utiliza otro motor de búsqueda | ElasticSuite está instalado, pero hay otro motor de búsqueda configurado. Actualice el `SEARCH_CONFIGURATION` variable de entorno para habilitar Elasticsearch y comprobar la configuración del servicio de Elasticsearch en `services.yaml` archivo. |
| 115 |  | Error de ejecución de consulta de base de datos |  |
| 116 | install-update: setup | Comando `/bin/magento setup:install` error | Compruebe la `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 117 | install-update: config-import | Comando `app:config:import` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 118 |  | No se ha encontrado la utilidad requerida (timeout, bash) |  |
| 119 | install-update: deploy-static-content | Comando `/bin/magento setup:static-content:deploy` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 120 | compress-static-content | Error de compresión de contenido estático | Compruebe la `cloud.log` para obtener más información. |
| 121 | deploy-static-content:generate | No se puede actualizar la versión implementada | No se puede actualizar el `./pub/static/deployed_version.txt` archivo. Compruebe los permisos del sistema de archivos. |
| 122 | clean-static-content | Error al limpiar los archivos de contenido estático |  |
| 123 | install-update: split-db | Comando `/bin/magento setup:db-schema:split` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 124 | clean-view-preprocessed | Error al limpiar el `var/view_preprocessed` carpeta | No se puede limpiar el `./var/view_preprocessed` carpeta. Compruebe los permisos del sistema de archivos. |
| 125 | install-update: reset-password | Error al actualizar el `/var/credentials_email.txt` archivo | Error al actualizar el `/var/credentials_email.txt` archivo. Compruebe los permisos del sistema de archivos. |
| 126 | install-update: update | Comando `/bin/magento setup:upgrade` error | Compruebe la `cloud.log` y `install_upgrade.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 127 | clean-cache | Comando `/bin/magento cache:flush` error | Compruebe la `cloud.log` para obtener más información. Para obtener resultados de comandos más detallados, agregue la variable `VERBOSE_COMMANDS: '-vvv'` a la opción `.magento.env.yaml` archivo. |
| 128 | disable-maintenance-mode | Comando `/bin/magento maintenance:disable` error | Compruebe la `cloud.log` para obtener más información. Añadir `VERBOSE_COMMANDS: '-vvv'` en `.magento.env.yaml` para obtener resultados de comandos más detallados. |
| 129 | install-update: reset-password | No se puede leer la plantilla para restablecer la contraseña |  |
| 130 | install-update: cache_type | Error del comando: `php ./bin/magento cache:enable` | Comando `php ./bin/magento cache:enable` solo se ejecuta cuando Adobe Commerce está instalado, pero `./app/etc/env.php` el archivo estaba ausente o vacío al principio de la implementación. Compruebe la `cloud.log` para obtener más información. Añadir `VERBOSE_COMMANDS: '-vvv'` en `.magento.env.yaml` para obtener resultados de comandos más detallados. |
| 131 | install-update | El `crypt/key`  el valor clave no existe en `./app/etc/env.php` para el `CRYPT_KEY` variable de entorno de nube | Este error se produce si la variable `./app/etc/env.php` no está presente cuando comienza la implementación de Adobe Commerce o si la variable `crypt/key` el valor no está definido. Si migró la base de datos desde otro entorno, recupere el valor de la clave de cifrado de ese entorno. A continuación, añada el valor a [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) variable de entorno de nube en el entorno actual. Consulte [Clave de cifrado de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials). Si ha eliminado accidentalmente el `./app/etc/env.php` utilice el siguiente comando para restaurarlo a partir de los archivos de copia de seguridad creados a partir de una implementación anterior: `./vendor/bin/ece-tools backup:restore` Comando CLI .&quot; |
| 132 |  | No se puede conectar con el servicio de Elasticsearch | Compruebe si hay credenciales de Elasticsearch válidas y compruebe que el servicio se está ejecutando |
| 137 |  | No se puede conectar al servicio OpenSearch | Compruebe si hay credenciales de OpenSearch válidas y que el servicio se esté ejecutando |
| 133 | validate-config | Elimine la configuración del módulo Braintree del Magento que ya no sea compatible con Adobe Commerce o con Magento Open Source 2.4 y versiones posteriores. | La compatibilidad con el módulo Braintree ya no se incluye con Adobe Commerce o Magento Open Source 2.4.0 y posteriores. Elimine la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la sección de variables de `.magento.app.yaml` archivo. Para la asistencia del Braintree, utilice una extensión oficial de pagos de Braintree del Commerce Marketplace. |
| 134 | validate-config | Adobe Commerce y Magento Open Source 2.4.0 requieren que esté instalado el servicio de Elasticsearch | Instalar servicio de Elasticsearch |
| 138 | validate-config | Adobe Commerce y Magento Open Source 2.4.4 requieren que esté instalado el servicio OpenSearch o Elasticsearch | Instalar el servicio OpenSearch |
| 135 | validate-config | El motor de búsqueda debe establecerse en Elasticsearch para Adobe Commerce y Magento Open Source >= 2.4.0 | Compruebe la variable SEARCH_CONFIGURATION para `engine` opción. Si está configurada, elimine la opción o establezca el valor en &quot;elasticsearch&quot;. |
| 136 | validate-config | La base de datos dividida se eliminó a partir de Adobe Commerce y Magento Open Source 2.5.0. | Si utiliza una base de datos dividida, debe revertirla a una sola base de datos o migrarla a ella, o utilizar un método alternativo. |
| 139 | validate-config | Motor de búsqueda incorrecto | Esta versión de Adobe Commerce o Magento Open Source no admite OpenSearch. Debe utilizar las versiones 2.3.7-p3, 2.4.3-p2 o posterior |

### Fase posterior a la implementación

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 201 | is-deploy-failed | Error de implementación de fase |  |
| 202 |  | El `./app/etc/env.php` el archivo no se puede escribir | El script de implementación no puede realizar los cambios necesarios en `/app/etc/env.php` archivo. Compruebe los permisos del sistema de archivos. |
| 203 |  | La configuración no está definida en `schema.yaml` archivo | La configuración no está definida en `./vendor/magento/ece-tools/config/schema.yaml` archivo. Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 204 |  | Error al analizar el `.magento.env.yaml` archivo | El `./.magento.env.yaml` formato de archivo no válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 205 |  | No se puede leer el `.magento.env.yaml` archivo | Compruebe los permisos del archivo. |
| 206 |  | No se puede leer el `.schema.yaml` archivo |  |
| 207 | calentamiento | Error al cargar previamente algunas páginas de calentamiento |  |
| 208 | tiempo hasta el primer byte | Error al probar el tiempo hasta el primer byte (TTFB) |  |
| 227 | clean-cache | Comando `/bin/magento cache:flush` error | Compruebe la `cloud.log` para obtener más información. Añadir `VERBOSE_COMMANDS: '-vvv'` en `.magento.env.yaml` para obtener resultados de comandos más detallados. |

### General

| Código de error | Paso general | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 243 |  | La configuración no está definida en `schema.yaml` archivo | Compruebe que el nombre de la variable de configuración es correcto y que está definido. |
| 244 |  | Error al analizar el `.magento.env.yaml` archivo | El `./.magento.env.yaml` formato de archivo no válido. Utilice un analizador de YAML para comprobar la sintaxis y corregir los errores. |
| 245 |  | No se puede leer el `.magento.env.yaml` archivo | No se puede leer el `./.magento.env.yaml` archivo. Compruebe los permisos del archivo. |
| 246 |  | No se puede leer el `.schema.yaml` archivo |  |
| 247 |  | No se puede generar un módulo para eventos | Compruebe la `cloud.log` para obtener más información. |
| 248 |  | No se puede habilitar un módulo para eventos | Compruebe la `cloud.log` para obtener más información. |
| 249 |  | Error al generar el módulo AdobeCommerceWebhookPlugins | Compruebe la `cloud.log` para obtener más información. |
| 250 |  | Error al habilitar el módulo AdobeCommerceWebhookPlugins | Compruebe la `cloud.log` para obtener más información. |

## Errores de advertencia

Los errores de advertencia indican un problema con la configuración del proyecto de Commerce en la infraestructura en la nube, como ajustes de configuración incorrectos, obsoletos, no compatibles o faltantes para funciones opcionales que pueden afectar al funcionamiento del sitio. Aunque una advertencia no provoca un error de implementación, debe revisar los mensajes de advertencia y actualizar la configuración para resolverlos.

### Fase de compilación

| Código de error | Paso Generar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 1001 | validate-config | El archivo app/etc/config.php no existe |  |
| 1002 | validate-config | El .El archivo /build_options.ini ya no es compatible |  |
| 1003 | validate-config | Falta la sección de módulos en el archivo de configuración compartido |  |
| 1004 | validate-config | La configuración no es compatible con esta versión del Magento |  |
| 1005 | validate-config | Opciones de SCD ignoradas |  |
| 1006 | validate-config | El estado configurado no es ideal |  |
| 1007 | empacador de carreras | No se puede utilizar el paquete JS de Baler |  |

### Implementar fase

| Código de error | Paso Implementar | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 2001 | implementación previa:caché | La caché está configurada para un servicio Redis que no está disponible. Se ignorará la configuración. |  |
| 2002 | validate-config | El estado configurado no es ideal |  |
| 2003 | validate-config | No se ha configurado el valor de nivel de anidamiento de directorio para los informes de errores |  |
| 2004 | validate-config | Configuración no válida en .Archivo /pub/errors/local.xml. |  |
| 2005 | validate-config | Los datos de administración se utilizan para crear un usuario administrador solo durante la instalación inicial. Los cambios realizados en los datos de administración se omiten durante el proceso de actualización. | Después de la instalación inicial, puede quitar los datos de administración de la configuración. |
| 2006 | validate-config | No se ha creado el usuario administrador porque no se ha definido un correo electrónico de administrador | Después de la instalación, puede crear un usuario administrador manualmente: utilice ssh para conectarse a su entorno. A continuación, ejecute el `bin/magento admin:user:create` comando. |
| 2007 | validate-config | Actualizar versión de php a la versión recomendada |  |
| 2008 | validate-config | La compatibilidad con Solr ha quedado obsoleta en Adobe Commerce y Magento Open Source 2.1. |  |
| 2009 | validate-config | Adobe Commerce y Magento Open Source 2.2 o posterior ya no admiten Solr. |  |
| 2010 | validate-config | El servicio Elasticsearch se instala en el nivel de infraestructura, pero no se utiliza como motor de búsqueda. | Considere la posibilidad de eliminar el servicio de Elasticsearch del nivel de infraestructura para optimizar el uso de los recursos. |
| 2011 | validate-config | La versión del servicio de Elasticsearch en el nivel de infraestructura no es compatible con la versión actual del módulo elasticsearch/elasticsearch, que utiliza la aplicación de Adobe Commerce. |  |
| 2012 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2013 | validate-config | Opciones de SCD ignoradas porque el proceso de implementación no se ejecutó en la fase de compilación |  |
| 2014 | validate-config | La configuración contiene variables o valores obsoletos |  |
| 2015 | validate-config | La configuración del entorno no es válida |  |
| 2016 | validate-config | No se puede descodificar la configuración de tipo JSON |  |
| 2017 | validate-config | La configuración actual no es compatible con esta versión de Adobe Commerce |  |
| 2018 | validate-config | Algunos servicios han superado el límite de vida |  |
| 2019 | validate-config | La opción de configuración de búsqueda MySQL está obsoleta | Utilice el Elasticsearch en su lugar. |
| 2029 | validate-config | La base de datos dividida estaba en desuso en Adobe Commerce y Magento Open Source 2.4.2 y se eliminará en 2.5. | Si utiliza una base de datos dividida, debe empezar a planificar la reversión a una base de datos única o la migración a ella, o bien utilizar un método alternativo. |
| 2020 | install-update | La instalación de Adobe Commerce se ha completado, pero el `app/etc/env.php` falta el archivo de configuración o está vacío. | Los datos necesarios se restaurarán desde las configuraciones de entorno y desde el archivo .magento.env.yaml. |
| 2021 | install-update:conexión-db | Para bases de datos divididas que utilizan conexiones personalizadas |  |
| 2022 | install-update:conexión-db | Ha cambiado a una configuración de base de datos que no es compatible con la conexión esclava. |  |
| 2023 | install-update:split-db | Se omitirá la activación de una base de datos dividida. |  |
| 2024 | install-update:split-db | Falta la configuración de la variable SPLIT_DB para los tipos de conexión dividida. |  |
| 2025 | install-update:split-db | Conexión esclava no establecida. |  |
| 2026 | implementación previa:restore-writable-dirs | No se pudieron restaurar algunos datos generados durante la fase de compilación en los directorios montados | Compruebe la `cloud.log` para obtener más información. |
| 2027 | validate-config:image-mode-variable | No se admite el valor de modo para la variable de entorno MAGE_MODE | Elimine la variable de entorno MAGE_MODE o cambie su valor a &quot;producción&quot;. Adobe Commerce en la infraestructura en la nube solo admite el modo &quot;producción&quot;. |
| 2028 | almacenamiento remoto | No se pudo habilitar el almacenamiento remoto. | Compruebe las credenciales de almacenamiento remoto. |
| 2030 | validate-config | Los servicios de Elasticsearch y OpenSearch se instalan en el nivel de infraestructura. Adobe Commerce y Magento Open Source 2.4.4 y posteriores utilizan OpenSearch de forma predeterminada | Considere la posibilidad de eliminar el servicio Elasticsearch u OpenSearch de la capa de infraestructura para optimizar el uso de los recursos. |

### Fase posterior a la implementación

| Código de error | Paso posterior a la implementación | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 3001 | validate-config | El registro de depuración está habilitado en Adobe Commerce | Para ahorrar espacio en disco, no habilite el registro de depuración para los entornos de producción. |
| 3002 | calentamiento | No se pueden recuperar las direcciones URL del almacén |  |
| 3003 | calentamiento | No se puede obtener la URL del almacén |  |
| 3004 | reserva | No se pueden crear archivos de copia de seguridad |  |

### General

| Código de error | Paso general | Descripción del error (título) | Acción sugerida |
| - | - | - | - |
| 4001 |  | No se puede obtener el recuento del procesador del sistema: |  |
