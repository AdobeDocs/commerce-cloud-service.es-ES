---
title: '[!DNL ECE-Tools] Paquete'
description: Obtenga información acerca de [!DNL ECE-Tools] y cómo ayuda a administrar e implementar Adobe Commerce.
exl-id: 5583a685-29c5-4de5-8d2e-94cff5ff37ab
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Paquete de herramientas ECE

El [!DNL ECE-Tools] es un conjunto de secuencias de comandos y herramientas diseñadas para administrar e implementar el [!DNL Commerce] aplicación. El `ece-tools` simplifica muchos procesos, como la administración de trabajos cron, la verificación de la configuración del proyecto y la aplicación de parches de Adobe y correcciones rápidas. Puede ver y contribuir al [de código abierto [!DNL ECE-Tools] repositorio de código en GitHub][ece-repo].

{{ece-tools-package}}

El `ece-tools` es compatible con Adobe Commerce (a partir de la versión 2.1.4) y contiene scripts y comandos de Adobe Commerce en la infraestructura de la nube diseñados para ayudarle a administrar su código y generar e implementar automáticamente sus proyectos.

A continuación se enumeran los disponibles `ece-tools` comandos:

```bash
php ./vendor/bin/ece-tools list
```

## Creación e implementación

El `ece-tools` Este paquete contiene comandos para realizar operaciones en las fases de compilación, implementación y posterior implementación del inicio de Adobe Commerce en la aplicación de infraestructura en la nube. Por ejemplo, la variable `php ./vendor/bin/ece-tools build` comienza el proceso de generación de la aplicación.

De forma predeterminada, estas `ece-tools` Los comandos de se encuentran en [propiedad hooks](../application/hooks-property.md) de la `.magento.app.yaml` archivo de configuración.

## Generador de configuración de Docker

El `ece-tools` El paquete incluye una dependencia para [magento/magento-cloud-docker] , que proporciona archivos de funcionalidad y configuración para que las imágenes de Docker inicien un entorno de desarrollo de Docker para Adobe Commerce en la infraestructura en la nube. También puede ejecutar Cloud Docker para Commerce como paquete independiente. Consulte [Desarrollo de Docker](../dev-tools/cloud-docker.md).

## Servicios, rutas y variables

Puede usar el complemento `ece-tools` para mostrar información detallada sobre el paquete codificado en Base64 [Variables de nube](../environment/variables-cloud.md) se utiliza en cualquier entorno de Cloud. El siguiente comando muestra todos los servicios, rutas y variables.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Para mostrar un conjunto específico de información, utilice el siguiente formato:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services`: permite mostrar los datos de relación del `MAGENTO_CLOUD_RELATIONSHIPS` variable de entorno, definida en la variable `services.yaml` archivo.
- `routes`: permite mostrar las rutas configuradas para el proyecto mediante la opción `MAGENTO_CLOUD_ROUTES` variable de entorno.
- `variables`: permite mostrar las variables configuradas para el proyecto mediante el `MAGENTO_CLOUD_VARIABLES` variable de entorno.

Salida de ejemplo para `services` opción:

```terminal
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Verificar configuración del entorno

Hay un conjunto de comandos de verificación disponibles para ayudar a evaluar la configuración del proyecto. Consulte [Asistentes inteligentes](../deploy/smart-wizards.md) en el _Optimizar implementación_ para obtener una descripción detallada de cada comando del asistente. El `wizard:ideal-state` El comando se ejecuta automáticamente durante la fase de compilación. Para verificar el estado ideal del proyecto:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Debe ejecutar el `wizard:ideal-state` en el entorno remoto de la nube. El comando siempre devuelve el `The configured state is not ideal` error al ejecutarse en el entorno de desarrollo local.

Salida de ejemplo:

```terminal
Ideal state is configured
```

Consulte [Notas de la versión de ece-tools](../release-notes/cloud-tools-suite.md).

## Parches de Adobe y parches personalizados

El `ece-tools` El paquete incluye una dependencia para [magento/magento-cloud-patch] , que ofrece parches de Adobe y correcciones rápidas que mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube y admiten el envío rápido de correcciones críticas. &quot;también ofrece parches personalizados que añade a su proyecto de infraestructura en la nube de Adobe Commerce. Consulte [Aplicar parches](../development/apply-patches.md).

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patch]: https://github.com/magento/magento-cloud-patches
