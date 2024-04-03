---
title: Resumen de servicios rápidos
description: Descubra cómo los servicios de Fastly incluidos con Adobe Commerce en la infraestructura en la nube le ayudan a optimizar y asegurar las operaciones de entrega de contenido para sus sitios de Adobe Commerce.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: dc4500bf-f037-47f0-b7ec-5cd1291f73a1
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1392'
ht-degree: 0%

---

# Resumen de servicios rápidos

>[!WARNING]
>
>Para mantener la compatibilidad con PCI en los sitios de Adobe Commerce implementados en la plataforma en la nube, configure Fastly en los entornos de rama principal de inicio, producción de Pro y ensayo de Pro. Si utiliza Adobe Commerce en una implementación sin encabezado, le recomendamos encarecidamente que utilice Fastly para almacenar en caché las respuestas de GraphQL. Consulte [Almacenamiento en caché con Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) en el *Guía para desarrolladores de GraphQL*.

Proporciona rápidamente los siguientes servicios para optimizar y asegurar las operaciones de entrega de contenido para Adobe Commerce en proyectos de infraestructura en la nube. Estos servicios se incluyen con Adobe Commerce en la infraestructura en la nube sin coste adicional.

- **Red de entrega de contenido (CDN)**: servicio basado en barnices que almacena en caché las páginas del sitio, los recursos, CSS y mucho más en los centros de datos back-end que configure. A medida que los clientes acceden al sitio y a las tiendas, las solicitudes llegan rápidamente para cargar las páginas en caché más rápido. El servicio CDN proporciona las siguientes funciones:

- **Administración de caché**: almacene en caché las páginas del sitio, los activos, CSS y mucho más en los centros de datos back-end que configure para reducir la carga y los costes del ancho de banda

   - Uso [Fragmentos de VCL personalizados de Fastly](fastly-vcl-custom-snippets.md) (Compatible con Varnish 2.1) para modificar la forma en que el almacenamiento en caché responde a las solicitudes

   - Configuración de [Compatibilidad con el servicio GeoIP](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [Forzar solicitudes sin cifrar a TLS](fastly-custom-cache-configuration.md#force-tls)

   - [Personalizar Fastly timeout](fastly-custom-cache-configuration.md#extend-fastly-timeout) configuración para evitar respuestas 503 en solicitudes de operación masiva

   - Crear [páginas de respuesta de error personalizadas](fastly-custom-response.md)

- **Seguridad**: después de habilitar los servicios de Fastly para los sitios de Adobe Commerce, hay funciones de seguridad adicionales disponibles para proteger los sitios y la red:

   - [Cortafuegos de aplicación web](fastly-waf-service.md) (WAF): servicio de cortafuegos de aplicaciones web gestionadas que proporciona protección compatible con PCI para bloquear el tráfico malintencionado antes de que pueda dañar su Adobe Commerce de producción en sitios y redes de infraestructura en la nube. El servicio WAF solo está disponible en entornos de producción Pro y Starter.

   - [Protección contra denegación de servicio distribuido (DDoS)](#ddos-protection): protección DDoS integrada contra ataques comunes como Ping of Death, Pitufo y otros ataques de inundación basados en ICMP.

   - [Certificados SSL/TLS](fastly-configuration.md#provision-ssltls-certificates): el servicio Fastly requiere un certificado SSL/TLS para servir tráfico seguro a través de HTTPS.

     Adobe Commerce proporciona un certificado Let&#39;s Encrypt SSL/TLS validado por el dominio para cada entorno de ensayo y producción. Adobe Commerce completa la validación del dominio y el aprovisionamiento de certificados durante el proceso de configuración rápida.

- **Enmascaramiento de origen**: evita que el tráfico omita el Fastly WAF y oculta las direcciones IP de sus servidores de origen para protegerlos del acceso directo y los ataques DDoS.

  El encubrimiento de origen está habilitado de forma predeterminada en los proyectos de Adobe Commerce en la infraestructura en la nube Pro Production. Para habilitar el encubrimiento de origen en Adobe Commerce en proyectos de infraestructura en la nube Starter Production, envíe un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Si tiene tráfico que no requiere almacenamiento en caché, puede personalizar la configuración del servicio de Fastly para permitir solicitudes a [omitir la caché de Fastly](fastly-vcl-bypass-to-origin.md).

- **[Optimización de imagen](fastly-image-optimization.md)**: descarga el procesamiento de imágenes y el cambio de tamaño de la carga al servicio Fastly para que los servidores puedan procesar pedidos y conversiones de forma más eficaz.

- **[Registros de CDN y WAF de Fastly](../monitor/new-relic-service.md#new-relic-log-management)**: para los proyectos de Adobe Commerce en la nube Pro, puede utilizar el servicio Registros de New Relic para revisar y analizar los datos de registro de CDN y WAF de Fastly.

## Módulo de CDN de Fastly para Magento 2

Los servicios de Fastly para Adobe Commerce en infraestructura en la nube utilizan el [Módulo de CDN de Fastly para Magento 2] instalado en los entornos siguientes: Pro Staging and Production, Starter Production (`master` rama).

Al aprovisionar o actualizar inicialmente el proyecto de Adobe Commerce, Adobe instala la versión más reciente del módulo CDN de Fastly en los entornos de ensayo y producción. Cuando publica actualizaciones de módulos de Fastly, recibe notificaciones en el administrador para sus entornos. El Adobe recomienda actualizar los entornos para utilizar la última versión. Consulte [Actualizar rápidamente](fastly-configuration.md#upgrade-the-fastly-module).

## Cuenta de servicio y credenciales de Fastly

Adobe Los comercios en proyectos de infraestructura en la nube no requieren una cuenta exclusiva de Fastly o un propietario de cuenta. En su lugar, cada entorno de ensayo y producción tiene credenciales de Fastly únicas (token de API e ID de servicio) para configurar y administrar los servicios de Fastly desde el administrador. También necesita las credenciales para enviar solicitudes de API de Fastly.

Durante el aprovisionamiento del proyecto, Adobe agrega el proyecto a la cuenta de servicio de Fastly para Adobe Commerce en la infraestructura de la nube y agrega las credenciales de Fastly a la configuración de los entornos de ensayo y producción. Consulte [Obtener credenciales rápidamente](fastly-configuration.md#get-fastly-credentials).

### Cambiar el token de API de Fastly

Envíe un ticket de asistencia de Adobe Commerce para cambiar la credencial del token de la API de Fastly. Cuando reciba el nuevo token, actualice el entorno de ensayo o producción para utilizar el nuevo token.

**Para cambiar la credencial del token de la API de Fastly**:

1. [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) solicitar nuevas credenciales de la API de Fastly.

   Incluya el ID de proyecto de Adobe Commerce en la infraestructura de la nube y los entornos que requieren una nueva credencial.

1. Cuando reciba el nuevo token de API, actualice su valor en la variable [Configuración de credenciales de Fastly](fastly-configuration.md#test-the-fastly-credentials) en el Administrador o desde el [[!DNL Cloud Console] variables de entorno](../project/overview.md#configure-environment).

1. [Prueba de la nueva credencial](fastly-configuration.md#test-the-fastly-credentials).

1. Después de actualizar la credencial, envíe un ticket de asistencia de Adobe Commerce para eliminar el token de API antiguo.

### Varias cuentas de Fastly y dominios asignados

Solo permite asignar un dominio Apex y subdominios asociados a un servicio y una cuenta de Fastly. Si tiene una cuenta existente de Facebook que vincula los mismos Apex y subdominios utilizados para su sitio de Adobe Commerce, tiene las siguientes opciones:

- Elimine el Apex y los subdominios de la cuenta existente antes de solicitar las credenciales de servicio de Fastly para sus entornos de proyecto de infraestructura en la nube de Adobe Commerce. Consulte [Uso de dominios] en la documentación de Fastly.

  Utilice esta opción para vincular el dominio Apex y todos los subdominios a la cuenta de servicio de Fastly para Adobe Commerce en la infraestructura en la nube.

- Envíe un ticket de asistencia de Adobe Commerce para solicitar la delegación de dominios de modo que los Apex y los subdominios se puedan vincular a cuentas diferentes.

  Utilice esta opción si tiene un dominio Apex con varios subdominios para sitios de Adobe Commerce y que no sean de Adobe Commerce y desea vincular estos subdominios a distintas cuentas de Fastly.

#### Solicitar delegación de dominio

*Escenario 1:*

El dominio Apex (`testweb.com` y `www.testweb.com`) está vinculado a una cuenta existente de Facebook. Tiene un proyecto de Adobe Commerce en la nube configurado con los siguientes subdominios: `mcstaging.testweb.com` y `mcprod.testweb.com`. No desea mover el dominio Apex a la cuenta de servicio de Fastly para Adobe Commerce en la infraestructura de la nube.

Enviar una [ticket de asistencia rápida] solicitar que los subdominios se deleguen de la cuenta existente de Fastly a la cuenta de Fastly para Adobe Commerce en la infraestructura de nube. Incluya su ID de proyecto de Adobe Commerce en el ticket.

Una vez completada la delegación, los subdominios del proyecto se pueden agregar a la cuenta de servicio rápido para Adobe Commerce en la infraestructura en la nube. Consulte [Obtener credenciales rápidamente](fastly-configuration.md#get-fastly-credentials).

*Escenario 2:*

El dominio Apex (`testweb.com` y `www.testweb.com`) está vinculado a la cuenta del servicio Fastly de Adobe Commerce en la infraestructura en la nube. Desea administrar los servicios de Fastly para `service.testweb.com` y `product-updates.testweb.com` subdominios de una cuenta diferente de Facebook.

Envíe un ticket de asistencia de Adobe Commerce solicitando que los subdominios se deleguen de la cuenta del servicio Fastly de Adobe Commerce en la infraestructura en la nube a la cuenta de Fastly. Incluya el ID de servicio de la cuenta de Facebook en el ticket.

## Protección DDoS

La protección DDOS está integrada en el servicio Fastly CDN. Una vez que haya habilitado los servicios de Fastly para sus sitios de Adobe Commerce, Fastly filtra todo el tráfico web y de administrador para detectar y bloquear ataques potenciales.

- Para los ataques dirigidos a la capa 3 o 4, el servicio Fastly filtra el tráfico según el puerto y el protocolo, inspeccionando solo las solicitudes HTTP o HTTPS. ICMP, UDP y otros ataques iniciados en la red se pierden en el perímetro de la red. Esto incluye ataques de reflexión y amplificación, que utilizan servicios UDP como SSDP o NTP. Al proporcionar este nivel de protección, bloqueamos efectivamente múltiples ataques comunes como Ping of Death, Pitufo, y otras inundaciones basadas en ICMP.

  Administra rápidamente los ataques a nivel TCP en la capa de caché. Esta estrategia proporciona la escala y el contexto necesarios por cliente para hacer frente a un ataque de inundación SYN y sus muchas variantes, incluida la pila TCP, los ataques de recursos y los ataques TLS dentro de los sistemas de Fastly.

- Fastly también proporciona protección contra ataques de nivel 7. Si su tienda tiene problemas de rendimiento y sospecha de un ataque DDoS de nivel 7, envíe un ticket de asistencia de Adobe Commerce. El Adobe puede crear y aplicar reglas personalizadas al servicio Fastly para inspeccionar y filtrar solicitudes maliciosas en función del encabezado, la carga útil o una combinación de atributos que identifican el tráfico de ataque. Consulte [Comprobación de ataques DDoS] y [Cómo bloquear el tráfico malintencionado] en el *Centro de ayuda de Adobe Commerce*.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Comprobación de ataques DDoS]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Módulo de CDN de Fastly para Magento 2]: https://github.com/fastly/fastly-magento2

[ticket de asistencia rápida]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[Cómo bloquear el tráfico malintencionado]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[Uso de dominios]: https://docs.fastly.com/en/guides/working-with-domains
