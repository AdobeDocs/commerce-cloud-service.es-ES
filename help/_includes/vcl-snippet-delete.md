---
source-git-commit: 8f1ed3067f6daed897151052c8b9f987d3df3a50
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Incluir archivo para eliminar VCL personalizado para Fastly

## Eliminar el fragmento de VCL personalizado

1. [Iniciar sesión](/help/get-started/onboarding.md#access-your-admin-panel) al administrador.

1. Clic **Tiendas** > **Configuración** > **Configuración** > **Avanzadas** > **Sistema**.

1. Expandir **Caché de página completa** > **Configuración rápida** > **Fragmentos de VCL personalizados**.

   ![Administrar fragmentos de VCL personalizados](/help/assets/cdn/fastly-manage-snippets.png)

1. En el _Acción_ , haga clic en el icono de papelera situado junto al fragmento que desea eliminar.

1. En la siguiente ventana modal, haga clic en **DELETE** y activar una nueva versión.

>[!WARNING]
>
>El _Fragmentos de VCL personalizados_ La opción IU solo muestra los fragmentos añadidos a través del administrador de Adobe Commerce. Si agrega fragmentos de código mediante la API de Fastly, utilice la API para [administrarlos](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
