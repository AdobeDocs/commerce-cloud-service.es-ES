---
title: Servicio de correo electrónico SendGrid
description: Obtenga información acerca del servicio de correo electrónico SendGrid para Adobe Commerce en la infraestructura en la nube y cómo puede probar la configuración de DNS.
exl-id: 30d3c780-603d-4cde-ab65-44f73c04f34d
source-git-commit: 2b106edcaaacb63c0e785f094b7e1b755885abd0
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 0%

---

# Servicio de correo electrónico SendGrid

El servicio proxy del Protocolo simple de transferencia de correo (SMTP) de SendGrid proporciona servicios de autenticación de correo electrónico saliente y de supervisión de reputación, incluida la compatibilidad con:

* Todos los correos electrónicos transaccionales salientes
* Direcciones IP dedicadas
* Registro de dominios, firmas de DomainKeys Identified Mail (DKIM) para la validación de dominios de correo electrónico (solo para entornos de ensayo y producción Pro)
* Registro de dominio personalizado (solo para Pro)
* Integración automatizada para entornos de integración de Starter y Pro. Los entornos de producción y ensayo de Pro requieren un aprovisionamiento y una configuración manuales durante el proceso de aprovisionamiento de hardware de Infrastructure as a Service (IaaS)

El proxy SMTP SendGrid no está diseñado para utilizarse como servidor de correo electrónico de uso general para recibir correo electrónico entrante o para utilizarse con campañas de marketing por correo electrónico.

>[!TIP]
>
>Puede encontrar los detalles de SendGrid para su cuenta en la [IU de incorporación](https://cloud.magento.com) y seleccione la **Detalles del proyecto** > **Información de alojamiento** pestaña.

## Habilitar o deshabilitar correo electrónico

Puede habilitar o deshabilitar los correos electrónicos salientes para cada entorno desde la consola de Cloud o desde la línea de comandos.

De forma predeterminada, los correos electrónicos salientes están habilitados en los entornos de Pro Production y Staging. Sin embargo, [!UICONTROL Outgoing emails] puede aparecer desactivado en la configuración del entorno hasta que configure la variable `enable_smtp` a través de [línea de comandos](outgoing-emails.md#enable-emails-in-the-cli) o [Consola de nube](outgoing-emails.md#enable-emails-in-the-cloud-console). Puede habilitar correos electrónicos salientes para los entornos de integración y ensayo para enviar correos electrónicos de autenticación de doble factor o restablecer la contraseña para los usuarios del proyecto en la nube. Consulte [Configuración de correos electrónicos para pruebas](outgoing-emails.md).

Si los correos electrónicos salientes deben desactivarse o volver a activarse en los entornos de Pro Production o Staging, puede enviar un [ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Actualización del [!UICONTROL enable_smtp] valor de propiedad por [línea de comandos](outgoing-emails.md#enable-emails-in-the-cli) también cambia el [!UICONTROL Enable outgoing emails] estableciendo el valor de este entorno en [Consola de nube](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Panel SendGrid

Todos los proyectos de Cloud se administran en una cuenta central, por lo que solo el equipo de asistencia tiene acceso al panel SendGrid. SendGrid no proporciona características de restricción de subcuenta.

Para revisar los registros de actividad para el estado de entrega o una lista de direcciones de correo electrónico rechazadas, rechazadas o bloqueadas, [enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). El equipo de asistencia **no puede** recupere los registros de actividad con más de 30 días.

Si es posible, incluya la siguiente información en su solicitud:

* las direcciones de correo electrónico afectadas
* el periodo en cuestión (solo en los últimos 30 días)
* asunto del correo electrónico

## Correo identificado por DomainKeys (DKIM)

DKIM es una tecnología de autenticación de correo electrónico que permite a los proveedores de servicios de Internet (ISP) identificar direcciones de remitente legítimas y falsas, una técnica comúnmente utilizada en estafas de phishing y correo electrónico. DKIM se basa en un propietario de dominio que administra los registros DNS. Al utilizar DKIM, el servidor remitente utiliza una clave privada para firmar los mensajes. Además, el propietario del dominio añade un registro DKIM, que es un registro modificado `TXT` registro, a los registros DNS del dominio del remitente. Esta `TXT` Este registro contiene una clave pública que los servidores de correo de los destinatarios utilizan para comprobar la firma de un mensaje. El procedimiento de criptografía de clave pública DKIM permite a los destinatarios verificar la autenticidad de un remitente. Consulte [Registros DKIM explicados](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Las firmas DKIM de SendGrid y la compatibilidad con la autenticación de dominios solo están disponibles para proyectos Pro y no Proyectos Starter. Como resultado, es probable que los correos electrónicos transaccionales salientes estén marcados por filtros de correo no deseado. El uso de DKIM mejora la tasa de entrega como remitente de correo electrónico autenticado. Para mejorar la tasa de entrega de mensajes, puede actualizar de Starter a Pro o utilizar su propio servidor SMTP o proveedor de servicios de entrega de correo electrónico. Consulte [Configuración de conexiones de correo electrónico](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications) en el _Guía de sistemas de administración_.

### Autenticación de remitente y dominio

Para que SendGrid envíe correos electrónicos transaccionales en su nombre desde entornos de Pro Production o Staging, debe configurar la configuración de DNS para incluir las tres entradas DNS del subdominio SendGrid. A cada cuenta de SendGrid se le asigna un único `TXT` registro que se utiliza para autenticar correos electrónicos salientes.

**Para habilitar la autenticación de dominio**:

1. Enviar una [ticket de asistencia](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar la activación de DKIM para un dominio específico (**Solo entornos de ensayo y producción profesionales**).
1. Actualice la configuración de DNS con `TXT` y `CNAME` registros proporcionados en el ticket de asistencia.

**Ejemplo `TXT` registro con ID de cuenta**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Ejemplo `CNAME` records**:

| Dominio | Apunta A | Tipo de registro |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### Firmas DKIM y seguridad automatizada

Puede seleccionar entre la seguridad automática y la manual al configurar un dominio autenticado. Si elige la seguridad automatizada, SendGrid gestiona automáticamente los registros DKIM y SPF. Cuando agrega una nueva dirección IP de envío dedicada a su cuenta, SendGrid actualiza inmediatamente la configuración de DNS y la firma DKIM. Si desactiva la seguridad automatizada, usted es el responsable de actualizar su firma DKIM cada vez que cambie su dominio de envío.

**Ejemplo de seguridad automatizada habilitada**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Ejemplo de seguridad automatizada deshabilitada**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Una vez configurada la autenticación de dominio, SendGrid administra automáticamente los registros del Marco de directivas de seguridad (SPF) y DKIM. Después de que SendGrid proporcione el `CNAME` registros para agregar a sus registros DNS, puede agregar direcciones IP dedicadas y realizar otras actualizaciones de la cuenta sin tener que administrar los registros SPF manualmente. Consulte [Seguridad automatizada y su firma DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Para probar la configuración de DNS:

```terminal
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Umbral de correo electrónico transaccional

El umbral de correo electrónico transaccional se refiere al número de mensajes de correo electrónico transaccionales que puede enviar desde entornos Pro en un período de tiempo específico, como 12 000 correos electrónicos al mes desde entornos que no son de producción. El umbral está diseñado para proteger contra el envío de correo no deseado y potencialmente dañar su reputación de correo electrónico.

No existen límites estrictos en la cantidad de correos electrónicos que se pueden enviar en el entorno de producción, siempre y cuando la puntuación de reputación del remitente sea superior al 95 %. La reputación se ve afectada por el número de correos electrónicos rechazados o rechazados y por si los registros de correo no deseado basados en DNS han marcado su dominio como una posible fuente de correo no deseado. Consulte [Correos electrónicos no enviados cuando se superan los créditos de SendGrid en Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) en el _Base de conocimiento de asistencia de Commerce_.

**Para comprobar si se han superado los créditos máximos**:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Compruebe la `/var/log/mail.log` para `authentication failed : Maxium credits exceeded` entradas.

   Si ve alguno `authentication failed` entradas de registro y el **Reputación de envío de correo electrónico** tiene un mínimo de 95, puede [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar un aumento de la asignación de crédito.

### Reputación de envío de correo electrónico

Una reputación de envío de correo electrónico es una puntuación asignada por un proveedor de servicios de Internet (ISP) a una compañía que envía mensajes de correo electrónico. Cuanto más alta sea la puntuación, más probable es que un ISP envíe mensajes a la bandeja de entrada de un destinatario. Si la puntuación cae por debajo de un determinado nivel, el ISP puede enrutar los mensajes a la carpeta de correo no deseado de los destinatarios o incluso rechazarlos por completo. La puntuación de reputación está determinada por varios factores, como un promedio de 30 días de clasificación de sus direcciones IP frente a otras direcciones IP y la tasa de quejas de spam. Consulte [8 formas de comprobar su reputación de envío de correo electrónico](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).
