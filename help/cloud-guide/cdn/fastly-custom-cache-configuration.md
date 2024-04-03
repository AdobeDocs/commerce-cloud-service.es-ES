---
title: Personalizar configuración de caché
description: Obtenga información sobre cómo revisar y personalizar los ajustes de configuración de la caché después de completar la instalación del servicio de Fastly.
feature: Cloud, Configuration, Iaas, Cache
exl-id: f1fc85d4-7867-4bb5-9f11-bc8d2d80383b
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '1808'
ht-degree: 0%

---

# Personalizar configuración de caché

Después de configurar y probar el servicio de Fastly en los entornos de ensayo y producción, revise y personalice la configuración de la caché. Por ejemplo, puede actualizar la configuración para permitir que TLS redirija solicitudes HTTP a Fastly, actualizar la configuración de depuración y habilitar la autenticación básica para proteger con contraseña el sitio durante el desarrollo.

Las siguientes secciones proporcionan información general e instrucciones para configurar algunos ajustes de la caché. Encuentre información adicional acerca de las opciones de configuración disponibles en la [Módulo Fastly de CDN para el Magento 2](https://github.com/fastly/fastly-magento2/tree/master/Documentation) documentación.

## Forzar TLS

Proporciona rápidamente la _Forzar TLS_ para redirigir solicitudes sin cifrar (HTTP) a Fastly. Una vez aprovisionado el entorno de ensayo o producción con un [certificado SSL/TLS válido](fastly-configuration.md#provision-ssltls-certificates), puede actualizar la configuración de Fastly para su tienda para habilitar la opción Forzar TLS. Ver el Fastly [Forzar guía TLS](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md) en el _Módulo Fastly de CDN para el Magento 2_ documentación.

>[!NOTE]
>
>Habilitar la opción Forzar TLS es una práctica recomendada para Adobe Commerce en almacenes de infraestructura en la nube.

## Extender el tiempo de espera rápido

La configuración del servicio Fastly especifica un periodo de tiempo de espera predeterminado de 180 segundos para las solicitudes HTTPS al administrador. Cualquier procesamiento de solicitud que supere el tiempo de espera devuelve un error 503. Como resultado, puede recibir 503 errores en respuesta a solicitudes que requieren un procesamiento prolongado o cuando intenta realizar operaciones por lotes.

Para completar acciones masivas que tarden más de 3 minutos, cambie el _Tiempo de espera de ruta de administrador_ value_ para evitar errores 503.

>[!NOTE]
>
>Para ampliar los parámetros de tiempo de espera de Fastly para otros usuarios que no sean administradores en la IU de Fastly, consulte [Aumentar tiempos de espera para trabajos largos](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md).

**Para ampliar el tiempo de espera de Fastly para el administrador**:

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** y ampliar **Caché de página completa**.

1. En el _Configuración rápida_ sección, expandir **Configuración avanzada**.

1. Configure las variables **Tiempo de espera de ruta de administrador** valor en segundos. Este valor no puede ser superior a 10 minutos (600 segundos).

1. Clic **Guardar configuración** en la parte superior de la página.

1. Una vez que la página se vuelva a cargar, seleccione **Cargar VCL a Fastly** en el _Configuración rápida_ sección.

Recupera rápidamente la ruta de administración para generar el archivo VCL desde el `app/etc/env.php` archivo de configuración.

## Configuración de opciones de depuración

Proporciona rápidamente varios tipos de opciones de depuración en la página Administración de caché de Magento, incluidas las opciones para depurar categoría de producto, recursos de producto y contenido. Cuando está habilitado, Fastly observa los eventos para purgar automáticamente esas cachés. Si desactiva una opción de depuración, puede depurar manualmente las cachés de Fastly después de finalizar las actualizaciones a través de la página Gestión de Cachés.

Las opciones de depuración incluyen:

- **Purgar categoría**-Purga el contenido de la categoría de producto (no el contenido del producto) cuando agrega y actualiza un solo producto. Es posible que desee mantener esto deshabilitado y habilitar la depuración de productos, que purga productos y categorías de productos.
- **Purgar producto**: purga todo el contenido de productos y categorías de productos al guardar una sola modificación en un producto. Habilitar el producto de purga puede resultar útil para obtener actualizaciones inmediatamente para los clientes al cambiar un precio, agregar una opción de producto y cuando el inventario de productos esté agotado.
- **Página Purgar CMS**-Purga el contenido de la página al actualizar y agregar páginas al CMS de Adobe Commerce. Por ejemplo, es posible que desee depurar al actualizar los Términos y condiciones o la política de devoluciones. Si no suele realizar estos cambios, puede deshabilitar la depuración automática.
- **Depuración suave**-Establece el contenido cambiado como obsoleto y lo purga según el tiempo de obsolescencia. Además de los horarios de obsolescencia, los clientes reciben contenido obsoleto mientras que Fastly actualiza el contenido en segundo plano.

![Configuración de opciones de depuración](../../assets/cdn/fastly-purge-options.png)

**Para configurar las opciones de depuración de Fastly**:

1. En el _Configuración rápida_ sección, expandir **Configuración avanzada** para mostrar las opciones de depuración.

1. Para cada opción de depuración, seleccione **Sí** para activar la depuración automática, o **No** para desactivar la depuración automática.

   Al desactivar una opción de depuración, debe depurar manualmente la caché de esa categoría desde el _Administración de caché_ página.

1. Clic **Guardar configuración** en la parte superior de la página.

1. Una vez que la página se vuelva a cargar, seleccione **Cargar VCL a Fastly** en el _Configuración rápida_ sección.

Para obtener más información, consulte [las Opciones de configuración de Fastly](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options).

## Configuración de la gestión GeoIP

El módulo Fastly incluye la administración de GeoIP para redirigir automáticamente a los visitantes o proporcionar una lista de tiendas que coincidan con el código de país obtenido. Si ya utiliza una extensión para administrar GeoIP, es posible que tenga que verificar las funciones con las opciones de Fastly.

**Para configurar la administración de GeoIp**:

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** y ampliar **Caché de página completa**.

1. En el _Configuración rápida_ sección, expandir **Configuración avanzada**.

1. Desplácese hacia abajo y seleccione **Sí** hasta **Habilitar GeoIP**. Se muestran las opciones de configuración adicionales.

1. Para Acción GeoIP, seleccione si el visitante se redirige automáticamente con **Redirigir** o ha proporcionado una lista de tiendas para seleccionar con **Diálogo**.

1. Para **Asignación de país**, seleccione **Añadir** para introducir un código de país de dos letras para asignarlo a un almacén de Adobe Commerce específico desde una lista.

   ![Agregar mapas de país de GeoIP](/help/assets/cdn/fastly-geo-code.png)

1. Clic **Guardar configuración** en la parte superior de la página.

1. Después de volver a cargar la página, seleccione **Cargar VCL a Fastly** en el _Configuración rápida_ sección.

>[!NOTE]
>
>La implementación actual del módulo Adobe Commerce Fastly GeoIP no admite redirecciones entre varios sitios web.

Fastly también proporciona una serie de [funciones de VCL relacionadas con la geolocalización](https://developer.fastly.com/reference/vcl/variables/geolocation/) para la codificación de geolocalización personalizada.

## Habilitar los módulos de Fastly Edge

Fastly Edge Modules es un marco flexible que permite definir componentes de interfaz de usuario y código VCL asociado a través de una plantilla. Estos módulos facilitan la personalización y ampliación de la configuración del servicio Fastly a través de la interfaz de usuario en lugar de utilizar fragmentos de VCL personalizados.

Los módulos Edge le permiten habilitar funcionalidades específicas como encabezados CORS, reescrituras de mapas de sitios en la nube y configurar la integración entre su tienda de Adobe Commerce y otros CMS o back-end.

Para acceder al menú Módulos de Edge y ver, configurar y administrar los módulos disponibles, active la opción _Habilitar los módulos de Fastly Edge_ opción. Consulte [Módulos de Fastly Edge](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md) en la documentación del módulo Fastly CDN.

## Configuración de back-ends y blindaje de origen

La configuración del back-end ofrece un ajuste preciso para un rendimiento rápido con blindaje de origen y tiempos de espera. A _back-end_ es una ubicación específica (IP o dominio) con el escudo de origen y los ajustes de tiempo de espera configurados para comprobar y proporcionar contenido almacenado en caché.

_Blindaje de origen_ enruta todas las solicitudes de su tienda a un punto de presencia (POP) específico. Cuando se recibe una solicitud, el POP comprueba el contenido almacenado en caché y lo proporciona. Si no se almacena en caché, continúa a la POP de escudo y, a continuación, al servidor de origen que almacena en caché el contenido. Los escudos reducen el tráfico directamente al origen.

El código VCL predeterminado de Fastly especifica los valores predeterminados para el blindaje de origen y los tiempos de espera para su Adobe Commerce en sitios de infraestructura en la nube. En algunos casos, es posible que tenga que modificar los valores predeterminados. Por ejemplo, si está obteniendo errores de Tiempo hasta el primer byte (TTFB), es posible que tenga que ajustar la variable _tiempo de espera del primer byte_ valor.

>[!NOTE]
>
>Si su sitio requiere una entrega funcional a través de una integración back-end como [Wordpress](fastly-vcl-wordpress.md), personalice la configuración del servicio Fastly para añadir el back-end y administrar las redirecciones de su tienda de Adobe Commerce a Wordpress. Para obtener más información, consulte [Módulos Fastly Edge: integración de otros CMS/back-end](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) en la documentación del módulo Fastly.

**Para revisar la configuración de la configuración del servidor**:

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** y ampliar **Caché de página completa**.

1. Expanda el **Configuración rápida** sección.

1. Expandir **Configuración del servidor** y seleccione el engranaje para comprobar el back-end predeterminado. Se abre un modal que muestra la configuración actual con opciones para cambiarla.

   ![Modificación del back end](../../assets/cdn/fastly-backend.png)

1. Seleccione el **Escudo** ubicación (o centro de datos).

   La configuración predeterminada de Fastly para el proyecto establece la ubicación más cercana a la región de Cloud Service. Si necesita cambiarla, seleccione una ubicación cerca de la ubicación predeterminada.

1. Modifique los valores de tiempo de espera (en microsegundos) para la conexión al escudo, el tiempo entre bytes y el tiempo para el primer byte. Se recomienda mantener la configuración de tiempo de espera predeterminada.

1. De forma opcional, seleccione para **Activar el backend y el Shield después de editar o guardar**.

1. Clic **Cargar** para guardar los cambios y cargarlos en los servidores de Fastly.

1. En el Administrador, seleccione **Guardar configuración**.

Para obtener más información, consulte la [Guía de configuración del servidor](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md) en la documentación del módulo Fastly.

## Autenticación básica

La autenticación básica es una función que protege todas las páginas y recursos del sitio con un nombre de usuario y una contraseña. Nosotros **no recomendar** activar la autenticación básica en el entorno de producción. Puede configurarlo en Ensayo para proteger el sitio durante el proceso de desarrollo. Consulte la [Guía de autenticación básica](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) en la documentación del módulo Fastly CDN.

Si agrega acceso de usuario y habilita la autenticación básica en Ensayo, aún puede acceder al administrador sin requerir credenciales adicionales.

## Creación de fragmentos de VCL personalizados

Fastly admite una versión personalizada del lenguaje de configuración de barniz (VCL) para personalizar la configuración del servicio Fastly. Por ejemplo, puede permitir, bloquear o redirigir el acceso para usuarios específicos o direcciones IP mediante bloques de código VCL con diccionarios Edge y Access Control List (ACL).

Para obtener instrucciones para crear fragmentos de VCL personalizados, diccionarios de Edge y ACL, consulte [Fragmentos de VCL personalizados de Fastly](fastly-vcl-custom-snippets.md).

>[!NOTE]
>
>Antes de agregar código VCL, diccionarios perimetrales y ACL personalizados a la configuración del módulo Fastly, compruebe que el servicio de almacenamiento en caché de Fastly funciona con la configuración predeterminada. Consulte [Configuración rápida](fastly-configuration.md).

## Administrar dominios

Para los proyectos Starter y Pro, puede utilizar el [!UICONTROL Domains] para añadir y gestionar la configuración de dominio de Fastly para su tienda.

- Para proyectos iniciales, vaya a la dirección URL del proyecto en la [!UICONTROL Domains] en la pestaña [!DNL Cloud Console] para añadir la URL del proyecto.

- Para proyectos Pro, envíe un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para agregar el dominio a la configuración del proyecto en la nube. El equipo de asistencia también actualiza la configuración de cuenta de Adobe Commerce Fastly para agregar el dominio.

**Para administrar la configuración de dominio de Fastly desde el administrador**:

{{admin-login-step}}

1. Seleccionar **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema** y ampliar **Caché de página completa**.

1. En el Administrador _Configuración rápida_ , seleccione **Domains**.

1. Clic **Administrar dominios** para abrir la página Dominios.

1. Añada los nombres de nivel superior y subdominio para las tiendas en el entorno de Cloud.

   Solo puede especificar dominios que ya se hayan agregado a la configuración de infraestructura de Cloud.

   ![Agregar la configuración de dominio de Fastly para Starter](../../assets/cdn/fastly-starter-activate-domain.png)

1. Clic **Activar** para actualizar la configuración de dominio de Fastly.

>[!NOTE]
>
>Si el mismo dominio se ha configurado en una cuenta diferente de Fastly, debe enviar un vale de soporte de Adobe Commerce para solicitar la delegación de dominio antes de agregar el dominio a Adobe Commerce. Consulte [Varias cuentas de Fastly y dominios asignados](fastly.md#multiple-fastly-accounts-and-assigned-domains).

## Activar modo de mantenimiento

Utilice el _Modo de mantenimiento_ opción para permitir el acceso administrativo al sitio desde direcciones IP especificadas, al mismo tiempo que se devuelve una página de error para todas las demás solicitudes.

**Para habilitar el modo de mantenimiento con acceso administrativo**:

1. Abra el _Configuración rápida_ de la sección Administración.

1. En el _Edge ACL_ , actualice la sección `maint_allow` acceder a la lista de control (ACL) con las direcciones IP administrativas que pueden acceder a su tienda mientras está en modo de mantenimiento.

   ![Actualizar lista de permitidos de modo de mantenimiento de IP](../../assets/cdn/fastly-maint-allowlist.png)

1. En el _Modo de mantenimiento_ , seleccione **Activar modo de mantenimiento**.

   Después de habilitar el modo de mantenimiento, se bloquea todo el tráfico excepto las solicitudes de las direcciones IP de `maint_allowlist` ACL. Puede actualizar el `maint_allowlist` para cambiar las direcciones IP en la ACL.

   Para obtener instrucciones de configuración detalladas, consulte la [Guía del modo de mantenimiento](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md) en la documentación del módulo Fastly CDN para Magento 2.
