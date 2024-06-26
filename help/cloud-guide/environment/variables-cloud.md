---
title: Variables específicas de la nube
description: Consulte una lista de variables de entorno específicas de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
exl-id: 84b7c0fc-f0b0-4ff5-9f33-9d17180a9306
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Variables específicas de la nube

Las variables de entorno específicas de Adobe Commerce en la infraestructura en la nube utilizan `MAGENTO_CLOUD_*` prefijo:

| Variable | Descripción |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | Ruta absoluta al directorio de la aplicación. |
| `MAGENTO_CLOUD_APPLICATION` | Objeto JSON con codificación base64 que describe la aplicación. Se asigna al `.magento.app.yaml` el contenido del archivo y tiene subclaves. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | El nombre de la aplicación configurada en la variable `.magento.app.yaml` archivo. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | La ruta absoluta a la raíz del documento web, si corresponde. |
| `MAGENTO_CLOUD_ENVIRONMENT` | Nombre de la rama de entorno. |
| `MAGENTO_CLOUD_PROJECT` | El ID del proyecto. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | Objeto JSON codificado en Base64 que representa la definición del extremo de clave (nombre de relación) y valor (matrices de pares de relaciones). Cada definición de extremo de relación es una forma descompuesta de una dirección URL. Tiene un `scheme`, a `host`, a `port`, y _opcionalmente_ a `username`, `password`, `path`y más información en `query`. |
| `MAGENTO_CLOUD_ROUTES` | Describir las rutas definidas en el entorno `.magento/routes.yaml` archivo. |
| `MAGENTO_CLOUD_TREE_ID` | El ID de árbol para la aplicación, que corresponde al SHA del árbol en Git. |
| `MAGENTO_CLOUD_VARIABLES` | Un objeto JSON codificado en Base64 con pares clave-valor, como `"key":"value"`. |
| `MAGENTO_CLOUD_LOCKS_DIR` | Proporciona la ruta al punto de montaje para el proveedor de bloqueos en la infraestructura de la nube. El proveedor de bloqueo evita el inicio de trabajos cron y grupos cron duplicados. |

>[!WARNING]
>
>Para agregar variables de entorno a [omitir configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) uso del [[!DNL Cloud Console]](../project/overview.md), debe anteponer el nombre de la variable con `env:` como en el ejemplo siguiente:
>
>![Ejemplo de variable de entorno](../../assets/set-env-variable-ui.png)

Dado que los valores pueden cambiar con el tiempo, es mejor inspeccionar la variable durante la ejecución y utilizarla para configurar la aplicación. Por ejemplo, utilice la variable `MAGENTO_CLOUD_RELATIONSHIPS` para recuperar relaciones relacionadas con el entorno de la siguiente manera:

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## Visualización de variables de entorno

Puede usar el complemento `env:config:show` comando desde [el `ece-tools` paquete](../dev-tools/package-overview.md) para mostrar una lista de variables para el entorno actual.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

Salida de ejemplo para `variables` opción:

```terminal
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
