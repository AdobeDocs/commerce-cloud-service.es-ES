---
title: Acceso a su panel de administración de Commerce
description: Obtenga información sobre cómo acceder a su Panel de administración de Commerce.
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 3ca09243dc0a714c1d86cccf9f0620a8a39fd1e1
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Acceso a su panel de administración de Commerce

Los usuarios que tienen acceso administrativo al Panel de administración de Commerce pueden agregar usuarios, configurar servicios de tienda, completar el trabajo de configuración y personalización de la tienda, etc.

En un proyecto nuevo, el primer paso después de recibir el correo electrónico de bienvenida es proteger el acceso de los administradores al proyecto cambiando la contraseña en la cuenta del propietario de la licencia. El nombre de usuario predeterminado para esta cuenta es la dirección de correo electrónico del propietario de la licencia.

Puede enviar una solicitud de cambio de contraseña utilizando cualquiera de los siguientes métodos:

- Busque el correo electrónico de bienvenida enviado a la dirección de correo electrónico del propietario de la licencia, siga el vínculo y cambie la contraseña.

- Copie la URL de la tienda desde el [[!DNL Cloud Console]](../cloud-guide/project/overview.md) en un explorador. A continuación, anexe `/admin` al final de la dirección URL para abrir la página de inicio de sesión. Haga clic en **¿Olvidó la contraseña?** para enviar una solicitud de cambio de contraseña a la dirección de correo electrónico del propietario de la licencia.

Después de enviar la solicitud de cambio de contraseña, compruebe en su correo electrónico la notificación de restablecimiento de contraseña. Si no recibe el correo electrónico, compruebe su carpeta de correo no deseado.

>[!TIP]
>
>Si el restablecimiento de la contraseña falla o no puede iniciar sesión en el Panel de administración, un usuario con acceso de administrador puede conectarse al proyecto mediante SSH y agregar un usuario administrador mediante el `admin:user:create` Comando CLI. Consulte [Crear, editar o desbloquear una cuenta de administrador](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) en el _Guía de instalación_.

## Monitorización del estado del sitio

El [Herramienta de análisis de todo el sitio](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) es una herramienta de autoservicio proactiva y un repositorio central que incluye información detallada del sistema y recomendaciones para garantizar la seguridad y la operabilidad de la instalación de Adobe Commerce. Proporciona monitorización del rendimiento, informes y consejos en tiempo real las 24 horas del día, los 7 días de la semana para identificar posibles problemas y una mejor visibilidad de las configuraciones de salud, seguridad y aplicaciones del sitio. Ayuda a reducir el tiempo de resolución y a mejorar la estabilidad y el rendimiento del sitio. Puede acceder a la herramienta de análisis de todo el sitio directamente desde el [Panel de administración](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel) o desde el [dominio dedicado](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-1-logging-in-to-your-site-wide-analysis-tool-dashboard-directly-from-the-site-wide-analysis-tool-domain-for-adobe-commerce-on-cloud-infrastructure-only) (solo Adobe Commerce en proyectos de infraestructura en la nube).
