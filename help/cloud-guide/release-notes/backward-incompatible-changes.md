---
title: Cambios incompatibles con versiones anteriores
description: Obtenga información acerca de la compatibilidad con versiones anteriores al actualizar proyectos existentes en la nube.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 9847c565-6d59-429a-9593-c2eba5cf2da4
source-git-commit: f9edcc85c14354a2eddacbc5219557e107a367cb
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Cambios incompatibles con versiones anteriores

Es posible que los cambios incompatibles con versiones anteriores requieran que ajuste la configuración y los procesos de Cloud para los proyectos de Cloud existentes al actualizar a la última versión de `ece-tools` u otros paquetes de Cloud Tools Suite para Commerce.

## Cambios en `ece-tools` paquete

Algunas funciones incluidas anteriormente en `ece-tools` ahora se proporciona en paquetes separados. Estos paquetes son dependencias del compositor para `ece-tools`, que se instalan y actualizan automáticamente al instalar o actualizar ece-tools.

La nueva arquitectura no debería afectar a los procesos de instalación o actualización. Sin embargo, es posible que tenga que cambiar algunos procesos y sintaxis de comandos al trabajar con Adobe Commerce en el proyecto de infraestructura en la nube. Para obtener más información, revise la siguiente información de cambios incompatibles con versiones anteriores y la [Notas de la versión de Cloud Tools Suite](cloud-tools-suite.md).

### Cambios en los requisitos de versión del servicio

Hemos cambiado el requisito mínimo de la versión de PHP de 7.0.x a 7.1.x para los proyectos en la nube que usan `ece-tools` v2002.1.0 y posterior. Si la configuración del entorno especifica PHP 7.0, actualice el [configuración de php](../application/php-settings.md) en el `.magento.app.yaml` archivo.

>[!WARNING]
>
>Debido al cambio en los requisitos de la versión de PHP, `ece-tools` 2002.1.0 sólo admite Adobe Commerce en proyectos de infraestructura en la nube que ejecuten Adobe Commerce 2.1.15 o posterior. Si el proyecto utiliza una versión anterior, deberá [actualización](../development/commerce-version.md) antes de actualizar a `ece-tools` 2002.1.0

### Cambios de configuración del entorno

La siguiente tabla proporciona información sobre variables de entorno y otros archivos de configuración de entorno que se eliminaron o quedaron obsoletos en `ece-tools` v2002.1.0.

| Elemento | Sustitución |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` variable | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` variable | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` variable | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` variable | Ninguna. Ahora, la generación siempre crea un enlace simbólico al directorio de contenido estático `pub/static`. |
| `build_options.ini` archivo | Utilice el [`.magento.env.yaml`](../application/configure-app-yaml.md) para configurar variables de entorno para administrar acciones de compilación e implementación en todos los entornos.<p>Si crea un entorno de nube que incluya la variable `build_options.ini` , la compilación falla. |

### Cambios en el comando CLI

La siguiente tabla resume los cambios de comandos CLI en ECE-Tools v2002.1.0 que pueden requerir la actualización de comandos o secuencias de comandos.

| Comando | Sustitución |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

En versiones anteriores de ECE-Tools, se podía usar el `m2-ece-build` y `m2-ece-deploy` comandos para configurar los vínculos de implementación en `.magento.app.yaml` archivo. Cuando actualice a la versión 2002.1.0, marque la `hooks` configuración en la `.magento.app.yaml` para los comandos obsoletos y sustitúyalos si es necesario.

## Cambios en Parches de nube

- **Eliminación de parches descargados**-El `magento/magento-cloud-patches` paquetes de todos los parches disponibles en el [descargas de software](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) y las aplica automáticamente al implementar en la nube. Para evitar conflictos de parches después de actualizar a ECE-Tools 2002.1.0 o posterior, elimine los parches suministrados por el Adobe que haya descargado y agregado al proyecto manualmente.

- **Actualización del comando Aplicar parches**-Hemos movido el comando para aplicar parches desde el `vendor/bin/ece-tools` al directorio `vendor/bin/ece-patches` directorio. Si utiliza este comando para aplicar parches manualmente, utilice la nueva ruta.

  > Aplicar parches manualmente

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cambios de Cloud Docker

- **El requisito mínimo de la versión de PHP es ahora PHP 7.1**-Si el host Cloud Docker para Commerce ejecuta una versión anterior, actualice a PHP v7.1 o posterior.

- **Cambios en los comandos de Cloud Docker para Commerce**-

   - **Actualización de los comandos de Cloud Docker para Commerce para las operaciones de generación de Docker**-Movimos los comandos de Cloud Docker para Commerce desde el `vendor/bin/ece-tools` al directorio `vendor/bin/ece-docker` directorio. Actualice los scripts y comandos para utilizar la nueva ruta.

     Después de actualizar a `ece-tools` 2002.1.0, utilice el siguiente comando para ver los archivos disponibles `ece-docker` comandos.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Actualizar los comandos de composición de Docker de Cloud**-Cambiamos el nombre de la ruta al archivo de comandos desde `./bin/docker` hasta `./bin/magento-docker`. Actualice los scripts y comandos para utilizar la nueva ruta.

   - **El contenedor de Cron ya no se incluye en la configuración predeterminada de Docker**-Ahora, debe agregar el `--with-cron` a la opción `ece-docker build:compose` para incluir el contenedor Cron en la configuración del entorno Docker. Consulte [Administrar trabajos cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) en el _Cloud Docker para Commerce_ guía.

     Los scripts que anteriormente generaban contenedores con trabajos cron ahora no tienen el contenedor cron.

   - **Uso de contenedores temporales**-En versiones anteriores, los contenedores creados por `bin/magento-docker` Las operaciones de comando no se eliminaron, por lo que se pueden utilizar para otras operaciones. Ahora, la `magento-docker` Los comandos eliminan cualquier contenedor que creen una vez finalizado el comando.

     Si desea conservar un contenedor creado por una operación de composición de docker, utilice el `docker-compose run` en lugar del comando `bin/magento-docker` comando.

   - **Ejecución de vínculos posteriores a la implementación**-El `cloud-deploy` El comando ya no ejecuta los enlaces posteriores a la implementación. Utilice el nuevo `cloud-post-deploy` para ejecutar los vínculos posteriores a la implementación después de la implementación. Actualice los scripts para agregar el comando y ejecutar los vínculos posteriores a la implementación.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Alternativamente, si utiliza `docker-compose` comandos directamente, ejecute el `docker-compose run deploy cloud-post-deploy` después del comando deploy.

- **Actualización de la base de datos**-El contenedor de base de datos ahora se almacena en el `magento-db` volumen Docker persistente. Al actualizar el entorno de Docker, la base de datos ya no se elimina automáticamente. Si es necesario, utilice uno de los siguientes comandos para eliminarlo manualmente.

   - Retire el `magento-db` contenedor:

     ```bash
     docker volume rm magento-db
     ```

   - Elimine todos los volúmenes asociados al cerrar los contenedores de Docker:

     ```bash
     docker-compose down -v
     ```

- **Anular la configuración de sincronización de archivos para archivos de almacenamiento y copia de seguridad**-Los archivos de copia de seguridad y archivo con las siguientes extensiones ya no se sincronizan al utilizar docker-sync o mutagen: SQL, GZ, ZIP y BZ2. Puede anular la sincronización de archivos predeterminada para estos tipos de archivos cambiando el nombre del archivo para que termine con una extensión diferente. Por ejemplo: `synchronize-me.zip-backup`
