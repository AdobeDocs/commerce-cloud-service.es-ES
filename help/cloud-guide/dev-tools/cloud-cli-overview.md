---
title: CLI de nube
description: Obtenga información acerca de la CLI de magento en la nube y cómo le ayuda a administrar los entornos de desarrollo local para su proyecto de infraestructura de Adobe Commerce en la nube.
exl-id: 70dddd62-0269-4af4-bd2a-1a4fbf11a131
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 0%

---


# CLI de nube

El `magento-cloud` La herramienta CLI permite a los desarrolladores y administradores de sistemas gestionar proyectos y entornos en la nube, realizar rutinas y ejecutar tareas de automatización. El `magento-cloud` CLI amplía las características y funcionalidades de la [[!DNL Cloud Console]](../../get-started/cloud-console.md). Después de instalar el `magento-cloud` CLI en su estación de trabajo local, puede utilizarla para administrar su Adobe Commerce en entornos de integración de infraestructura en la nube Starter y Pro.

**Para instalar el `magento-cloud` CLI**:

1. En la estación de trabajo local, cambie al directorio donde desea clonar el proyecto de Cloud y donde la variable [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) tiene _escribir_ acceso.

1. Instale el `magento-cloud` CLI.

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Añadir `magento-cloud` CLI al perfil bash.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. Vuelva a cargar el perfil de bash actualizado.

   ```bash
   . ~/.bash_profile
   ```

1. Para iniciar la CLI, llame a `magento-cloud` e introduzca las credenciales de su cuenta de Cloud cuando se le solicite.

   ```bash
   magento-cloud
   ```

   ```terminal
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. Compruebe el `magento-cloud` El comando está en su ruta. En el ejemplo siguiente se enumeran los comandos disponibles.

   ```bash
   magento-cloud list
   ```

## Comandos comunes

El Adobe ha diseñado estos comandos para administrar los entornos de integración en la nube y recomienda que ejecute el `magento-cloud` CLI de un directorio de proyecto para que pueda omitir el `-p <project-ID>` parámetro.

La siguiente lista de los más utilizados `magento-cloud` Los comandos CLI solo incluyen las opciones requeridas. Puede usar el complemento `--help` con cualquier comando para ver más información.

| Comando | Descripción |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | Inicie sesión en el proyecto. |
| `magento-cloud list` | Enumerar los comandos disponibles para la herramienta CLI. |
| `magento-cloud environment:list` | Enumerar los entornos del proyecto actual. |
| `magento-cloud environment:checkout` | Consulte un entorno existente. |
| `magento-cloud environment:merge -e` | Combinar cambios en este entorno con su elemento principal. |
| `magento-cloud variables` | Variables de lista en este entorno. |
| `magento-cloud ssh` | Utilice SSH para conectarse al entorno remoto. |
| `magento-cloud url` | Abra la tienda de Adobe Commerce en un explorador. |
| `magento-cloud web` | Abra el [!DNL Cloud Console]. |

## Comandos de entorno

El entorno _name_ es diferente del entorno _ID_ solo si utiliza espacios o mayúsculas en el nombre del entorno. Un ID de entorno consta de todas las letras minúsculas, números y símbolos permitidos. Las letras mayúsculas en un nombre de entorno se convierten a minúsculas en el ID.; los espacios en un nombre de entorno se convierten en guiones.

Un nombre de entorno _no puede_ incluye caracteres reservados para su shell de Linux o para expresiones regulares. Los caracteres prohibidos incluyen llaves (`{ }`), paréntesis, asterisco (`*`), corchetes angulares (`< >`), ampersand (`&`), porcentaje (`%`) y otros caracteres.

El `magento-cloud environment:list` muestra las jerarquías del entorno, mientras que `git branch` no lo tiene. Si tiene entornos anidados, utilice lo siguiente:

```bash
magento-cloud environment:list
```

### Volver a implementar el entorno

Déclencheur una reimplementación sin utilizar una notificación push. Compruebe y confirme el entorno que desea volver a implementar. No utilice volver a implementar si hay una compilación en estado pendiente.

```bash
magento-cloud environment:redeploy
```

Respuesta de ejemplo:

```terminal
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Comandos Git

Puede observar que algunos de estos comandos son similares a los comandos Git. El `magento-cloud` Los comandos de se conectan directamente al proyecto de Cloud basado en Git con funciones adicionales. Si crea una rama sin utilizar la variable `magento-cloud` CLI, no está &quot;activada&quot; y no se genera automáticamente cuando se insertan cambios en el entorno remoto. El `magento-cloud` El comando CLI incluye la activación.

Para crear una rama, utilice el `magento-cloud` para activar la rama.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

Para el estado de sucursal:

- Utilice el `magento-cloud env` para ver una lista de las ramas del entorno y su estado: activa o inactiva.
- Utilice el `magento-cloud environment:activate` para activar una rama de entorno.

Inserte un compromiso Git vacío para almacenar en déclencheur una implementación. Por ejemplo:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

Algunas acciones, como agregar un usuario, no resultan en una implementación.

### Crear una rama de entorno

Los siguientes pasos muestran el uso de los comandos CLI y Git de forma intercambiable para administrar el entorno local:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cambie a la [propietario del sistema de archivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. Inicie sesión en el proyecto.

   ```bash
   magento-cloud login
   ```

1. Enumere sus proyectos.

   ```bash
   magento-cloud project:list
   ```

1. Enumerar entornos en el proyecto. Cada entorno incluye una rama de Git activa que contiene el código, la base de datos, las variables de entorno, las configuraciones y los servicios.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >Es importante usar el complemento `magento-cloud environment:list` porque muestra jerarquías de entorno, mientras que la variable `git branch` El comando no.

1. Recupere las ramas de origen para obtener el código más reciente.

   ```bash
   git fetch origin
   ```

1. Cierre la compra o cambie a una rama y un entorno específicos.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Los comandos Git solo comprueban la rama Git. El `magento-cloud checkout` El comando extrae la rama y cambia al entorno activo.

   >[!TIP]
   >
   >Puede crear una rama de entorno utilizando el `magento-cloud environment:branch <environment-name> <parent-environment-ID>` sintaxis del comando. Puede llevar algún tiempo adicional crear y activar una rama de entorno.

1. Utilice el ID de entorno para extraer cualquier código actualizado de su local de. Esto no es necesario si la rama de entorno es nueva.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_Opcional_) Crear un [instantánea](../storage/snapshots.md) del entorno como copia de seguridad.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## Actualizar la CLI

El `magento-cloud` CLI comprueba las actualizaciones disponibles cuando inicia sesión, pero puede buscar actualizaciones mediante el `self:update` comando. Si hay una actualización disponible, siga las instrucciones para actualizar la CLI.

Si su `magento-cloud` CLI está actualizada; verá la siguiente respuesta:

```bash
magento-cloud update
```

```terminal
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
