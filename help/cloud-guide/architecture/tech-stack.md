---
title: Pila de tecnología
description: Consulte la pila de tecnología que forma la infraestructura de Commerce en Cloud.
feature: Cloud, Iaas, Paas
exl-id: e456db25-c44b-4053-b96d-517d3d1606d0
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Pila de tecnología

Considere Adobe Commerce en la infraestructura en la nube como cinco capas funcionales, como se muestra a continuación:

![Pila de nube](../../assets/CloudStack.svg)

1. [**Infraestructura en la nube**](pro-architecture.md): Elija Amazon Web Service (AWS) o Microsoft Azure como base de Infrastructure as a Service (IaaS) para sus proyectos de Adobe Commerce en la nube Pro.

   Adobe analiza de forma rutinaria el uso de recursos de computación virtual (vCPU) y asigna automáticamente recursos para optimizar el uso a largo plazo y mitigar el riesgo de superar la asignación de días de vCPU anual máxima. Si espera un aumento del tráfico del sitio para periodos de tiempo específicos, debe seguir abriendo un ticket de asistencia a [solicitar un aumento temporal](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html).

1. [**Platform as a Service**](cloud-architecture.md): Cada proyecto de infraestructura de Adobe Commerce en la nube proporciona un entorno de integración de Platform as a Service (PaaS) para desarrollar, probar e integrar servicios.
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce en la infraestructura en la nube proporciona una infraestructura aprovisionada previamente que incluye PHP, MySQL (MariaDB), Redis, [!DNL RabbitMQ]y tecnologías de motores de búsqueda compatibles.
1. [**Herramientas de rendimiento**](../monitor/new-relic-service.md): las herramientas de rendimiento de New Relic le permiten depurar, supervisar y administrar sus aplicaciones e infraestructura recopilando, analizando y mostrando datos de su Adobe Commerce en proyectos de infraestructura en la nube.
1. [**Red de distribución de contenido (CDN), cortafuegos de aplicaciones web ([!DNL WAF]) y Optimización de imágenes (IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection): proporciona servicios de CDN seguros con protección integrada frente a ataques de denegación de servicio (DDoS) distribuidos como [!DNL Ping of Death], [!DNL Smurf] ataques y otros ataques de inundación basados en el Protocolo de mensajes de control de Internet (ICMP, Internet Control Message Protocol ).
   * [Firewall de aplicaciones web (WAF)](../cdn/fastly-waf-service.md): los servicios WAF garantizan la compatibilidad con PCI para tiendas Adobe Commerce en entornos de producción y políticas WAF que protegen sus aplicaciones web Adobe Commerce de ataques de inyección, entradas malintencionadas, scripts entre sitios, exfiltración de datos, infracciones del protocolo HTTP y otros [[!DNL OWASP] Diez principales amenazas a la seguridad](https://owasp.org/www-project-top-ten/).
   * [Optimización de imágenes (IO)](../cdn/fastly-image-optimization.md): permite la manipulación y optimización de imágenes en tiempo real para acelerar la entrega de imágenes y simplificar el mantenimiento de los conjuntos de fuentes de imágenes para aplicaciones web adaptables. Fastly IO descarga el procesamiento de imágenes y el cambio de tamaño de la carga, lo que libera a los servidores para procesar pedidos y conversiones de forma eficiente.

Las aplicaciones monolíticas consumen muchos recursos y son difíciles de escalar y servir rápidamente. Con la infraestructura en la nube, los clientes de Commerce obtienen un acceso sin igual a los microservicios basados en SaaS que son ricos, inteligentes y con rendimiento. Consulte [Software y servicios compatibles](cloud-architecture.md#supported-software-and-services).

Utilice el [Guía de introducción de Commerce](../../get-started/overview.md) para configurar su nuevo programa en la nube y comenzar a administrar su [!DNL Commerce] aplicación en un entorno nativo de la nube.
