---
title: Aplicar parches
description: Obtenga información sobre cómo aplicar parches en el proyecto de infraestructura de Adobe Commerce en la nube.
feature: Cloud, Upgrade
exl-id: a7bf672f-7b89-45cd-8436-e885bca9029d
source-git-commit: e67d3259b1b5195147e4e441fe9efd82e48241ab
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Aplicar parches

[Parches de nube para Commerce](https://github.com/magento/magento-cloud-patches) y el [Herramienta Parches de calidad](https://github.com/magento/quality-patches) envíe parches a la aplicación de Adobe Commerce instalada.

- El paquete Cloud Patches for Commerce ofrece los parches necesarios con correcciones críticas
- Los parches de calidad ofrecen correcciones de calidad opcionales y de bajo impacto como [parches individuales](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html#individual-patch) que no contengan cambios incompatibles con versiones anteriores

Consulte [Parches disponibles](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) en el _Guía de herramientas de operaciones de Commerce_ para revisar una lista completa de parches publicados.

Ambos paquetes mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube y admiten la entrega rápida de correcciones críticas, opcionales y personalizadas. Puede utilizar estos paquetes para aplicar, revertir y ver información general sobre todos los parches individuales que están disponibles para Commerce.

>[!TIP]
>
>Puede usar el complemento [Herramienta Parches de calidad](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) y Parches de nube para Commerce como paquetes independientes para proyectos de Magento Open Source y Adobe Commerce. Se recomienda utilizar la herramienta Parches de calidad para proyectos que no estén en la nube.

Al implementar cambios en el entorno remoto, la variable `ece-tools` el paquete utiliza `magento/magento-cloud-patches` y `magento/quality-patches` para comprobar parches pendientes y aplicarlos automáticamente en el siguiente orden:

1. Aplique todos los parches de Commerce necesarios incluidos en el paquete Parches de Cloud para Commerce.
1. Aplicar los parches de comercio opcionales seleccionados incluidos en la herramienta Parches de calidad.
1. Aplicar parches personalizados en `/m2-hotfixes` en orden alfabético por nombre de parche.

>[!NOTE]
>
>Cuando actualice el `ece-tools` para el paquete de Cloud Patches for Commerce, los parches necesarios más recientes se aplican la próxima vez que implemente el proyecto o puede implementarlos inmediatamente utilizando `ece-patches apply` Comando CLI y reimplementación de su entorno de nube. No puede omitir [parches necesarios](https://github.com/magento/magento-cloud-patches/tree/develop/patches) durante el proceso de implementación.

## Requisitos previos

{{upgrade-tip}}

La herramienta Parches de calidad es una dependencia de Cloud Patches para Commerce y `ece-tools` paquete. Para aplicar los parches más recientes, debe tener [la última versión de ECE-Tools](../dev-tools/update-package.md) instalado. La versión mínima requerida de ECE-Tools es 2002.1.2.

## Ver parches y estado disponibles

Para ver la lista de parches individuales disponibles:

```bash
php ./vendor/bin/ece-patches status
```

Respuesta de ejemplo:

```terminal
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

La tabla de estado contiene los siguientes tipos de información:

- **Tipo**:
   - `Optional`: todos los parches de la herramienta Parches de calidad y del paquete Parches de nube son opcionales para las instalaciones de Adobe Commerce y de Magento Open Source. Para Adobe Commerce en la infraestructura en la nube, todos los parches son opcionales.
   - `Required`: todos los parches del paquete Parches de nube para Commerce son necesarios para los clientes de Cloud.
   - `Deprecated`: el parche individual está marcado como obsoleto y se recomienda revertirlo si se ha aplicado. Después de revertir un parche obsoleto, ya no se mostrará en la tabla de estado.
   - `Custom`: todos los parches del directorio &quot;m2-hotfixes&quot;.

- **Estado**:
   - `Applied`: se ha aplicado el parche.
   - `Not applied`: no se ha aplicado el parche.
   - `N/A`: el estado del parche no se puede definir debido a conflictos.

- **Detalles**:
   - `Affected components`: lista de módulos afectados.
   - `Required patches`: lista de parches necesarios (dependencias).
   - `Recommended replacement`: parche que se recomienda para sustituir un parche obsoleto.

## Aplicación de un parche en un entorno local

Puede aplicar parches manualmente en un entorno local y probarlos antes de la implementación.

**Para aplicar parches individuales en un entorno de desarrollo local**:

1. Agregue la variable &quot;QUALITY_PATCH&quot; a `.magento.env.yaml` y enumere los parches necesarios debajo de.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Desde la raíz del proyecto, aplique los parches.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   El `ece-patches apply` El comando aplica parches en el siguiente orden:
   - Parches necesarios
   - Parches individuales opcionales
   - Parches personalizados del `/m2-hotfixes` directorio

1. Borre la caché.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Pruebe los parches y realice los cambios necesarios en los parches personalizados.

## Aplicación de un parche en un entorno remoto

>[!WARNING]
>
>Se recomienda encarecidamente probar todos los parches en una integración o entornos de ensayo antes de implementarlos en el entorno de producción.

**Para aplicar parches en un entorno remoto**:

1. Añada el `QUALITY_PATCHES` a la `.magento.env.yaml` y enumere los parches necesarios debajo de.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >Después de actualizar a una nueva versión de Adobe Commerce, debe volver a aplicar los parches si estos no están incluidos en la nueva versión.

1. Añada, confirme e inserte el actualizado `.magento.env.yaml` archivo.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Aplicar un parche personalizado

Al implementar, ECE-Tools aplica todos los parches de Adobe y todos los parches personalizados que agregue al `/m2-hotfixes` en la raíz del proyecto.

>[!NOTE]
>
>Todos los nombres de archivos de parche deben terminar con `.patch` extensión.

**Para aplicar y probar un parche personalizado en un entorno de Cloud**:

1. En la raíz del proyecto, cree un directorio llamado `m2-hotfixes` si no existe.

   ```bash
   mkdir m2-hotfixes
   ```

1. Copie el archivo de parche en `/m2-hotfixes` directorio.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Asegúrese de probar todos los parches en un entorno de preproducción. Para Adobe Commerce en la infraestructura en la nube, puede crear ramas con `magento-cloud environment:branch <branch-name>` Comando CLI.

## Revertir un parche personalizado

Para revertir o desinstalar un parche personalizado aplicado anteriormente:

1. Elimine el archivo de parche de la `/m2-hotfixes` directorio.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Asegúrese de realizar la prueba en un entorno de preproducción. Para Adobe Commerce en la infraestructura en la nube, puede crear ramas con `magento-cloud environment:branch <branch-name>` Comando CLI.

## Aplicación de parches a un proyecto que no esté en la nube

Utilice el [Herramienta Parches de calidad](https://github.com/magento/quality-patches) para proyectos de Magento Open Source y Adobe Commerce.

## Reversión de un parche en un entorno local

Puede revertir todos los parches aplicados anteriormente en un entorno de desarrollo local utilizando `ece-patches` CLI.

Para revertir todos los parches aplicados:

```bash
php ./vendor/bin/ece-patches revert
```

Este comando revierte todos los parches en el siguiente orden:

- Revierte todos los parches personalizados aplicados desde el directorio /m2-hotfixes.
- Revierte todos los parches individuales opcionales aplicados.
- Revierte todos los parches necesarios aplicados.

## Registro

La herramienta Parches de calidad registra todas las operaciones en `<Project_root>/var/log/patch.log` archivo.
