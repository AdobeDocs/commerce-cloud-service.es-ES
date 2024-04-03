---
title: Prácticas recomendadas para actualizar el proyecto
description: Consulte una lista de prácticas recomendadas para actualizar los archivos de proyecto.
feature: Cloud, Best Practices, Upgrade
exl-id: 7d0a2627-e4c5-46b4-9e6c-24d20fa4f92f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Prácticas recomendadas para actualizar el proyecto

Siga las prácticas recomendadas para compilaciones e implementación, y use [Actualizaciones y parches](../development/commerce-version.md) flujo de trabajo para actualizar la aplicación. Siga estas directrices para planificar el trabajo de actualización y posterior a la actualización:

- **Copia de seguridad del proyecto**: Antes de actualizar Adobe Commerce y cualquier extensión de terceros o personalizada, realice una copia de seguridad de la base de datos en los entornos de integración, ensayo y producción. Consulte [Realizar copia de seguridad de la base de datos](../development/commerce-version.md#project-backup).

- **Comprobar problemas de compatibilidad**-

   - Asegúrese de que todas las temáticas personalizadas sean compatibles con la nueva versión de Adobe Commerce

   - Después de actualizar extensiones de terceros y personalizadas, utilice el `magento-cloud local:build` para validar las dependencias del Compositor antes de implementar.

   - Revise las notas de la versión y la documentación de la extensión de Adobe Commerce para asegurarse de que ha implementado las soluciones alternativas o los cambios de configuración necesarios para solucionar problemas funcionales conocidos y errores relacionados con la versión y las extensiones de Adobe Commerce actualizadas.

   - Asegúrese de que las versiones del servicio instalado sean compatibles con la nueva versión de Adobe Commerce y actualice los servicios según sea necesario. Consulte [Servicios](../services/services-yaml.md).

   - Pruebe la base de datos para abordar cualquier problema que puedan introducir las actualizaciones de la versión y las extensiones de Adobe Commerce.

   - Realice las actualizaciones necesarias en la configuración específica del entorno antes de implementarla en el entorno remoto.

   - Asegúrese de que la versión del servicio de búsqueda sea compatible con la versión del cliente PHP. Consulte [Configurar Elasticsearch](../services/elasticsearch.md) o [Configuración de OpenSearch](../services/opensearch.md).

- **Compruebe la conectividad de la base de datos y el almacenamiento disponible en entornos remotos**-

   - Utilice SSH para iniciar sesión en el servidor remoto y verificar la conexión con la base de datos MySQL. Consulte [Conexión a la base de datos](../services/mysql.md#connect-to-the-database).

   - Verificar el almacenamiento disponible en el entorno remoto: utilice `disk free` para ver y administrar el espacio en disco disponible en los entornos de la nube. Consulte [Administrar espacio en disco](../storage/manage-disk-space.md).

      - Compruebe el tamaño de la base de datos actualizada y compruebe que la variable `services.yaml` tiene suficiente espacio en disco asignado.

      - Libere espacio en disco: borre la caché y limpie el `/log` y `/tmp` directorios antes de implementar.

- **Planifique y realice una actualización correcta en los entornos local e de integración, antes de implementarla en Ensayo**-Después de la actualización, pruebe la implementación y resuelva cualquier problema.

- **Combine el código en Ensayo y, a continuación, en Producción**: pruebe y resuelva cualquier problema en el entorno de ensayo antes de aplicar cambios al entorno de producción.

- **Completar tareas posteriores a la actualización**-

   - Utilice SSH para iniciar sesión en el servidor remoto y compruebe lo siguiente:

      - Compruebe el estado del indexador y vuelva a indexar según sea necesario. Consulte [Administrar los indexadores](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html) en el _Guía de configuración_.

      - Compruebe la `cron` registros y el `cron_schedule` en la base de datos de Adobe Commerce para comprobar el estado de cron y volver a ejecutar los trabajos de cron según sea necesario.
Consulte [Registro](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging) en el _Guía de configuración_.

   - Complete el UAT de prueba de aceptación de usuarios posterior a la actualización en entornos de ensayo y producción y corrija cualquier problema relacionado con las actualizaciones de extensiones personalizadas y de terceros.
