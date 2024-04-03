---
title: '[!DNL Onboarding] a Commerce'
description: Acceda a su cuenta de la nube y configure un proyecto de Adobe Commerce en la nube.
role: Admin
recommendations: noDisplay, catalog
exl-id: c6b768d7-d835-4a8d-aad9-1c0324f7570d
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---

# [!DNL Onboarding] a Commerce

Después de que el Adobe active una suscripción de Commerce on cloud Infrastructure, el acceso inicial al proyecto y al código solo estará disponible para la persona designada como Propietario de la licencia (propietario de la cuenta).

El propietario de la licencia es la persona de su empresa u organización financiera que administra pagos y otras transacciones comerciales para la cuenta de Adobe Commerce en la infraestructura de la nube. Esta persona sirve como punto de contacto con el Adobe. Si debe cambiar el propietario de la licencia en su cuenta, debe ponerse en contacto con el equipo de la cuenta de Adobe.

Para incorporar rápidamente el proyecto, de modo que pueda empezar a desarrollar el sitio para su implementación en directo, debe completar la configuración y los pasos necesarios [!DNL onboarding] tareas. Normalmente, el propietario de la licencia inicia el proceso asegurando el acceso del administrador y creando usuarios de administrador técnico que puedan ayudarle con el trabajo de configuración, personalización y desarrollo.

## Regístrese para obtener una cuenta de Cloud

Si no tiene una cuenta de Adobe Commerce en la infraestructura de la nube, póngase en contacto con [Ventas]. Al registrarse, Adobe crea su cuenta y le envía un correo electrónico de bienvenida con instrucciones sobre cómo acceder a la interfaz del proyecto. El correo electrónico contiene un vínculo para que pueda iniciar sesión en su cuenta y completar la configuración inicial del proyecto.

### Nube [!DNL Onboarding] IU

La página Proyecto de Adobe Commerce en la infraestructura en la nube en ([!DNL Onboarding] (IU) proporciona una lista de comprobación de introducción para configurar el proyecto y los servicios, determinar el acceso y comenzar con el desarrollo. Desde la interfaz de usuario de OBUI, puede:

- Añada un administrador técnico, un superusuario que pueda administrar el proyecto y las ramas
- Acceda al entorno del proyecto, incluido un vínculo al [!DNL Cloud Console]
- Complete una lista de comprobación de la prueba de aceptación rápida del usuario (UAT) con vínculos a pruebas adicionales

**Para abrir la página del proyecto**:

1. Inicie sesión en su [cuenta de cliente de Adobe Commerce](https://account.magento.com/customer/account/login).

1. En el _Mi cuenta_ , haga clic en **[!UICONTROL Commerce]** para ver los proyectos de su cuenta.

1. Clic **Ver página del proyecto** en el [Sección Proyectos](https://cloud.magento.com/cloud/project/).

1. Haga clic en el nombre del proyecto y abra la página Proyecto en la nube ([!DNL Onboarding] UI).

   ![Página del proyecto OBUI](../assets/onboarding-ui.png)

   Examine el portal para obtener información útil y opciones para empezar a planificar el proyecto, desarrollar código y prepararse para UAT y el lanzamiento del sitio.

## Acceso al proyecto y adición de usuarios

El propietario de la licencia puede añadir cuentas de usuario para proporcionar acceso al código, administrar ramas, introducir tickets y entornos de asistencia. Estas cuentas de usuario pueden incluir desarrollo interno, consultores y especialistas en soluciones.

Normalmente, el único usuario que el propietario de la licencia debe crear es el _Administrador técnico_. El administrador técnico necesita una cuenta de usuario con acceso de administrador para crear cuentas de usuario para desarrolladores, definir permisos de entorno y administrar todas las ramas y entornos. El administrador técnico puede ser un desarrollador, un consultor y un [partner de soluciones de Adobe](https://business.adobe.com/products/magento/partners.html), o usted mismo.

Puede crear un administrador técnico a través del portal del proyecto, desde el [!DNL Cloud Console], o desde la línea de comandos utilizando `magento-cloud` CLI.

### Registro de usuario

Solo puede agregar usuarios registrados a su Adobe Commerce en proyectos y entornos de infraestructura en la nube. Si tiene un usuario nuevo, pídale que lo haga [registrarse para obtener una cuenta](https://account.magento.com/customer/account/login/) y para proporcionar la dirección de correo electrónico asociada a su perfil de cuenta.

### Acceso a cuenta compartida

El propietario de la licencia puede configurar el acceso compartido para la cuenta. El acceso compartido permite a los empleados y proveedores de servicios de confianza utilizar el Centro de ayuda para enviar y rastrear vales de soporte relacionados con su Adobe Commerce en proyectos de infraestructura en la nube. Para obtener instrucciones de configuración, consulte la [Acceso compartido] artículo en el Centro de ayuda.

### [!DNL Cloud Console]

Puede usar el complemento [[!DNL Cloud Console]](cloud-console.md) para administrar el proyecto, agregar cuentas de usuario y comenzar a desarrollar la tienda. El propietario de la licencia, los usuarios administradores técnicos y los desarrolladores pueden utilizar el [!DNL Cloud Console] para administrar todos los entornos y ramas, variables de entorno, configuraciones de entorno y rutas.

**Para acceder a[!DNL Cloud Console]**:

1. Iniciar sesión en [Mi cuenta](https://account.magento.com/customer/account/login).

1. En el _Mi cuenta_ , haga clic en **[!UICONTROL Commerce]** para ver los proyectos de su cuenta.

1. Haga clic en **Proyectos** y seleccione un proyecto.

1. Clic **Acceso a infraestructura** y haga clic en **Acceso a proyectos (IU web)**.

   ![Portal de proyecto en nube](../assets/obui-project-access.png)

## Suscribirse al estado del Adobe

Obtenga actualizaciones acerca de Adobe Commerce en entornos de plataformas de infraestructura en la nube y servicios relacionados desde [Página de estado].

La página proporciona un estado para los componentes y servicios de Adobe Commerce seguido de notificaciones sobre informes de incidentes, actualizaciones de servicios, interrupciones planificadas y mantenimiento programado. Cualquier persona que trabaje en el proyecto puede suscribirse al sitio de estado de Adobe Commerce para recibir notificaciones de eventos y actualizaciones por correo electrónico o por Slack. Puede personalizar la suscripción del estado del Adobe para realizar un seguimiento de productos específicos por regiones y eventos.

>[!TIP]
>
> Abra el nuevo [!DNL Cloud Console] y vea las actividades de proyecto y entorno.
>
>**Siguiente paso**: [Iniciar sesión en la consola Cl[!DNL ]Cloud](cloud-console.md)

<!-- link definitions -->

[Ventas]: https://business.adobe.com/products/magento/get-demo.html
[Acceso compartido]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#shared-access
[Página de estado]: https://status.adobe.com/products/503473
