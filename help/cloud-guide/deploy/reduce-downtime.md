---
title: Implementación sin tiempo de inactividad
description: Aprenda a reducir el tiempo de inactividad general al implementar Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Deploy, SCD, Themes
exl-id: ff89d2e1-dfc8-4f6d-bd98-947559af13f0
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# Implementación sin tiempo de inactividad

Adobe Commerce en la infraestructura de la nube ejecuta la aplicación en [_mantenimiento_ modo](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode) durante la fase de implementación, que deja el sitio sin conexión hasta que se completa la implementación. El tiempo que el sitio de producción esté en modo de mantenimiento depende del tamaño del sitio, el número de cambios aplicados durante la implementación y la configuración para la implementación de contenido estático. Es posible configurar el proyecto para que se implemente con una **zero** efecto de tiempo de inactividad.

Durante el proceso de implementación, todas las conexiones se ponen en cola durante un máximo de 5 minutos y conservan las sesiones activas y las acciones pendientes, como agregar al carro de compras o cerrar la compra. Después de la implementación, la cola se libera y las conexiones continúan sin interrupción. Para usar esto _suspensión de conexión_ aproveche al máximo y reduzca la implementación a _zero_ tiempo de inactividad, debe configurar el proyecto para que utilice la estrategia de implementación más eficiente.

Siga estos pasos para reducir el tiempo que tarda su tienda en implementar una actualización en Producción:

1. [Actualice a la `ece-tools` paquete](../dev-tools/install-package.md) o [actualice el `ece-tools` version](../dev-tools/update-package.md)
El proyecto de infraestructura de Adobe Commerce en la nube debe tener la última versión `ece-tools` para disponer de las herramientas necesarias para configurar una implementación óptima. Si tiene la última versión `ece-tools`, continúe con el paso siguiente.

   >[!NOTE]
   >
   >Aunque es recomendable utilizar la última versión de `ece-tools` , el método de implementación de tiempo de inactividad cero funciona con `ece-tools` [versión 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) y más tarde.

1. [Configuración de la implementación de contenido estático](static-content.md)
Si la implementación de contenido estático falla en la fase de implementación, el sitio se queda atascado en el modo de mantenimiento. Cuando se produce un error durante la fase de compilación, el proceso evita el tiempo de inactividad porque nunca comienza la fase de implementación. [Generación de contenido estático durante la fase de compilación con el HTML minificado](static-content.md#setting-the-scd-on-build), también conocido como el estado ideal, es la configuración óptima para implementaciones sin tiempo de inactividad y _previene_ tiempo de inactividad si se produce un error.

1. [Configuración del vínculo posterior a la implementación](../application/hooks-property.md)
Debe configurar el vínculo posterior a la implementación para limpiar y calentar la caché. De forma predeterminada, la limpieza de caché se produce durante la fase de implementación cuando el sitio está inactivo. Si mueve la caché limpia a la fase posterior a la implementación, significa que la caché permanece activa hasta que se complete la fase de implementación y, a continuación, puede limpiar con seguridad la caché.

   Personalice la lista de páginas utilizadas para cargar previamente la caché con el [Variable de entorno WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Reducir archivos de tema](../environment/variables-deploy.md#scdmatrix)
Puede reducir el número de archivos de temas innecesarios configurando la variable de entorno SCD\_MATRIX.

1. [Acelerar la implementación de contenido estático](../environment/variables-deploy.md#scdthreads)
Puede acelerar el proceso de implementación actualizando la variable de entorno SCD\_THREADS para aumentar el número de subprocesos para la implementación de contenido estático.

>[!NOTE]
>
>Puede validar la configuración del proyecto para una implementación óptima haciendo lo siguiente [ejecución del asistente de estado ideal](smart-wizards.md#verifying-an-ideal-configuration).
