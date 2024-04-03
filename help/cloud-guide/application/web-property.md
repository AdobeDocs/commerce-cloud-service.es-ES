---
title: Propiedad web
description: Consulte ejemplos sobre cómo configurar la propiedad web en la [!DNL Commerce] archivo de configuración de la aplicación.
feature: Cloud, Configuration
exl-id: 2ca94908-6fe1-42fd-bc3b-ef2dd473f1bb
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Propiedad web

El `web` define cómo se expone la aplicación a la web (en HTTP), determina cómo la aplicación web proporciona contenido y controla cómo el contenedor de la aplicación responde a las solicitudes entrantes estableciendo reglas en cada ubicación _bloquear_. Un bloque representa una ruta absoluta que lleva a una barra diagonal (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

Puede ajustar su `locations` mediante los siguientes valores clave para cada `locations` block:

| Atributo | Descripción |
| :--- | :--- |
| `allow` | Proporcione archivos que no coincidan con las &quot;reglas&quot;. Valor predeterminado = `true` |
| `expires` | Establezca el número de segundos para almacenar en caché el contenido en el explorador. Esta clave habilita el `cache-control` y `expires` encabezados para contenido estático. Si no se establece este valor, la variable `expires` y los encabezados resultantes no se incluyen al servir archivos de contenido estático. Un negativo 1 (`-1`) valor no genera almacenamiento en caché y es el valor predeterminado. Puede expresar el valor de tiempo con las siguientes unidades:  `ms` (milisegundos), `s` (segundos), `m` (minutos), `h` (horas), `d` (días), `w` (semanas), `M` (meses, 30d), o `y` (años, 365d) |
| `headers` | Establecer encabezados personalizados, como `X-Frame-Options`, para el contenido estático servido desde esta ubicación. |
| `index` | Enumerar los archivos estáticos para servir a la aplicación, como el `index.html` archivo. Esta clave espera una colección. Esto solo funciona si el acceso al archivo o archivos está &quot;permitido&quot; por el `allow` o `rules` clave para esta ubicación. |
| `rules` | Especificar invalidaciones para una ubicación. Utilice una expresión regular para que coincida con una solicitud. Si una solicitud entrante coincide con la regla, las claves utilizadas en ella anulan la gestión regular de la solicitud. |
| `passthru` | Configure la URL utilizada en caso de que no se encuentre un archivo estático o PHP. Normalmente, esta dirección URL es el controlador principal para las aplicaciones, como `/index.php` o `/app.php`. |
| `root` | Establezca la ruta relativa a la raíz de la aplicación que se expone en la web. El directorio público (ubicación &quot;/&quot;) para un proyecto de Cloud está establecido en &quot;pub&quot; de forma predeterminada. |
| `scripts` | Permitir la carga de scripts en esta ubicación. Establezca el valor en `true` para permitir scripts. |

La configuración predeterminada permite lo siguiente:

- Desde la raíz (`/`), solo se puede acceder a la web y a los medios
- Desde el `~/pub/static` y `~/pub/media` rutas, se puede acceder a cualquier archivo

El siguiente ejemplo muestra la configuración predeterminada en la variable `.magento.app.yaml` para un conjunto de ubicaciones accesibles para la web asociadas con una entrada en el  [`mounts` propiedad](properties.md#mounts):

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>Este ejemplo muestra la configuración web predeterminada de un proyecto en la nube configurado para admitir un solo dominio. Para un proyecto que requiere compatibilidad con varios sitios web o tiendas, la variable `web` La configuración de debe estar configurada para admitir dominios compartidos. Consulte [Configuración de ubicaciones para dominios compartidos](../store/multiple-sites.md#configure-locations-for-shared-domains).
