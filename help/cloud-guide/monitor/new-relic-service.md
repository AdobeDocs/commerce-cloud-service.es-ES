---
title: servicio de New Relic
description: Obtenga información acerca del servicio New Relic disponible con su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 613f0694-5338-4037-8ee4-ac5eca376159
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Información general del servicio New Relic

Todos los proyectos de Adobe Commerce en la infraestructura en la nube incluyen acceso al servicio New Relic para ayudar a monitorizar el rendimiento e investigar los eventos del [!DNL Commerce] aplicación e infraestructura en la nube.

Las siguientes funciones de New Relic están disponibles para su uso con los entornos de producción y ensayo:

- [APM NEW RELIC](#new-relic-apm) (Pro y Starter)
- [Infraestructura de New Relic](#new-relic-infrastructure) (Solo Pro)
- [New Relic Log Management](#new-relic-logs) (Solo Pro)

>[!INFO]
>
>Otras funciones de New Relic no están disponibles en proyectos de Adobe Commerce.

## APM NEW RELIC

[New Relic para la administración del rendimiento de las aplicaciones (APM)](https://docs.newrelic.com/introduction-apm/) es un producto de análisis de software que le ayuda a analizar y mejorar las interacciones entre aplicaciones. New Relic APM está disponible para todos los proyectos de infraestructura en la nube de Adobe Commerce y proporciona las siguientes funciones:

- **Enfoque en transacciones específicas**: marque y supervise de forma activa las acciones clave del cliente en su sitio, como añadir al carro de compras, extraer o procesar un pago.
- **Supervisión de consultas de base de datos**: permite localizar y supervisar las consultas de base de datos que afectan al rendimiento.
- **Mapa de aplicación**: permite ver todas las dependencias de la aplicación dentro del sitio, las extensiones y los servicios externos.
- **[!DNL Apdex]puntuaciones**: evalúe el rendimiento y cree alertas que identifiquen problemas y le notifiquen cuándo se producen, como el rendimiento del sitio afectado por una venta flash o un evento web. Consulte [Puntuación Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Alertas administradas para Adobe Commerce**: utilice esta directiva de alertas de New Relic para supervisar el rendimiento de las aplicaciones y de la infraestructura en función de las prácticas recomendadas del sector. Consulte [Supervisar el rendimiento con la directiva Alertas administradas para alertas de Adobe Commerce](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Seguimiento de implementaciones**: supervise los eventos de implementación y analice el impacto de la implementación en el rendimiento general. Consulte [Seguimiento de implementaciones](track-deployments.md).

Su proyecto de infraestructura de Adobe Commerce en la nube incluye el software para el servicio APM de New Relic junto con una clave de licencia. No es necesario adquirir ni instalar ningún software adicional.

## Infraestructura de New Relic

Los proyectos profesionales incluyen [Infraestructura de New Relic (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) , que se conecta automáticamente con los datos de la aplicación y el análisis de rendimiento para proporcionar una monitorización dinámica del servidor. Este servicio está disponible en entornos de ensayo y producción profesional.

## New Relic Log Management

Todos los proyectos de infraestructura en la nube incluyen [Administración de registros de New Relic](log-management.md). El servicio está preconfigurado para agregar todos los datos de registro de los entornos de ensayo y producción y mostrarlos en un panel de administración de registros centralizado.
