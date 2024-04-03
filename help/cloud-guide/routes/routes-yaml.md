---
title: Configuración de rutas
description: Obtenga información sobre cómo definir las rutas para las solicitudes de HTTPS entrantes para Adobe Commerce en entornos de infraestructura en la nube.
feature: Cloud, Configuration, Routes
exl-id: a33797e5-14cc-45eb-a048-96180b872a4a
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Configuración de rutas

El `routes.yaml` archivo en el `.magento/routes.yaml` define rutas para su Adobe Commerce en los entornos de integración, ensayo y producción de la infraestructura en la nube. Las rutas determinan cómo la aplicación procesa las solicitudes HTTP y HTTPS entrantes.

El valor predeterminado `routes.yaml` Este archivo especifica las plantillas de ruta para procesar solicitudes HTTP como HTTPS en proyectos que tienen un único dominio predeterminado y en proyectos configurados para varios dominios:

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Utilice el `magento-cloud` CLI para ver una lista de las rutas configuradas:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Actualizaciones de configuración para entornos Pro

{{pro-self-service-warning}}

## Plantillas de ruta

El `routes.yaml` es una lista de rutas con plantillas y sus configuraciones. Puede utilizar los siguientes marcadores de posición en las plantillas de ruta:

- El `{default}` marcador de posición representa el nombre de dominio completo configurado como predeterminado para el proyecto.

  Por ejemplo, un proyecto con el dominio predeterminado `example.com` y las siguientes plantillas de ruta:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Estas plantillas se resuelven en las siguientes direcciones URL en un entorno de producción:

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- El `{all}` Un marcador de posición representa todos los nombres de dominio configurados para el proyecto.

  Por ejemplo, un proyecto con `example.com` y `example1.com` dominios con las siguientes plantillas de ruta:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Estas plantillas se resuelven en las siguientes rutas para todos los dominios del proyecto:

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  El `{all}` el marcador de posición es útil para proyectos configurados para varios dominios. En una rama que no sea de producción, `{all}` se reemplaza por el ID de proyecto y el ID de entorno de cada dominio.

  Si un proyecto no tiene ningún dominio configurado, lo que es común durante el desarrollo, la variable `{all}` placeholder se comporta de la misma manera que el `{default}` marcador.

Adobe Commerce también genera rutas para cada entorno de integración activo. Para entornos de integración, la variable `{default}` el marcador de posición se reemplaza con el siguiente nombre de dominio:

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

Por ejemplo, la variable `refactorcss` rama de para `mswy7hzcuhcjw` proyecto alojado en el `us` el clúster tiene el dominio siguiente:

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Si el proyecto de Cloud admite varias tiendas, siga las instrucciones de configuración de ruta de [varios sitios web o tiendas](../store/multiple-sites.md).

### Barra final

Las definiciones de ruta contienen una barra diagonal para indicar una carpeta o directorio; sin embargo, el mismo contenido se puede proporcionar con o sin una barra diagonal. Las siguientes direcciones URL resuelven lo mismo, pero se pueden interpretar como _dos diferentes_ URL:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>Se recomienda utilizar una barra diagonal para los directorios, pero sea cual sea el método que elija, es importante **mantener la coherencia** para evitar la generación de dos ubicaciones.

## Protocolos de ruta

Todos los entornos admiten HTTP y HTTPS automáticamente.

- Si la configuración solo especifica la ruta HTTP, las rutas HTTPS se crean automáticamente, lo que permite que el sitio se proporcione desde HTTP y HTTPS sin requerir redirecciones.

  Por ejemplo, un proyecto con el dominio predeterminado `example.com` y la siguiente plantilla de ruta:

  ```text
  http://{default}/
  ```

  Esta plantilla responde a las siguientes direcciones URL:

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Si la configuración especifica solo la ruta HTTPS, todas las solicitudes HTTP se redirigen a HTTPS.

  Por ejemplo, un proyecto con el dominio predeterminado `example.com` con la siguiente plantilla de ruta:

  ```text
  https://{default}/
  ```

  Esta plantilla responde a la siguiente URL:

  ```text
  https://example.com/
  ```

  También procesa la siguiente redirección:

  `http://example.com/` ==> `https://example.com/`

Enviar todas las páginas a través de TLS. Para esta configuración, debe configurar las redirecciones de todas las solicitudes sin cifrar al equivalente TLS mediante uno de los siguientes métodos:

- Cambie el protocolo a HTTPS en la `routes.yaml` archivo.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- Para los entornos de Ensayo y Producción, habilite la variable [Forzar TLS en Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html) de la IU de administración. Cuando se utiliza esta opción, Fastly gestiona la redirección a HTTPS, por lo que no tiene que actualizar el `routes.yaml` configuración.

## Opciones de ruta

Configure cada ruta por separado con las siguientes propiedades:

| Propiedad | Descripción |
| ---------------- | ----------- |
| `type: upstream` | Sirve a una aplicación. Además, tiene un `upstream` que especifica el nombre de la aplicación (tal como se define en `.magento.app.yaml`) seguido del `:http` punto final. |
| `type: redirect` | Redirige a otra ruta. Le siguen las etiquetas `to` , que es una redirección HTTP a otra ruta identificada por su plantilla. |
| `cache:` | Controles [almacenamiento en caché para la ruta](caching.md). |
| `redirects:` | Controles [reglas de redirección](redirects.md). |
| `ssi:` | Controla la activación de [Server Side Includes](server-side-includes.md). |

## Rutas simples

En los ejemplos siguientes, la configuración de ruta enruta el dominio Apex y la variable `www` subdominio a `mymagento` aplicación. Esta ruta no redirige las solicitudes HTTPS.

**Ejemplo 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

En este ejemplo, el enrutamiento de solicitud sigue estas reglas:

- El servidor responde directamente a las solicitudes con el siguiente patrón de URL:

  ```text
  http://example.com/path
  ```

- El servidor emite un _Redirección 301_ para solicitudes con el siguiente patrón de URL:

  ```text
  http://www.example.com/mypath
  ```

  Estas solicitudes se redirigen al dominio Apex, por ejemplo:

  ```text
  http://example.com/mypath
  ```

En el ejemplo siguiente, la configuración de ruta no redirige las direcciones URL del dominio www al dominio apex. En su lugar, las solicitudes se proporcionan desde el dominio www y Apex.

**Ejemplo 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Rutas comodín

Adobe Commerce en la infraestructura de la nube admite rutas comodín, por lo que puede asignar varios subdominios a la misma aplicación. Esto funciona para rutas de redireccionamiento y de subida. La ruta se codifica con un asterisco (\*). Por ejemplo, las siguientes rutas a la misma aplicación:

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Funciona como un dominio global en un entorno en directo.

### Enrutamiento de un dominio no asignado

Puede enrutar a un sistema que no esté asignado a un dominio mediante un punto (`.`) para separar el subdominio.

**Ejemplo:**

Un proyecto con una `add-theme` la rama enruta a la siguiente URL:

```text
http://add-theme-projectID.us.magento.com/
```

Si define la siguiente plantilla de ruta:

```text
http://www.{default}/
```

La ruta responde a la siguiente URL:

```text
http://www.add-theme-projectID.us.magento.com/
```

Puede insertar cualquier subdominio antes del punto y la ruta se resolverá.

**Ejemplo:**

Defina la siguiente plantilla de ruta:

```text
http://*.{default}/
```

Esta plantilla resuelve las dos direcciones URL siguientes:

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

Puede ver el patrón de ruta para los dominios no asignados estableciendo una conexión SSH con el entorno y utilizando `magento-cloud` CLI para enumerar las rutas:

```terminal
web@mymagento.0:~$ vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Redirecciones y almacenamiento en caché

Como se analiza en más detalle en [Redirecciones](redirects.md), puede administrar reglas de redirección complejas, como _redirecciones parciales_ y especifique reglas para rutas basadas en rutas [almacenamiento en caché](caching.md):

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
