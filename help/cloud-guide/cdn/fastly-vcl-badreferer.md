---
title: Bloquear spam de referencia
description: Bloquee el correo no deseado de referencia de su sitio mediante el diccionario Fastly Edge y un fragmento de VCL personalizado.
feature: Cloud, Configuration, Security
exl-id: 665bac93-75db-424f-be2c-531830d0e59a
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Bloquear spam de referencia

El siguiente ejemplo muestra cómo configurar [Diccionario de Fastly Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) con un fragmento de VCL personalizado para bloquear el correo no deseado de referencia de su sitio de Adobe Commerce en la infraestructura de la nube.

>[!NOTE]
>
>Se recomienda añadir configuraciones de VCL personalizadas a un entorno de ensayo en el que pueda probarlas antes de ejecutarlas en el entorno de producción.

**Requisitos previos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Revise los registros del sitio para ver si hay direcciones URL de referencia falsas y haga una lista de dominios para bloquear.

## Creación de una lista de bloqueados de referente

Los diccionarios de Edge crean pares de clave-valor accesibles para las funciones de VCL durante el procesamiento de fragmentos de VCL. En este ejemplo, se crea un diccionario de Edge que proporciona la lista de sitios web de referencia que se van a bloquear.

{{admin-login-step}}

1. Clic **Tiendas** > **Configuración** > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Diccionarios de Edge**.

1. Cree el contenedor Diccionario:

   - Clic **Agregar contenedor**.

   - En el *Contenedor* página, introduzca un **Nombre del diccionario**—`referrer_blocklist`.

   - Seleccionar **Activar después del cambio** para implementar los cambios en la versión de la configuración del servicio de Fastly que está editando.

   - Clic **Cargar** para adjuntar el diccionario a la configuración del servicio de Fastly.

1. Añada la lista de nombres de dominio para bloquear a `referrer_blocklist` diccionario:

   - Haga clic en el icono Configuración para la `referrer_blocklist` diccionario.

   - Agregue y guarde pares de clave-valor en el nuevo diccionario. Para este ejemplo, cada **Clave** es el nombre de dominio de una URL de referente que se va a bloquear y **Valor** es `true`.

     ![Agregar elementos de diccionario de referente incorrectos](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Clic **Cancelar** para volver a la página de configuración del sistema.

1. Clic **Guardar configuración**.

1. Actualice la caché según la notificación que aparece en la parte superior de la página.

Para obtener más información acerca de los diccionarios de Edge, consulte [Crear y usar diccionarios de Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) y [fragmentos de VCL personalizados](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) en la documentación de Fastly.

## Cree un fragmento de VCL personalizado para bloquear el correo no deseado del referente

El siguiente código de fragmento de VCL personalizado (formato JSON) muestra la lógica para comprobar y bloquear solicitudes. El fragmento de VCL captura el host de un sitio web de referente en un encabezado y, a continuación, compara el nombre de host con la lista de direcciones URL de `referrer_blocklist` diccionario. Si el nombre de host coincide, la solicitud se bloquea con un `403 Forbidden` error.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "set req.http.Referer-Host = regsub(req.http.Referer, \"^https?:\/\/?([^:\/s]+).*$\", \"\\1\"); if (table.lookup(referrer_blocklist, req.http.Referer-Host)) { error 403 \"Forbidden\"; }"
}
```

Antes de crear un fragmento basado en este ejemplo, revise los valores para determinar si necesita realizar algún cambio:

- `name` — Nombre del fragmento de VCL. Para este ejemplo, se ha utilizado `block_bad_referrer`.

- `dynamic` — El valor 0 indica un [fragmento normal](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) para cargar en el VCL con versiones para la configuración de Fastly.

- `priority` — determina cuándo se ejecuta el fragmento de VCL. La prioridad es `5` para ejecutar este código de fragmento antes de cualquiera de los fragmentos de VCL de Magento predeterminados (`magentomodule_*`) tiene asignada una prioridad de 50. Establezca la prioridad de cada fragmento personalizado por encima o por debajo de 50, según el momento en el que desee que se ejecute el fragmento. Los fragmentos con números de prioridad más bajos se ejecutan primero.

- `type` — especifica una ubicación para insertar el fragmento en la versión de VCL. En este ejemplo, el fragmento de VCL es un `recv` fragmento. Cuando el fragmento se inserta en la versión de VCL, se añade a la variable `vcl_recv` subrutina, debajo del código predeterminado Fastly VCL y encima de cualquier objeto.

- `content` — El fragmento de código VCL que se ejecutará en una línea, sin saltos de línea.

Después de revisar y actualizar el código para su entorno, utilice cualquiera de los siguientes métodos para agregar el fragmento de VCL personalizado a la configuración del servicio de Fastly:

- [Añadir el fragmento de VCL personalizado desde el administrador](#add-the-custom-vcl-snippet). Se recomienda utilizar este método si puede acceder al administrador. (Requiere [Fastly versión 1.2.58](fastly-configuration.md#upgrade) o más tarde.)

- Guarde el ejemplo de código JSON en un archivo (por ejemplo, `allowlist.json`) y [cárguelo mediante la API de Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilice este método si no puede acceder al administrador.

## Añadir el fragmento de VCL personalizado

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Fragmentos de VCL personalizados**.

1. Clic **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `block_bad_referrer`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - **VCL** fragmento de contenido —

     ```conf
     set req.http.Referer-Host = regsub(req.http.Referer,
     "^https?://?([^:/\s]+).*$", "1");
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Clic **Crear**.

   ![Crear un fragmento VCL de bloque de referente personalizado](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. Cuando la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en el *Configuración rápida* sección.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

Valida rápidamente la versión de VCL actualizada durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar cualquier problema. A continuación, vuelva a cargar la VCL.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
