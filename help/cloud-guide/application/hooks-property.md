---
title: Propiedad Hooks
description: Consulte ejemplos sobre cómo configurar la propiedad hooks en la [!DNL Commerce] archivo de configuración de la aplicación.
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Propiedad Hooks

Utilice el `hooks` para ejecutar comandos del shell durante las fases de generación, implementación y posterior a la implementación:

- **`build`**: ejecutar comandos _antes_ empaquetar la aplicación. Los servicios, como la base de datos o Redis, no están disponibles porque la aplicación aún no se ha implementado. Agregar comandos personalizados _antes_ el valor predeterminado `php ./vendor/bin/ece-tools` para que el contenido generado a medida continúe con la fase de implementación.

- **`deploy`**: ejecutar comandos _después_ empaquetar e implementar la aplicación. En este punto, puede acceder a otros servicios. Dado que el valor predeterminado `php ./vendor/bin/ece-tools` El comando copia el `app/etc` en la ubicación correcta, debe agregar comandos personalizados _después_ Utilice el comando deploy para evitar que los comandos personalizados produzcan errores.

- **`post_deploy`**: ejecutar comandos _después_ implementar la aplicación y _después_ el contenedor empieza a aceptar conexiones. El `post_deploy` El gancho borra la caché y la precarga (calienta). Puede personalizar la lista de páginas mediante el `WARM_UP_PAGES` en la variable [Fase posterior a la implementación](../environment/variables-post-deploy.md). Aunque no es obligatorio, esto funciona junto con el `SCD_ON_DEMAND` variable de entorno.

El siguiente ejemplo muestra la configuración predeterminada en la variable `.magento.app.yaml` archivo. Agregue comandos CLI en la `build`, `deploy`, o `post_deploy` secciones _antes_ el `ece-tools` comando:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Además, puede personalizar aún más la fase de compilación utilizando `generate` y `transfer` comandos para realizar acciones adicionales al generar código o mover archivos específicamente.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e`: permite que los enlaces falle en el primer comando fallido, en lugar del último comando fallido.
- `build:generate`: aplica parches, valida la configuración, genera ID y genera contenido estático si SCD está activado para la fase de compilación.
- `build:transfer`: permite transferir código generado y contenido estático al destino final.

Los comandos se ejecutan desde la aplicación (`/app`). Puede usar el complemento `cd` para cambiar el directorio. Los enlaces fallan si falla el comando final en ellos. Para que fallen en el primer comando fallido, agregue `set -e` al principio del gancho.

**Para compilar archivos Sass utilizando grunt**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Compilar archivos Sass utilizando `grunt` antes de la implementación de contenido estático, lo que sucede durante la compilación. Coloque el `grunt` antes del `build` comando.

{{scenarios}}
