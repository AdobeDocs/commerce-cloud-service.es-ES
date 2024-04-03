---
title: Redirecciones
description: Obtenga información sobre cómo administrar las reglas de redirección para su proyecto de Adobe Commerce en la nube.
feature: Cloud, Routes
exl-id: 7089a790-6341-4443-990a-df42091f0680
source-git-commit: 649c11b111aa9c9105e54908bf9c6f48741f10e4
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Redirecciones

La administración de reglas de redirección es un requisito común de las aplicaciones web, especialmente en los casos en que no desea perder los vínculos entrantes que han cambiado o se han eliminado con el tiempo.

A continuación se muestra cómo administrar las reglas de redirección en su Adobe Commerce en proyectos de infraestructura en la nube mediante `routes.yaml` archivo de configuración. Si los métodos de redirección mencionados en este tema no funcionan, puede utilizar encabezados de almacenamiento en caché para hacer lo mismo.

{{route-placeholder}}

## Actualizaciones en entornos Pro

{{pro-self-service-warning}}

>[!WARNING]
>
>Para Adobe Commerce en proyectos de infraestructura en la nube, la configuración de numerosos redireccionamientos y reescrituras que no son de regex en `routes.yaml` puede causar problemas de rendimiento. Si su `routes.yaml` tiene 32 KB o más, descargue las redirecciones que no sean de regex y vuelva a escribir en Fastly. Consulte [Descargar redirecciones no regex a Fastly en lugar de Nginx (rutas)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) en el _Centro de ayuda de Adobe Commerce_.

## Redirecciones de ruta completa

Con las redirecciones de ruta completa, puede definir rutas simples mediante las opciones `routes.yaml` archivo. Por ejemplo, puede redirigir de un dominio Apex a un `www` como se indica a continuación:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Redirecciones de ruta parcial

En el `.magento/routes.yaml` , puede agregar reglas de redireccionamiento parciales a rutas existentes basadas en la coincidencia de patrones:

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

Las redirecciones parciales funcionan con cualquier tipo de ruta, incluidas las rutas servidas directamente por la aplicación.

Hay dos claves disponibles en `redirects`:

- **caduca**: opcional, especifica la cantidad de tiempo para almacenar en caché la redirección en el explorador. Algunos ejemplos de valores válidos son `3600s`, `1d`, `2w`, `3m`.

- **rutas**: uno o varios pares de clave-valor que especifican la configuración de las reglas de redireccionamiento de ruta parcial.

  Para cada regla de redirección, la clave es una expresión para filtrar las rutas de solicitud para la redirección. El valor es un objeto que especifica el destino de destino de la redirección y las opciones para procesar la redirección.

  El objeto value tiene las propiedades siguientes:

  | Propiedad | Descripción |
  | ---------- | ----------- |
  | `to` | Obligatorio, una ruta absoluta parcial, una dirección URL con protocolo y host o un patrón que especifique el destino de destino de la regla de redirección. |
  | `regexp` | Opcional, el valor predeterminado es `false`. Especifica si la clave de ruta de acceso debe interpretarse como una expresión regular PCRE. |
  | `prefix` | Especifica si la redirección se aplica tanto a la ruta de acceso como a todas sus rutas secundarias, o solo a la propia ruta de acceso. El valor predeterminado es `true`. Este valor no es compatible si `regexp` es `true`. |
  | `append_suffix` | Determina si el sufijo se transfiere con el redireccionamiento. El valor predeterminado es `true`. Este valor no es compatible si `regexp` la clave es `true` o* si la variable `prefix` la clave es `false`. |
  | `code` | Especifica el código de estado HTTP. Los códigos de estado válidos son [`301` (Movido permanentemente)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8), y [`308`](https://www.rfc-editor.org/rfc/rfc7238). El valor predeterminado es `302`. |
  | `expires` | Opcional, especifica la cantidad de tiempo para almacenar en caché el redireccionamiento en el explorador. El valor predeterminado es `expires` valor definido directamente bajo la variable `redirects` , pero en este nivel puede ajustar la caducidad de la caché para las redirecciones parciales individuales. |

## Ejemplos de redirecciones de ruta parcial

Los siguientes ejemplos muestran cómo especificar redirecciones de ruta parcial en la variable `routes.yaml` archivo con varios `paths` opciones de configuración.

### Coincidencia de patrones de expresión regular

Utilice el siguiente formato para configurar solicitudes de redirección basadas en una expresión regular.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Esta configuración filtra las rutas de solicitud en una expresión regular y redirige las solicitudes coincidentes a `https://example.com`. Por ejemplo, una solicitud a `https://example.com/regexp/a/b/c/match` redirige a `https://example.com/a/b/c`.

### Coincidencia de patrones de prefijo

Utilice el siguiente formato para configurar solicitudes de redirección para rutas que comienzan con un patrón de prefijo especificado.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Esta configuración funciona de la siguiente manera:

- Redirige solicitudes que coinciden con el patrón `/from` a la ruta `http://{default}/to`.

- Redirige solicitudes que coinciden con el patrón `/from/another/path` hasta `https://{default}/to/another/path`.

- Si cambia el `prefix` propiedad a `false`, solicitudes que coinciden con el `/from` el déclencheur de patrones es una redirección, pero las solicitudes que coinciden con el `/from/another/path` el patrón no.

### Coincidencia de patrones de sufijo

Utilice el siguiente formato para configurar solicitudes de redireccionamiento que adjuntan el sufijo de ruta de la solicitud al destino de destino:

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Esta configuración funciona de la siguiente manera:

- Redirige solicitudes que coinciden con el patrón `/from/path/suffix` a la ruta `https://{default}/to`.

- Si cambia el `append_suffix` propiedad a `true`, luego solicitudes que coincidan `/from/path/suffix`  redirigir a la ruta `https://{default}/to/path/suffix`.

### Configuración de caché específica de la ruta

Utilice el siguiente formato para personalizar el tiempo para almacenar en caché una redirección desde una ruta específica:

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
    paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Esta configuración funciona de la siguiente manera:

- Redirecciones desde la primera ruta (`/from`) se almacenan en caché durante un día.

- Redirecciones desde la segunda ruta (`/here`) se almacenan en caché durante dos semanas.
