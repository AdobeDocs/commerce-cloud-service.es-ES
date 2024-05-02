---
title: Administración de backup
description: Obtenga información sobre cómo crear y restaurar manualmente una copia de seguridad para su proyecto de Adobe Commerce en la nube.
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: 4c3f3f2775e8327476233520e52b589f7264786f
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Administración de backup

Puede realizar una copia de seguridad manual de los entornos de inicio activos en cualquier momento mediante el **[!UICONTROL Backup]** botón en el [!DNL Cloud Console] o utilizando el `magento-cloud snapshot:create` comando.

Una copia de seguridad o _instantánea_ es una copia de seguridad completa de los datos del entorno que incluye todos los datos persistentes de los servicios en ejecución (base de datos MySQL) y cualquier archivo almacenado en los volúmenes montados (var, pub/media, app/etc). La instantánea sí _no_ incluir código, ya que el código ya se almacena en el repositorio basado en Git. No se puede descargar una copia de una instantánea.

La función de copia de seguridad/instantánea sí **no** se aplican a los entornos de ensayo y producción de Pro, que reciben copias de seguridad regulares para fines de recuperación ante desastres de forma predeterminada. Consulte [Pro Backup y recuperación ante desastres](../architecture/pro-architecture.md#backup-and-disaster-recovery) para obtener más información. A diferencia de los backups automáticos en los entornos de ensayo y producción de Pro, los backups son **no** automático. Lo es _su_ responsabilidad de crear manualmente una copia de seguridad o configurar un trabajo cron para crear periódicamente una copia de seguridad de los entornos de integración de Starter o Pro.

## Creación de una copia de seguridad manual

Puede crear una copia de seguridad manual de cualquier entorno de inicio activo y de integración de Pro desde el [!DNL Cloud Console] o cree una instantánea desde la CLI de la nube. Debe tener un [Función de administrador](../project/user-access.md) para el medio ambiente.

**Para crear una copia de seguridad de cualquier entorno de inicio utilizando[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de la barra de navegación del proyecto. El entorno debe estar activo.
1. En el _Copias de seguridad_ ver, haga clic en **[!UICONTROL Backup]**. Esta opción no está disponible para un entorno Pro.

   ![Copia de seguridad](../../assets/button-backup.png){width="150"}

**Para crear una copia de seguridad de un entorno de integración con[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de integración/desarrollo en la barra de navegación del proyecto. El entorno debe estar activo.
1. Seleccione el **[!UICONTROL Backup]** en el menú superior derecho. Esta opción está disponible tanto para entornos Starter como Pro.
1. Haga clic en **[!UICONTROL Yes]** botón.

**Para crear una instantánea utilizando `magento-cloud` CLI**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Consulte la rama de entorno para acceder a la instantánea.
1. Cree la instantánea.

   ```bash
   magento-cloud snapshot:create --live
   ```

   Como alternativa, puede utilizar la variable `magento-cloud backup` comando en corto. El `--live` deja el entorno en ejecución para evitar el tiempo de inactividad. Para obtener una lista completa de opciones, escriba `magento-cloud snapshot:create --help`.

   Respuesta de ejemplo:

   ```terminal
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Compruebe las instantáneas más recientes.

   ```bash
   magento-cloud snapshot:list
   ```

   La lista devuelve información sobre el estado de la instantánea:

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Restaurar una copia de seguridad manual

Debe tener [Acceso de administrador](../project/user-access.md) al entorno. Tiene hasta **siete días** hasta _restaurar_ un respaldo manual. La restauración de una copia de seguridad no cambia el código de la rama de Git actual. La restauración de una copia de seguridad de esta manera no se aplica a los entornos de ensayo y producción de Pro; consulte [Pro Backup y recuperación ante desastres](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Los tiempos de restauración varían según el tamaño de la base de datos:

- una base de datos de gran tamaño (más de 200 GB) puede tardar 5 horas
- la base de datos media (150 GB) puede tardar 2 horas y media
- una base de datos pequeña (60 GB) puede tardar una hora

>[!TIP]
>
>Restauración sin copia de seguridad:
>
>- Para volver al código anterior o quitar extensiones agregadas en un entorno, consulte [Revertir código](#roll-back-code).
>- Para restaurar un entorno inestable que sí lo tiene _no_ haga una copia de seguridad, consulte [Restaurar un entorno](../development/restore-environment.md).

**Para restaurar una copia de seguridad mediante[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un entorno de la barra de navegación del proyecto.
1. En el _Copias de seguridad_ seleccione una copia de seguridad de la vista _Almacenado_ lista. La función de copia de seguridad sí **no** se aplican a los entornos de Pro.
1. En el ![Más](../../assets/icon-more.png){width="32"} (_más_), haga clic en **Restaurar**.
1. Revise la información de Restaurar desde copia de seguridad y haga clic en **Sí, restaurar**.

**Para restaurar una instantánea mediante la CLI de Cloud**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Consulte la rama de entorno para restaurar.
1. Enumerar todas las instantáneas disponibles.

   ```bash
   magento-cloud snapshot:list
   ```

   La lista devuelve información sobre las instantáneas disponibles:

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Restaure una instantánea con el ID de instantánea de la lista.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Restaurar una instantánea de recuperación ante desastres

Para restaurar la instantánea de recuperación ante desastres en entornos de ensayo y producción Pro, [Importar el volcado de la base de datos directamente desde el servidor](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Revertir código

Las copias de seguridad e instantáneas sí _no_ incluya una copia del código. El código ya está almacenado en el repositorio basado en Git, por lo que puede utilizar comandos basados en Git para revertir código. Por ejemplo, use `git log --oneline` para desplazarse por las confirmaciones anteriores, utilice [`git revert`](https://git-scm.com/docs/git-revert) para restaurar el código desde una confirmación específica.

Además, puede elegir almacenar código en un _inactivo_ Rama. Utilice comandos de Git para crear una rama en lugar de utilizar `magento-cloud` comandos. Ver acerca de [Comandos Git](../dev-tools/cloud-cli-overview.md#git-commands) en el tema CLI de nube.
