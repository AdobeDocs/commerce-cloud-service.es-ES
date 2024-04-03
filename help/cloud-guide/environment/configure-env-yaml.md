---
title: Configurar el entorno
description: Aprenda a configurar acciones de compilación e implementación en todos los entornos de infraestructura de Commerce en la nube, incluidos Pro Staging y Production, mediante variables de entorno.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: 66e257e2-1eca-4af5-9b56-01348341400b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configuración de variables de entorno para la implementación

El `.magento.env.yaml` Este archivo utiliza variables de entorno para centralizar la administración de las acciones de compilación e implementación en todos los entornos, incluidos Pro Staging y Production. Para configurar acciones únicas en cada entorno, debe modificar este archivo en cada entorno.

>[!TIP]
>
>Los archivos YAML distinguen entre mayúsculas y minúsculas y no permiten tabulaciones. Tenga cuidado de utilizar una sangría uniforme en todo el `.magento.env.yaml` o puede que la configuración no funcione según lo esperado. Los ejemplos de la documentación y del archivo de muestra utilizan _de dos espacios_ sangría. Utilice el [comando ece-tools validate](#validate-configuration-file) para comprobar su configuración.

## Estructura de archivos

El `.magento.env.yaml` El archivo contiene dos secciones: `stage` y `log`. El `stage` controla las acciones que se producen durante las fases del [Proceso de implementación en la nube](../deploy/process.md).

- `stage`: utilice la sección de fase para definir determinadas acciones para las siguientes fases de despliegue:
   - `global`: permite controlar las acciones en las fases de compilación, implementación y posterior a la implementación. Puede anular esta configuración en las secciones compilación, implementación y posterior a la implementación.
   - `build`: permite controlar las acciones sólo en la fase de compilación. Si no especifica la configuración en esta sección, la fase de compilación utiliza la configuración de la sección global.
   - `deploy`: permite controlar las acciones sólo en la fase de despliegue. Si no especifica la configuración en esta sección, la fase de implementación utiliza la configuración de la sección global.
   - `post-deploy`: permite controlar las acciones _después_ implementar la aplicación y _después_ el contenedor empieza a aceptar conexiones.
- `log`: utilice la sección de registro para configurar [notificaciones](set-up-notifications.md), incluidos los tipos de notificación y el nivel de detalle.
   - `slack`: permite configurar un mensaje para enviarlo a un bot de Slack.
   - `email`: permite configurar un correo electrónico para enviarlo a uno o varios destinatarios de correo electrónico.
   - [controladores de registro](log-handlers.md): permite configurar los mensajes de aplicación de hardware y software enviados a un servidor de registro remoto.

### Variables de entorno

El `ece-tools` el paquete establece valores en la variable `env.php` archivo basado en valores de [Variables de nube](variables-cloud.md), las variables configuradas en la variable [!DNL Cloud Console], y el `.magento.env.yaml` archivo de configuración. Las variables de entorno en la variable `.magento.env.yaml` personalice el entorno de la nube anulando la configuración de Commerce existente. Si un valor predeterminado es `Not Set`y, a continuación, el `ece-tools` el paquete toma **NO** y utiliza el [!DNL Commerce] por defecto o el valor de la configuración MAGENTO_CLOUD_RELATIONSHIPS. Si se establece el valor predeterminado, la variable `ece-tools` El paquete de actúa para establecer ese valor predeterminado.

Los temas siguientes contienen definiciones detalladas, como si se ha establecido o no un valor predeterminado, de todas las variables que puede utilizar en el `.magento.env.yaml` archivo:

- [Global](variables-global.md): las variables controlan las acciones en cada fase: generar, implementar y posimplementar
- [Generar](variables-build.md): las variables controlan las acciones de compilación
- [Implementar](variables-deploy.md)—variables control implementar acciones
- [Posterior a la implementación](variables-post-deploy.md): las variables controlan las acciones después de la implementación

### Crear archivo de configuración desde CLI

Puede generar un `.magento.env.yaml` archivo de configuración para un entorno de nube que utiliza lo siguiente `ece-tools` comandos.

>Crea un archivo de configuración

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Actualizar valores en el archivo de configuración

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Ambos comandos requieren un solo argumento: una matriz con formato JSON que especifique un valor para al menos una variable de compilación, implementación o posterior a la implementación. Por ejemplo, el comando siguiente establece los valores para la variable `SCD_THREADS` y `CLEAN_STATIC_FILES` variables:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

Y crea un `.magento.env.yaml` archivo con la siguiente configuración:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Puede usar el complemento `cloud:config:update` para actualizar el nuevo archivo. Por ejemplo, el comando siguiente cambia el `SCD_THREADS` y añade el `SCD_COMPRESSION_TIMEOUT` configuración:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

El archivo actualizado contiene la siguiente configuración:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Validar archivo de configuración

Utilice lo siguiente `ece-tools` para validar el `.magento.env.yaml` archivo de configuración antes de insertar cambios en el entorno de nube remoto.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

La siguiente respuesta de ejemplo proporciona una lista de elementos para corregir:

```terminal
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Constantes de PHP

Puede utilizar constantes de PHP en `.magento.env.yaml` definiciones de archivos en lugar de valores de codificación. El siguiente ejemplo define la variable `driver_options` uso de una constante PHP:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>El análisis constante no funciona cuando se utiliza un `symfony/yaml` versión del paquete anterior a 3.2.

## Control de errores

Cuando se produce un error debido a un valor inesperado en `.magento.env.yaml` archivo de configuración, recibe un mensaje de error. Por ejemplo, el siguiente mensaje de error presenta una lista de cambios sugeridos para cada elemento con un valor inesperado, a veces proporcionando opciones válidas:

```terminal
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Realice las correcciones necesarias, confirme y presione los cambios. Si no recibe un mensaje de error, los cambios realizados en el archivo de configuración superan la validación.

## Optimización de administración de configuración

Si ha habilitado la administración de la configuración después de volcar las configuraciones, debe mover las variables SCD_* de la implementación a la fase de compilación. Consulte [Estrategias de implementación de contenido estático](../deploy/static-content.md).

>Antes de la administración de configuración:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>Después de habilitar la administración de la configuración, mueva las variables SCD_* a la fase de compilación:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
