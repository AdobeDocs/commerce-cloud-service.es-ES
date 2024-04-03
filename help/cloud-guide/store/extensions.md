---
title: Administración de extensiones
description: Obtenga información sobre cómo instalar y administrar extensiones en Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Extensions, Upgrade
exl-id: 9c6e98ca-85da-4342-8402-d576eb382ba2
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---

# Administración de extensiones

Puede ampliar las funcionalidades de la aplicación de Adobe Commerce añadiendo una extensión desde el [Commerce Marketplace](https://marketplace.magento.com). Por ejemplo, puede agregar una temática para cambiar la apariencia de su tienda, o puede agregar un paquete de idioma para localizar su tienda y administrador.

## Nombre del compositor de una extensión

Aunque en esta sección se explica cómo obtener el nombre y la versión del compositor de una extensión de Commerce Marketplace, puede encontrar el nombre y la versión de _cualquiera_ en el archivo Composer del módulo. Abra el `composer.json` en un editor de texto y anote el `"name"` y `"version"` valores.

**Para obtener el nombre del compositor de un módulo del Commerce Marketplace**:

1. Iniciar sesión en [Commerce Marketplace](https://marketplace.magento.com) con el nombre de usuario y la contraseña que utilizó para adquirir el componente.

1. En la esquina superior derecha, haga clic en su nombre de usuario y seleccione **Mi perfil**.

   ![Acceder a su cuenta de Marketplace](../../assets/marketplace/my-profile.png)

1. En el _Mi cuenta_ página, haga clic en **Mis compras**.

   ![Historial de compras de Marketplace](../../assets/marketplace/my-purchases.png)

1. En el _Mis compras_ , seleccione un módulo que haya adquirido y haga clic en **Detalles técnicos**.

1. Clic **Copiar** para copiar el [!UICONTROL Component name] al portapapeles.

1. Abra un editor de texto, pegue el nombre del componente y añada dos caracteres (`:`).

1. Entrada **Detalles técnicos**, haga clic en **Copiar** para copiar el [!UICONTROL Component version] al portapapeles.

1. En el editor de texto, anexe el número de versión al nombre del componente después de dos puntos. Por ejemplo:

   ```text
   extension-name/magento2:1.0.1
   ```

## Instalación de una extensión

Adobe recomienda trabajar en una rama de desarrollo al añadir una extensión a la implementación. Al instalar una extensión, el nombre de la extensión (`<VendorName>_<ComponentName>`) se inserta automáticamente en [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html) archivo. No es necesario editar el archivo directamente.

**Para instalar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree o desproteja una rama de desarrollo. Consulte [ramificación](../development/cli-branches.md).

1. Utilizando el nombre y la versión del compositor, añada la extensión a `require` de la sección `composer.json` archivo.

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. Actualice las dependencias del proyecto.

   ```bash
   composer update
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >Al instalar una extensión, debe incluir el `composer.lock` cuando inserta cambios de código en el entorno remoto. El `composer install` El comando lee el `composer.lock` para habilitar las dependencias definidas en el entorno remoto.

1. Una vez finalizada la generación y la implementación, utilice un SSH para iniciar sesión en el entorno remoto y comprobar la extensión instalada.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Un nombre de extensión utiliza el formato: `<VendorName>_<ComponentName>`.

   Respuesta de ejemplo:

   ```terminal
   Module is enabled
   ```

   Si encuentra errores de implementación, consulte [error de implementación de extensión](../deploy/recover-failed-deployment.md).

## Administración de extensiones

Al añadir una extensión mediante Composer, el proceso de implementación habilita automáticamente la extensión. Si ya tiene la extensión instalada, puede habilitarla o deshabilitarla mediante la CLI. Al administrar las extensiones, utilice el formato: `<VendorName>_<ComponentName>`

Nunca habilite ni deshabilite una extensión mientras esté conectado a entornos remotos.

**Para habilitar o deshabilitar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Habilite o deshabilite un módulo. El `module` El comando actualiza el `config.php` archivo con el estado solicitado del módulo.

   >Habilite un módulo.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Deshabilite un módulo.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Si ha habilitado un módulo, utilice `ece-tools` para actualizar la configuración.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Compruebe el estado de un módulo.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Actualización de una extensión

Antes de continuar, necesita el nombre y la versión del compositor para la extensión de. Además, confirme que la extensión es compatible con el proyecto y la versión de Adobe Commerce. En particular, [compruebe la versión de PHP requerida](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) antes de empezar.

**Para actualizar una extensión**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree o desproteja una rama de desarrollo. Consulte [ramificación](../development/cli-branches.md).

1. Abra el `composer.json` en un editor de texto.

1. Busque la extensión y actualice la versión.

1. Guarde los cambios y salga del editor de texto.

1. Actualice las dependencias del proyecto.

   ```bash
   composer update
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Si encuentra errores, consulte [Recuperar de un error de componente](../deploy/recover-failed-deployment.md). Para obtener más información sobre el uso de extensiones con Adobe Commerce, consulte [Extensiones](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html) en el _Guía del administrador_.
