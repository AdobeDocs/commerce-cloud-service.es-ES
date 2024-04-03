---
title: Acceda a su panel de administración de Commerce
description: Obtenga información sobre cómo acceder a su Panel de administración de Commerce.
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Acceda a su panel de administración de Commerce

Los usuarios que tienen acceso administrativo al Panel de administración de Commerce pueden agregar usuarios, configurar servicios de tienda, completar el trabajo de configuración y personalización de la tienda, etc.

En un proyecto nuevo, el primer paso después de recibir el correo electrónico de bienvenida es proteger el acceso de los administradores al proyecto cambiando la contraseña en la cuenta del propietario de la licencia. El nombre de usuario predeterminado para esta cuenta es la dirección de correo electrónico del propietario de la licencia.

Puede enviar una solicitud de cambio de contraseña utilizando cualquiera de los siguientes métodos:

- Busque el correo electrónico de bienvenida enviado a la dirección de correo electrónico del propietario de la licencia, siga el vínculo y cambie la contraseña.

- Copie la URL de la tienda desde el [[!DNL Cloud Console]](../cloud-guide/project/overview.md) en un explorador. A continuación, anexe `/admin` al final de la dirección URL para abrir la página de inicio de sesión. Haga clic en **¿Olvidó la contraseña?** para enviar una solicitud de cambio de contraseña a la dirección de correo electrónico del propietario de la licencia.

Después de enviar la solicitud de cambio de contraseña, compruebe en su correo electrónico la notificación de restablecimiento de contraseña. Si no recibe el correo electrónico, compruebe su carpeta de correo no deseado.

>[!TIP]
>
>Si el restablecimiento de la contraseña falla o no puede iniciar sesión en el Panel de administración, un usuario con acceso de administrador puede conectarse al proyecto mediante SSH y agregar un usuario administrador mediante el `admin:user:create` Comando CLI. Consulte [Crear, editar o desbloquear una cuenta de administrador](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) en el _Guía de instalación_.
