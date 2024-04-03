---
title: Actualizar proyecto para utilizar ECE-Tools
description: Aprenda a actualizar su proyecto de infraestructura de Adobe Commerce en la nube para utilizar el paquete ECE-Tools y aprovechar las últimas correcciones y funciones.
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Actualizar proyecto para utilizar el paquete ECE-Tools

Adobe obsoleto para `magento/magento-cloud-configuration` y `magento/ece-patches` paquetes a favor de la `ece-tools` , que simplifica muchos procesos de la nube. Si utiliza un proyecto de infraestructura de Adobe Commerce en la nube anterior que sí lo haga, _no_ contener el `ece-tools` paquete, entonces debe realizar un único, manual _actualización_ procesar en el proyecto.

>[!WARNING]
>
>Si el proyecto contiene `ece-tools` puede omitir la siguiente actualización. Para verificarlo, recupere el [!DNL Commerce] versión con el `php vendor/bin/ece-tools -V` comando en el directorio raíz del proyecto local.

Este proceso de actualización del proyecto requiere que actualice el `magento/magento-cloud-metapackage` restricción de versión en `composer.json` en el directorio raíz. Esta restricción permite actualizar Adobe Commerce en los metapaquetes de infraestructura en la nube (incluida la eliminación de paquetes obsoletos) sin actualizar la versión actual de Adobe Commerce.

{{upgrade-tip}}

## Eliminación de paquetes obsoletos

Antes de realizar una actualización para utilizar el `ece-tools` paquete, compruebe el `composer.lock` para los siguientes paquetes obsoletos:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Actualización del metapaquete

Cada versión de Adobe Commerce requiere una restricción diferente en función de lo siguiente:

```terminal
>=current_version <next_version
```

- Para `current_version`, especifique la versión de Adobe Commerce que desea instalar.
- Para `next_version`, especifique la siguiente versión del parche después del valor especificado en `current_version`.

Si desea instalar Adobe Commerce `2.3.5-p2`, configurado `current_version` hasta `2.3.5` y el `next_version` hasta `2.3.6`. La restricción `">=2.3.5 <2.3.6"` instala el último paquete disponible para 2.3.5.

Siempre puede encontrar la restricción del metapaquete más reciente en la variable [`magento-cloud` plantilla](https://github.com/magento/magento-cloud/blob/master/composer.json).

El siguiente ejemplo coloca una restricción para el metapaquete de infraestructura en la nube de Adobe Commerce en cualquier versión superior o igual a la versión actual 2.4.5 e inferior a la siguiente versión 2.4.6:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.5 <2.4.6"
},
```

## Actualizar el proyecto

Para actualizar el proyecto y utilizar `ece-tools` paquete, debe actualizar el metapaquete y el `.magento.app.yaml` enlaza propiedades de y realiza una actualización de Composer.

**Para actualizar un proyecto para que utilice ece-tools**:

1. Actualice el `magento/magento-cloud-metapackage` restricción de versión en `composer.json` archivo.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.5 <2.4.6" --no-update
   ```

1. Actualice el metapaquete.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modifique los comandos de enlace en la `magento.app.yaml` archivo.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Compruebe y extraiga el [paquetes obsoletos](#remove-deprecated-packages). Los paquetes obsoletos pueden impedir que la actualización se realice correctamente.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Puede ser necesario actualizar el `ece-tools` paquete.

   ```bash
   composer update magento/ece-tools
   ```

1. Añada y confirme los cambios de código. En este ejemplo, se actualizaron los siguientes archivos:

   ```terminal
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Inserte los cambios de código en el servidor remoto y combine esta rama con la `integration` Rama.

   ```bash
   git push origin <branch-name>
   ```
