---
title: Configuración de correos electrónicos salientes
description: Obtenga información sobre cómo habilitar los correos electrónicos salientes para Adobe Commerce en la infraestructura en la nube.
exl-id: 814fe2a9-15bf-4bcb-a8de-ae288fd7f284
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Configuración de correos electrónicos salientes

Puede habilitar y deshabilitar los correos electrónicos salientes para cada entorno desde el [!DNL Cloud Console] o desde la línea de comandos. Habilite los correos electrónicos salientes para los entornos de integración y ensayo a fin de enviar correos electrónicos de autenticación de doble factor o restablecer la contraseña para los usuarios del proyecto en la nube.

De forma predeterminada, el correo electrónico saliente está habilitado en los entornos de producción. El [!UICONTROL Enable outgoing emails] puede aparecer desactivado en la configuración del entorno independientemente del estado hasta que configure el [`enable_smtp` propiedad](#enable-emails-in-the-cli).

{{redeploy-warning}}

## Habilitar correos electrónicos en [!DNL Cloud Console]

Utilice el **[!UICONTROL Outgoing emails]** alternar en _Configurar el entorno_ ver para habilitar o deshabilitar la compatibilidad con el correo electrónico.

**Para administrar la compatibilidad de correo electrónico desde[!DNL Cloud Console]**:

1. Inicie sesión en [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleccione un proyecto del _Todos los proyectos_ lista.
1. En el panel Proyecto, haga clic en el icono de configuración en la esquina superior derecha.
1. Clic **[!UICONTROL Environments]** y seleccione un entorno específico de la lista.
1. Para habilitar o deshabilitar los correos electrónicos salientes, cambie _Habilitar correos electrónicos salientes_ **Activado** o **Desactivado**.

   ![Habilitar configuración de correo electrónico saliente](../../assets/outgoing-emails.png)

Después de cambiar la configuración, el entorno se genera e implementa con la nueva configuración.

## Habilitar correos electrónicos en la CLI

Puede cambiar la configuración de correo electrónico de un entorno activo mediante la variable `magento-cloud` CLI `environment:info` para establecer el `enable_smtp` propiedad. Al habilitar SMTP, se actualiza el `MAGENTO_CLOUD_SMTP_HOST` variable de entorno con la dirección IP del host SMTP para enviar correo.

**Para administrar la compatibilidad con el correo electrónico desde la línea de comandos**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Compruebe la configuración del correo electrónico saliente para el entorno.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Cambie la configuración de soporte de correo electrónico estableciendo las `enable_smtp` variable de entorno a `true` o `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Espere a que el entorno se cree e implemente.

1. Utilice un SSH para iniciar sesión en el entorno remoto.

1. Compruebe que el correo electrónico funciona; envíe un correo electrónico de prueba a una dirección que pueda comprobar.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```
