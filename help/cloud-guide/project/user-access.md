---
title: Administrar el acceso de usuario
description: Obtenga información sobre cómo administrar el acceso de los usuarios a Adobe Commerce en proyectos y entornos de infraestructura en la nube.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 3357a3ea-bf86-4a65-95d1-6b24f1152248
source-git-commit: b85a163ff62f8a63430dff7c96b5cf391cf38d79
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 0%

---

# Administrar el acceso de usuario

Los proyectos de Adobe Commerce en infraestructura en la nube utilizan el acceso basado en roles. Hay dos funciones disponibles en el nivel de proyecto:

- **Administrador de proyecto**: acceso de escritura a todos los entornos de proyecto y puede administrar usuarios, insertar código y actualizar la configuración del proyecto.
- **Visualizador de proyectos**: acceso de solo vista a todos los entornos de proyecto.

Los visualizadores de proyectos no pueden realizar tareas en ningún entorno; sin embargo, puede conceder a los visualizadores de proyectos acceso de escritura a un tipo de entorno específico.

El acceso a nivel de entorno se basa en el tipo de entorno: producción, ensayo y desarrollo. Concesión de un usuario _visualizador_ permiso para _desarrollo_ entornos significa que pueden ver **todo** entornos de desarrollo en el proyecto. La siguiente tabla aclara las capacidades concedidas a cada nivel de permiso:

| Nivel de permisos | Acceso | Acceso SSH |
| ------------------ | ----------- | :----------: |
| **Administrador** | Realizar tareas de administrador, como cambiar la configuración, insertar código, realizar tareas y administrar ramas, incluida la combinación con el entorno principal | Sí |
| **Colaborador** | Código push y bifurcación del entorno; no se puede cambiar la configuración ni ejecutar acciones | Sí |
| **Visor** | Acceso de solo vista al tipo de entorno | No |
| **Sin acceso** | Sin acceso al tipo de entorno | No |

{style="table-layout:auto"}

Puede agregar usuarios y asignar funciones mediante el `magento-cloud` CLI para [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**Requisitos previos:**

- Un usuario registrado con una Adobe ID. Un usuario debe [registrarse para una cuenta de Adobe](https://account.adobe.com) y luego [inicialice su cuenta de Cloud](https://console.adobecommerce.com) para poder agregarlos a un proyecto de Cloud.
- Un usuario ha asignado el **Administrador** La función no puede administrar usuarios con `magento-cloud` CLI. Solo los usuarios a los que se les haya concedido la **Propietario de cuenta** La función puede administrar usuarios.

>[!ENDSHADEBOX]

## Administrar usuarios con la CLI

Utilice el `magento-cloud` CLI para administrar usuarios e integrar con sistemas automatizados:

- `magento-cloud user:add`-agregar un usuario al proyecto
- `magento-cloud user:delete`Eliminar un usuario
- `magento-cloud user:list [users]`-enumerar usuarios de proyecto
- `magento-cloud user:role`-ver o cambiar la función de usuario
- `magento-cloud user:update`-actualizar rol de usuario en un proyecto

Los siguientes ejemplos utilizan la variable `magento-cloud` CLI para agregar un usuario, configurar funciones, modificar asignaciones de proyectos y asignar funciones de usuario.

**Para agregar un usuario y asignar funciones**:

1. Utilice el `magento-cloud` CLI para añadir al usuario.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >El usuario debe tener un Adobe ID; consulte la [requisitos previos](#add-users-and-manage-access).

1. Siga las indicaciones: especifique la dirección de correo electrónico del usuario, defina las funciones de tipo de entorno y proyecto, y añada el usuario.

   > Ejemplos de peticiones de datos

   ```terminal
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   Después de agregar el usuario, el Adobe de envía un correo electrónico a la dirección especificada con instrucciones para acceder al proyecto de infraestructura de Adobe Commerce en la nube.

### Ver la función de proyecto de un usuario

```bash
magento-cloud user:get alice@example.com
```

>Respuesta de ejemplo:

```terminal
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### Añadir un usuario a varios entornos

Para agregar un usuario como `viewer` en un `Production` entorno y as a `contributor` en un `Integration` entorno:

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### Actualizar permisos de entorno de usuario

Para actualizar los permisos de entorno de usuario a `admin` en el `Production` entorno:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## Administrar usuarios desde [!DNL Cloud Console]

Puede usar el complemento [[!DNL Cloud Console]](../../get-started/cloud-console.md) para añadir permisos y utilizar _Editar_ para modificar los permisos de un usuario existente.

>[!IMPORTANT]
>
>El usuario debe tener un Adobe ID; consulte la [requisitos previos](#add-users-and-manage-access).

### Agregar un usuario al proyecto

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. En el panel Proyecto, haga clic en el icono de configuración en la esquina superior derecha.

1. En _Configuración de proyecto_, haga clic en **[!UICONTROL Access]**.

1. En el _Acceso_ ver, haga clic en **[!UICONTROL Add]**.

1. Complete la _[!UICONTROL Add User]_formulario:

   - Introduzca la dirección de correo electrónico del usuario.

   - **[!UICONTROL Project admin]**: otorgue derechos de administrador a todas las configuraciones y tipos de entorno.

   - **[!UICONTROL Environment types and permissions]**: permite conceder acceso y niveles de permiso específicos a determinados tipos de entorno. _Sin acceso_, _Administrador_ (cambiar configuración, ejecutar acción, combinar código), _Colaborador_ (código push), o _Visor_ (solo vista).

   >[!TIP]
   >
   >Solo una **Administrador de proyecto** puede administrar usuarios en cualquier entorno. Para conceder a un usuario acceso a **Acceso** pestaña, otro **Administrador de proyecto** o el **Propietario de cuenta** debe asignar a ese usuario el **Administrador de proyecto** función.

1. Clic **[!UICONTROL Add User]**.

   >[!IMPORTANT]
   >
   >Añadir un usuario no almacena en déclencheur una implementación automáticamente.

1. Después de agregar usuarios, vuelva a implementar todos los entornos para aplicar los cambios. Añadir un usuario no almacena en déclencheur una implementación automáticamente. La reimplementación es un paso importante para garantizar que el usuario puede acceder a un entorno mediante SSH o realizar tareas de administrador.

Después de agregar el usuario, el Adobe de envía un correo electrónico a la dirección especificada con instrucciones para acceder al proyecto de infraestructura de Adobe Commerce en la nube.

## Requisitos de autenticación de usuario

Para obtener más seguridad, el Adobe de proporciona aplicación de autenticación multifactor (MFA) a nivel de proyecto para requerir autenticación de doble factor (TFA) para el acceso SSH a Adobe Commerce en el código fuente y los entornos del proyecto de infraestructura en la nube. Consulte [Habilitar MFA para SSH](multi-factor-authentication.md).

Cuando la aplicación de MFA está habilitada en un proyecto de infraestructura en la nube de Adobe Commerce, todos los usuarios con acceso SSH a un entorno de ese proyecto deben habilitar TFA en su cuenta de infraestructura en la nube de Adobe Commerce. Para los procesos automatizados, puede crear un usuario del equipo y un token de API para autenticarse desde la línea de comandos.

Después de agregar un usuario a un proyecto de Cloud, pídale que revise la configuración de seguridad de su cuenta y agregue las siguientes configuraciones de seguridad según sea necesario:

- **Habilitar TFA**: cumpla los estándares de seguridad y conformidad mediante la configuración de la autenticación de doble factor. Proyectos configurados con [Aplicación de MFA](multi-factor-authentication.md) requiere un FTA en las cuentas que utilizan SSH para acceder a los proyectos.

- **Habilitar las claves SSH**: los usuarios que requieran acceso a Adobe Commerce en repositorios de código fuente de infraestructura en la nube deben habilitar las claves SSH en su cuenta. Consulte [Conexiones seguras](../development/secure-connections.md).

- **Creación de un token de API**: los usuarios deben generar un token de API que se utilice para el acceso SSH a un entorno. Necesita el token para habilitar los flujos de trabajo de autenticación para los procesos automatizados.

  En proyectos con la aplicación MFA habilitada, debe usar el token de API para autenticar solicitudes de acceso SSH desde cuentas automatizadas. El token permite que los procesos automatizados omitan los flujos de trabajo de autenticación que requieren TFA.

### Habilitar TFA para cuentas de Cloud

Adobe Commerce en la infraestructura en la nube es compatible con TFA mediante cualquiera de las siguientes aplicaciones:

- [Google Authenticator (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [Authy (Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth Authenticator (Firefox OS, escritorio, otros)](https://github.com/gbraad-apps/gauth)

Las instrucciones para instalar la aplicación autenticadora y habilitar TFA están disponibles en la _Configuración de cuenta_ página en la [!DNL Cloud Console].

**Para habilitar TFA en su cuenta de usuario**:

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com).

1. En el menú superior derecho de la cuenta, haga clic en **[!UICONTROL My Profile]**.

1. En el _Seguridad_ pestaña, haga clic en **[!UICONTROL Set up application]**.

1. Si no tiene una aplicación autenticadora aprobada en su dispositivo móvil, utilice las instrucciones vinculadas para instalar una.

1. Añada su cuenta de Adobe Commerce en la infraestructura de la nube a la aplicación autenticadora.

   - En el dispositivo móvil, abra la aplicación autenticadora. A continuación, añada el código de configuración a la aplicación.

   - En el [!UICONTROL **[!UICONTROL TFA set up - Application]**] , escriba el código TFA desde su dispositivo móvil en la **[!UICONTROL Application verification code]** field.

   - Clic **[!UICONTROL Verify and save]**.

     Si el código es válido, Adobe envía una notificación a la dirección de correo electrónico de la cuenta para confirmar que la cuenta ahora tiene TFA.

1. Opcional. Activar _Explorador de confianza_ configuración para almacenar en caché el código de autenticación en el explorador durante 30 días.

   Esta configuración reduce el número de desafíos de autenticación durante el inicio de sesión del proyecto.

1. Clic **Guardar** o **Omitir**.

1. Guarde los códigos de recuperación.

   - En el _Configuración de TFA: recuperación_ página de códigos, copie y guarde los códigos de recuperación para poder iniciar sesión en su proyecto de infraestructura de Adobe Commerce en la nube cuando no pueda acceder a su dispositivo móvil o aplicación de autenticación.

   - Copie los códigos de recuperación en otra ubicación o anótelos en caso de que pierda el acceso a su dispositivo o aplicación de autenticación.

   - Clic **Guardar** para guardar los códigos en su cuenta y así poder verlos y administrarlos desde la configuración de seguridad de la cuenta.

     >[!WARNING]
     >
     >Si pierde el acceso a una cuenta con TFA y no tiene la lista de códigos de recuperación, debe ponerse en contacto con el administrador del proyecto o [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para restablecer la aplicación TFA.

1. Después de completar la configuración de TFA, haga clic en **Guardar** para actualizar su cuenta.

1. Autentique la sesión actual con TFA.

   - Cierre la sesión de su cuenta.
   - Inicie sesión con su nombre de usuario y contraseña.
   - Cuando se le solicite, introduzca el código TFA para `accounts.magento.cloud` entrada desde la aplicación autenticadora en su dispositivo móvil.

### Administrar la configuración de TFA y los códigos de recuperación

Puede administrar la configuración de TFA para una cuenta de Adobe Commerce en la infraestructura en la nube desde _Seguridad_ en la sección _Mi perfil_ página.

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com).

1. En el menú superior derecho de la cuenta, haga clic en **[!UICONTROL My Profile]**.

1. En el _Mi perfil_ , haga clic en **[!UICONTROL Security]** pestaña.

1. Utilice los vínculos disponibles para actualizar la configuración de TFA de su cuenta de Adobe Commerce en la infraestructura de la nube:

   - Deshabilitar TFA
   - Restablecer la aplicación autenticadora
   - Agregar o quitar exploradores de confianza
   - Ver o actualizar los códigos de recuperación de TFA en su cuenta

### Creación de un token de API

Se puede intercambiar un token de API por un token de acceso de OAuth 2, que luego se puede utilizar para autenticar solicitudes.

En los proyectos que tengan habilitada la aplicación MFA, debe tener un token de API para habilitar el acceso SSH para los usuarios del equipo y los procesos automatizados.

>[!IMPORTANT]
>
>Valores de token de la API de Protect para su cuenta. No exponga el valor en muestras de código, capturas de pantalla o comunicaciones cliente-servidor no seguras. Además, no exponga el valor en el código fuente almacenado en repositorios públicos.

**Para crear un token de API**:

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com).

1. En el menú superior derecho de la cuenta, haga clic en **[!UICONTROL My Profile]**.

1. En el _Mi perfil_ , haga clic en **[!UICONTROL API tokens]** pestaña.

1. Clic **[!UICONTROL Create API token]** e introduzca un nombre, por ejemplo, especifique un nombre que coincida con el usuario del equipo o el proceso automatizado que utiliza el token de API.

   ![tokens de API](../../assets/api-token-name.png)

1. Clic **[!UICONTROL Create API token]**.
