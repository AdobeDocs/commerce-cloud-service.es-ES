---
title: Ejemplo de administración de la configuración específica del sistema
description: Consulte un ejemplo de cómo administrar y sincronizar las opciones de configuración de tienda en todos los entornos de Adobe Commerce en la nube.
hidefromtoc: true
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 0%

---


# Ejemplo de administración de la configuración específica del sistema

Este ejemplo muestra cómo utilizar la administración de la configuración para mantener la coherencia de la configuración de las tiendas en todos los entornos.

El ejemplo utiliza el siguiente procedimiento definido en [Configuración de tienda](store-settings.md):

1. Introduzca sus configuraciones en el entorno de integración y almacene el administrador.
1. Crear un `config.php` y transfiéralo a su estación de trabajo local.
1. Push `config.php` al entorno de integración remota.
1. Compruebe que la configuración no se pueda editar en Admin.
1. Realice las modificaciones necesarias:

   * Cambie las opciones de configuración en el entorno de integración.
   * Para agregar configuraciones, ejecute el comando para crear `config.php` otra vez. Las nuevas configuraciones se anexan al archivo.
   * Para quitar o editar las configuraciones existentes, edite manualmente el archivo.
   * Compromiso y push.

Por ejemplo, es posible que desee establecer la siguiente configuración:

* Deshabilitar [locale](https://glossary.magento.com/locale) y la configuración de optimización de archivos estáticos en el entorno de integración
* Habilitar la optimización de archivos estáticos en entornos de ensayo y producción
* Configure Fastly en Ensayo y producción con credenciales específicas para cada

_Optimización de archivo estático_ significa combinar y minificar hojas de estilos en cascada y JavaScript, y minificar plantillas de HTML. Consulte [Estrategias de implementación de contenido estático](../deploy/static-content.md).

## Requisitos previos

Para completar estas tareas de administración de la configuración, necesita lo siguiente:

* Función de lector de proyectos con [entorno &quot;admin&quot;](../project/user-access.md) privilegios
* URL y credenciales de administrador para los entornos de integración, ensayo y producción

## Configuración del administrador de Commerce

En el entorno de integración, puede iniciar sesión en Admin para modificar los ajustes de configuración del sistema para tiendas, sitios web, módulos o extensiones, optimización de archivos estáticos y valores del sistema relacionados con la implementación de contenido estático. Consulte [Datos de configuración](store-settings.md#scd-performance).

**Para cambiar la configuración regional y de optimización de archivos estáticos**:

1. Inicie sesión en el administrador del entorno de integración. Puede acceder a esta dirección URL a través del [[!DNL Cloud Console]](../project/overview.md).
1. Vaya a **Tiendas** > Configuración > **Configuración** > General > **General**.
1. En la barra de navegación de páginas, expanda **Opciones de configuración regional**.
1. Desde el **Configuración regional** , cambie la configuración regional. Puede volver a cambiarlo más tarde.

   ![Cambiar la configuración regional](../../assets/locale-options.png)

1. Clic **Guardar configuración**.
1. Si se le solicita, [vaciar la caché](https://docs.magento.com/user-guide/system/cache-management.html).
1. Cierre la sesión del administrador.

## Exportar valores y transferir config.php a su sistema local

Este paso crea y transfiere el `config.php` archivo de configuración en el entorno de integración mediante un comando que se ejecuta en el equipo local.

Este procedimiento corresponde al paso 2 de la [procedimiento recomendado](store-settings.md). Después de crear `config.php`, transfiéralo al sistema local para que pueda añadirlo a Git.

**Para crear y transferir`config.php`**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Cambio en el entorno de integración de.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. Cree un volcado local de la base de datos remota.

   ```bash
   magento-cloud db:dump
   ```

El siguiente fragmento de `config.php` muestra un ejemplo de cómo cambiar la configuración regional predeterminada a `en_GB` y cambiar la configuración de optimización de archivos estáticos:

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## Insertar e implementar config.php en entornos

Ahora que ha creado `config.php` y lo transfirió a su sistema local, configúrelo en Git y envíelo a sus entornos. Este procedimiento corresponde a los pasos 3 y 4 de la [procedimiento recomendado](store-settings.md).

El siguiente comando agrega, confirma e inserta en `master` rama:

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

Implementación completa del código en Ensayo y producción. Para empezar, debe pulsar hasta `staging` y `master` ramas. Para obtener más información sobre los comandos de implementación, consulte [Implementar la tienda](../deploy/staging-production.md).

Espere a que se complete la implementación en todos los entornos.

## Verifique los cambios de configuración

Después de presionar `config.php` En los entornos, cualquier valor que haya cambiado debe ser de solo lectura en el Administrador. En este ejemplo, la configuración regional predeterminada modificada y la configuración de optimización de archivos estáticos no deben poder editarse en Admin. Estas opciones de configuración se establecen en `config.php`.

Para comprobar los cambios de configuración:

1. Cierre la sesión del administrador en uno de los entornos.
1. Vuelva a iniciar sesión en Admin.
1. Clic **Tiendas** > Configuración > **Configuración** > General > **General**.
1. En el panel derecho, expanda **Opciones de configuración regional**.

   Tenga en cuenta que no se pueden editar varios campos, como se muestra en el siguiente ejemplo. Estas opciones de configuración las mantiene `config.php`.

   ![Algunos valores ya no se pueden editar en el Administrador](../../assets/locale-options-disabled.png)

1. Cierre la sesión del administrador.

## Cambiar y actualizar la configuración específica del sistema

Si necesita modificar cualquiera de estas configuraciones, modifique la `config.php` archivo manualmente con un editor de texto. Después de completar las ediciones o eliminaciones, puede confirmarlo y enviarlo al entorno remoto siguiendo los pasos anteriores.

Para agregar configuraciones, modifique el entorno de integración y ejecute de nuevo el comando para generar el archivo. Las nuevas configuraciones se anexan al código del archivo. Colóquelo en Git siguiendo los pasos anteriores.

Para este ejemplo, modifique la configuración de optimización de archivos estáticos y agregue una nueva configuración para JavaScript.

### Añadir configuraciones en la integración

Para añadir valores de configuración en el entorno de integración Administrador. En este ejemplo se combinan archivos JavaScript.

1. Cierre la sesión del administrador de integración.
1. Vuelva a iniciar sesión en el administrador de integración.
1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Desarrollador**.
1. En el panel derecho, expanda **Configuración de JavaScript**.
1. Desde el **Combinar archivos JavaScript** , haga clic en **Sí**.
1. Clic **Guardar configuración**.
1. Si se le solicita, [vaciar la caché](https://docs.magento.com/user-guide/system/cache-management.html).
1. Cierre la sesión del administrador.

Al volver a ejecutar el comando de volcado, la nueva configuración se anexa al archivo.

```bash
magento-cloud db:dump
```

### Editar config.php con nuevos ajustes

En su local de, utilice un editor de texto para editar el actualizado `app/etc/config.php` archivo. Edite esta configuración para habilitar la minificación para archivos JavaScript, HTML y CSS.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

Para modificar la configuración y permitir la minificación, edite `'0'` hasta `'1'` para `'minify_html'` y cada `'minify_files'` opción:

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

Guarde los cambios en el archivo.

### Insertar los cambios en Git

Para insertar los cambios, escriba lo siguiente:

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

Espere a que se complete la implementación.

Repita el proceso de implementación para insertar el código en todos los entornos.
