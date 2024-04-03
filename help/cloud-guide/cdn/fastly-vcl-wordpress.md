---
title: Redireccionar solicitudes a un servidor CMS
description: Aprenda a redireccionar las solicitudes entrantes de una tienda de Adobe Commerce a un sitio de WordPress independiente mediante el módulo Fastly edge.
feature: Cloud, Configuration, Routes
exl-id: 5bd9c56f-4412-4643-89b6-590a8ec65ac0
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# Redireccionar solicitudes a un servidor CMS

Redireccione las solicitudes entrantes de una tienda de Adobe Commerce a un sitio de WordPress separado usando el módulo Fastly Edge _Otra integración CMS/back-end_ con un diccionario de Edge. Puede seguir un proceso similar para redireccionar las solicitudes a otros backends de CMS.

Utilice los módulos de Fastly Edge para crear y cargar código VCL personalizado desde el administrador en lugar de escribir manualmente el código VCL y cargarlo mediante la API de Fastly.

>[!NOTE]
>
>Se recomienda añadir configuraciones de VCL personalizadas a un entorno de ensayo en el que pueda probarlas antes de actualizar la configuración del servicio Fastly en el entorno de producción.

**Requisitos previos**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Para redireccionar solicitudes de Adobe Commerce a WordPress**:

1. Habilite los módulos de Fastly Edge en el entorno de ensayo o producción.

   - Inicie sesión en Admin.

   - Vaya a **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** > **Caché de página completa** > **Configuración rápida** > **Configuración avanzada**.

   - Establezca el valor para **Módulos de Fastly Edge** hasta **Sí**.

   - Guarde la configuración.

1. Identifique las rutas URL para redireccionar al backend de WordPress.

1. Complete las siguientes tareas para configurar el servicio Fastly y crear el código VCL personalizado para redireccionar solicitudes al back-end de WordPress.

   - Cree un diccionario de Edge que especifique las rutas que se volverán a enrutar desde el almacén de Adobe Commerce al servidor.

   - Agregue el servidor de WordPress a la configuración del servicio Fastly y adjunte la condición de solicitud para las reescrituras de URL.

   - Configure las variables _Otra integración CMS/back-end_ Módulo Edge para gestionar las reescrituras de URL desde Adobe Commerce al backend de WordPress.

     Para obtener instrucciones detalladas, consulte [Módulos Fastly Edge: integración de otros CMS/back-end](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) en el _Módulo de CDN de Fastly para Magento 2_ documentación.

1. Después de actualizar la configuración del servicio Fastly, pruebe la tienda de Adobe Commerce para asegurarse de que las solicitudes de URL especificadas para WordPress se redireccionan correctamente.
