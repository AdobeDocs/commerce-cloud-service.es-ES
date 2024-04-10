---
title: Configuración de PHP
description: Obtenga información acerca de la configuración óptima de PHP para la configuración de aplicaciones de Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 94c1e16a07567471d446478e3bd2a33977247ef3
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# Configuración de PHP

Puede elegir cuál [versión de PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) para ejecutar en su `.magento.app.yaml` archivo:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Si actualiza a PHP 8.1 y versiones posteriores, elimine JSON del [`runtime: extensions:` propiedad](properties.md#runtime) en el `.magento.app.yaml` y vuelva a implementarlo. La extensión JSON viene instalada en el entorno de la nube desde PHP 8.0.

## Configuración de PHP

Puede personalizar la configuración de PHP para su entorno mediante una `php.ini` que se adjunta a la configuración mantenida por Adobe Commerce.

En el repositorio, agregue `php.ini` a la raíz de la aplicación (la raíz del repositorio).

>[!TIP]
>
>Configurar PHP incorrectamente puede causar problemas, por lo que solo los administradores avanzados deben configurar estas opciones.

### Aumentar el límite de memoria PHP

Para aumentar el límite de memoria PHP, agregue el siguiente ajuste al `php.ini` archivo:

```ini
memory_limit = 1G
```

Para la depuración, aumente el valor a 2G.

### Optimizar configuración de realpath_cache

Configure lo siguiente `realpath_cache` configuración para mejorar el rendimiento de la aplicación.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Estos ajustes permiten a los procesos de PHP almacenar en caché las rutas a los archivos en lugar de buscarlos para cada carga de página. Consulte [Ajuste del rendimiento](https://www.php.net/manual/en/ini.core.php) en la documentación de PHP.

>[!NOTE]
>
>Para ver una lista de los ajustes de configuración de PHP recomendados, consulte [Configuración de PHP requerida](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) en el _Guía de instalación_.

### Compruebe la configuración personalizada de PHP

Después de empujar el `php.ini` cambios en su entorno de Cloud, puede comprobar que la configuración personalizada de PHP se ha añadido a su entorno. Por ejemplo, utilice SSH para iniciar sesión en el entorno remoto y ver el archivo con algo similar a lo siguiente:

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>Si utiliza Cloud Docker para Commerce para el desarrollo local, consulte [Contenedores de servicio Docker](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) para obtener información sobre el uso de un personalizado `php.ini` en un entorno de Docker.

## Habilitar extensiones

Puede habilitar o deshabilitar las extensiones PHP en el `runtime:extension` sección. Además, las extensiones especificadas están disponibles en los contenedores Docker PHP.

>[!IMPORTANT]
>
>Antes de habilitar las extensiones, es importante comprender que la versión de PHP debe ser compatible con el sistema operativo que aloja el proyecto. Es posible que el entorno del proyecto requiera una actualización del sistema operativo por parte del equipo de infraestructura para poder continuar.

Ejemplo en `.magento.app.yaml` archivo:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Utilice SSH para iniciar sesión en un entorno y enumerar las extensiones PHP.

```bash
php -m
```

Para obtener más información sobre una extensión PHP específica, consulte la [Lista de extensiones PHP](https://www.php.net/manual/en/extensions.alphabetical.php).

La siguiente tabla muestra las extensiones PHP compatibles al implementar Adobe Commerce en la plataforma Cloud.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

Los requisitos del módulo PHP están vinculados a la versión de Adobe Commerce. Consulte [Requisitos de PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Compatibilidad con extensiones

Para proyectos Pro, las siguientes extensiones requieren compatibilidad adicional para su instalación:

- `sourceguardian`

Por ejemplo, para configurar PHP para que ejecute solo scripts protegidos por SourceGuardian en todos los entornos, se debe establecer la siguiente opción en la variable `php.ini` archivo:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Consulte [sección 3.5 de la documentación de SourceGuardian](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Este es un vínculo a un PDF_.

[Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obtener ayuda sobre la instalación de estas extensiones PHP en todos los entornos de producción y entornos de ensayo Pro. Incluya su actualizado `.magento/services.yaml` archivo, `.magento.app.yaml` archivo con la versión actualizada de PHP y cualquier extensión adicional de PHP. Para realizar cambios en un entorno de producción activo, debe proporcionar un aviso mínimo de 48 horas. El equipo de infraestructura en la nube puede tardar hasta 48 horas en actualizar el proyecto.

>[!WARNING]
>
>PHP compilado con debug no es compatible y el Sondeo puede entrar en conflicto con [!DNL XDebug] o [!DNL XHProf]. Deshabilite esas extensiones al habilitar el sondeo. El Sondeo entra en conflicto con algunas extensiones PHP como [!DNL Pinba] o IonCube.
