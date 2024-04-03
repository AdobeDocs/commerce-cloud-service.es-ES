---
title: Proceso de implementación
description: Descubra cómo funciona la implementación de Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Build, Deploy, SCD
exl-id: 378fa290-5a71-4ac2-816a-a7c837e45b2f
source-git-commit: 3d9e3294872fedcc43744f54a71c71d8951ed853
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Proceso de implementación

El proceso de implementación comienza cuando se realiza una combinación, inserción o sincronización del entorno o cuando se almacena en déclencheur un [redistribución manual](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). El proceso de implementación lleva tiempo, pero hay formas de optimizar la implementación que dependen de si está desarrollando y probando o trabajando con un sitio activo. Lo más destacable es que puede controlar las [implementación de contenido estático](static-content.md).

Existen tres fases distintas del proceso de implementación: generación, implementación y posterior a la implementación. Cada fase realiza acciones específicas con recursos limitados:

## ![Fase de compilación](../../assets/status-build.png) Fase de compilación

El _generar_ fase monta contenedores para los servicios definidos en los archivos de configuración e instala dependencias basadas en la variable `composer.lock` y ejecuta los vínculos de generación definidos en el archivo `.magento.app.yaml` archivo. Sin la capacidad de conectarse a ningún servicio ni acceder a la base de datos, la fase de compilación depende de los recursos limitados al entorno.

## ![Fase de implementación](../../assets/status-deploy.png) Fase de implementación

El _implementar_ esta fase suspende temporalmente las solicitudes entrantes y pasa el sitio a [modo de mantenimiento](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). La fase de implementación utiliza los nuevos contenedores y, después de montar el sistema de archivos, abre las conexiones de red, activa los servicios definidos en la variable `relationships` de la sección `.magento.app.yaml` y ejecuta los vínculos de implementación definidos en la variable `.magento.app.yaml` archivo. Todo es _solo lectura_, excepto para los directorios definidos en la variable `.magento.app.yaml` archivo. De forma predeterminada, la variable [`mounts` propiedad](../application/properties.md#mounts) incluye los siguientes directorios:

- `app/etc`: contiene el `env.php` y `config.php` archivos de configuración
- `pub/media`: contiene todos los datos de medios, como productos o categorías.
- `pub/static`: contiene ficheros estáticos generados
- `var`: contiene ficheros temporales creados durante el tiempo de ejecución

Todos los demás directorios tienen permisos de solo lectura. El nuevo sitio se activa al final de la fase de implementación, a medida que pasa del modo de mantenimiento al modo de mantenimiento y libera la suspensión temporal de las solicitudes entrantes.

En la fase de implementación, copias del `app/etc/config.php` y `app/etc/env.php` Los archivos de configuración de implementación se guardan con la extensión BAK. Consulte [Configuración de tienda](../store/store-settings.md#restore-configuration-files) para obtener más información sobre la restauración de estos archivos.

## ![Fase posterior a la implementación](../../assets/status-post-deploy.png) Fase posterior a la implementación

El _posterior a la implementación_ ejecuta los vínculos posteriores a la implementación definidos en la `.magento.app.yaml` archivo. Realizar cualquier acción en esta fase puede afectar al rendimiento del sitio; sin embargo, puede utilizar la variable [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) variable de entorno para rellenar la caché.

## ![Verificar estado](../../assets/status-verify.png) Comprobar configuraciones

Puede probar la configuración óptima para el estado del proyecto ejecutando el [Asistentes inteligentes](smart-wizards.md).

>[!NOTE]
>
>Con `ece-tools` 2002.1.0 y versiones posteriores puede utilizar la función de implementación basada en escenarios para personalizar los procesos de compilación, implementación y posteriores a la implementación para su proyecto de infraestructura de Adobe Commerce en la nube. Consulte [Implementación basada en escenarios](scenario-based.md).
