---
title: Variables de entorno
description: Consulte una lista de variables de entorno específicas de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Build, Configuration, Deploy
exl-id: bfee2f69-93a6-4d26-bb9e-be8acc5673c3
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Variables de entorno

Adobe Commerce en la infraestructura de la nube le permite asignar variables de entorno para anular las opciones de configuración. El `ece-tools` el paquete establece valores en la variable `env.php` archivo basado en valores de [Variables de nube](variables-cloud.md), las variables configuradas en la variable [!DNL Cloud Console], y el `.magento.env.yaml` archivo de configuración.

Las variables de entorno en la variable `.magento.env.yaml` personalice el entorno de la nube anulando la configuración de Commerce existente. Si un valor predeterminado es `Not Set`y, a continuación, el `ece-tools` el paquete toma **NO** y utiliza el [!DNL Commerce] valor predeterminado o el valor de `MAGENTO_CLOUD_RELATIONSHIPS` configuración. Si se establece el valor predeterminado, la variable `ece-tools` El paquete de actúa para establecer ese valor predeterminado.

Los tipos de variables de entorno incluyen:

- [ADMINISTRADOR](variables-admin.md): las variables anulan las variables ADMIN del proyecto
- [MAGENTO_CLOUD](variables-cloud.md): variables específicas de la infraestructura en la nube
- Variables utilizadas en `.magento.env.yaml` archivo:
   - [Global](variables-global.md): las variables afectan a las fases de compilación, implementación y posterior a la implementación
   - [Generar](variables-build.md): las variables controlan las acciones de compilación
   - [Implementar](variables-deploy.md)—variables control implementar acciones
   - [Posterior a la implementación](variables-post-deploy.md): las variables controlan las acciones después de la implementación

Las variables son _jerárquico_, lo que significa que si una variable no se anula, se hereda del entorno principal.

Puede establecer [Variables de ADMINISTRACIÓN](variables-admin.md) desde el [!DNL Cloud Console] o utilizando la CLI de Adobe Commerce. Puede administrar otras variables de entorno desde el [`.magento.env.yaml`](configure-env-yaml.md) para administrar las acciones de compilación e implementación en todos sus entornos, incluidos el ensayo y la producción de Pro, sin necesidad de un vale de soporte.

>[!TIP]
>
>Los archivos YAML distinguen entre mayúsculas y minúsculas y no permiten tabulaciones. Tenga cuidado de utilizar una sangría uniforme en todo el `.magento.env.yaml` o puede que la configuración no funcione según lo esperado. Los ejemplos de esta documentación y del archivo de muestra utilizan _de dos espacios_ sangría. Utilice el [comando ece-tools validate](configure-env-yaml.md#validate-configuration-file) para comprobar su configuración.
