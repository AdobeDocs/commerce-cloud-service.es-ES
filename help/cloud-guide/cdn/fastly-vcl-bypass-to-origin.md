---
title: VCL personalizado para omitir la caché de Fastly
description: Solucione el tráfico de solicitud al servidor de origen creando un fragmento de VCL personalizado para evitar la caché de Fastly.
feature: Cloud, Configuration, Cache
exl-id: a2e9dc57-9b5e-4716-9965-a4324442ad00
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# VCL personalizado para omitir la caché de Fastly

Puede crear un fragmento de VCL personalizado para omitir la caché de Fastly y así poder solucionar problemas del tráfico de solicitud al servidor de origen. Por ejemplo, puede crear un fragmento para determinar si los problemas del sitio se deben al almacenamiento en caché o para solucionar problemas de los encabezados.

Puede configurar el fragmento para que omita el almacenamiento en caché de Fastly para las solicitudes de una dirección IP o URL específica.

>[!NOTE]
>
>Antes de combinar la configuración de VCL personalizada en un entorno de producción, asegúrese de probar el código en el entorno de ensayo.

**Requisitos previos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Para omitir la caché de Fastly en función de la dirección IP o URL**:

{{admin-login-step}}

1. Clic **Tiendas** > Configuración > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Fragmentos de VCL personalizados**.

1. Clic **Crear fragmento personalizado**.

1. Añada los valores de fragmento de VCL:

   - **Nombre** — `bypass_fastly`

   - **Tipo** — `recv`

   - **Prioridad** — `5`

   - **VCL** fragmento de contenido —

     El siguiente ejemplo omite Fastly para una dirección IP específica:

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     El siguiente ejemplo omite Fastly para un patrón de URL específico:

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     Para una coincidencia URL exacta, utilice el `==` en lugar del operador `~` operador. Consulte la [Referencia de VCL de Fastly] para obtener más información.

1. Clic **Crear**.

   ![Crear fragmento de VCL de omisión rápida](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. Cuando la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en el *Configuración rápida* sección.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

   Valida rápidamente la versión de VCL actualizada durante el proceso de carga. Si la validación falla, edite el fragmento de VCL personalizado para solucionar cualquier problema. A continuación, vuelva a cargar la VCL.

Después de agregar el fragmento de VCL, puede utilizar comandos cURL para enviar solicitudes al servidor de origen desde la dirección IP o URL especificada, como se muestra en el siguiente ejemplo:

```bash
curl -svo /dev/null www.example.com/index.html
```

A continuación, inspeccione la respuesta para solucionar los problemas con el contenido no almacenado en caché.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Referencia de VCL de Fastly]: https://docs.fastly.com/vcl/
