---
title: Implementación basada en escenarios
description: Obtenga información sobre cómo personalizar Adobe Commerce en implementaciones de infraestructura en la nube mediante archivos de configuración personalizados.
feature: Cloud, Configuration, Deploy, Build
exl-id: df243068-c7a3-46dd-abe9-9e05198fa343
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Implementación basada en escenarios

Con `ece-tools` 2002.1.0 y versiones posteriores, puede utilizar la característica de implementación basada en escenarios para personalizar el comportamiento de implementación predeterminado.
Esta función utiliza **escenarios** y **pasos** en la configuración:

- **Configuración de escenario**-Cada vínculo de implementación es una *escenario*, que es un archivo de configuración XML que describe la secuencia y los parámetros de configuración para completar las tareas de implementación. Los escenarios se configuran en la variable `hooks` de la sección `.magento.app.yaml` archivo.

- **Configuración de pasos**-Cada escenario utiliza una secuencia de *pasos* que describen mediante programación las operaciones necesarias para completar las tareas de implementación. Los pasos se configuran en un archivo de configuración de escenario basado en XML.

Adobe Commerce en la infraestructura en la nube proporciona un conjunto de [escenarios predeterminados](https://github.com/magento/ece-tools/tree/2002.1/scenario) y [pasos predeterminados](https://github.com/magento/ece-tools/tree/2002.1/src/Step) en el `ece-tools` paquete. Puede personalizar el comportamiento de la implementación creando archivos de configuración XML personalizados para anular o personalizar la configuración predeterminada. También puede utilizar escenarios y pasos para ejecutar código desde módulos personalizados.

## Adición de escenarios mediante los vínculos de generación e implementación

Agregue los escenarios para crear e implementar Adobe Commerce a la `hooks` de la sección `.magento.app.yaml` archivo. Cada vínculo especifica los escenarios que se ejecutarán durante cada fase. El siguiente ejemplo muestra la configuración de escenario predeterminada.

> `magento.app.yaml` ganchos

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Con el lanzamiento de `ece-tools` 2002.1.x, hay un nuevo [configuración de enlaces](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/hooks-property.html) formato. El formato heredado de `ece-tools` Las versiones 2002.0.x siguen siendo compatibles. Sin embargo, debe actualizar al nuevo formato para utilizar la función de implementación basada en escenarios.

## Revisar pasos del escenario

En la configuración del vínculo, cada escenario es un archivo XML que contiene los pasos para ejecutar las tareas de generación, implementación o posteriores a la implementación. Por ejemplo, la variable `scenario/transfer` El archivo incluye tres pasos: `compress-static-content`, `clear-init-directory`, y `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Ampliación de un escenario predeterminado

El siguiente ejemplo amplía el escenario de implementación predeterminado añadiendo archivos de configuración de implementación adicionales a la configuración de enlace.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Durante la implementación, los escenarios personalizados se combinan con el escenario predeterminado en función de las siguientes reglas:

- Los escenarios se priorizan en función de su secuencia en la definición del vínculo, y el último escenario enumerado tiene la prioridad más alta.

  En el ejemplo, los escenarios tienen la siguiente prioridad:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (escenario predeterminado o básico)

- Los pasos del escenario de mayor prioridad anulan los pasos que tienen el mismo nombre en los demás escenarios. Se añaden nuevos pasos a la configuración. Las mismas reglas se aplican a más de dos escenarios, y cada escenario se prioriza de derecha a izquierda, por ejemplo (C → B → A).

### Eliminar pasos predeterminados

Los pasos de los escenarios predeterminados se eliminan mediante la variable `skip` parámetro.

Por ejemplo, para omitir la variable `enable-maintenance-mode` y `set-production-mode` pasos en el escenario de implementación predeterminado, cree un archivo de configuración que incluya la siguiente configuración.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Para utilizar el archivo de configuración personalizada, actualice el valor predeterminado `.magento.app.yaml` archivo.

> `.magento.app.yaml` con escenario de implementación personalizada

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Reemplazar pasos predeterminados

Los escenarios personalizados pueden reemplazar los pasos predeterminados para proporcionar una implementación personalizada. Para ello, utilice el nombre de paso predeterminado como nombre para el paso personalizado.

Por ejemplo, en la variable [escenario de implementación predeterminado] el `enable-maintenance-mode` Este paso ejecuta el predeterminado [EnableMaintenanceMode: script PHP].

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Para anular este paso, cree un archivo de configuración de escenario personalizado para ejecutar un script diferente cuando la variable `enable-maintenance-mode` el paso se ejecuta.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Cambio de la prioridad de paso

Los escenarios personalizados pueden cambiar la prioridad de los pasos predeterminados. El siguiente paso cambia la prioridad del `enable-maintenance-mode` paso de `300` hasta `10` para que el paso se ejecute antes en el escenario de implementación.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

En este ejemplo, la variable `enable-maintenance-mode` Este paso se desplaza al principio del escenario porque tiene una prioridad menor que todos los demás pasos del escenario de implementación predeterminado.

### Ejemplo: Ampliación del escenario de implementación

El siguiente ejemplo personaliza el [escenario de implementación predeterminado] con los siguientes cambios:

- Reemplaza el `remove-deploy-failed-flag` paso con un paso personalizado
- Omite el `clean-redis-cache` subpaso en el paso previo a la implementación
- Omite el `unlock-cron-jobs` escalón
- Omite el `validate-config` paso para deshabilitar los validadores esenciales
- Agrega un nuevo paso previo a la implementación

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Para utilizar esta secuencia de comandos en el proyecto, agregue la configuración siguiente al `.magento.app.yaml` para su proyecto de infraestructura de Adobe Commerce en la nube:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Puede revisar las [escenarios predeterminados](https://github.com/magento/ece-tools/tree/2002.1/scenario) y [configuración de paso predeterminada](https://github.com/magento/ece-tools/tree/2002.1/src/Step) en el `ece-tools` El repositorio de GitHub para determinar qué escenarios y pasos personalizar para las tareas de creación, implementación y posteriores a la implementación de su proyecto.

## Añadir un módulo personalizado para ampliar `ece-tools`

El `ece-tools` proporciona interfaces de API predeterminadas que siguen los estándares de la versión semántica. Todas las interfaces de API están marcadas con **@api** anotación. Puede reemplazar la implementación de API predeterminada con la suya propia creando un módulo personalizado y modificando el código predeterminado según sea necesario.

Para utilizar el módulo personalizado con Adobe Commerce en la infraestructura en la nube, debe registrar el módulo en la lista de extensiones de para `ece-tools` paquete. El proceso de registro es similar al proceso que se utiliza para registrar módulos en Adobe Commerce.

**Para registrar un módulo con `ece-tools` paquete**:

1. Crear o ampliar el `registration.php` en la raíz del módulo.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Actualice el `autoload` para que el archivo de configuración del módulo incluya la sección `registration.php` archivo para cargar automáticamente archivos de módulo en `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Añada el `config/services.xml` en el módulo. Esta configuración se combina con `config/services.xml` de `ece-tools` paquete.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Para obtener más información sobre la inyección de dependencia, consulte [Inyección de dependencia de Symfony](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[escenario de implementación predeterminado]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode: script PHP]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
