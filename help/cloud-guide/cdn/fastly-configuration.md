---
title: Configurar servicios de Fastly
description: Aprenda a configurar los servicios de Fastly para su proyecto de Adobe Commerce.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: c53ff3bd-3df2-45fb-933e-d3b29f7edf4e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '1961'
ht-degree: 0%

---

# Configurar servicios de Fastly

Se requiere rápidamente para Adobe Commerce en entornos de ensayo y producción de infraestructura en la nube.

Fastly funciona con Varnish para proporcionar capacidades de almacenamiento en caché rápidas y un [Red de distribución de contenido](https://glossary.magento.com/content-delivery-network) (CDN) para recursos estáticos. Fastly también proporciona un firewall de aplicaciones web (WAF) para proteger el sitio y la infraestructura de nube. Para proteger su sitio y la infraestructura de Cloud del tráfico y los ataques maliciosos, enrute todo el tráfico entrante del sitio a través de Fastly.

>[!NOTE]
>
>Fastly no está disponible en entornos de integración.

Complete los siguientes pasos para habilitar, configurar y probar Fastly al principio del proceso de desarrollo del sitio para habilitar el acceso seguro al sitio.

- Obtenga rápidamente credenciales para los entornos de ensayo y producción
- Habilitar el almacenamiento en caché de CDN
- Cargar fragmentos de VCL de Fastly
- Actualizar la configuración de DNS para enrutar el tráfico al servicio de Fastly
- Probar el almacenamiento en caché rápido

>[!NOTE]
>
>Después de habilitar y verificar la configuración inicial de Fastly, puede personalizar la configuración. Por ejemplo, puede habilitar opciones adicionales como optimización de imágenes, módulos Edge y código VCL personalizado. Consulte [Personalizar configuración de caché](fastly-custom-cache-configuration.md).

## Obtener credenciales rápidamente

Durante el aprovisionamiento del proyecto, Adobe agrega el proyecto al [Cuenta de servicio de Fastly](fastly.md#fastly-service-account-and-credentials) para Adobe Commerce en la infraestructura en la nube y crea credenciales de cuenta de Fastly para la plataforma de inicio `master` y entornos de ensayo y producción Pro. Cada entorno tiene credenciales únicas.

Necesita las credenciales de Fastly para configurar los servicios de CDN de Fastly desde el administrador y enviar solicitudes de API de Fastly.

>[!NOTE]
>
>Con Adobe Commerce en la infraestructura en la nube, no puede acceder directamente al administrador de Fastly. Utilice el administrador para revisar y actualizar la configuración de Fastly para sus entornos. Si no puede resolver un problema mediante las funciones de Fastly en el administrador, envíe un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

Utilice los siguientes métodos para buscar y guardar el ID de servicio de Fastly y el token de API para su entorno:

**Para ver sus credenciales de Fastly**:

El método para ver las credenciales es diferente para los proyectos Pro y Starter.

- Directorio compartido montado en IaaS: en proyectos Pro, utilice SSH para conectarse al servidor y obtener las credenciales de Fastly desde el `/mnt/shared/fastly_tokens.txt` archivo. Los entornos de ensayo y producción tienen credenciales únicas. Debe obtener las credenciales de cada entorno.

- Espacio de trabajo local: en la línea de comandos, utilice el `magento-cloud` CLI a [lista y revisión](../environment/variables-cloud.md#viewing-environment-variables) Variables de entorno rápidamente.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console]: compruebe las siguientes variables de entorno en la [Configuración del entorno](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Si no encuentra las credenciales de Fastly para los entornos de ensayo o producción, póngase en contacto con el asesor técnico del cliente (CTA) de Adobe.

## Habilitar almacenamiento en caché rápido

Necesita los siguientes componentes para habilitar y configurar los servicios de Fastly:

- Última versión de [Módulo Fastly CDN para Magento 2](fastly.md#fastly-cdn-module-for-magento-2) instalado en los entornos de ensayo y producción. Consulte [Actualizar rápidamente](#upgrade-the-fastly-module).

- [Credenciales de Fastly](#get-fastly-credentials) para Adobe Commerce en entornos de ensayo y producción de infraestructura en la nube

**Para habilitar el almacenamiento en caché de CDN Fastly en Ensayo y producción**:

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** y ampliar **Caché de página completa**.

   ![Expandir para seleccionar Rápidamente](../../assets/cdn/fastly-menu.png)

1. En el _Aplicación de almacenamiento en caché_ , quite la selección de **Usar valor del sistema**, y luego seleccione **Fastly CDN** en la lista desplegable.

   ![Elegir rápidamente](../../assets/cdn/fastly-enable-admin.png)

1. Expandir **Configuración rápida** y [elegir opciones de almacenamiento en caché](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. Después de configurar las opciones de almacenamiento en caché, haga clic en **Guardar configuración** en la parte superior de la página.

1. Borre la caché según la notificación.

1. Continúe configurando Fastly volviendo a **Tiendas** > **Configuración** > **Configuración** > **Avanzadas** > **Sistema** > **Configuración rápida**.

### Probar las credenciales de Fastly

1. En Admin, vaya a **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** > **Configuración rápida**.

1. Si es necesario, agregue la variable **ID de servicio rápido** y **Token de API** valores para el entorno del proyecto.

   ![Administrador de credenciales de Fastly](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >No seleccione el vínculo para crear el token de API de Fastly. En su lugar, utilice el [Credenciales de Fastly (ID de servicio y token de API) proporcionadas por el Adobe](#get-fastly-credentials) proporcionadas por el Adobe.

1. Clic **Credenciales de prueba**.

1. Si la prueba se realiza correctamente, haga clic en **Guardar configuración** y, a continuación, borre la caché.

   Si la prueba falla, compruebe que los valores correctos del ID de servicio y del token de API coinciden con las credenciales del entorno actual.

   Si la prueba vuelve a fallar, envíe un ticket de asistencia de Adobe Commerce o póngase en contacto con el representante de la cuenta de Adobe. En los proyectos Pro, incluya las direcciones URL de los sitios de producción y ensayo. En Proyectos iniciales, incluya las direcciones URL de su `Master` y el sitio de ensayo.

>[!NOTE]
>
>Para obtener instrucciones para cambiar las credenciales de token de la API de Fastly para un entorno de ensayo o producción, consulte [Cambiar credenciales de Fastly](fastly.md#change-fastly-api-token).

### Cargar VCL a Fastly

Después de habilitar el módulo de Fastly, cargue el predeterminado [Código VCL](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) a los servidores de Fastly. Este código proporciona una serie de fragmentos de VCL que especifican los ajustes de configuración para habilitar el almacenamiento en caché y otros servicios de Fastly CDN para su Adobe Commerce en la infraestructura en la nube.

>[!NOTE]
>
>Los servicios de almacenamiento en caché de Fastly no funcionarán hasta que complete la carga inicial del código VCL de Fastly en los sitios de ensayo y producción de Adobe Commerce.

**Para cargar el VCL de Fastly**:

1. En el _Configuración rápida_ , haga clic en **Cargar VCL a Fastly** como se muestra en la siguiente figura.

   ![Cargar un VCL de Magento a Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

## Aprovisionar certificados SSL/TLS

El Adobe de proporciona un certificado SSL/TLS validado por el dominio para servir tráfico HTTPS seguro desde Fastly. Adobe proporciona un certificado para cada entorno de Producción profesional, Ensayo y Producción inicial para proteger todos los dominios de ese entorno. Para obtener información detallada sobre el certificado proporcionado, consulte [Certificados SSL (TLS) de Adobe para Adobe Commerce en la infraestructura en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html).

>[!NOTE]
>
>Puede proporcionar su propio certificado TLS o SSL en lugar de utilizar el certificado Let&#39;s Encrypt proporcionado por el Adobe. Sin embargo, este proceso requiere un trabajo adicional de configuración y mantenimiento. Para elegir esta opción, envíe un vale de soporte de Adobe Commerce o trabaje con el Adobe para añadir certificados personalizados y alojados a su Adobe Commerce en entornos de infraestructura en la nube.

Para habilitar los certificados SSL/TLS para entornos de Adobe Commerce, la automatización de la Adobe completa los siguientes pasos:

- Valida la propiedad del dominio
- Proporciona un certificado Let&#39;s Encrypt SSL/TLS que cubre subdominios y de nivel superior especificados para sus tiendas
- Carga el certificado en el entorno de nube cuando el sitio está activo

Esta automatización requiere que actualice la configuración DNS del sitio para proporcionar información de validación del dominio. Uso **uno** de los siguientes métodos:

- **Validación de DNS**-Para sitios activos, actualice la configuración DNS con registros CNAME que apunten al servicio Fastly
- **Registros CNAME de desafío ACME**-Actualice la configuración DNS con los registros CNAME de desafío ACME proporcionados por el Adobe para cada dominio de su entorno

>[!TIP]
>
>Si tiene un dominio de producción que no está activo, utilice los registros CNAME del desafío ACME para la validación del dominio. Añadir los registros a la configuración DNS antes de tiempo permite al Adobe aprovisionar el certificado SSL/TLS con los dominios correctos antes del inicio del sitio. Antes de iniciar la producción, debe reemplazar estos registros de marcador de posición con los registros CNAME proporcionados por Adobe.

Cuando la validación del dominio se completa, Adobe aprovisiona el certificado TLS/SSL de Let&#39;s Encrypt y lo carga en entornos de ensayo o producción activos. Este proceso puede tardar hasta 12 horas. Le recomendamos que complete las actualizaciones de configuración de DNS con varios días de antelación para evitar retrasos en el desarrollo del sitio y el inicio del mismo.

## Actualizar la configuración de DNS con la configuración de desarrollo

Durante el proceso inicial de configuración de Fastly, puede utilizar las siguientes direcciones URL para configurar y probar el almacenamiento en caché de Fastly en los entornos de ensayo y producción:

- Para Ensayo y producción profesionales:

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Solo para la producción inicial:

   - `mcprod.<your-domain>.com`

Estas direcciones URL predeterminadas de preproducción están disponibles después de aprovisionar el proyecto. El valor de `"your-domain"` es el nombre de dominio que especificó durante el proceso de incorporación.

>[!NOTE]
>
>No se puede especificar un dominio personalizado para un entorno que no sea de producción en proyectos iniciales.

Para enrutar el tráfico desde las direcciones URL de la tienda al servicio Fastly, actualice la configuración DNS. Al actualizar la configuración, Adobe aprovisiona automáticamente los certificados SSL/TLS necesarios y los carga en los entornos de nube. Este aprovisionamiento puede tardar hasta 12 horas.

>[!NOTE]
>
>Cuando esté listo para iniciar el sitio de producción, debe actualizar de nuevo la configuración DNS para que los dominios de producción apunten al servicio de Fastly y completar las tareas de configuración adicionales. Consulte [Iniciar lista de comprobación](../launch/checklist.md).

**Requisitos previos:**

- Habilite el módulo Fastly.
- Cargue el código VCL predeterminado de Fastly.
- Proporcione una lista de subdominios y de nivel superior para cada entorno de Adobe o envíe un ticket de asistencia de Adobe Commerce.
- Espere a que se confirme que los dominios especificados se han agregado a los entornos de nube.
- En Proyectos iniciales, agregue los dominios a la configuración del servicio de Fastly. Consulte [Administrar dominios](fastly-custom-cache-configuration.md#manage-domains).
- Para obtener información acerca de cómo actualizar la configuración de DNS, consulte con su [registrador de DNS](https://lookup.icann.org/) para obtener el método correcto para el servicio de dominio.

**Para actualizar la configuración DNS para desarrollo**:

1. Apunte las direcciones URL de preproducción al servicio Fastly añadiendo registros CNAME: `prod.magentocloud.map.fastly.net`, por ejemplo:

   | Dominio o subdominio | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Cuando los registros CNAME están activos, el Adobe aprovisiona certificados y carga los certificados SSL/TLS.

   >[!NOTE]
   >
   >Si planea utilizar dominios Apex (`your-domain.com`) para el sitio de producción, debe configurar los registros de direcciones DNS (registros A) para que apunten a las direcciones IP del servidor de Fastly. Consulte [Actualizar la configuración de DNS con la configuración de producción](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Agregue registros CNAME de desafío ACME para la validación del dominio y el aprovisionamiento previo de certificados SSL/TLS de producción, por ejemplo:

   | Dominio o subdominio | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >Los registros de desafío ACME de este ejemplo son marcadores de posición que no están pensados para aprovisionar los sitios de ensayo y producción de Adobe Commerce. Póngase en contacto con el Adobe para obtener la información correcta del registro de desafíos ACME para su proyecto.

   Después de agregar los registros CNAME, Adobe valida los dominios y aprovisiona el certificado SSL/TLS para el entorno. Al actualizar la configuración DNS para enrutar el tráfico de estos dominios al servicio Fastly, el Adobe carga el certificado en el entorno.

1. Actualice la dirección URL base de Adobe Commerce.

   - Utilice SSH para iniciar sesión en el entorno de producción.

     ```bash
     magento-cloud ssh
     ```

   - Utilice la CLI de nube para cambiar la dirección URL base de su tienda.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Como alternativa al uso de la CLI de nube, puede actualizar la URL base desde el [Administrador](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)

1. Reinicie el explorador web.

1. Pruebe el sitio web.

## Probar el almacenamiento en caché rápido

Una vez completados los cambios de configuración de DNS, utilice el [cURL](https://curl.se/) herramienta de línea de comandos para comprobar que la caché de Fastly funciona.

**Para comprobar los encabezados de respuesta**:

1. En un terminal, utilice lo siguiente `curl` para probar la URL de su sitio activo:

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Si no ha establecido una ruta estática o completado la configuración DNS para los dominios de su sitio activo, utilice el `--resolve` indicador, que evita la resolución de nombres DNS.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. En la respuesta, compruebe lo siguiente [encabezados](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) para garantizar que Fastly funcione. Debería ver los siguientes encabezados únicos en la respuesta:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Si los encabezados no tienen los valores correctos, consulte [Resolución de errores encontrados en los encabezados de respuesta](fastly-troubleshooting.md#curl) para obtener ayuda sobre solución de problemas.

## Actualización del módulo de Fastly

Actualiza rápidamente el módulo de Fastly CDN para Magento 2 para resolver problemas, aumentar el rendimiento y proporcionar nuevas funciones.
Le recomendamos que actualice el módulo Fastly en los entornos de ensayo y producción a la [última versión](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Después de actualizar el módulo, debe cargar el código VCL para aplicar los cambios a la configuración del servicio de Fastly.

>[!WARNING]
>
> Si ha personalizado el código VCL predeterminado de Fastly con una versión personalizada, al actualizar el módulo Fastly se sobrescriben los cambios. Si ha añadido fragmentos de VCL personalizados con nombres únicos, esos cambios se conservan durante el proceso de actualización. Como práctica recomendada, actualice el entorno de ensayo y valide los cambios antes de aplicar los cambios al entorno de producción.

**Para comprobar la versión del módulo Fastly CDN para el Magento 2**:

1. Cambie al directorio raíz del entorno de Cloud.

1. Use Composer para comprobar la versión instalada.

   ```bash
   composer show *fastly*
   ```

1. Si la variable [última versión](https://github.com/fastly/fastly-magento2/releases) no está instalado, complete los pasos para actualizar el módulo de Fastly.

**Para actualizar el módulo de Fastly**:

1. En su entorno de integración local, utilice la siguiente información del módulo para [actualizar el módulo de Fastly](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Inserte las actualizaciones en el entorno de ensayo.

1. Inicie sesión en el administrador de su entorno de ensayo para. [cargar el código VCL](#upload-vcl-to-fastly).

1. [Verificar servicios de Fastly](fastly-troubleshooting.md#verify-or-debug-fastly-services) en el sitio Adobe Commerce Staging.

Después de comprobar los servicios de Fastly en el sitio de ensayo, repita el proceso de actualización en el entorno de producción.

>[!TIP]
>
> Si tiene problemas con los servicios de Fastly en los entornos de Adobe Commerce, consulte la [Solucionador de problemas de Adobe Commerce Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html).
