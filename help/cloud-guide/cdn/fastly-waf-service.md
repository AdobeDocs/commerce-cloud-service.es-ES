---
title: Firewall de aplicaciones web (WAF)
description: Descubra cómo el servicio Fastly WAF detecta, registra y bloquea el tráfico de solicitudes maliciosas antes de que pueda dañar la red o los sitios de Adobe Commerce.
feature: Cloud, Configuration, Security
exl-id: 40bfe983-7f32-4155-ae77-7cd18866f6e2
source-git-commit: 6b01bf91c3bf63ba268d0f8e10e19db4cb355997
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# Firewall de aplicaciones web (WAF)

Con la tecnología Fastly, el servicio de cortafuegos de aplicaciones web (WAF) para Adobe Commerce en la infraestructura en la nube detecta, registra y bloquea el tráfico de solicitudes malintencionadas antes de que pueda dañar sus sitios o red. El servicio WAF solo está disponible en entornos de producción.

El servicio WAF ofrece las siguientes ventajas:

- **Conformidad con PCI**: la habilitación de WAF garantiza que las tiendas de Adobe Commerce en entornos de producción cumplan los requisitos de seguridad de PCI DSS 6.6.
- **Directiva WAF predeterminada**: la política WAF predeterminada, configurada y mantenida por Fastly, proporciona una colección de reglas de seguridad adaptadas para proteger las aplicaciones web de Adobe Commerce de una amplia gama de ataques, incluidos ataques de inyección, entradas malintencionadas, scripts entre sitios, exfiltración de datos, violaciones del protocolo HTTP y otros [Diez principales de OWASP](https://owasp.org/www-project-top-ten/) amenazas a la seguridad.
- **Incorporación y habilitación de WAF**: el Adobe implementa y habilita la directiva WAF predeterminada en el entorno de producción en un plazo de 2 a 3 semanas después de que el aprovisionamiento sea final.
- **Soporte de operaciones y mantenimiento**—
   - Adobe y configure y administre rápidamente sus registros y alertas para el servicio WAF.
   - El Adobe clasifica los tickets de asistencia al cliente relacionados con problemas del servicio WAF que bloquean el tráfico legítimo como problemas de prioridad 1.
   - Las actualizaciones automatizadas a la versión del servicio WAF garantizan una cobertura inmediata para vulnerabilidades nuevas o en evolución. Consulte [Mantenimiento y actualizaciones de WAF](#waf-maintenance-and-updates).

>[!TIP]
>
>Para obtener información adicional sobre cómo mantener la conformidad con PCI para su Adobe Commerce en los almacenes de infraestructura en la nube, consulte [Conformidad con PCI](https://business.adobe.com/products/magento/pci-compliance.html).

## Activación del WAF

El Adobe habilita el servicio WAF en nuevas cuentas en un plazo de 2 a 3 semanas después de que el aprovisionamiento sea definitivo. El WAF se implementa mediante el servicio Fastly de CDN. No tiene que instalar ni mantener ningún hardware o software.

>[!NOTE]
>
>Antes de poder utilizar el servicio WAF, debe enviar todo el tráfico externo a su proyecto de infraestructura de Adobe Commerce en la nube debe enrutarse a través del servicio Fastly. Consulte [Configuración rápida](fastly-configuration.md).

## Cómo funciona

El servicio WAF se integra con Fastly y utiliza la lógica de caché dentro del servicio CDN de Fastly para filtrar el tráfico en los nodos globales de Fastly. Habilitamos el servicio WAF en su entorno de producción con una política WAF predeterminada basada en [Reglas de ModSecurity de Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) y las diez principales amenazas a la seguridad de OWASP.

El servicio WAF filtra el tráfico HTTP y HTTPS (solicitudes de GET y POST) contra el conjunto de reglas WAF y bloquea el tráfico que es malicioso o que no cumple con reglas específicas. El servicio filtra únicamente el tráfico enlazado al origen que intenta actualizar la caché. Como resultado, detenemos la mayoría del tráfico de ataques en la caché de Fastly, lo que protege el tráfico de origen de ataques maliciosos. Al procesar únicamente el tráfico de origen, el servicio WAF preserva el rendimiento de la caché e introduce solo una latencia estimada de 1,5 a 20 milisegundos en cada solicitud no almacenada en caché.

## Solución de problemas de solicitudes bloqueadas

Cuando el servicio WAF está habilitado, filtra todo el tráfico web y de administración en relación con las reglas WAF y bloquea cualquier solicitud web que déclencheur una regla. Cuando se bloquea una solicitud, el solicitante ve un valor predeterminado `403 Forbidden` página de error que incluye un ID de referencia para el evento de bloqueo.

![Página de error WAF](../../assets/cdn/fastly-waf-403-error.png)

Puede personalizar esta página de respuesta de error desde el Administrador. Consulte [Personalización de la página de respuesta WAF](fastly-custom-response.md#customize-the-waf-error-page).

Si la página de administración o tienda de Adobe Commerce devuelve un `403 Forbidden` página de error en respuesta a una solicitud de URL legítima, envíe un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Copie el ID de referencia de la página de respuesta de error y péguelo en la descripción del ticket.

## Mantenimiento y actualizaciones de WAF

Fastly mantiene y actualiza el conjunto de reglas de WAF en función de las actualizaciones de reglas de terceros comerciales, investigación de Fastly y fuentes abiertas. Actualiza rápidamente las reglas publicadas en una directiva según sea necesario o cuando los cambios en las reglas están disponibles en sus respectivas fuentes. Además, Fastly puede agregar reglas que coincidan con las clases de reglas publicadas en la instancia WAF de cualquier servicio después de habilitar el servicio WAF. Estas actualizaciones garantizan una cobertura inmediata para las vulnerabilidades nuevas o en evolución.

Adobe y administre rápidamente el proceso de actualización para garantizar que las reglas WAF nuevas o modificadas funcionen correctamente en el entorno de producción antes de que las actualizaciones se implementen en modo de bloqueo.

## Limitaciones

El servicio WAF estándar con tecnología de Fastly no admite las siguientes funciones:

- Protección contra malware o mitigación de bots: Considere la posibilidad de utilizar [listas de control de acceso](./fastly-vcl-allowlist.md) o un servicio de terceros.
- Limitación de velocidad: consulte [Limitación de velocidad](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) en la documentación de Fastly o consulte [Limitación de velocidad](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) en el _API web de Commerce_ sección de seguridad.
- Configuración de un punto final de registro para el cliente: consulte [Servicio PrivateLink](../development/privatelink-service.md) como alternativa.

Aunque el servicio WAF no permite bloquear ni permitir tráfico basado en direcciones IP, puede agregar listas de control de acceso (ACL) y fragmentos de VCL personalizados al servicio Fastly para especificar las direcciones IP y la lógica VCL para bloquear o permitir tráfico. Consulte [Fragmentos de VCL personalizados de Fastly](fastly-vcl-custom-snippets.md).

El servicio WAF no admite el filtrado para solicitudes TCP, UDP o ICMP. Sin embargo, esta funcionalidad la proporciona la protección DDoS integrada incluida con el servicio Fastly CDN. Consulte [Protección DDoS](fastly.md#ddos-protection).
