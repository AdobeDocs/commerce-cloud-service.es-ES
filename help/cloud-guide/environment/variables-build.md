---
title: Variables de compilación
description: Consulte la lista de variables de entorno que controlan las acciones en la fase de creación de la infraestructura en la nube de Adobe Commerce.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
exl-id: 243aaa45-a5ef-4ed2-8800-3d0f07bf3740
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Variables de compilación

Lo siguiente _generar_ Las variables controlan las acciones en la fase de compilación y pueden heredar y anular los valores de [Variables globales](variables-global.md). Inserte estas variables en la variable `build` fase de la `.magento.env.yaml` archivo:

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

Las siguientes variables se eliminaron en la versión 2.2:

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Predeterminado**—`1`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Defina el nivel de anidamiento de directorios para guardar archivos de informes de errores a fin de evitar rellenar el directorio de informes con decenas de miles de archivos, lo que puede dificultar la administración y revisión de los datos. Esta configuración predeterminada es `1`. Normalmente, no es necesario cambiar el valor predeterminado a menos que tenga problemas para administrar los archivos de informe de errores en `<magento_root>/var/report/` directorio.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifique una lista de parches de calidad de Adobe Commerce para aplicar durante la implementación.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

El ejemplo siguiente especifica tres parches que se deben aplicar durante la implementación.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Consulte [Aplicar parches](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Predeterminado**—`6`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifica qué [gzip](https://www.gnu.org/software/gzip) nivel de compresión (`0` hasta `9`) para usar al comprimir contenido estático; `0` deshabilita la compresión.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predeterminado**—`600`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Cuando el tiempo necesario para comprimir los recursos estáticos supera el límite de tiempo de espera de compresión, interrumpe el proceso de implementación. Establezca el tiempo máximo de ejecución, en segundos, para el comando de compresión de contenido estático.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Predeterminado**—`false`
- **Versión**: Adobe Commerce 2.4.2 y posterior

Configure como. `true` para evitar la generación de contenido estático para temáticas principales durante la fase de compilación.

Establecer `SCD_NO_PARENT: false` durante la fase de compilación, de modo que la generación de contenido estático para las temáticas principales no afecte a la implementación del sitio ni cause un tiempo de inactividad innecesario del sitio. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Puede configurar varias configuraciones regionales por tema. Esta personalización ayuda a acelerar el proceso de compilación al reducir el número de archivos de temas innecesarios. Por ejemplo, puede generar la variable _magento/backend_ temática en inglés y temática personalizada en otros idiomas.

En el siguiente ejemplo se genera la variable `Magento/backend` tema con tres configuraciones regionales:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

El siguiente ejemplo crea tres temas con tres configuraciones regionales:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

O bien, puede elegir _no_ implementar un tema:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.2.0 y posterior

Permite aumentar el tiempo de ejecución máximo esperado para la implementación de contenido estático.

De forma predeterminada, Adobe Commerce en la infraestructura en la nube establece la ejecución máxima esperada en 900 segundos, pero en algunos casos puede necesitar más tiempo para completar la implementación de contenido estático para un proyecto en la nube.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Predeterminado**—`quick`
- **Versión**: Adobe Commerce 2.2.0 y posterior

Personalización de [estrategia de implementación](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) para contenido estático. Consulte [Implementación de archivos de vista estática](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilice estas opciones _solamente_ si tiene más de una configuración regional:

- `standard`: permite desplegar todos los ficheros de vista estática para todos los paquetes.
- `quick`—(_predeterminado_) minimiza el tiempo de implementación.
- `compact`: permite ahorrar espacio en disco en el servidor. En la versión 2.2.4 y anteriores de Adobe Commerce, esta configuración anula el valor de `scd_threads` con un valor de `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predeterminado**—Automático
- **Versión**: Adobe Commerce 2.1.4 y posterior

Establece el número de subprocesos para la implementación de contenido estático. El valor predeterminado se establece en función del recuento de subprocesos de CPU detectado y no supera un valor de 4. El aumento del número de subprocesos acelera la implementación de contenido estático; reducir el número de subprocesos hace que se ralentice. Puede establecer el valor del subproceso, por ejemplo:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Para reducir aún más el tiempo de implementación, utilice [Administración de configuración](../store/store-settings.md) con el `scd-dump` para mover la implementación estática a la fase de compilación.

## `SCD_USE_BALER`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.3.0 y posterior

[Empacadora](https://github.com/magento/baler) analiza el código JavaScript generado y crea un paquete de JavaScript optimizado. La implementación del paquete optimizado en el sitio puede reducir el número de solicitudes de red al cargar el sitio y mejorar los tiempos de carga de las páginas.

Configure como. `true` para ejecutar Baler después de realizar la implementación de contenido estático.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Como Baler está en versión alfa, no es aconsejable utilizarlo en entornos de producción.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Predeterminado**— _Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure como. `true` para omitir el `composer dump-autoload` durante una instalación de Cloud Docker. Esta variable solo es relevante para los contenedores de Cloud Docker con sistemas de archivos editables. En estos casos, omitir el comando evita errores de otros comandos que intentan acceder al código desde el archivo eliminado `generated` directorio.

Cuando se ejecuta Adobe Commerce `composer dump-autoload`, crea archivos de carga automática con vínculos a clases generadas en el `generated` , que no es un problema en entornos de producción con sistemas de archivos de solo lectura. Sin embargo, para instalaciones de Cloud Docker con sistemas de archivos editables (creados únicamente para pruebas y desarrollo con `./vendor/bin/ece-docker build:compose --with-test`), puede ejecutar el `bin/magento -n setup:upgrade` sin el comando `--keep-generated` , que elimina la opción `generated` directorio. Si se elimina el directorio, la variable `composer dump-autoload` El comando falla porque la carga automática contiene vínculos a archivos del directorio eliminado.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Predeterminado**— _Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configure como. `true` para omitir la implementación de contenido estático durante la fase de compilación.

Si ya implementa contenido estático durante la fase de compilación con [Administración de configuración](../store/store-settings.md)Además, puede omitir la implementación de contenido estático para una prueba de generación rápida.

En la fase de compilación, establezca `SKIP_SCD: false` de modo que la generación de contenido estático se produzca durante la fase de generación en la que el proceso no afecte a la implementación del sitio ni cause un tiempo de inactividad innecesario en el sitio. Consulte [Implementación de contenido estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Habilitar o deshabilitar el [Symfony](https://symfony.com/doc/current/console/verbosity.html) depurar nivel de detalle para `bin/magento` Comandos CLI ejecutados durante la fase de implementación.

>[!NOTE]
>
>Para utilizar VERBOSE_COMMANDS para controlar los detalles de la salida del comando tanto para los resultados correctos como para los erróneos `bin/magento` Comandos CLI, debe establecer [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Elija el nivel de detalle proporcionado en los registros:

- `-v`= salida normal
- `-vv`= salida más detallada
- `-vvv` = salida detallada ideal para depurar

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
