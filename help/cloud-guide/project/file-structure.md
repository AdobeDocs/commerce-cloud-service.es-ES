---
title: Estructura del proyecto
description: Obtenga información acerca de la estructura de archivos y las plantillas de proyecto para Adobe Commerce en la infraestructura en la nube.
exl-id: 6dc559bd-116b-4745-a85b-731508e113ff
source-git-commit: 47ef728ea46b137eeaabbdbada940143d8773ef0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Estructura del proyecto

Un proyecto de Adobe Commerce en la nube incluye archivos esenciales para las credenciales y la configuración de la aplicación. Estos archivos están disponibles en como plantilla según la versión de Adobe Commerce. Consulte las plantillas en la nube basadas en la versión de Adobe Commerce en la [`magento/magento-cloud` Repositorio de GitHub](https://github.com/magento/magento-cloud).

En la tabla siguiente se describen los archivos incluidos en un proyecto de la nube:

| Archivo | Descripción |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Archivo de configuración que redirige `www` al dominio apex y `php` aplicación para servir HTTP. Consulte [Configuración de rutas](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Archivo de configuración que define una instancia de MySQL (MariaDB), Redis y OpenSearch o un Elasticsearch. Consulte [Configurar servicios](../services/services-yaml.md). |
| `/app` | El `code` se utiliza para módulos personalizados. El `design` La carpeta se utiliza para [temáticas personalizadas](../store/custom-theme.md). El `etc` contiene archivos de configuración para la aplicación. |
| `/m2-hotfixes` | Se utiliza para parches personalizados. |
| `/update` | Carpeta de servicio utilizada por el módulo de soporte técnico. |
| `.gitignore` | Especifique los archivos y directorios que desea omitir. Consulte [`.gitignore` reference](#ignoring-files). |
| `.magento.app.yaml` | Archivo de configuración que define las propiedades para generar la aplicación. Consulte [Configurar aplicación](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Archivo de configuración para las fases de compilación, implementación y posterior implementación. El `ece-tools` incluye una muestra de este archivo. Consulte [Configuración de entornos](../environment/configure-env-yaml.md). |
| `composer.json` | Obtiene Adobe Commerce y los scripts de configuración para preparar la aplicación. Consulte [Paquetes necesarios](../development/overview.md#required-packages). |
| `composer.lock` | Almacena las dependencias de versión de cada paquete. Consulte [Paquetes necesarios](../development/overview.md#required-packages). |
| `magento-vars.php` | Se utiliza para definir [varias tiendas](../store/multiple-sites.md) y sitios que utilizan variables. |

{style="table-layout:auto"}

>[!NOTE]
>
>Al insertar los cambios locales en el servidor remoto, el script de implementación utiliza los valores definidos por los archivos de configuración en `.magento` y, a continuación, la secuencia de comandos elimina el directorio y su contenido. Su entorno de desarrollo local no se ve afectado.

## Directorio raíz de la aplicación

La ubicación del directorio raíz de la aplicación depende del entorno.

- **Integración de Starter y Pro**: `/app`
- **Producción inicial**: `/<project-ID>`
- **Ensayo profesional**: `/<project-ID>_stg`
- **Producción profesional**: `/<project-ID>`

### Directorios grabables

Los entornos remotos Integration, Staging y Production son de solo lectura. Los siguientes directorios son los *solamente* directorios editables por motivos de seguridad:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>En los entornos de Producción y Ensayo, cada nodo del clúster de tres nodos tiene un `/tmp` que no se comparte con los otros nodos.

## Omitir archivos

Hay una base `.gitignore` archivo con el repositorio de proyectos de Adobe Commerce en la nube. Consulte las últimas [archivo .gitignore en el repositorio de magento en la nube](https://github.com/magento/magento-cloud/blob/master/.gitignore). Para agregar un archivo que se encuentra en `.gitignore` , puede usar el complemento `-f` Opción (forzar) al almacenar en zona intermedia una confirmación:

```bash
git add <path/filename> -f
```

## Cambiar plantilla base

Puede seguir los siguientes pasos para cambiar la estructura de un proyecto existente y reflejar la plantilla base más reciente para Adobe Commerce en la infraestructura en la nube.

1. Clone el proyecto en una estación de trabajo local.

1. Actualice el `composer.json` archivo con los siguientes valores para `extra` sección.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Añada el `.gitignore` archivo diseñado para la plantilla base. Por ejemplo, si necesita la variable `.gitignore` para la plantilla versión 2.2.6, utilice el archivo [.gitignore para 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) como referencia.

1. Borre la caché de Git.

   ```bash
   git rm -r --cached .
   ```

1. Agregar y confirmar cambios.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
