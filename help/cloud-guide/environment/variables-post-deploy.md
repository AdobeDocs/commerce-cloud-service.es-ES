---
title: Variables posteriores a la implementación
description: Consulte la lista de variables de entorno que controlan las acciones en la fase posterior a la implementación de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: 8b02757591c4e8f607e936de4eda74d76953d9b7
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variables posteriores a la implementación

Lo siguiente _posterior a la implementación_ las variables controlan las acciones en la fase posterior a la implementación y pueden heredar y anular los valores de [Variables globales](variables-global.md). Inserte estas variables en la variable `post-deploy` fase de la `.magento.env.yaml` archivo:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Para obtener más información sobre cómo personalizar el proceso de generación e implementación:

- [Configuración de implementación](configure-env-yaml.md)
- [Proceso de implementación](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Predeterminado**— `[]` (una matriz vacía)
- **Versión**: Adobe Commerce 2.1.4 y posterior

Configurar _Tiempo hasta el primer byte_ (TTFB) Pruebas para páginas especificadas para probar el rendimiento del sitio. Especifique una referencia de ruta absoluta o una dirección URL con protocolo y host para cada página que requiera la prueba.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Después de especificar las páginas para probar y confirmar los cambios, la variable _Tiempo hasta el primer byte_ la prueba se ejecuta durante la fase posterior a la implementación y publica los resultados de cada ruta en el registro de la nube:

```terminal
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Para las rutas redirigidas, el registro indica la ruta del destino de redirección en lugar de la configurada en la variable de entorno. Si especifica una ruta no válida, el registro muestra un mensaje de advertencia.

## `WARM_UP_CONCURRENCY`

- **Predeterminado**—_Sin configurar_
- **Versión**: Adobe Commerce 2.1.4 y posterior

Especifique el límite de solicitudes simultáneas que se enviarán durante las operaciones de calentamiento de caché para reducir la carga del servidor. Este valor limita el número de conexiones paralelas y es útil para configuraciones de entorno en las que la variable `WARM_UP_PAGES` la variable posterior a la implementación especifica varias páginas para la precarga de la caché.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Predeterminado**— `index.php`
- **Versión**: Adobe Commerce 2.1.4 y posterior

Personalice la lista de páginas utilizadas para cargar previamente la caché en la `post_deploy` escenario. Debe configurar el vínculo posterior a la implementación. Consulte la [sección de enlaces](../application/hooks-property.md) de la `.magento.app.yaml` archivo.

- **páginas únicas**: permite especificar una sola página para añadirla a la caché. No es necesario indicar la dirección URL base predeterminada. El siguiente ejemplo almacena en caché el `BASE_URL/index.php` página:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **varios dominios**: permite enumerar varias direcciones URL. El siguiente ejemplo almacena en caché páginas de dos dominios:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **varias páginas**: utilice el siguiente formato para almacenar en caché varias páginas según un patrón de expresión regular específico:

  ```terminal
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: posibles variantes `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: utilice un `regexp` patrón o coincidencia exacta `url` para filtrar las direcciones URL, o utilice un asterisco (\*) para todas las páginas. Utilice el SKU del producto para `product` tipo de entidad
   - `store_id|store_code`: utilice el ID o el código de la tienda o un asterisco (\*) para todas las tiendas, puede pasar varios ID de tienda o códigos separados con `|`

  El siguiente ejemplo almacena en caché para `category` y `cms-page` tipos de entidades en función de estos criterios:
   - todas las páginas de categorías de la tienda con identificador `1`
   - todas las páginas de categorías de las tiendas con código `store1` y `store2`
   - página de categoría `cars` para tienda con código `store_en`
   - página de cms `contact` para todas las tiendas
   - página de cms `contact` para tiendas con ID `1` y `2`
   - cualquier página de categoría que contenga `car_` y termina por `html` para tienda con ID 2
   - cualquier página de categoría que contenga `tires_` para tienda con código `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  El siguiente ejemplo almacena en caché para `product` tipo de entidad basado en estos criterios:
   - todos los productos para todas las tiendas (con una programación limitada a 100 por tienda para evitar problemas de rendimiento)
   - todos los productos para tienda `store1`
   - productos con `sku1` para todas las tiendas
   - productos con `sku1` para tiendas con código `store1` y `store2`
   - productos con `sku1`, `sku2` y `sku3` para tiendas con código `store1` y `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  El siguiente ejemplo almacena en caché para `store-page` tipo de entidad basado en estos criterios:
   - página `/contact-us` para todas las tiendas
   - página `/contact-us` para tienda con ID `1`
   - página `/contact-us` para tiendas con código `code1` y `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
