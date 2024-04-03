---
title: Administración de configuración de tienda
description: Obtenga información sobre cómo administrar y sincronizar los ajustes de configuración de la tienda en todos los entornos de Adobe Commerce en la nube.
feature: Cloud, Configuration, SCD
exl-id: f2dd876d-24ee-4d47-b9ac-44fcf77b61b5
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Administración de configuración de tienda

Las configuraciones predeterminadas de la tienda se almacenan en un `config.xml` para el módulo adecuado. Al cambiar la configuración en el Administrador de comercio o en la CLI `bin/magento config:set` , los cambios se reflejan en la base de datos principal, específicamente en la `core_config_data` tabla. Estos ajustes sobrescriben las configuraciones predeterminadas almacenadas en el `config.xml` archivo.

Configuración de almacenamiento, que hace referencia a las configuraciones en el Administrador **Tiendas** > **Configuración** > **Configuración** , se almacenan en los archivos de configuración de implementación en función del tipo de configuración:

- `app/etc/config.php`: configuración para tiendas, sitios web, módulos o extensiones, optimización de archivos estáticos y valores del sistema relacionados con la implementación de contenido estático. Consulte la [config.php reference](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) en el _Guía de configuración_.
- `app/etc/env.php`: valores para las anulaciones específicas del sistema y la configuración confidencial que deben _NO_ se almacenarán en el control de código fuente. Consulte la [referencia env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) en el _Guía de configuración_.

>[!NOTE]
>
>Como Adobe Commerce en la infraestructura en la nube solo admite los modos de producción y mantenimiento, la variable **Avanzadas** > **Desarrollador** No se puede acceder a la sección desde el Administrador. Debe tener [privilegios de administrador del entorno](../project/user-access.md) para completar las tareas de administración de configuración. Puede configurar opciones adicionales mediante [variables de entorno](../environment/configure-env-yaml.md).

La administración de la configuración proporciona una forma de implementar configuraciones de tienda coherentes en todos los entornos con un tiempo de inactividad mínimo mediante la implementación de canalización. El proyecto de infraestructura de Adobe Commerce en la nube incluye el servidor de compilación, los scripts de compilación e implementación y los entornos de implementación diseñados con [estrategia de implementación de canalización](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) en mente.

## Esquema de anulación de configuración

Todas las configuraciones del sistema se establecen durante las fases de generación e implementación según el siguiente esquema de invalidación:

1. Si existe una variable de entorno, utilice la configuración personalizada e ignore la configuración predeterminada.
1. Si no existe una variable de entorno, utilice la configuración de una `MAGENTO_CLOUD_RELATIONSHIPS` par nombre-valor en [`.magento.app.yaml` archivo](../application/configure-app-yaml.md). Omitir la configuración predeterminada.
1. Si una variable de entorno no existe y `MAGENTO_CLOUD_RELATIONSHIPS` no contiene un par nombre-valor, quite toda la configuración personalizada y utilice los valores de la configuración predeterminada.

En resumen, las variables de entorno anulan todos los demás valores.

>[!TIP]
>
>Consulte [Administración de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) en el _Guía de configuración_ para obtener más información sobre el esquema de anulación para la implementación de la canalización.

Si la misma configuración se configura en varios lugares, la aplicación depende de la siguiente jerarquía de configuración para determinar qué valor se aplicará al entorno

| Prioridad | Configuración<br>Método | Descripción |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variables de entorno | Valores añadidos desde _Variables_ pestaña de configuración del entorno en la [!DNL Cloud Console]. Especifique aquí los valores para las configuraciones sensibles o específicas del entorno. La configuración especificada aquí no se puede editar desde el administrador. Consulte [Variables de configuración de entorno](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valores añadidos en `variables` de la sección `.magento.app.yaml` archivo. Especifique valores aquí para garantizar una configuración coherente en todos los entornos. **No especifique valores confidenciales en `.magento.app.yaml` archivo.** Consulte [Configuración de aplicación](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Los valores de configuración específicos del entorno almacenados aquí se agregan mediante el uso del `app:config:dump` comando. Establezca los valores confidenciales y específicos del sistema utilizando variables de entorno o la CLI. Consulte [Datos confidenciales](#sensitive-data). El `env.php` el archivo es **no** incluido en el control de código fuente. |
| 4 | `app/etc/config.php` | Los valores almacenados aquí se agregan mediante el `app:config:dump` comando. Los valores de configuración compartidos se añaden a `config.php`. Establezca la configuración compartida desde el administrador o utilizando la CLI. El `config.php` se incluye en el control de código fuente. |
| 5 | Base de datos | Los valores almacenados aquí se agregan estableciendo configuraciones en el Administrador. Las configuraciones configuradas con cualquiera de los métodos anteriores están bloqueadas (atenuadas) y no se pueden editar desde el administrador. |
| 6 | `config.xml` | Muchas configuraciones tienen valores predeterminados establecidos en `config.xml` para un módulo. Si Adobe Commerce no encuentra ningún valor establecido por ninguno de los métodos anteriores, vuelve al valor predeterminado, si está establecido. |

{style="table-layout:auto"}

## Volcado de configuración

Puede utilizar lo siguiente `ece-tools` para generar un `config.php` que contiene todas las configuraciones de almacén actuales:

```bash
./vendor/bin/ece-tools config:dump
```

Los datos &quot;volcados&quot; a `app/etc/config.php` se convierte en archivo _bloqueado_, lo que significa que el campo correspondiente del Administrador de comercio se convierte en **de solo lectura**. El `config.php` incluye solo las opciones que configure. No bloquea los valores predeterminados. Bloquear solo los valores que actualice también garantiza que todas las extensiones utilizadas en los entornos de ensayo y producción no se rompan debido a configuraciones de solo lectura, especialmente Fastly.

>[!WARNING]
>
>El `ece-tools config:dump` El comando no recupera configuraciones detalladas para módulos, como B2B. Si necesita un volcado de configuración completo, utilice el `app:config:dump` , pero este comando bloquea los valores de configuración en un estado de solo lectura.

### Datos confidenciales

Todas las configuraciones confidenciales se exportan a `app/etc/env.php` cuando se utiliza el archivo `bin/magento app:config:dump` comando. Puede establecer valores confidenciales mediante el comando CLI: `bin/magento config:sensitive:set`. Consulte  [Configuración confidencial y específica del entorno](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) en el _Extensiones PHP de Commerce_ para obtener información sobre cómo designar los ajustes de configuración como confidenciales o específicos del sistema.

Ver una lista de [Configuración confidencial o específica del sistema](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) en el _Guía de configuración_.

### Rendimiento de SCD

Según el tamaño del almacén, puede tener un gran número de archivos de contenido estático para implementar. Normalmente, el contenido estático se implementa durante la fase de implementación cuando la aplicación está en modo de mantenimiento. La configuración más óptima es generar contenido estático durante la fase de compilación. Consulte [Elección de una estrategia de implementación](../deploy/static-content.md).

Si ha habilitado la administración de la configuración después de volcar las configuraciones, debe mover las variables SCD_* de la fase de implementación a la fase de compilación para habilitar correctamente la generación de contenido estático durante la fase de compilación. Consulte [Variables de entorno](../environment/configure-env-yaml.md#environment-variables).

**Antes de la administración de configuración**:

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

**Después de habilitar la administración de configuración**:

Mueva las variables SCD_* a la fase de compilación:

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

>[!NOTE]
>
>Antes de implementar archivos estáticos, las fases de compilación e implementación comprimen el contenido estático mediante GZIP. La compresión de archivos estáticos reduce las cargas del servidor y aumenta el rendimiento del sitio. Consulte [opciones de compilación](../environment/variables-build.md) para obtener más información sobre cómo personalizar o deshabilitar la compresión de archivos.

## Procedimiento para administrar la configuración

A continuación se ofrece una descripción general de alto nivel de este proceso:

![Descripción general de la administración de configuración de inicio](../../assets/starter/configuration-management-flow.png)

**Para configurar el almacén y generar un archivo de configuración**:

1. Complete todas las configuraciones de sus tiendas en Admin para uno de los entornos:

   - Starter: una rama de desarrollo activa
   - Pro: rama activa en el entorno de integración

   Estas configuraciones no incluyen los productos reales a menos que piense en descargar la base de datos de este entorno a los entornos de ensayo y producción. Normalmente, las bases de datos de desarrollo no incluyen todos los datos del almacén.

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree un volcado local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

1. Agregue, confirme y envíe cambios en el código para actualizar un entorno remoto.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Una vez completada la implementación, inicie sesión en el administrador del entorno actualizado para comprobar la configuración. Continúe combinando las configuraciones adicionales en los entornos de ensayo y producción, según sea necesario.

### Actualizar configuraciones

Cuando modifique el entorno mediante el Administrador y ejecute de nuevo el comando, las nuevas configuraciones se anexan al código en la variable `config.php` archivo.

>[!WARNING]
>
>Mientras que puede editar manualmente la variable `config.php` en los entornos de Ensayo y Producción, es **no** recomendado. El archivo ayuda a mantener la coherencia de todas las configuraciones en todos los entornos. Nunca borre el `config.php` para volver a crearlo. Al eliminar el archivo, se pueden quitar configuraciones y valores específicos necesarios para los procesos de generación e implementación.

### Restaurar archivos de configuración

Copias del original `app/etc/env.php` y `app/etc/config.php` Los archivos de se crearon durante el proceso de implementación y se almacenan en la misma carpeta. A continuación se muestra el BAK (archivos de copia de seguridad) y PHP (archivos originales) en el mismo `app/etc` carpeta:

```terminal
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Las configuraciones más antiguas utilizaban `app/etc/config.local.php` archivo. Consulte [Migración de configuraciones antiguas](#migrate-older-configurations).

**Para restaurar los archivos de configuración**:

1. En la estación de trabajo local, utilice SSH para iniciar sesión en el proyecto y entorno remotos.

   ```bash
   magento-cloud ssh
   ```

1. Compruebe la ubicación y disponibilidad de los archivos de copia de seguridad.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Respuesta de ejemplo:

   ```terminal
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Restaurar archivos de copia de seguridad.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migración de configuraciones antiguas

Si actualiza a Adobe Commerce en la infraestructura en la nube 2.2 o posterior, es posible que desee migrar la configuración del `config.local.php` al nuevo `config.php` archivo. Si los ajustes de configuración de la administración coinciden con el contenido del archivo, siga las instrucciones para generar y agregar el `config.php` archivo.

Si son diferentes, puede anexar contenido desde el `config.local.php` al nuevo `config.php` archivo:

1. Siga las instrucciones para generar el `config.php` archivo.

1. Abra el `config.php` y elimine la última línea.

1. Abra el `config.local.php` y copie el contenido.

1. Pegue el contenido en `config.php` , guarde y complete agregándolo a Git.

1. Implemente en todos sus entornos.

Solo puede completar esta migración una vez. Después de la migración, utilice el `config.php` archivo.

### Cambiar configuraciones regionales

Puede cambiar las configuraciones regionales de la tienda sin seguir un complejo proceso de importación y exportación de la configuración, _if_ usted tiene [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) activado. Puede actualizar las configuraciones regionales mediante el Administrador.

Puede agregar otra configuración regional al entorno de ensayo o producción activando `SCD_ON_DEMAND` en una rama de integración, genere un `config.php` con la nueva información de configuración regional y copie el archivo de configuración en el entorno de destino.

>[!WARNING]
>
>Este proceso **sobrescribe** la configuración de la tienda; solo haga lo siguiente si los entornos contienen las mismas tiendas.

1. En el entorno de integración, habilite la `SCD_ON_DEMAND` usando la variable [`.magento.env.yaml` archivo](../environment/configure-env-yaml.md).

1. Añada las configuraciones regionales necesarias con su administrador.

1. Utilice SSH para iniciar sesión en el entorno remoto y generar el `/app/etc/config.php` archivo que contiene todas las configuraciones regionales.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copie el nuevo archivo de configuración del entorno de integración remota al directorio de entorno local.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Agregue, confirme y envíe cambios en el código para actualizar un entorno remoto.
