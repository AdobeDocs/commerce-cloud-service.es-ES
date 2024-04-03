---
title: Propiedad Variables
description: Utilice la propiedad variables para personalizar las opciones de configuración del almacén para [!DNL Commerce] aplicación.
feature: Cloud, Configuration
exl-id: 5cd92fbb-8bff-48b1-9658-500140591344
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Propiedad Variables

Puede utilizar variables de entorno basadas en aplicaciones para personalizar las configuraciones de tienda. Estas variables utilizan una sintaxis específica. Consulte [Anular los ajustes de configuración](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) en el _Guía de configuración_.

Las siguientes variables de entorno están incluidas en la variable `.magento.app.yaml` son necesarios para versiones específicas del archivo [!DNL Commerce] aplicación.

Necesario para Adobe Commerce 2.2.x a 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Para Adobe Commerce 2.4.x, configure las siguientes variables:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
