---
title: Preparación para el desarrollo
description: Reúna las credenciales y conozca las herramientas disponibles para configurar un espacio de trabajo de desarrollo para utilizarlo con su proyecto de infraestructura de Commerce en la nube.
recommendations: noDisplay, catalog
exl-id: 8f88161f-3580-453b-b977-2c6e3824cc02
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Preparación para el desarrollo

Tanto si es su primera vez en Commerce como si es un propietario de Commerce existente que se traslada a la infraestructura en la nube, siga estos pasos para preparar un espacio de trabajo de desarrollo para su proyecto en la nube. Si ya ha completado algunos de estos pasos o ya tiene un entorno de desarrollo de Adobe Commerce, revise lo siguiente para ver los resultados esperados y continúe con el siguiente paso. Algunas configuraciones y flujos de trabajo difieren de una instalación local típica.

## Credenciales

Antes de configurar un espacio de trabajo, recopile las siguientes claves y acceso a la cuenta:

- **Claves de autenticación (claves de composición)**

  Las claves de autenticación son tokens de autenticación de 32 caracteres que proporcionan acceso seguro al repositorio del Compositor de Adobe Commerce (`repo.magento.com`) y cualquier otro servicio Git necesario para el desarrollo de aplicaciones, como GitHub. Su cuenta puede tener varias claves de autenticación. Para la configuración del espacio de trabajo, comience con una clave específica para el repositorio de código. Si no tiene ninguna clave, póngase en contacto con el propietario del proyecto o cree el [claves de autenticación](../cloud-guide/development/authentication-keys.md) usted mismo.

- **Cuenta de proyecto de Cloud**

  El propietario del proyecto debe invitarle al proyecto de infraestructura de Adobe Commerce en la nube. Cuando reciba la invitación por correo electrónico, haga clic en el vínculo y siga las indicaciones para crear su cuenta. Consulte [Incorporación](onboarding.md).

- **Clave de cifrado de Adobe Commerce**

  Al importar un sistema existente solamente, capture la clave de cifrado utilizada para proteger el acceso y los datos de la base de datos. Para obtener más información sobre esta clave, consulte [Resolver problemas con la clave de cifrado](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)

## Herramientas para desarrolladores

- **Instale la CLI de Cloud**

  Instale el `magento-cloud` CLI para que pueda administrar entornos en la nube y ejecutar tareas de automatización. Consulte [CLI de nube](../cloud-guide/dev-tools/cloud-cli-overview.md) para obtener instrucciones de instalación.

- **Instalación de Docker para el desarrollo y las pruebas locales**

  De forma opcional, utilice el entorno Docker para emular Commerce en la infraestructura en la nube `integration` entorno para el desarrollo local. Hay tres componentes esenciales: una plantilla de Adobe Commerce v2, Docker Compose y `ece-tools` paquete.

   - [Arquitectura de Docker y comandos comunes](../cloud-guide/dev-tools/cloud-docker.md)
   - [Entorno de desarrollo de Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [Paquete ECE-Tools](../cloud-guide/dev-tools/package-overview.md)

- **Integración de servicios basados en Git**

  Opcionalmente, integre un servicio de alojamiento basado en Git, como GitHub o GitLab, con Adobe Commerce en la infraestructura en la nube. Consulte [Integraciones](../cloud-guide/integrations/overview.md).

## Código de proyecto

Una conexión segura es esencial para interactuar con los entornos remotos. Para un nuevo proyecto, [inicie sesión en [!DNL Cloud Console]](https://console.adobecommerce.com) y haga clic en **[!UICONTROL No SSH key]**. Este icono se encuentra a la derecha del campo de comando y es visible cuando el proyecto no contiene una clave SSH. Consulte [Conexiones seguras](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account).

**Para clonar el código base en la estación de trabajo local**:

1. En el [[!DNL Cloud Console]](https://console.adobecommerce.com), haga clic en **[!UICONTROL code]** y seleccione la **[!UICONTROL Git]** pestaña.

   ![Clonar el código](../assets/ui-git-code.png){width="450"}

1. Copie el `git clone ...` comando proporcionado.

1. En un terminal, cree y cambie al directorio de trabajo.

1. Pegue y ejecute el `git clone ...` comando.

>[!TIP]
>
>El Adobe aprovisiona el entorno inicial del proyecto mediante un repositorio de plantillas que incluye instrucciones de paquete para una versión específica de Adobe Commerce. Revise la [estructura de archivos del proyecto](../cloud-guide/project/file-structure.md) y obtenga más información sobre los archivos de proyecto y las plantillas de nube importantes.
