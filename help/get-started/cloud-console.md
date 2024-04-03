---
title: "Iniciar sesión en [!DNL Cloud Console]"
description: Obtenga información acerca de [!DNL Cloud Console] para Adobe Commerce en la infraestructura en la nube.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: c19a36b6-e5e8-461c-a82c-68b7bf121999
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Iniciar sesión en [!DNL Cloud Console]

El [!DNL Cloud Console] proporciona métodos interactivos para generar, administrar e implementar código de Commerce. El [!DNL Cloud Console] es una experiencia más moderna y fácil de usar, y sienta las bases para futuras mejoras en la interfaz.

[Inicie sesión en [!DNL Cloud Console]](https://console.adobecommerce.com) para ver la lista de proyectos.

![Lista de proyectos](../assets/ui-allprojects-list.png)

## Funciones

Las funciones nuevas o mejoradas incluyen:

- Información general clara sobre las características del proyecto y del entorno
- Flujo de actividad con historial ordenable
- Administración manual de copias de seguridad e historial para proyectos iniciales
- Vistas de registro mejoradas
- Listas ordenables
- Formularios sencillos y directrices para agregar integraciones
- Directrices de accesibilidad del contenido web (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

Las funciones nuevas o mejoradas son las siguientes:

| Función | Mejoras |
| -------------- | ----------------------------------- |
| [Flujo de actividad](../cloud-guide/project/activity-stream.md) | Interactúe con una lista ordenable de acciones en ejecución, pendientes o históricas. Seleccione una actividad y vea los registros o cancele una compilación en ejecución. |
| [Información general sobre proyectos y entornos](../cloud-guide/project/overview.md#project-overview) | Abra el proyecto y vea la descripción general de los detalles del proyecto y la lista de entornos. La descripción general del entorno proporciona más detalles sobre el estado del entorno, el acceso a la aplicación y las actividades recientes. |
| [Formularios de integración](../cloud-guide/integrations/overview.md) | Utilice formularios y directrices sencillos para agregar integraciones, como Bitbucket o notificaciones a Slack. |
| [Lista de proyectos](../cloud-guide/project/overview.md#cloud-console) | El _Todos los proyectos_ La vista enumera todos los proyectos para los que tiene permiso de acceso. Puede hacer clic en **[!UICONTROL Show filters]** y filtre la lista de proyectos por tipo, región o plan. |
| [Opciones de visibilidad variable](../cloud-guide/environment/variable-levels.md) | Limite la visibilidad de una variable de nivel de proyecto o de nivel de entorno durante la compilación o el tiempo de ejecución. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Preguntas de consola

**_¿Dónde puedo encontrar la función Instantáneas?_**?

Para [!DNL Starter] Proyectos, la función Instantáneas ahora se llama _Copias de seguridad_. Puede crear una copia de seguridad manual de su [!DNL Starter] entorno desde el [!DNL Cloud Console] o cree una instantánea desde la CLI de la nube. Debe tener una función de administrador para el entorno.

Seleccione un entorno de la barra de navegación del proyecto. El entorno debe estar activo. Seleccione el **[!UICONTROL Backups]** pestaña. Actualmente, esta opción no está disponible para entornos Pro.

**_Donde es la lista de rutas configuradas para el entorno_**?

Puede encontrar la lista de rutas configuradas en la _Servicios_ para un entorno.

Seleccione un entorno de la barra de navegación del proyecto. Seleccione el **[!UICONTROL Services]** pestaña. El **Enrutador** información general muestra las rutas configuradas. Actualmente, no se puede agregar una ruta desde el nuevo [!DNL Cloud Console].

## Menú Cuenta

En la esquina superior derecha se encuentra el menú de la cuenta. Haga clic en la flecha hacia abajo del menú y seleccione **[!UICONTROL My Profile]**. En el _Mi perfil_ , puede controlar los detalles del usuario y la configuración de visualización, administrar [autenticación de seguridad](../cloud-guide/project/user-access.md#user-authentication-requirements), [tokens de API](../cloud-guide/project/user-access.md#create-an-api-token), y [Claves SSH](../cloud-guide/development/secure-connections.md).
