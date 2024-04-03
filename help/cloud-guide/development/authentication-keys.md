---
title: Claves de autenticación
description: Obtenga información sobre cómo aplicar claves de autenticación a un proyecto de desarrollo en Adobe Commerce en una infraestructura en la nube.
feature: Cloud, Security
topic: Security
exl-id: b05cd4c2-0804-49c8-980a-4c7b6932082b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Claves de autenticación

Debe tener una clave de autenticación para acceder al repositorio de Adobe Commerce y habilitar los comandos de instalación y actualización para su proyecto de Adobe Commerce en la nube. Existen dos métodos para especificar las credenciales de autorización del Compositor.

- **archivo de autenticación**: un fichero que contiene el Adobe Commerce [credenciales de autorización](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html) en el directorio raíz de la infraestructura de Adobe Commerce en la nube.
- **variable de entorno**: una variable de entorno para configurar claves de autenticación en el proyecto de Adobe Commerce en la nube para evitar una exposición accidental.

>[!BEGINSHADEBOX]

**Nota de seguridad**

El Adobe recomienda utilizar la variable [variable de entorno](#composer-auth-environment-variable) con su proyecto en la nube para evitar la exposición accidental de sus credenciales de autorización.

El método del archivo de autenticación es ideal cuando se utiliza Cloud Docker para Commerce como herramienta de desarrollo local, pero tenga cuidado de no cargar `auth.json` a un repositorio público basado en Git. Puede añadir la variable `auth.json` archivo a la [`.gitignore` archivo](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Archivo de autenticación

**Para crear un `auth.json` archivo**:

1. Si no tiene un `auth.json` en el directorio raíz del proyecto, cree uno.

   - Con un editor de texto, cree un `auth.json` en el directorio raíz del proyecto.
   - Copie el contenido del [muestra `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) en el nuevo `auth.json` archivo.

1. Reemplazar `<public-key>` y `<private-key>` con sus credenciales de autenticación de Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Guarde los cambios y salga del editor de texto.

## Variable de entorno de autenticación del compositor

El siguiente método es la mejor manera de evitar la exposición accidental de credenciales confidenciales en un repositorio público basado en Git.

**Para agregar claves de autenticación mediante una variable de entorno**:

1. En el _[!DNL Cloud Console]_, haga clic en el icono de configuración en la parte derecha de la navegación del proyecto.

   ![Configurar proyecto](../../assets/icon-configure.png){width="36"}

1. En el _Configuración de proyecto_ , haga clic en **[!UICONTROL Variables]**.

1. Clic **[!UICONTROL Create variable]**.

1. En el **[!UICONTROL Variable name]** , introduzca `env:COMPOSER_AUTH`.

1. En el _Valor_ , agregue lo siguiente y reemplace `<public-key>` y `<private-key>` con sus credenciales de autenticación de Adobe Commerce:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Seleccionar **[!UICONTROL Available during buildtime]** y deseleccione **[!UICONTROL Available during runtime]**.

1. Clic **[!UICONTROL Create variable]**.

1. Retire el `auth.json` de cada entorno.
