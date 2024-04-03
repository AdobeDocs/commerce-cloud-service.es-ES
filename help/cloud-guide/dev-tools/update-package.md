---
title: Actualizar el paquete ECE-Tools
description: Aprenda a actualizar el paquete ECE-Tools para aprovechar las últimas correcciones y funciones aplicadas a Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Upgrade
exl-id: 7cce45eb-ae53-4468-b16d-4f4d3422ac52
source-git-commit: 513bc5b52f046ffd98005d80f34725b7f60b38bd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Actualizar el paquete ECE-Tools

Una actualización de la `ece-tools` El paquete también actualiza el otro [Cloud Tools Suite para paquetes de Commerce](../release-notes/cloud-tools-suite.md), que son dependencias para `ece-tools`. Por lo tanto, debe utilizar una versión de Adobe Commerce en la infraestructura en la nube que admita `ece-tools` paquete.

{{ece-tools-package}}

**Requisitos previos**:

- Antes de actualizar `ece-tools`, revise la [Notas de la versión de Cloud Tools Suite for Commerce](../release-notes/cloud-tools-suite.md).
- Si está actualizando desde `ece-tools` 2002.0.22 o anterior a 2002.1.0, revisar [Cambios incompatibles con versiones anteriores](../release-notes/backward-incompatible-changes.md) y realice los cambios necesarios en su proyecto de Adobe Commerce en la nube.
- Revisar [Actualizaciones y parches](../development/commerce-version.md#upgrade-from-older-versions) para determinar las versiones de ECE-Tools compatibles con su proyecto de infraestructura de Adobe Commerce en la nube.

{{upgrade-tip}}

**Para actualizar el `ece-tools` paquete**:

1. En la estación de trabajo local, realice una actualización con Composer.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Si no puede actualizar más allá `ece-tools` versión 2002.0.8, consulte [Actualizar proyecto para utilizar el paquete ECE-Tools](install-package.md).

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Después de la validación de prueba, combine esta rama con la rama de integración.
