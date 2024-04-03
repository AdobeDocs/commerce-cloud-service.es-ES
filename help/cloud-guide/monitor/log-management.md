---
title: Administración de registros de New Relic
description: Aprenda a utilizar el registro de New Relic
feature: Cloud, Logs, Observability
exl-id: d8afb5c0-9727-4123-8944-81cd6ad7cbb7
source-git-commit: ebe1746ee9fd08480da5ad07d7f1f8299ad9af7e
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Administración de registros de New Relic

Todos los proyectos de infraestructura en la nube incluyen [Administración de registros de New Relic](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). El servicio está preconfigurado para agregar todos los datos de registro de los entornos de ensayo y producción y mostrarlos en un panel de administración de registros centralizado.

Los datos agregados incluyen información de los siguientes registros:

- Todo `ece-tools` y registros de aplicación de `~/var/log` directorio
- Registros para servicios en la nube de `var/log/platform/<project-ID>` directorio
- Fastly CDN y WAF

Cuando el proyecto esté conectado a New Relic, puede utilizar el servicio Registros de New Relic para completar tareas como las siguientes:

- Usar consultas de New Relic para buscar datos de registro agregados
- Visualizar datos de registro mediante la aplicación New Relic Logs
- Creación de gráficos, tableros y alertas personalizados
- Solucionar problemas de rendimiento desde un solo panel

## Visualización y análisis de datos de registro

Utilice la aplicación New Relic Logs para buscar en los datos de registro agregados y solucionar errores de aplicación, infraestructura, CDN y WAF. Puede crear gráficos, tableros y alertas mediante los datos de registro recopilados de los servicios de infraestructura y APM de New Relic.

**Para utilizar la aplicación New Relic Logs**:

1. Inicie sesión en su [cuenta de New Relic](https://login.newrelic.com/login).

1. Seleccionar **Registros** en el menú de navegación del Explorador.

1. Compruebe que su Cuenta está seleccionada en la parte superior de la _Todos los registros_ vista.

1. Seleccione un intervalo de tiempo para la consulta Registros.

1. Para revisar los datos de registro de la infraestructura para los servicios en la nube (registros de `~/var/log/`), introduzca la cadena de consulta `has: "filePath"` en el _Buscar registros_ field. A continuación, haga clic en **[!UICONTROL Query logs]**.

   Los nombres de los archivos de registro se almacenan en `filePath` con rutas completas al archivo de registro.

   ![Datos de registro del servicio New Relic del proyecto en la nube](../../assets/new-relic/var-log-query.png)

1. Para revisar los datos de registro rápido, introduzca la cadena de consulta `has: "client_ip"` en el _Buscar registros_ field. A continuación, haga clic en **[!UICONTROL Query logs]**.

1. Para filtrar los resultados de registro rápido por código de país, haga clic en **[!UICONTROL Add column]**, luego seleccione **[!UICONTROL geo_country_code]**.

   ![Filtro de atributo de registro de CDN de New Relic del proyecto en la nube](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>Puede guardar la vista de consulta desde el _Vistas guardadas_ desplegable. Clic **[!UICONTROL Create new]**, proporcione un nombre, seleccione opciones y haga clic en **[!UICONTROL Save view]**.
>
>Consulte [Introducción a la administración de registros](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) y [Introducción al lenguaje de consulta New Relic](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) en el _Documentos de New Relic_ sitio.
