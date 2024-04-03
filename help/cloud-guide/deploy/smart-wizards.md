---
title: Asistentes inteligentes
description: Aprenda a utilizar asistentes inteligentes para evaluar si su proyecto de Adobe Commerce en la nube sigue las prácticas recomendadas de implementación.
feature: Cloud, Build, Deploy, SCD
exl-id: eb79431c-8835-4ae4-b453-9c4932c5d5ac
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Asistentes inteligentes

Los asistentes inteligentes pueden ayudarle a determinar si la configuración de Cloud cumple las prácticas recomendadas. Los asistentes disponibles le ayudarán con las siguientes configuraciones:

- Estado ideal para un tiempo de inactividad mínimo de la implementación
- Configuración de equilibrio de carga para la base de datos y Redis
- Implementación de contenido estático (SCD) para bajo demanda, la fase de compilación o la fase de implementación

Cada uno de los comandos del asistente inteligente proporciona una respuesta de verificación y, si procede, una recomendación para la configuración adecuada.

| Comando | Descripción |
| ------- | ------------|
| `wizard:ideal-state` | Compruebe que el SCD está en _generar_ escenario, el `SKIP_HTML_MINIFICATION` la variable es `true`y el vínculo post_deploy configurado en el entorno de Cloud. No se debe utilizar en el entorno de desarrollo local. |
| `wizard:master-slave` | Compruebe que la variable `REDIS_USE_SLAVE_CONNECTION` y la variable `MYSQL_USE_SLAVE_CONNECTION` la variable es `true`. |
| `wizard:scd-on-demand` | Compruebe que la variable `SCD_ON_DEMAND` la variable de entorno global es `true`. |
| `wizard:scd-on-build` | Compruebe que la variable `SCD_ON_DEMAND` la variable de entorno global es `false` y el `SKIP_SCD` la variable de entorno es `false` para el _generar_ escenario. Comprueba que la variable `config.php` contiene información sobre tiendas, grupos de tiendas y sitios web. |
| `wizard:scd-on-deploy` | Compruebe que la variable `SCD_ON_DEMAND` la variable de entorno global es `false` y el `SKIP_SCD` la variable de entorno es `false` para el _implementar_ escenario. Comprueba que la variable `config.php` el archivo sí _NO_ contiene la lista de tiendas, grupos de tiendas y sitios web con información relacionada. |

Por ejemplo, puede comprobar que la configuración habilita correctamente la función SCD bajo demanda:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Una configuración correcta devuelve:

```terminal
SCD on-demand is enabled
```

Una configuración fallida devuelve lo siguiente:

```terminal
SCD on-demand is disabled
```

## Verificación de una configuración ideal

El _ideal_ La configuración de para su proyecto de Cloud ayuda a minimizar el tiempo de inactividad de la implementación al calentar la caché y generar contenido estático cuando el usuario lo solicita. Este asistente se ejecuta automáticamente durante el proceso de implementación. Si la nube no está configurada para esto _estado ideal_, recibirá un mensaje similar al siguiente:

```terminal
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

En función del resultado, debe realizar las siguientes correcciones en la configuración:

1. Habilite la variable de minificación Omitir HTML.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Configure el vínculo posterior a la implementación.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Inserte los cambios del código y ejecute la prueba de nuevo. Cuando la configuración es _ideal_, recibirá el siguiente mensaje.

   ```terminal
   Ideal state is configured
   ```
