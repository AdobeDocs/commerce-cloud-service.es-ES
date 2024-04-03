---
title: Monitorización de New Relic
description: Obtenga información sobre cómo acceder a su tablero de New Relic y analizar los datos de su proyecto de Adobe Commerce en la nube.
feature: Cloud, Observability
topic: Performance
exl-id: 8d1e2ad6-14d0-4754-9703-960d93e135a9
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Monitorización de New Relic

New Relic conecta y supervisa su infraestructura y [!DNL Commerce] aplicación que utiliza agentes PHP. Una vez que un entorno de Cloud se conecte a New Relic, puede iniciar sesión en su cuenta de New Relic para revisar los datos recopilados por el agente.

En el _APM y servicios_ , seleccione la **Resumen** para ver información transaccional sobre su aplicación. Esta vista le ayuda a identificar posibles errores y a comprobar el estado general de su aplicación y servicios.

![Página de información general de New Relic del proyecto en la nube](../../assets/new-relic/dashboard.png)

Desde esta vista, puede rastrear transacciones que encuentran respuestas lentas o cuellos de botella, rendimiento de la aplicación, errores web y más.

Revisar datos rastreados:

- **Más tiempo**: permite determinar el consumo de tiempo mediante el seguimiento de solicitudes en paralelo. Por ejemplo, es posible que tenga el tiempo de transacción empleado más alto en las vistas de productos y categorías. Si una página de cuenta de cliente clasifica repentinamente el consumo de tiempo en un nivel superior, la aplicación podría verse afectada por el rendimiento de llamada o de arrastre de consultas.

- **Máximo rendimiento**: identifique las páginas que más se visiten en función del tamaño y la frecuencia de los bytes transmitidos.

Todos los datos recopilados detallan el tiempo empleado en las acciones que transmiten datos, consultas o _Redis_ datos. Si las consultas causan problemas, New Relic proporciona información para realizar un seguimiento de estos problemas y responder a ellos.

>[!TIP]
>
>Para obtener más información sobre el uso de estos datos para solucionar problemas de rendimiento de la aplicación, consulte [Solución de problemas de rendimiento con New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html) en el _Centro de ayuda de Adobe Commerce_.

## Monitorización del rendimiento con alertas administradas

El Adobe proporciona el _Alertas administradas para Adobe Commerce_ directiva de alertas para realizar un seguimiento de las métricas de rendimiento. La directiva incluye una colección de alertas que establecen umbrales y advertencias de déclencheur, así como notificaciones críticas cuando los problemas de infraestructura o de aplicaciones afectan al rendimiento del sitio. La directiva realiza el seguimiento de las siguientes métricas en los entornos de producción:

| Métrica | Recopilación de datos | Disponibilidad |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] puntuación | APM | Pro y Starter |
| uso de CPU | NRI | Pro |
| Espacio en disco | NRI | Pro |
| Tasa de error | APM | Pro y Starter |
| Uso de memoria | NRI | Pro |
| Carga de consulta de MariaDB | NRI | Pro |
| Memoria Redis | NRI | Pro |

Cuando la infraestructura del sitio o las condiciones de la aplicación alcanzan un umbral de déclencheur, New Relic envía notificaciones de alerta para que pueda solucionar el problema de forma proactiva. Consulte [Alertas administradas para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) en el _Centro de ayuda de Adobe Commerce_ para obtener más información sobre los umbrales de alerta y los pasos de solución de problemas para resolver los problemas que activaron la alerta.

>[!TIP]
>
>Para los entornos de ensayo e integración de Pro y los entornos de inicio, utilice [Notificaciones de estado](../integrations/health-notifications.md) para supervisar el espacio en disco.

>[!PREREQUISITES]
>
>- **Credenciales de New Relic**: credenciales para iniciar sesión en la cuenta de New Relic para su proyecto de Cloud
>- **Integración activa con New Relic**: compruebe que su entorno de nube está conectado a New Relic.
>- **Workflow notification**: permite configurar al menos uno [workflow](#set-up-a-workflow-for-notifications) para recibir las notificaciones de alerta

**Para revisar la directiva Alertas administradas para Adobe Commerce**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Busque el _Alertas administradas para Adobe Commerce_ directiva:

   - En el menú de navegación del Explorador, haga clic en **[!UICONTROL Alerts & AI]**.

   - En _Detectar_, haga clic en **[!UICONTROL Alert Conditions & Policies]**.

   - Compruebe que su Cuenta está seleccionada en la parte superior de la _Condiciones y políticas de alerta_ vista.

   - En el _Política_ , seleccione **Alertas administradas para Adobe Commerce** directiva.

     ![Políticas de alerta generadas](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Si la variable _Alertas administradas para Adobe Commerce_ La directiva no está disponible. Consulte [Alertas administradas para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) en el _Centro de ayuda de Adobe Commerce_.

1. Haga clic en **[!UICONTROL Alert conditions]** para revisar las condiciones de alerta definidas en la directiva.

## Crear directivas de alerta

No modifique ninguna alerta incluida en la directiva Alertas administradas para Adobe Commerce. La Adobe actualiza y mejora las condiciones de alerta de esta directiva con el tiempo, lo que sobrescribe las personalizaciones que agregue a la directiva.

En lugar de modificar una alerta existente, puede crear una directiva de alertas. A continuación, copie las condiciones de alerta en la nueva directiva. Consulte [Actualizar directivas o condiciones](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/alert-policies/update-or-disable-policies-conditions/) en el _New Relic_ documentación.

>[!TIP]
>
>Consulte [Introducción a las alertas](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/learn-alerts/alerts-concepts-workflow/) en el _New Relic_ para obtener información más detallada sobre Alertas, políticas de alerta y flujos de trabajo.

## Configuración de un flujo de trabajo para notificaciones

Ahora puede configurar una _workflow_, anteriormente denominado canal de notificación, para recibir notificaciones sobre el rendimiento del sitio en función de los datos filtrados, como una directiva de alertas. Las notificaciones sobre problemas de rendimiento se dirigen a todos los flujos de trabajo asociados a una directiva de alerta cuando las condiciones de la aplicación o infraestructura generan una déclencheur. También recibe notificaciones cuando se reconoce y se cierra un problema.

New Relic proporciona plantillas para configurar diferentes tipos de notificaciones de flujo de trabajo, como correo electrónico, Slack, PagerDuty, webhooks y mucho más.

**Para configurar un flujo de trabajo**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Cree un flujo de trabajo.

   - En el menú de navegación del Explorador, haga clic en **[!UICONTROL Alerts & AI]**.

   - En el panel de navegación izquierdo, debajo de _Enriquecimiento y notificación_, haga clic en **[!UICONTROL Workflows]**.

   - Clic **[!UICONTROL Add a workflow]** en el lado derecho.

     ![New Relic añade un flujo de trabajo](../../assets/new-relic/add-a-workflow.png)

   - En el _Configuración del flujo de trabajo_ , introduzca un nombre para el flujo de trabajo.

   - En el _Filtrado de datos_ , seleccione **[!UICONTROL Managed Alerts for Adobe Commerce]** desde el **[!UICONTROL Policy]** lista desplegable.

   - En el _Notificar_ , seleccione un canal y siga las instrucciones.

   - Clic **[!UICONTROL Test workflow]** para comprobar la configuración.

1. Clic **[!UICONTROL Activate workflow]**.

Consulte la documentación de New Relic sobre [Flujos de trabajo](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Las alertas de la directiva Alertas administradas para Adobe Commerce tienen flujos de trabajo predeterminados configurados para notificar a los equipos de Adobe que admiten Adobe Commerce en clientes de infraestructura en la nube. No modifique la configuración de estos canales predeterminados y no elimine ninguna directiva de alerta asignada a ellos.
