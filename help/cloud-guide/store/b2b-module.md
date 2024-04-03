---
title: Habilitar el módulo B2B
description: Obtenga información sobre cómo habilitar el módulo de empresa a empresa para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, B2B
exl-id: 01d02ea0-1e7d-4608-adbf-1dad7f5e2182
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Habilitar el módulo B2B

Si sus clientes son empresas, puede instalar el módulo B2B para Adobe Commerce para ampliar su proyecto de Adobe Commerce en la infraestructura en la nube Pro y adaptarlo a un modelo de empresa a empresa. Aunque en este tema se proporciona información específica sobre la instalación y configuración del módulo B2B para Adobe Commerce en la infraestructura en la nube, puede encontrar información adicional sobre B2B en las siguientes guías:

- [Guía para desarrolladores de Adobe Commerce B2B](https://developer.adobe.com/commerce/webapi/rest/b2b/)
- [Guía del usuario de Adobe Commerce B2B](https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html)

>[!TIP]
>
>Dado que B2B es un módulo para Adobe Commerce en la infraestructura en la nube, Adobe recomienda implementar la aplicación de Adobe Commerce en un entorno de integración o ensayo antes de comenzar.

## Instalación del módulo B2B

Adobe recomienda trabajar en una rama de desarrollo al añadir el módulo B2B al proyecto. Si no tiene una rama, consulte [Crear una rama para desarrollo](../development/cli-branches.md#create-a-branch-for-development). Al instalar el módulo B2B, la `Magento_B2b` nombre del módulo se inserta automáticamente en `app/etc/config.php` archivo. No es necesario editar el archivo directamente.

**Para instalar el módulo B2B**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cree o desproteja una rama de desarrollo.

1. Añada el módulo B2B a `require` de la sección `composer.json` archivo.

   ```bash
   composer require magento/extension-b2b --no-update
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
   git commit -m "Install the B2B module."
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Una vez finalizada la compilación y la implementación, utilice SSH para iniciar sesión en el entorno remoto y comprobar que el módulo B2B está instalado.

   ```bash
   bin/magento module:status Magento_B2b
   ```

   Un nombre de extensión utiliza el formato: `<VendorName>_<ComponentName>`.

   Respuesta de ejemplo:

   ```terminal
   Magento_B2b : Module is enabled
   ```

   Si encuentra errores de implementación, consulte [Recuperar de un error de componente](../deploy/recover-failed-deployment.md).

## Habilitar el módulo B2B

Al instalar el módulo B2B mediante Composer, el proceso de implementación habilita automáticamente el módulo. Si ya tiene instalado el módulo B2B, puede habilitarlo o deshabilitarlo mediante la CLI

Habilite el módulo B2B:

```bash
bin/magento module:enable Magento_B2b
```

Respuesta de ejemplo:

```terminal
The following modules have been enabled:
- Magento_B2b

Cache cleared successfully.
Generated classes cleared successfully. Please run the 'setup:di:compile' command to generate classes.
Info: Some modules might require static view files to be cleared. To do this, run 'module:enable' with the --clear-static-content option to clear them.
```

Consulte [Administración de extensiones](extensions.md).

## Configuración del módulo B2B

Después de instalar el módulo B2B para Adobe Commerce, debe [inicio del mensaje consumidores](https://experienceleague.adobe.com/docs/commerce-admin/b2b/install.html#start-message-consumers) para que pueda activar el _Catálogo compartido_ módulo, y debe [activar las funciones B2B](https://experienceleague.adobe.com/docs/commerce-admin/b2b/enable-basic-features.html).
