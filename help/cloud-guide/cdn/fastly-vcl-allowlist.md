---
title: VCL personalizado para permitir solicitudes
description: Filtre las solicitudes entrantes y permita el acceso por dirección IP a los sitios de Adobe Commerce mediante una lista de ACL de Fastly Edge y un fragmento de VCL personalizado.
feature: Cloud, Configuration, Security
exl-id: a6ee958a-c3d3-47be-b2df-510707f551fc
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# VCL personalizado para permitir solicitudes

Puede utilizar una lista ACL de Fastly Edge con un fragmento de código VCL personalizado para filtrar solicitudes entrantes y permitir el acceso por dirección IP. La lista ACL especifica las direcciones IP que se permiten.

Cree una lista de permitidos para limitar el acceso al entorno de ensayo, de modo que solo se permitan las solicitudes de direcciones IP especificadas para desarrolladores internos y servicios externos aprobados. También puede crear una lista de permitidos para proteger el acceso al administrador en los entornos de ensayo y producción.

El siguiente ejemplo muestra cómo utilizar un fragmento de VCL personalizado con una [Lista de control de acceso rápido (ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls) para proteger el acceso al administrador para un entorno de proyecto de Adobe Commerce en la nube. Al agregar el fragmento de VCL personalizado al entorno de Cloud, Fastly solo permite solicitudes de direcciones IP incluidas en la ACL.

>[!TIP]
>
>Para entornos de ensayo e integración que no deben ser de acceso público, utilice la opción de control de acceso HTTP disponible en el [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) para administrar el acceso a todo el sitio por dirección IP.

**Requisitos previos:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista de direcciones IP de cliente para incluir en la lista de permitidos

## Crear una ACL de Edge para permitir direcciones IP de cliente

Las ACL de Edge crean listas de direcciones IP para administrar el acceso al sitio. En este ejemplo, se crea una ACL de Edge y se añade la lista de direcciones IP de cliente permitidas para acceder al administrador del entorno del proyecto.

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Edge ACL**.

1. Cree el contenedor ACL:

   - Clic **Agregar ACL**.

   - En el *Contenedor ACL* página, introduzca un **Nombre de ACL**—`allowlist`.

   - Seleccionar **Activar después del cambio** para implementar los cambios en la versión de la configuración del servicio de Fastly que está editando.

   - Clic **Cargar** para adjuntar la ACL a la configuración del servicio de Fastly.

1. Añada la lista de direcciones IP permitidas para acceder al administrador:

   - Haga clic en el icono Configuración para la `allowlist` ACL.

   - Añada y guarde el *Valor de IP* para cada dirección IP del cliente.

   - Clic **Cancelar** para volver a la página de configuración del sistema.

1. Clic **Guardar configuración**.

1. Actualice la caché según la notificación que aparece en la parte superior de la página.

## Cree el fragmento de VCL personalizado para proteger el acceso de administrador

El siguiente código de fragmento de VCL personalizado (formato JSON) muestra la lógica para filtrar solicitudes al administrador y permitir el acceso si la dirección IP del cliente coincide con una dirección en la `allowlist` ACL.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Antes [crear un fragmento personalizado](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet) en este ejemplo, revise los valores para determinar si necesita realizar algún cambio. A continuación, introduzca cada valor en los campos respectivos, como `type` en el campo Tipo, `content` en el campo Contenido.

- `name` — Nombre del fragmento de VCL. Para este ejemplo, `allowlist`.

- `priority` — determina cuándo se ejecuta el fragmento de VCL. La prioridad es `5` para ejecutar inmediatamente y comprobar si las solicitudes de un administrador provienen de una dirección IP permitida. El fragmento se ejecuta antes que cualquiera de los fragmentos de VCL de Magento predeterminados (`magentomodule_*`) tiene asignada una prioridad de 50. Establezca la prioridad de cada fragmento personalizado por encima o por debajo de 50, según el momento en el que desee que se ejecute el fragmento. Los fragmentos con números de prioridad más bajos se ejecutan primero.

- `type` — especifica una ubicación para insertar el fragmento en el código de VCL con versiones. Este VCL es un `recv` tipo de fragmento que agrega el código de fragmento a `vcl_recv` subrutina debajo del código predeterminado Fastly VCL y encima de cualquier objeto.

- `content` — El fragmento de código VCL que se va a ejecutar. En este ejemplo, el código filtra las solicitudes al administrador y permite el acceso si la dirección IP del cliente coincide con una dirección de `allowlist` ACL. Si la dirección no coincide, la solicitud se bloquea con un `403 Forbidden` error.

  Si la dirección URL del administrador ha cambiado, reemplace el valor de ejemplo `/admin` con la dirección URL de su entorno. Por ejemplo, `/company-admin`.

En el ejemplo de código, la condición `!req.http.Fastly-FF` es importante cuando se utiliza [Blindaje de origen](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). No elimine ni edite este código.

Después de revisar y actualizar el código para su entorno, utilice cualquiera de los siguientes métodos para agregar el fragmento de VCL personalizado a la configuración del servicio de Fastly:

- [Añadir el fragmento de VCL personalizado desde el administrador](#add-the-custom-vcl-snippet). Se recomienda utilizar este método si puede acceder al administrador. (Requiere [Módulo CDN de Fastly para Magento 2 versión 1.2.58](fastly-configuration.md#upgrade) o más tarde.)

- Guarde el ejemplo de código JSON en un archivo (por ejemplo, `allowlist.json`) y [cárguelo mediante la API de Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilice este método si no puede acceder al administrador.

## Añadir el fragmento de VCL personalizado

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Fragmentos de VCL personalizados**.

1. Clic **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `allowlist`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - Añada el **VCL** contenido de fragmento:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Clic **Crear** para generar el archivo de fragmento de VCL con el patrón de nombre `type_priority_name.vcl`, por ejemplo `recv_5_allowlist.vcl`

1. Cuando la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en el *Configuración rápida* para agregar el archivo a la configuración del servicio de Fastly.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

Valida rápidamente la versión actualizada del código VCL durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar el problema. A continuación, vuelva a cargar la VCL.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
