---
title: Implementación de contenido estático
description: Obtenga información acerca de las estrategias para implementar contenido estático, como imágenes, scripts y CSS, en Adobe Commerce en proyectos de infraestructura en la nube.
feature: Cloud, Build, Deploy, SCD
exl-id: e128d0d5-1326-44e5-a822-009e11ba105f
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---

# Estrategias de implementación de contenido estático

La implementación de contenido estático (SCD) tiene un impacto significativo en el proceso de implementación de tiendas que depende de la cantidad de contenido que se genere (como imágenes, scripts, CSS, vídeos, temáticas, configuraciones regionales y páginas web) y de cuándo se genere el contenido. Por ejemplo, la estrategia predeterminada genera contenido estático durante la [fase de implementación](process.md#deploy-phase-deploy-phase) cuando el sitio está en modo de mantenimiento; sin embargo, esta estrategia de implementación tarda en escribir el contenido directamente en el `pub/static` directorio. Tiene varias opciones o estrategias para ayudarle a mejorar el tiempo de implementación según sus necesidades.

## Optimización del contenido de JavaScript y HTML

Puede utilizar el agrupamiento y la minificación para crear contenido HTML y JavaScript optimizado durante la implementación de contenido estático.

### Minimizar contenido

Puede mejorar el tiempo de carga del SCD durante el proceso de implementación si omite copiar los archivos de vista estática en el `var/view_preprocessed` directorio y generación _minificado_ HTML cuando se solicita. Puede activar esto configurando la variable [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) variable de entorno global a `true` en el `.magento.env.yaml` archivo.

>[!NOTE]
>
>Comenzando por `ece-tools` versión del paquete 2002.0.13, el valor predeterminado de la variable SKIP_HTML_MINIFICATION se establece en `true`.

Puede guardar **más** tiempo de implementación y espacio en disco al reducir el número de archivos de temas innecesarios. Por ejemplo, puede implementar el `magento/backend` temática en inglés y temática personalizada en otros idiomas. Puede configurar estos ajustes de la temática con la variable [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix) variable de entorno.

## Elección de una estrategia de implementación

Las estrategias de implementación difieren en función de si elige generar contenido estático durante la _generar_ fase, la _implementar_ fase, o _a la carta_. Como se ve en el gráfico siguiente, la generación de contenido estático durante la fase de implementación es la opción menos óptima. Incluso con el HTML minificado, cada archivo de contenido debe copiarse al montado `~/pub/static` , lo que puede llevar mucho tiempo. La generación de contenido estático bajo demanda parece la opción óptima. Sin embargo, si el archivo de contenido no existe en la caché que genera en el momento en que se solicita, lo que añade tiempo de carga a la experiencia del usuario. Por lo tanto, la generación de contenido estático durante la fase de compilación es la mejor opción.

![Comparación de carga de SCD](../../assets/scd-load-times.png)

### Configuración del SCD durante la compilación

La generación de contenido estático durante la fase de compilación con el HTML minificado es la configuración óptima para [**cero tiempo de inactividad** implementaciones](reduce-downtime.md), también conocido como **estado ideal**. En lugar de copiar archivos en una unidad montada, crea un enlace simbólico desde la `./init/pub/static` directorio.

La generación de contenido estático requiere acceso a temáticas y configuraciones regionales. Adobe Commerce almacena las temáticas en el sistema de archivos, al que se puede acceder durante la fase de compilación; sin embargo, Adobe Commerce almacena las configuraciones regionales en la base de datos. La base de datos es _no_ disponible durante la fase de compilación. Para generar el contenido estático durante la fase de compilación, debe utilizar el `config:dump` comando en la `ece-tools` para mover configuraciones regionales al sistema de archivos. Lee las configuraciones regionales y las guarda en la variable `app/etc/config.php` archivo.

**Para configurar el proyecto para que genere un SCD durante la compilación**:

1. En la estación de trabajo local, cambie al directorio del proyecto.
1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Mueva configuraciones regionales al sistema de archivos y actualice la [`config.php` archivo](../development/commerce-version.md#create-a-configphp-file).

1. El `.magento.env.yaml` El archivo de configuración debe contener los siguientes valores:

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) es `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) en la fase de compilación es `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) es `compact`

1. Compruebe la configuración de [Gancho posterior a la implementación](../application/hooks-property.md) en el `.magento.app.yaml` archivo.

1. Compruebe la configuración ejecutando la variable [Asistente inteligente para el estado ideal](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### Configuración del SCD bajo demanda

La generación de SCD bajo demanda es óptima para un flujo de trabajo de desarrollo en el entorno de integración. Reduce el tiempo de implementación para que pueda revisar rápidamente las implementaciones y ejecutar las pruebas de integración. Habilite la [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) variable de entorno en la fase global de `.magento.env.yaml` archivo. La variable SCD_ON_DEMAND anula todas las demás configuraciones relacionadas con SCD y borra el contenido existente del `~/pub/static` directorio.

Al utilizar la estrategia bajo demanda de SCD, ayuda precargar la caché con páginas que espera solicitar, como la página de inicio. Añada la lista de páginas esperadas en la [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) variable de entorno en la fase posterior a la implementación de `.magento.env.yaml` archivo.

>[!WARNING]
>
>No utilice la estrategia bajo demanda de SCD en el entorno de producción.

### Omitiendo SCD

A veces puede optar por omitir la generación de contenido estático por completo. Puede configurar las variables [SKIP_SCD](../environment/variables-build.md#skipscd) en la fase global para ignorar otras configuraciones relacionadas con la SCD. Esto no afecta al contenido existente en `~/pub/static` directorio.
