---
title: Recuperar de un error de componente
description: Descubra cómo puede recuperarse si un componente no se implementa correctamente en Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Deploy
exl-id: 4855be0c-6883-4ab1-a364-316d10e97250
source-git-commit: b44d97f82ef09288807c648010202422c9ac04eb
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Recuperar de un error de componente

En este tema se explica cómo recuperar si un componente no se implementa correctamente. Algunos ejemplos típicos son componentes que tienen dependencias que su entorno remoto no cumple, como versiones de PHP incompatibles.

Puede recuperarse de una implementación fallida de cualquiera de las siguientes maneras:

- [Restaurar una copia de seguridad](../storage/snapshots.md#restore-a-snapshot)
- Limpiar el proyecto y el código de cambios anteriores y volver a implementarlos

## Limpiar, quitar y volver a implementar

Para realizar una limpieza desde la implementación anterior, identifique el componente que se añadió o actualizó y, a continuación, elimínelo. En primer lugar, inicie sesión en el entorno remoto y borre manualmente el contenido del `var` directorio. A continuación, elimine el componente de `composer.json` y vuelva a implementar el entorno.

**Para limpiar `var` directorios**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Borre la `var` directorios.

   ```shell
   rm -rf var/*
   ```

1. Cerrar sesión.

**Para quitar el componente**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Borre la caché.

   ```bash
   composer clear-cache
   ```

1. Extraiga el componente del `composer.json` archivo.

   ```bash
   composer remove <component-name>:<version>
   ```

   Si se muestra el siguiente mensaje, no es necesario que haga nada más:

   ```terminal
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Espere mientras se actualizan las dependencias.

1. Agregar, confirmar y enviar cambios de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Obtenga más información sobre la restauración de un entorno sin copia de seguridad en [Restaurar un entorno](../development/restore-environment.md).

{{stuck-deployment-tip}}
