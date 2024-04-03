---
title: Niveles variables y opciones
description: Obtenga información acerca de los diferentes niveles de variables y opciones que se utilizan para personalizar el entorno de tiempo de ejecución del proyecto de infraestructura en la nube de Adobe Commerce.
feature: Cloud, Configuration, Security
exl-id: 11aa0862-94c0-47fb-946a-0148f75cc24c
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Niveles variables

Las variables de proyecto se aplican a todos los entornos dentro del proyecto. Las variables de entorno se aplican a un entorno o rama específicos. Un entorno _hereda_ definiciones de variables del entorno principal.

Puede anular un valor heredado definiendo la variable específicamente para el entorno. Por ejemplo, para establecer variables para el desarrollo, defina los valores de las variables en la variable `.magento.env.yaml` en el entorno de integración. Todos los entornos que se ramifican desde el entorno de integración heredan esos valores. Consulte [Configuración de implementación](configure-env-yaml.md) para obtener más información acerca de cómo configurar su entorno con `.magento.env.yaml` archivo.

>[!BEGINTABS]

>[!TAB CLI]

**Para establecer variables mediante la CLI de nube**:

- **Variables específicas del proyecto**: para definir el mismo valor para _todo_ entornos en el proyecto. Estas variables están disponibles durante la compilación y el tiempo de ejecución en todos los entornos.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **Variables específicas del entorno**: para definir un valor único para una _específico_ entorno. Estas variables están disponibles durante la ejecución y las heredan los entornos secundarios. Especifique el entorno en el comando mediante la variable `-e` opción.

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

Después de establecer las variables específicas del proyecto, debe volver a implementar manualmente el entorno remoto para que el cambio surta efecto. Insertar los nuevos compromisos para almacenar en déclencheur una reimplementación.

>[!TAB Consola]

**Para establecer variables mediante[!DNL Cloud Console]**:

1. En el _[!DNL Cloud Console]_, haga clic en el icono de configuración en la parte derecha de la navegación del proyecto.

   ![Configurar proyecto](../../assets/icon-configure.png){width="36"}

1. Para establecer una variable de nivel de proyecto, en _Configuración de proyecto_ click **Variables**.

   ![Variables de proyecto](../../assets/ui-project-variables.png)

1. Para establecer una variable de nivel de entorno, en la variable _Entornos_ , seleccione un entorno y haga clic en **[!UICONTROL Variables]** pestaña.

   ![Pestaña Variables de entorno](../../assets/ui-environment-variables.png)

1. Clic **[!UICONTROL Create variable]**.

1. Proporcione un nombre y un valor para la variable. Elija entre las siguientes opciones:

   - Disponible durante el tiempo de ejecución
   - Disponible durante la compilación
   - Valor JSON
   - Variable confidencial (valor oculto en la consola y respuestas CLI)
   - Hacer heredable (los entornos secundarios pueden heredar variables de nivel de entorno)

1. Clic **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>Configurar variables específicas del entorno en la variable [!DNL Cloud Console] vuelve a implementar automáticamente el entorno.

>[!ENDTABS]

## Visibilidad

Puede limitar la visibilidad de una variable durante la compilación o el tiempo de ejecución mediante `--visible-<build|runtime>` comando. Además, hay opciones para establecer la herencia y la confidencialidad.

Utilice las siguientes opciones para evitar que se vea o herede una variable:

- `--inheritable false`: desactiva la herencia para entornos secundarios. Esto resulta útil para configurar valores solo de producción en `master` y permite que todos los demás entornos utilicen una variable de nivel de proyecto del mismo nombre.
- `--sensitive true`: permite marcar la variable como _ilegible_ en el [!DNL Cloud Console]. No puede ver la variable en la interfaz de usuario; sin embargo, puede verla desde el contenedor de aplicaciones, como cualquier otra variable.

A continuación se muestra un caso específico para evitar que se vea o herede una variable. Solo puede especificar estas opciones en la CLI. Este caso no se aplica a todas las variables de entorno disponibles.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## Comprobar niveles y valores de variables

Puede ver una lista de las variables existentes mediante la CLI de nube.

```bash
magento-cloud variables
```

```terminal
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
