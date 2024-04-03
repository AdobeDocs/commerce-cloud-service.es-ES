---
title: Propiedades
description: Utilice la lista de propiedades como referencia al configurar la variable [!DNL Commerce] aplicación para crear e implementar en la infraestructura de la nube.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 58a86136-a9f9-4519-af27-2f8fa4018038
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# Propiedades para la configuración de aplicaciones

El `.magento.app.yaml` utiliza propiedades para administrar la compatibilidad del entorno con el [!DNL Commerce] aplicación.

| Nombre | Descripción | Predeterminado | Requerido |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Personalizar funciones de usuario | — | No |
| [`crons`](crons-property.md) | Actualizar especificaciones y programar trabajos cron | — | No |
| [`dependencies`](#dependencies) | Habilitar dependencias adicionales | `php:composer/composer: '2.2.4'` | No |
| [`disk`](#disk) | Definir el tamaño del disco persistente | `5120` | Sí |
| [`firewall`](firewall-property.md) | (Solo Starter) Controlar el tráfico saliente | — | No |
| [`hooks`](hooks-property.md) | Personalice los comandos del shell para las fases de compilación, implementación y posterior a la implementación | — | No |
| [`mounts`](#mounts) | Definir rutas | Rutas:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | No |
| [`name`](#name) | Definición del nombre de la aplicación | `mymagento` | Sí |
| [`relationships`](#relationships) | Servicios de mapa | Servicios:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | No |
| [`runtime`](#runtime) | La propiedad Runtime incluye extensiones que requiere el [!DNL Commerce] aplicación. | Extensiones:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Sí |
| [`type`](#type-and-build) | Establecer la imagen del contenedor base | `php:8.1` | Sí |
| [`variables`](variables-property.md) | Aplicar una variable de entorno para una versión específica de Commerce | — | No |
| [`web`](web-property.md) | Gestión de solicitudes externas | — | Sí |
| [`workers`](workers-property.md) | Gestión de solicitudes externas | — | Sí, si no se utiliza la propiedad web |

{style="table-layout:auto"}

## `name`

El `name` proporciona el nombre de la aplicación utilizado en la propiedad [`routes.yaml`](../routes/routes-yaml.md) para definir el flujo ascendente HTTP (de forma predeterminada), `mymagento:http`). Por ejemplo, si el valor de `name` es `app`, debe utilizar `app:http` en el campo upstream.

>[!WARNING]
>
>No cambie el nombre de la aplicación después de implementarla. Al hacerlo, se pierden datos.

## `type` y `build`

El `type`  y `build` las propiedades proporcionan información sobre la imagen del contenedor base para generar y ejecutar el proyecto.

El compatible `type` el lenguaje es PHP. Especifique la versión de PHP de la siguiente manera:

```yaml
type: php:<version>
```

El `build` determina qué sucede de forma predeterminada al crear el proyecto. El `flavor` especifica un conjunto predeterminado de tareas de generación para ejecutar. El siguiente ejemplo muestra la configuración predeterminada para `type` y `build` de `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.1
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.2.4'
```

### Instalación y uso de Composer 2

El `build: flavor:` La propiedad no se utiliza para Composer 2.x; por lo tanto, debe instalar Composer manualmente durante la fase de compilación. Para instalar y utilizar Composer 2.x en sus proyectos de Starter y Pro, debe realizar tres cambios en su `.magento.app.yaml` configuración:

1. Eliminar `composer` como el `build: flavor:` y agregue `none`. Este cambio evita que Cloud use la versión predeterminada 1.x de Composer para ejecutar tareas de compilación.
1. Añadir `composer/composer: '^2.0'` as a `php` para instalar Composer 2.x.
1. Añada el `composer` crear tareas en una `build` enlace para ejecutar las tareas de compilación utilizando Composer 2.x.

Utilice los siguientes fragmentos de configuración por su cuenta `.magento.app.yaml` configuración:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Consulte [Paquetes necesarios](../development/overview.md#required-packages) para obtener más información sobre Composer.

## `dependencies`

Especifique las dependencias que la aplicación pueda necesitar durante el proceso de generación.

Adobe Commerce admite dependencias en los siguientes idiomas:

- PHP
- Rubí
- Node.js

Estas dependencias son independientes de las dependencias finales de la aplicación y están disponibles en el `PATH`, durante el proceso de compilación y en el entorno de tiempo de ejecución de la aplicación.

Puede especificar esas dependencias de la siguiente manera:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Use para modificar la configuración de PHP en tiempo de ejecución, como habilitar extensiones. Se requieren las siguientes extensiones:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Consulte [Configuración de PHP](php-settings.md) para obtener más información sobre la activación de extensiones.

## `disk`

Define el tamaño del disco persistente de la aplicación en MB.

```yaml
disk: 5120
```

El tamaño mínimo de disco recomendado es de 256 MB. Si ve el error `UserError: Error building the project: Disk size may not be smaller than 128MB`, aumente el tamaño a 256 MB.

>[!NOTE]
>
>Para los entornos de ensayo y producción de Pro, debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para actualizar el `mounts` y `disk` para su aplicación. Cuando envíe el ticket, indique los cambios de configuración necesarios e incluya una versión actualizada de su `.magento.app.yaml` archivo.

## `relationships`

Define la asignación de servicios en la aplicación.

La relación `name` está disponible para la aplicación en el `MAGENTO_CLOUD_RELATIONSHIPS` variable de entorno. El `<service-name>:<endpoint-name>` la relación se asigna a los valores de nombre y tipo definidos en la variable `.magento/services.yaml` archivo.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

A continuación se muestra un ejemplo de las relaciones predeterminadas:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

Consulte [Servicios](../services/services-yaml.md) para obtener una lista completa de los tipos de servicio y extremos admitidos actualmente.

## `mounts`

Un objeto cuyas claves son rutas relativas a la raíz de la aplicación. El montaje es un área de escritura en el disco para archivos. A continuación se muestra una lista predeterminada de los montajes configurados en la `magento.app.yaml` usando el archivo `volume_id[/subpath]` sintaxis:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

El formato para agregar el montaje a esta lista es el siguiente:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared`: permite compartir un volumen entre las aplicaciones dentro de un entorno.
- `disk`: permite definir el tamaño disponible para el volumen compartido.

>[!NOTE]
>
>Para los entornos de ensayo y producción de Pro, debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para actualizar el `mounts` y `disk` para su aplicación. Cuando envíe el ticket, indique los cambios de configuración necesarios e incluya una versión actualizada de su `.magento.app.yaml` archivo.

Puede hacer que la web de montaje sea accesible añadiéndola al [`web`](web-property.md) bloque de ubicaciones.

>[!WARNING]
>
>Una vez que el sitio tenga datos, no cambie el `subpath` parte del nombre del montaje. Este valor es el identificador único del `files` área. Si cambia este nombre, perderá todos los datos del sitio almacenados en la ubicación antigua.

## `access`

El `access` indica un nivel mínimo de función de usuario que puede acceder SSH a los entornos. Las funciones de usuario disponibles son:

- `admin`: puede cambiar la configuración y ejecutar acciones en el entorno; tiene _colaborador_ y _visualizador_ derechos.
- `contributor`: puede insertar código en este entorno y ramificarlo desde el entorno; tiene _visualizador_ derechos.
- `viewer`: sólo se puede ver el entorno.

La función de usuario predeterminada es `contributor`, que restringe el acceso SSH de los usuarios con solo _visualizador_ derechos. Puede cambiar la función de usuario a `viewer` para permitir el acceso SSH solo a los usuarios con _visualizador_ derechos:

```yaml
access:
    ssh: viewer
```
