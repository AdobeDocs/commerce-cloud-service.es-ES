---
title: Configurar varios sitios web o tiendas
description: Obtenga información sobre cómo configurar varios sitios web o tiendas para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 16e932ef-f083-44d7-977f-0c78899e151a
source-git-commit: 85aa54af10e7ea44adde5403b69ff03d4a0c622f
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Configurar varios sitios web o tiendas

Puede configurar Adobe Commerce para que tenga varios sitios web o tiendas, como una tienda en inglés, una tienda en francés y una tienda en alemán. Consulte [Explicación de sitios web, tiendas y vistas de tiendas](best-practices.md#store-views).

>[!WARNING]
>
>Los datos de catálogo se amplían a medida que aumenta el número de sitios web y tiendas. Según la arquitectura del proyecto, las tiendas adicionales pueden provocar un proceso de indexación más largo y tiempos de respuesta más lentos para las páginas de catálogo no almacenadas en caché. El Adobe recomienda supervisar de cerca el rendimiento del sitio.

El proceso para configurar varios almacenes depende de si elige utilizar dominios únicos o compartidos.

Varios almacenes con dominios únicos:

```terminal
https://first.store.com/
https://second.store.com/
```

Varios almacenes con el mismo dominio:

```terminal
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Para agregar una vista de tienda a la dirección URL base del sitio, no es necesario crear varios directorios. Consulte [Añadir el código de tienda a la URL base](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) en el _Guía de configuración_.

## Añadir dominios

Los dominios personalizados se pueden añadir a Pro Staging y a cualquier entorno de producción; no se pueden añadir a entornos de integración.

El proceso para agregar un dominio depende del tipo de cuenta de Cloud:

- Para ensayo y producción profesionales

  Añadir el nuevo dominio a Fastly, consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains), o abra un ticket de asistencia para solicitar ayuda. Además, debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar que se agreguen nuevos dominios a un clúster.

- Solo para producción inicial

  Añadir el nuevo dominio a Fastly, consulte [Administrar dominios](../cdn/fastly-custom-cache-configuration.md#manage-domains), o [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar ayuda. Además, debe agregar el nuevo dominio a **Domains** en la pestaña [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Configuración de la instalación local

Para configurar la instalación local de modo que utilice varios almacenes, consulte [Varios sitios web o tiendas][config-multiweb] en el _Guía de configuración_.

Después de crear y probar correctamente la instalación local para utilizar varias tiendas, debe preparar su entorno de integración:

1. **Configuración de rutas o ubicaciones**: especifique cómo Adobe Commerce gestiona las direcciones URL entrantes

   - [Rutas para dominios independientes](#configure-routes-for-separate-domains)
   - [Ubicaciones para dominios compartidos](#configure-locations-for-shared-domains)

1. **Configuración de sitios web, tiendas y vistas de tiendas**: configurar mediante la IU de administración de Adobe Commerce
1. **Modificar variables**: permite especificar los valores del `MAGE_RUN_TYPE` y `MAGE_RUN_CODE` variables en la variable `magento-vars.php` archivo
1. **Implementación y prueba de entornos**—implementar y probar el `integration` ramificación

>[!TIP]
>
>Puede utilizar un entorno local para configurar varios sitios web o tiendas. Consulte las instrucciones de Cloud Docker para [Configurar varios sitios web o tiendas](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/).

### Actualizaciones de configuración para entornos Pro

{{pro-self-service-warning}}

### Configuración de rutas para dominios independientes

Las rutas definen cómo procesar las direcciones URL entrantes. Si hay varios almacenes con dominios únicos, es necesario definir cada dominio en la variable `routes.yaml` archivo. La forma de configurar las rutas depende de cómo desee que funcione el sitio.

**Para configurar rutas en un entorno de integración**:

1. En la estación de trabajo local, abra `.magento/routes.yaml` en un editor de texto.

1. Defina el dominio y los subdominios. El `mymagento` el valor de flujo ascendente es el mismo valor que la propiedad de nombre en la variable `.magento.app.yaml` archivo.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Guarde los cambios en `routes.yaml` archivo.

1. Continuar a [Configuración de sitios web, tiendas y vistas de tiendas](#set-up-websites-stores-and-store-views).

### Configuración de ubicaciones para dominios compartidos

Donde la configuración de rutas define cómo se procesan las direcciones URL, la variable `web` propiedad en el `.magento.app.yaml` define cómo la aplicación se expone a la web. Web _ubicaciones_ permiten una mayor granularidad en las solicitudes entrantes. Por ejemplo, si el dominio es `store.com`, puede utilizar `/first` (sitio predeterminado) y `/second` para solicitudes a dos almacenes diferentes que comparten un dominio.

**Para configurar una nueva ubicación web**:

1. Cree un alias para la raíz (`/`). En este ejemplo, el alias es `&app` en la línea 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Crear un paso a través para el sitio web (`/website`) y haga referencia a la raíz con el alias del paso anterior.

   El alias permite `website` para acceder a los valores desde la ubicación raíz. En este ejemplo, el sitio web `passthru` está en la línea 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Para configurar una ubicación con un directorio diferente**:

1. Cree un alias para la raíz (`/`) y para la estática (`/static`) ubicaciones.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Cree un subdirectorio para el sitio web en `pub` directorio: `pub/<website>`

1. Copie el `pub/index.php` en el archivo `pub/<website>` y actualice el `bootstrap` ruta (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Crear un paso a través para `index.php` archivo.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Confirme y envíe los archivos modificados.

   - `pub/<website>/index.php` (Si este archivo se encuentra en `.gitignore`, la notificación push puede requerir la opción force ).
   - `.magento.app.yaml`

### Configuración de sitios web, tiendas y vistas de tiendas

En el _IU de administración_, configure su Adobe Commerce **Sitios web**, **Tiendas**, y **Vistas de tienda**. Consulte [Configure varios sitios web, tiendas y vistas de tiendas en el Administrador de](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) en el _Guía de configuración_.

Es importante utilizar el mismo nombre y código de sus sitios web, tiendas y vistas de tiendas del administrador al configurar la instalación local. Necesita estos valores cuando actualice el `magento-vars.php` archivo.

### Modificar variables

En lugar de configurar un host virtual NGINX, pase el `MAGE_RUN_CODE` y `MAGE_RUN_TYPE` variables que utilizan el `magento-vars.php` en el directorio raíz del proyecto.

**Para pasar variables mediante `magento-vars.php` archivo**:

1. Abra el `magento-vars.php` en un editor de texto.

   El [predeterminado `magento-vars.php` archivo](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) debe tener el aspecto siguiente:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Mover el elemento comentado `if` bloquear para que sea _después_ el `function` bloqueo y ya no se comentan.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Reemplace los siguientes valores en la variable `if (isHttpHost("example.com"))` block:
   - `example.com`: con la URL base de su _sitio web_
   - `default`: con el CÓDIGO único para su _sitio web_ o _vista de tienda_
   - `store`: con uno de los siguientes valores:
      - `website`: cargue el _sitio web_ en la tienda
      - `store`: carga una _vista de tienda_ en la tienda

   Para varios sitios con dominios únicos:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Para varios sitios con el mismo dominio, debe comprobar el _host_ y el _URI_:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Guarde los cambios en `magento-vars.php` archivo.

### Implementación y pruebas en el servidor de integración

Inserte los cambios en su entorno de integración de Adobe Commerce en la infraestructura de la nube y pruebe su sitio.

1. Agregar, confirmar y enviar cambios de código a la rama remota.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Espere a que se complete la implementación.

1. Después de la implementación, abra la URL de la tienda en un explorador web.

   Con un dominio único, utilice el formato: `http://<magento-run-code>.<site-URL>`

   Por ejemplo, `http://french.master-name-projectID.us.magentosite.cloud/`

   Con un dominio compartido, utilice el formato: `http://<site-URL>/<magento-run-code>`

   Por ejemplo, `http://master-name-projectID.us.magentosite.cloud/french/`

1. Pruebe exhaustivamente el sitio y combine el código con la variable `integration` para una implementación posterior.

## Implementar en ensayo y producción

Siga el proceso de implementación para [implementación en Ensayo y producción](../deploy/staging-production.md). Para los entornos Starter y Pro, se utiliza la variable [!DNL Cloud Console] para insertar código en entornos.

El Adobe recomienda realizar todas las pruebas en el entorno de ensayo antes de pasar al entorno de producción. Realice cambios en el código en el entorno de integración e inicie el proceso para implementar de nuevo en todos los entornos.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
