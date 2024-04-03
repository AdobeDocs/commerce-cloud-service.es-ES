---
title: "Administrar ramas con [!DNL Cloud Console]"
description: Obtenga información sobre cómo administrar las ramas de entorno para Adobe Commerce en la infraestructura en la nube mediante [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c4ef149-fdb9-473f-91fd-5e6421ac5a43
source-git-commit: a5af3cc6e174a731a8f2ff198acd794384ef4585
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Administrar ramas con [!DNL Cloud Console]

Puede administrar los entornos mediante las opciones [!DNL Cloud Console] o el `magento-cloud` CLI. Los archivos de proyecto se almacenan en un repositorio Git. Puede utilizar comandos Git para administrar el código, pero la variable `magento-cloud` CLI está diseñado para interactuar con funciones de plataforma, mientras que los comandos Git no lo hacen. Consulte [Comandos Git](../dev-tools/cloud-cli-overview.md#git-commands) en el tema CLI de la nube.

En este tema se explica cómo utilizar el [!DNL Cloud Console] hasta:

- Agregar o eliminar un entorno
- Sincronizar (`git pull`) del entorno principal
- Combinar (`git push`) al entorno principal

>[!TIP]
>
>No puede crear ramas desde entornos de ensayo y producción profesionales. Puede ramificar desde el `master` Rama.

## Crear un entorno

La estrategia de ramificación utiliza un flujo de trabajo Git común en el que se desarrolla código y se añaden extensiones en una rama de desarrollo. Consulte [Starter](../architecture/starter-architecture.md) y [Pro](../architecture/starter-develop-deploy-workflow.md) información general de arquitectura.

- Para empezar, cree un `staging` rama del `master` rama, luego rama desde `staging` para el desarrollo.
- Para Pro, cree una rama de desarrollo desde el `Integration` entorno.

Su cuenta admite un número limitado de ![rama activa](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inactivo) ramas de desarrollo. Administre las ramas activas e inactivas agregando o eliminando una rama utilizando solo la variable [!DNL Cloud Console] o la CLI de nube. Antes de poder eliminar una rama, debe desactivarla, que permanece en el _Entornos_ enumerar como _inactivo_. Puede reactivar la rama más tarde o puede hacer lo siguiente [eliminar la rama](../dev-tools/cloud-cli-overview.md#) en la configuración del entorno o mediante la CLI de nube.

Si necesita entornos activos adicionales para el desarrollo, envíe un [Ticket de asistencia](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Para agregar una rama**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. Seleccione un entorno.

   >[!TIP]
   >
   >La nueva rama se clona desde este entorno. Elija un entorno principal similar al entorno que está a punto de crear.

1. Clic **[!UICONTROL Branch]**.

   ![Crear una rama](../../assets/button-branch.png){width="150"}

1. En el _Ramificando desde..._ , introduzca un nombre de rama.

   El entorno _name_ es diferente del entorno _ID_ solo si utiliza espacios o mayúsculas en el nombre del entorno. Un ID de entorno consta de todas las letras minúsculas, números y símbolos permitidos. Las letras mayúsculas en un nombre de entorno se convierten a minúsculas en el ID.; los espacios en un nombre de entorno se convierten en guiones.

   Un nombre de entorno **no puede** incluye caracteres reservados para su shell de Linux o para expresiones regulares. Los caracteres prohibidos incluyen llaves (`{ }`), paréntesis, asterisco (`*`), corchetes angulares (`>`), ampersand (`&`), porcentaje (<code>%</code>) y otros caracteres.

1. Seleccione un **[!UICONTROL Environment type]**.

1. Clic **[!UICONTROL Create Branch]**.

1. Espere mientras se implementa el entorno.

   Durante la implementación, el estado del entorno es  **En proceso**. Después de una implementación correcta, el estado cambia a una marca de verificación verde para **success**.

## Crear rama inactiva

No puede crear una rama inactiva desde la consola de Adobe Commerce Cloud o CLI. Si desea crear una rama inactiva, créela en el repositorio de Git y haga clic en Insertar usando `environment.Parent` en el comando.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Eliminar un entorno

Antes de poder eliminar un entorno, debe desactivarlo. Una vez que un entorno esté inactivo, puede eliminarlo.

**Para desactivar un entorno**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. Seleccione el entorno en la barra de navegación _Entorno_ lista.

1. Haga clic en el icono de configuración en la parte derecha de la barra de navegación superior, que abre la configuración del entorno.

1. En el _[!UICONTROL General]_, desplácese hacia abajo hasta la pestaña_[!UICONTROL Deactivate environment]_ y haga clic en **[!UICONTROL Deactivate environment and delete data]** y siga las instrucciones.

## Sincronizar un entorno

La sincronización de un entorno (o rama) es lo mismo que `git pull origin <parent>`. Puede sincronizar el código actualizado desde un entorno principal. Puede utilizar esta función a través de las [!DNL Cloud Console] para todos los entornos Starter y Pro.

En el plan Pro, puede sincronizar desde Ensayo y producción a su `master` Rama. Esta sincronización solo extrae y inserta código, no datos. Para sincronizar datos, volque los datos de la base de datos y empújelos a la base de datos de otro entorno. Consulte [Migración e implementación de datos y archivos estáticos](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Para sincronizar un entorno**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. En la lista de entornos, haga clic en el nombre de la rama que desea sincronizar.

1. Haga clic en (sincronizar).

   ![Sincronizar un entorno](../../assets/button-sync.png){width="150"}

1. Seleccione los elementos que desea sincronizar.

   - Reemplazar los datos (datos y archivos): sincroniza los cambios de la base de datos y los archivos de contenido de la rama principal.
   - Combinar: (código) sincroniza el código actualizado de la rama principal.

   Esto también genera un comando CLI para que usted copie y utilice.

1. Clic **Sincronización**.

## Combinar con el entorno principal

La combinación de un entorno (o rama) es lo mismo que `git push origin`. Puede combinar para insertar el código actualizado de un entorno a su entorno principal. Puede combinar este código con `master`. Puede implementar en Ensayo y producción utilizando el `merge` comando.

**Para combinar con el entorno principal**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. En la lista entorno, haga clic en el nombre de la rama que desea combinar.

1. Haga clic en (combinar).

   ![Combinar un entorno](../../assets/button-merge.png){width="150"}

1. Clic **Combinar** y confirme la acción.

## Ver registros

A través de [!DNL Cloud Console], puede revisar varios registros para entornos, incluidos los de compilación, implementación e historial de implementación.

Para **Starter**, puede revisar los registros de generación e implementación y el historial de implementación. Estos entornos incluyen `master` Rama (Producción) y todas las ramas creadas a partir de ella.

Para **Pro**, puede revisar los siguientes registros en cada entorno:

- Integración: generación e implementación e historial de implementación
- Ensayo: registros de compilación e historial de implementación. Utilice SSH para iniciar sesión en el servidor y ver los registros de implementación.
- Producción: registros de compilación e historial de implementación. Utilice SSH para iniciar sesión en el servidor y ver los registros de implementación.

**Para ver los registros de[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. Seleccione un entorno.

   La vista de entorno proporciona un [Lista de actividades](activity-stream.md) que muestra _reciente_ eventos, una entrada por acción intentada que incluye sincronizaciones, combinaciones, ramas, copias de seguridad y más. Clic **Todo** para obtener el historial de implementación completo.

1. Para ver el registro de generación, seleccione el vínculo Éxito o Error por registro de implementación en la cuenta.

>[!TIP]
>
>Haga clic en **Filtrar por** para una lista desplegable y seleccione el tipo de mensajes que desea ver.

## Extraer código de un repositorio Git privado

Su proyecto de infraestructura de Adobe Commerce en la nube puede incluir código de un repositorio Git privado. Por ejemplo, puede tener código para un módulo o tema personalizado en un repositorio privado. Para ello, debe añadir la clave SSH pública del proyecto a su repositorio Git privado y actualizar el proyecto `composer.json` archivo.

Para agregar una clave de implementación al repositorio privado de GitHub, debe ser el administrador de dicho repositorio. GitHub le permite utilizar una clave de implementación solo para un repositorio.

Si prefiere que su proyecto acceda a varios repositorios, puede adjuntar una clave SSH a una cuenta de usuario automatizada. Dado que esta cuenta no la utiliza un ser humano, se denomina [usuario del equipo](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Agregue la cuenta del equipo como colaborador o agregue el usuario del equipo a un equipo con acceso a los repositorios.

>[!INFO]
>
>El Adobe recomienda añadir y combinar este código en los repositorios Git del proyecto. Si no configura la conexión, es posible que experimente problemas de compilación.

**Para encontrar la clave pública SSH**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. Haga clic en el icono de configuración en la parte derecha de la barra de navegación superior.

1. Entrada _Configuración de proyecto_, haga clic en **[!UICONTROL Deploy Key]**.

1. Copie la clave de implementación en el portapapeles para usarla en uno de los siguientes métodos basados en Git:

>[!BEGINTABS]

>[!TAB GitHub]

### Introduzca la clave de implementación de GitHub

En GitHub, las claves de implementación son de solo lectura de forma predeterminada.

**Para introducir la clave pública del proyecto como clave de implementación de GitHub**:

1. Inicie sesión en el repositorio de GitHub como administrador.
1. Haga clic en el repositorio **[!UICONTROL Settings]** pestaña.

   >[!NOTE]
   >
   >Si no ve esta opción, no ha iniciado sesión como administrador del repositorio y no puede completar esta tarea. Pida al administrador del repositorio de GitHub que lo haga.

1. En el _Configuración_ en el panel de navegación izquierdo, haga clic en **[!UICONTROL Deploy Keys]**.
1. Clic **[!UICONTROL Add deploy key]**.
1. Siga las indicaciones.

Entrada `composer.json`, use el `<user>@<host>:<.git</code>` formato, o `ssh://<user>@<host>:<port>/<path>.git` si se utiliza un puerto no estándar.

>[!TAB Bitbucket]

### Introduzca la clave de implementación de Bitbucket

**Para introducir la clave pública del proyecto como clave de implementación de Bitbucket**:

1. Inicie sesión en el repositorio de Bitbucket como administrador.
1. En el panel de navegación izquierdo, haga clic en **[!UICONTROL Settings]**.
1. Haga clic en General > **[!UICONTROL Deployment Keys]**.
1. Clic **[!UICONTROL Add Key]**.
1. Siga las indicaciones.

>[!TAB GitLab]

### Introduzca la clave de implementación de GitLab

**Para añadir la clave SSH pública para su proyecto como clave de implementación de GitLab, haga lo siguiente**:

1. Inicie sesión en el repositorio de GitLab como propietario.
1. Compruebe que la variable _Canalizaciones_ La opción está habilitada para su proyecto:

   1. En la configuración del proyecto, expanda el **[!UICONTROL Visibility, project, features, permissions]** sección.
   1. Si es necesario, haga clic en **[!UICONTROL Pipelines]** para activar la opción.

1. Añada la clave SSH pública a la configuración de CI/CD.

   1. En el panel de navegación izquierdo, haga clic en Configuración > **[!UICONTROL CI / CD]**.
   1. Haga clic en Implementar claves **Expandir** para configurar la clave.
   1. En el _Implementar clave_ formulario, agregue un nombre de clave de implementación a **[!UICONTROL Title]** y pegue la clave SSH pública en la variable **[!UICONTROL Key]** field.
   1. Clic **[!UICONTROL Add Key]** para guardar la configuración.

>[!ENDTABS]

## Entornos y ramas seguros

Puede acceder a su proyecto y a sus entornos desde cualquier ubicación a través de un explorador web utilizando [!DNL Cloud Console]. Es posible que tenga la seguridad configurada para su entorno de producción, tiendas y sitios. Esta sección le ayuda a proteger los entornos de integración y ensayo estrictamente para desarrolladores, DBA y mucho más.

>[!WARNING]
>
>**NO HACER** utilice los siguientes métodos para proteger los entornos de ensayo y producción de Pro. Esto interrumpe el almacenamiento en caché de Fastly. Utilice el [Bloqueo](../cdn/fastly-vcl-blocking.md) función disponible en Fastly CDN para Adobe Commerce.

**Para proteger entornos**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleccione un proyecto del _Todos los proyectos_ lista.

1. Seleccione un entorno y haga clic en el icono de configuración en la barra de navegación.

1. En la configuración del entorno _General_ pestaña, haga clic en **ACTIVADO** para **[!UICONTROL HTTP access control enabled]** para habilitar el acceso seguro. Puede elegir entre credenciales o direcciones IP para filtrar el acceso.

1. Para filtrar por credenciales, haga clic en **[!UICONTROL Add Login]**, introduzca un nombre de usuario y una contraseña y haga clic en **[!UICONTROL Add Login]** para agregar.

1. Para filtrar por dirección IP, introduzca las direcciones IP en una lista con `deny` o `allow`. Por ejemplo:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Clic **[!UICONTROL Save]**. Esto vuelve a implementar el entorno para actualizar la seguridad y la configuración. El Adobe recomienda probar el entorno después de completar la configuración de seguridad.
