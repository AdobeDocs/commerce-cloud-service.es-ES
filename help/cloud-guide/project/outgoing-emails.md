---
title: Configuración de correos electrónicos salientes
description: Obtenga información sobre cómo habilitar los correos electrónicos salientes para Adobe Commerce en la infraestructura en la nube.
exl-id: 814fe2a9-15bf-4bcb-a8de-ae288fd7f284
source-git-commit: 59f82d891bb7b1953c1e19b4c1d0a272defb89c1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Configuración de correos electrónicos salientes

Puede habilitar y deshabilitar los correos electrónicos salientes para cada entorno desde el [!DNL Cloud Console] o desde la línea de comandos. Habilite los correos electrónicos salientes para los entornos de integración y ensayo a fin de enviar correos electrónicos de autenticación de doble factor o restablecer la contraseña para los usuarios del proyecto en la nube.

De forma predeterminada, los correos electrónicos salientes están habilitados en los entornos Producción y Ensayo. Sin embargo, [!UICONTROL Enable outgoing emails] puede aparecer desactivado en la configuración del entorno hasta que configure la variable `enable_smtp` a través de [línea de comandos](#enable-emails-in-the-cli) o [Consola de nube](outgoing-emails.md#enable-emails-in-the-cloud-console).

Actualización del [!UICONTROL enable_smtp] valor de propiedad por [línea de comandos](#enable-emails-in-the-cli) también cambia el [!UICONTROL Enable outgoing emails] valor de configuración para este entorno en la consola de Cloud.

{{redeploy-warning}}

## Habilitar correos electrónicos en la consola de Cloud

Utilice el **[!UICONTROL Outgoing emails]** alternar en _Configurar el entorno_ ver para habilitar o deshabilitar la compatibilidad con el correo electrónico.

Si los correos electrónicos salientes deben desactivarse o volver a activarse en los entornos de Pro Production o Staging, puede enviar un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Es posible que el estado del correo electrónico saliente no se refleje en los entornos Pro de Cloud Console. En su lugar, utilice el [línea de comandos](#enable-emails-in-the-cli) para activar y probar correos electrónicos salientes.

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

1. Compruebe que SendGrid recoge el correo electrónico.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
