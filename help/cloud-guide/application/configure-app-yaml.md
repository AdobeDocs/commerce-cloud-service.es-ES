---
title: Configurar la implementación de aplicaciones
description: Obtenga información sobre cómo configurar las propiedades del archivo de configuración de la aplicación que controlan la forma en que se usa la variable [!DNL Commerce] La aplicación se genera e implementa en el entorno de Cloud.
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Configurar la implementación de aplicaciones

El `.magento.app.yaml` controla la forma en que se genera e implementa la aplicación. Aunque Adobe Commerce en la infraestructura en la nube admite varias aplicaciones por proyecto, normalmente un proyecto tiene una sola aplicación con el `.magento.app.yaml` en la raíz del repositorio.

El `.magento.app.yaml` tiene muchos valores predeterminados, consulte [una muestra `.magento.app.yaml` archivo](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Revise siempre la `.magento.app.yaml` para la versión instalada. Este archivo puede diferir según la Adobe Commerce en las versiones de infraestructura en la nube.

Utilice el `.magento.app.yaml` para definir los siguientes valores de configuración:

- [Propiedades](properties.md): permite definir valores de propiedad para la instancia de aplicación.
- [Propiedad Variables](variables-property.md): revise las variables de entorno necesarias para [!DNL Commerce] versión de la aplicación.
- [Configuración de PHP](php-settings.md)—Configure las opciones de PHP en tiempo de ejecución.
- [Establecer Caché Para Archivos Estáticos](set-cache.md): permite definir el TTL de caché para los ficheros estáticos y de medios.
