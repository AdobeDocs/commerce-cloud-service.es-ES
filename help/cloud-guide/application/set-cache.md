---
title: Establecer caché para archivos estáticos
description: Aprenda a configurar las opciones de almacenamiento en caché en la [!DNL Commerce] archivo de configuración de la aplicación.
feature: Cloud, Configuration, Cache, SCD
exl-id: ca6db004-47fc-45ea-b8db-c0ecc3c2136b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Establecer caché para archivos estáticos

El TTL de caché (tiempo de vida) para sus archivos multimedia y estáticos se establece en la variable `.magento.app.yaml` archivo de configuración con la variable `expires` clave.

>[!NOTE]
>
>Antes de actualizar el entorno de producción, es importante probar los cambios en el entorno de ensayo. [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obtener ayuda sobre cómo actualizar la configuración de estos entornos.

1. Especifique el tiempo TTL (en segundos) en la variable [`web` propiedad](web-property.md) de la `.magento.app.yaml` archivo. Puede añadir la variable `expires` clave debajo de `locations` o en `"/media"` y `"/static"`.

   Para evitar que la caché caduque, utilice el `expires: -1` par clave-valor. Consulte el siguiente ejemplo:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```
