---
title: Configurar la implementación de aplicaciones
description: Aprenda a configurar las propiedades del archivo de configuración de la aplicación que controlan la forma en que la aplicación  [!DNL Commerce] se genera e implementa en el entorno de nube.
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Configurar la implementación de aplicaciones

El archivo `.magento.app.yaml` controla la forma en que se genera e implementa la aplicación. Aunque Adobe Commerce en la infraestructura de la nube admite varias aplicaciones por proyecto, normalmente un proyecto tiene una sola aplicación con el archivo `.magento.app.yaml` en la raíz del repositorio.

`.magento.app.yaml` tiene muchos valores predeterminados, vea [un archivo de muestra `.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Revise siempre `.magento.app.yaml` para la versión instalada. Este archivo puede diferir según la Adobe Commerce en las versiones de infraestructura en la nube.

Utilice el archivo `.magento.app.yaml` para definir los siguientes valores de configuración:

- [Propiedades](properties.md): defina los valores de propiedad para la instancia de aplicación.
- [Variables property](variables-property.md): revise las variables de entorno necesarias para la versión de la aplicación [!DNL Commerce].
- [Configuración de PHP](php-settings.md): configure las opciones de PHP en tiempo de ejecución.
- [Establecer caché para archivos estáticos](set-cache.md): establezca el TTL de caché para los archivos estáticos y multimedia.
