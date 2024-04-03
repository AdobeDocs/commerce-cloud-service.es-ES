---
title: Ingesta de datos
description: Obtenga información sobre cómo ver y administrar la ingesta de datos de Commerce en New Relic.
feature: Cloud, Observability
exl-id: f88bf20c-604b-4986-b71c-bb726b2f00b8
source-git-commit: bf3debc5986d51a721537b52ffced58b2ee521ea
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Ingesta de datos

New Relic depende de los datos enriquecidos para proporcionar una monitorización y un análisis eficaces, pero los conjuntos de datos grandes pueden afectar a los resultados, el rendimiento y el cumplimiento normativo a tiempo. En este tema se proporcionan algunas directrices sobre la administración de la ingesta de datos y las estrategias para refinar los datos de modo que sean más eficaces.

New Relic proporciona un _Administración de datos_ vista que resume el uso del plan por origen de datos.

**Para ver los datos y las fuentes de ingesta**:

1. En el menú de usuario de New Relic, haga clic en **[!UICONTROL Manage your data]**.
1. Clic **[!UICONTROL Data management]** en el _Administration_ lista.

   ![Administración de datos](../../assets/new-relic/data-ingestion.png)

   El **[!UICONTROL Data ingestion]** La pestaña muestra los datos introducidos para el día y el origen de los datos.
La pestaña de retención de datos muestra y controla cuánto tiempo se almacenan los datos.

1. Seleccione el **[!UICONTROL Limits]** y ver los límites de su cuenta.

Las fuentes de datos de Adobe Commerce incluyen:

- **Eventos de APM**: datos de evento utilizados en gráficos y paneles
- **Infraestructura**: métricas de proceso y host, como CPU, almacenamiento, redes
- **Registro**: registros para CDN, APM y servidor de aplicaciones

Los datos de registro contribuyen a una gran parte de la ingesta. Consulte cómo [Visualización y análisis de datos de registro](log-management.md#view-and-analyze-log-data) y trabaje con su representante de Adobe para formar una estrategia para las necesidades de ingesta y retención de datos. Más información sobre [administración de ingesta de datos](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) en el _Documentación de New Relic_.
