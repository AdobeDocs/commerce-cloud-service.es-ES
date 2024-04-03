---
title: Añadir robots de mapa del sitio y de motor de búsqueda
description: Aprenda a añadir robots de mapa del sitio y de motores de búsqueda a Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: b98f43fa-1878-466d-8ea0-1e7207af8b60
source-git-commit: ee1db75c73c086e0ea54e1a7591ca7f2b4d2b36d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# Añadir robots de mapa del sitio y de motor de búsqueda

Un intento de generar y escribir el `sitemap.xml` al directorio raíz da como resultado el siguiente error:

```terminal
Please make sure that "/" is writable by the web-server.
```

Con Adobe Commerce en la infraestructura en la nube, solo puede escribir en directorios específicos, como `var`, `pub/media`, `pub/static`, o `app/etc`. Cuando genere el `sitemap.xml` mediante el panel de administración, debe especificar la variable `/media/` ruta.

No es necesario que genere un `robots.txt` porque genera el archivo `robots.txt` contenido bajo demanda y lo almacena en la base de datos. Puede ver el contenido en su explorador con la variable `<domain.your.project>/robots.txt` o `<domain.your.project>/robots` vínculo.

Esto requiere ECE-Tools versión 2002.0.12 y posterior con un actualizado `.magento.app.yaml` archivo. Consulte un ejemplo de estas reglas en la [repositorio de magento en la nube](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Para generar un `sitemap.xml` en la versión 2.2 y posterior de**:

1. Acceda al administrador.
1. En el _Marketing_ , haga clic en **Mapa del sitio** en el _SEO y búsqueda_ sección.
1. En el _Mapa del sitio_ ver, haga clic en **Añadir mapa del sitio**.
1. En el _Nuevo mapa del sitio_ Consulte e introduzca los siguientes valores:

   - **Nombre de archivo**:`sitemap.xml`
   - **Ruta**:`/media/`

1. Clic **Guardar y generar**. El nuevo mapa del sitio está disponible en la _Mapa del sitio_ rejilla.
1. Haga clic en la ruta en el _Vínculo para Google_ columna.

**Para añadir contenido a `robots.txt` archivo**:

1. Acceda al administrador.
1. En el _Contenido_ , haga clic en **Configuración** en el _Diseño_ sección.
1. En el _Configuración de diseño_ ver, haga clic en **Editar** para el sitio web en _Acción_ columna.
1. En el _Sitio web principal_ ver, haga clic en **Robots del motor de búsqueda**.
1. Actualice el **Editar instrucciones personalizadas de robots.txt** field.
1. Clic **Guardar configuración**.
1. Compruebe el `<domain.your.project>/robots.txt` archivo o `<domain.your.project>/robots` URL en el explorador.

>[!NOTE]
>
>Si la variable `<domain.your.project>/robots.txt` el archivo genera un `404 error`, [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para quitar la redirección de `/robots.txt` hasta `/media/robots.txt`.

## Reescribir utilizando el fragmento de VCL de Fastly

Si tiene dominios diferentes y necesita mapas de sitio independientes, puede crear una VCL para enrutar al mapa del sitio adecuado. Genere el `sitemap.xml` en el panel de administración como se ha descrito anteriormente y, a continuación, cree un fragmento personalizado de VCL de Fastly para administrar la redirección. Consulte [Fragmentos de VCL personalizados de Fastly](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Puede cargar fragmentos de VCL personalizados desde la IU de administración o mediante la API de Fastly. Consulte [Tutoriales y ejemplos de fragmentos de VCL personalizados](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Usar un fragmento de VCL de Fastly para redireccionar

Cree un fragmento de VCL personalizado para reescribir la ruta `sitemap.xml` hasta `/media/sitemap.xml` uso del `type` y `content` pares clave-valor.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

En el siguiente ejemplo se muestra cómo reescribir la ruta de acceso para `robots.txt` y `sitemap.xml` hasta `/media/robots.txt` y `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Para utilizar un fragmento de VCL de Fastly para una redirección de dominio particular**:

Crear un `pub/media/domain_robots.txt` , donde se encuentra el dominio `domain.com`y utilice el siguiente fragmento de VCL:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

El fragmento de VCL enruta `http://domain.com/robots.txt` y presenta el `pub/media/domain_robots.txt` archivo.

Para configurar una redirección para `robots.txt` y `sitemap.xml` en un solo fragmento, cree `pub/media/domain_robots.txt` y `pub/media/domain_sitemap.xml` archivos, donde el dominio es `domain.com` y utilice el siguiente fragmento de VCL:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

En el `sitemap` admin config, debe especificar la ubicación del archivo utilizando `pub/media/` en lugar de `/`.

### Configurar la indexación por motor de búsqueda

Para activar `robots.txt` personalizaciones, debe habilitar la variable **La indexación por motores de búsqueda está activada para`<environment-name>`** en la configuración del proyecto.

![Utilice el [!DNL Cloud Console] para administrar entornos](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>Si utiliza PWA Studio y no puede acceder a la configuración de `robots.txt` archivo, agregar `robots.txt` a la [Lista de permitidos de nombre](https://github.com/magento/magento2-upward-connector#front-name-allowlist) en **Tiendas** > Configuración > **General** > **Web** > Configuración del PWA ASCENDENTE.
