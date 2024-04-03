---
source-git-commit: b08443d937dfc18120daa0d6a1277b9c7bca67aa
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---
# Fragmentos de nube

## advertencia del Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>Adobe Commerce no admite Elasticsearch 7.11 y versiones posteriores en infraestructuras en la nube. Las versiones de Adobe Commerce 2.3.7-p3, 2.4.3-p2 y 2.4.4 y posteriores admiten el servicio OpenSearch. Las instalaciones locales siguen siendo compatibles con Elasticsearch.

## Integración mejorada {#enhanced-integration-envs}

>[!NOTE]
>
>Los proyectos aprovisionados antes del 5 de junio de 2020 tenían varios entornos de integración más pequeños. Si necesita un entorno de integración más grande para pruebas y desarrollo, solicite una actualización a entornos de integración mejorados. Consulte la [Solicitud del entorno de integración](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) artículo en la _Centro de ayuda de Adobe Commerce_ para obtener más información.

## Opciones de combinación {#merge-options}

De forma predeterminada, el proceso de implementación sobrescribe todos los ajustes de la `env.php` archivo; sin embargo, puede optar por combinar uno o más valores para una configuración de servicio sin sobrescribir todos los valores.

Configure las variables `_merge` a una de las siguientes opciones:

- `true`—**Combinar** los valores de servicio configurados con los valores de variable de entorno.
- `false`—**Sobrescribir** los valores de servicio configurados con los valores de variable de entorno.

## Repositorio privado {#private-repository}

>[!NOTE]
>
>Adobe recomienda encarecidamente utilizar un repositorio privado para su proyecto de Adobe Commerce en la nube para proteger cualquier trabajo de desarrollo o información de propiedad, como extensiones y configuraciones confidenciales.

## Advertencia de autoservicio de Pro {#pro-self-service-warning}

>[!WARNING]
>
>Algunos **Proyectos Pro** requerir un ticket de asistencia para actualizar la configuración de ruta en la `routes.yaml` y la configuración de cron en el archivo `.magento.app.yaml` archivo. El Adobe recomienda actualizar y probar los archivos de configuración de YAML en un entorno de integración y, a continuación, implementar los cambios en el entorno de ensayo. Si los cambios no se aplican a los sitios de ensayo después de volver a implementar y no hay mensajes de error relacionados en el registro, debe **MUST** [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) que describe los cambios de configuración intentados. Incluya todos los archivos de configuración de YAML actualizados en el ticket.

## Asistencia de servicios Pro {#pro-update-service}

>[!TIP]
>Para proyectos Pro, debe [enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para instalar o actualizar [servicios](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html) in `Staging` y `Production` solo entornos de.
>
>Indique los cambios de servicio necesarios e incluya el actualizado `.magento.app.yaml` y `services.yaml` y especifique la versión de PHP en el ticket. Para ver los cambios de autoservicio en la versión, las extensiones o la configuración del entorno de PHP, consulte [Configuración de PHP](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/php-settings.html) in _Configuración de aplicación_.
>
>Para cambios en un _live_ Entorno de producción (**Solo Pro**), debe proporcionar un aviso con un mínimo de 48 horas para permitir que el equipo de infraestructura de Cloud tenga tiempo suficiente para recopilar recursos y realizar una actualización segura.

## Backups Pro {#pro-backups}

>[!TIP]
>
>En los entornos de ensayo y producción de Pro, debe [enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para recuperar una copia de seguridad específica teniendo en cuenta la fecha, la hora y la zona horaria del ticket.
>
>El Adobe sí **no** restaure cualquier entorno desde un backup automático. Consulte [Restaurar una instantánea de base de datos desde Ensayo o Producción](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) para obtener ayuda sobre cómo elegir un método para restaurar una instantánea de Ensayo o Producción.

## Advertencia de reimplementación {#redeploy-warning}

>[!WARNING]
>
>El proceso de implementación comienza cuando se realiza una combinación, inserción o sincronización del entorno, o cuando se déclencheur una reimplementación manual, durante la cual el proceso de [!DNL Commerce] La aplicación está en modo de mantenimiento. Para un entorno de producción, Adobe recomienda completar este trabajo durante las horas de menor actividad para evitar interrupciones en el servicio.

## Marcador de posición de ruta {#route-placeholder}

>[!NOTE]
>
>Los siguientes ejemplos de configuración de rutas utilizan plantillas de rutas con marcadores de posición. El `{default}` placeholder representa el dominio predeterminado configurado para su sitio. Si el proyecto tiene varios dominios, utilice el `{all}` para configurar el enrutamiento para el dominio predeterminado y todos los alias. Consulte [Configuración de rutas](/help/cloud-guide/routes/routes-yaml.md).

## Sincronización de SCD {#scd-timing-warning}

>[!WARNING]
>
>Si tiene problemas con archivos de contenido estático en la aplicación después de la implementación, como la falta de archivos de temas personalizados, aumente el tiempo de ejecución máximo esperado a 900 segundos o superior.

## Implementación basada en escenarios {#scenarios}

>[!NOTE]
>
>Con [!DNL ECE-Tools] 2002.1.0 y versiones posteriores puede utilizar la función de implementación basada en escenarios para personalizar los procesos de compilación, implementación y posteriores a la implementación para su proyecto de infraestructura de Adobe Commerce en la nube. Consulte [Implementación basada en escenarios](/help/cloud-guide/deploy/scenario-based.md).

## Instrucción de servicio {#service-instruction}

Siga estas instrucciones para la configuración del servicio en entornos de integración profesional y entornos de inicio, incluido el `master` Rama.

>[!NOTE]
>
>[Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para cambiar la configuración del servicio en entornos de ensayo y producción profesional.

## Cambio de servicio {#service-change-tip}

>[!TIP]
>
>Después de la instalación inicial del servicio, puede cambiar la versión del software de un servicio instalado actualizando el `services.yaml` y `.magento.app.yaml` archivos de configuración. Consulte [Cambiar la versión del servicio](/help/cloud-guide/services/services-yaml.md#change-service-version) para obtener instrucciones sobre cómo actualizar o degradar un servicio.

## Sugerencia de implementación atascada {#stuck-deployment-tip}

>[!TIP]
>
>Para obtener ayuda con las implementaciones bloqueadas, utilice el [Solucionador de problemas de implementación de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) en el _Centro de ayuda de Commerce_.

## Actualización a ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Si utiliza una versión de Adobe Commerce en una infraestructura en la nube que no contenga el `ece-tools` paquete, debe realizar una [actualización única](/help/cloud-guide/dev-tools/install-package.md) a su proyecto en la nube para eliminar los paquetes obsoletos. Si usa actualmente la variable `ece-tools` y debe actualizarlo, consulte la sección [Actualizar el paquete ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Sugerencia de actualización {#upgrade-tip}

>[!TIP]
>
>Antes de comenzar una actualización o un proceso de aplicación de parches, cree una rama activa desde el entorno de integración y extraiga la nueva rama a su estación de trabajo local. La dedicación de una rama al proceso de actualización o de revisión ayuda a evitar interferencias con el trabajo en curso.

<!-- Fastly-related snippets begin -->

## Inicio de sesión de administrador {#admin-login-step}

1. [Iniciar sesión](/help/get-started/onboarding.md#access-your-admin-panel) al administrador.

## Automatización de la implementación de fragmentos VCL personalizados {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>En lugar de cargar manualmente fragmentos de VCL personalizados, puede añadir fragmentos a `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` en su entorno. Los fragmentos de este directorio se cargan automáticamente al hacer clic en _cargar VCL en Fastly_ en el Administrador de comercio. Consulte [Implementación automatizada de fragmentos VCL personalizados](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) en el módulo Fastly de CDN para la documentación de Magento 2.

<!-- Fastly-related snippets end -->
