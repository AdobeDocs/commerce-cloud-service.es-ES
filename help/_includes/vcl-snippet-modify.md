---
source-git-commit: 2d902a3926c6bbc6a9dc8afcbd667eddeaf3be7e
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Incluir archivo para modificar VCL personalizado para Fastly

## Modificación del fragmento de VCL personalizado

1. [Iniciar sesión](/help/get-started/onboarding.md#access-your-admin-panel) al administrador.

1. Clic **Tiendas** > **Configuración** > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Fragmentos de VCL personalizados**.

   ![Administrar fragmentos de VCL personalizados](/help/assets/cdn/fastly-manage-snippets.png)

1. En el _Acción_ , haga clic en el icono de configuración situado junto al fragmento de código que desea editar.

1. Cuando la página se vuelva a cargar, haga clic en **Cargar VCL a Fastly** en el _Configuración rápida_ sección.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

>[!WARNING]
>
>El _Fragmentos de VCL personalizados_ La opción IU solo muestra los fragmentos añadidos a través del administrador de Adobe Commerce. Si agrega fragmentos de código mediante la API de Fastly, utilice la API para [administrarlos](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
