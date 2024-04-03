---
title: Escalado automático
description: Descubra cómo se puede ampliar Adobe Commerce en la infraestructura en la nube para satisfacer las demandas de recursos.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 2ba49c55-d821-4934-965f-f35bd18ac95f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Escalado automático

El escalado automático añade o elimina automáticamente los recursos a la infraestructura de la nube para mantener un rendimiento óptimo y costes razonables. Actualmente, esta función solo está disponible para proyectos configurados con una [Arquitectura a escala](scaled-architecture.md).

## Nodos del servidor web

El [nivel web](scaled-architecture.md#web-tier) se adapta para dar cabida al aumento de las solicitudes de procesos y a los requisitos de tráfico más elevados. Actualmente, la función de escalado automático solo se escala horizontalmente añadiendo o eliminando nodos de servidor web.

Se produce un evento de escalado automático cuando el uso y el tráfico de la CPU alcanzan un umbral predefinido:

- **Nodos añadidos**: las CPU/núcleos en todos los nodos web activos tienen una capacidad del 75 % durante 1 minuto y el tráfico aumenta un 20 % durante 5 minutos consecutivos.
- **Nodos eliminados**: las CPU/núcleos en todos los nodos web activos se cargan al 60 % durante 20 minutos. Los nodos se eliminan en el orden en que se agregaron.

Los umbrales mínimo y máximo se determinan y establecen en función de los límites de recursos contratados de cada comerciante; esto reduce el riesgo de escalado infinito.

## Monitorización de umbrales con New Relic

Puede usar el complemento [servicio de New Relic](../monitor/new-relic-service.md) para monitorizar ciertos umbrales, como el recuento de hosts y el uso de CPU. Las siguientes consultas de New Relic utilizan una notación de variable para `cluster-id` solo para fines de ejemplo.

>[!TIP]
>
>Para obtener una referencia sobre la creación de consultas, consulte [Sintaxis, cláusulas y funciones de NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) en el _New Relic_ documentación.
>Utilice sus consultas para crear una [Panel de New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Recuento de hosts

La siguiente consulta de ejemplo de New Relic muestra el recuento de hosts dentro del entorno:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

En la siguiente captura de pantalla, **Hosts de APM vistos** hace referencia al número de hosts con transacciones registradas durante el período seleccionado.

![Recuento de hosts de New Relic](../../assets/new-relic/host-count.png)

### uso de CPU

La siguiente consulta de ejemplo de New Relic muestra el uso de CPU para nodos web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Uso de CPU de nodos web de New Relic](../../assets/new-relic/web-node-cpu-usage.png)

## Habilitar escalado automático

Para habilitar o deshabilitar el escalado automático para su proyecto de infraestructura de Adobe Commerce en la nube, [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Elija las siguientes razones en el ticket:

- **Motivo de contacto**: Solicitud de cambio de infraestructura
- **Motivo de contacto de infraestructura de Adobe Commerce**: Solicitud de cambio de otra infraestructura

>[!IMPORTANT]
>
>La función de escalado automático captura eventos no anticipados. Incluso si tiene habilitado el escalado automático, Adobe recomienda seguir usando [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) si espera un evento próximo.

### Prueba de carga

El Adobe permite el escalado automático en su proyecto de Cloud _puesta en escena_ clúster primero. Después de realizar y completar las pruebas de carga en el entorno, Adobe habilita el escalado automático en el clúster de producción. Para obtener instrucciones sobre las pruebas de carga, consulte [Pruebas de rendimiento](../launch/checklist.md#performance-testing).

### LISTA DE PERMITIDOS IP

Después de habilitar el escalado automático, el tráfico del nodo web saliente se origina desde las direcciones IP de los nodos de servicio. Si utiliza una lista de permitidos con un servicio de terceros que no está empaquetado con su proyecto de Adobe Commerce en la nube, compruebe las direcciones IP en la lista de permitidos de servicio de terceros.

Por ejemplo:

- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3), no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP de los nodos de servicio (1, 2 y 3) y los nodos web (4, 5 y 6), en este caso los seis nodos, no se requiere ninguna acción.
- Si la lista de permitidos contiene las direcciones IP _solamente_ para los nodos web (4, 5 y 6), debe actualizar la lista de permitidos para incluir las direcciones IP de los nodos de servicio.
