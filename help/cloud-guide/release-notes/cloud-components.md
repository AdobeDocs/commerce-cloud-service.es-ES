---
title: Componentes de nube para Commerce
description: Consulte la lista de las mejoras más recientes en el paquete de componentes en la nube.
recommendations: noDisplay, catalog
exl-id: b4e2508a-3558-4fa8-bae0-3eb76c7b2775
source-git-commit: c02dfd2709cdc63ac1630edaa8c89cad5f737ea1
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Componentes de nube para Commerce

El [Componentes de nube](https://github.com/magento/magento-cloud-components) proporciona funcionalidad principal extendida de Adobe Commerce para sitios implementados en infraestructura en la nube. Este paquete es una dependencia para el paquete ECE-Tools. Estas notas de la versión describen las mejoras más recientes realizadas en este paquete, que es un componente de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

El `magento/magento-cloud-components` El paquete utiliza la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas funciones
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## Versión 1.0.14 {#latest}

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Añadido soporte para PHP 8.3.

## Versión 1.0.13

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **Compatibilidad mejorada con PHP 8.2**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.

## Versión 1.0.12

Fecha de la versión: 13 de septiembre de 2022

- ![icono de corrección](../../assets/fix.svg) **Errores en el calentamiento**: se ha corregido un problema que intentaba [calentamiento](../environment/variables-post-deploy.md#warm_up_pages) cuando la visibilidad de la página está establecida en [**No visible individualmente**](https://docs.magento.com/user-guide/system/data-attributes-product.html#simple-product-csv-file-structure) en el Administrador, lo que resulta en `ERROR: Warming up failed: <link to page>` errores en el registro de implementación.<!-- MCLOUD-9134 -->

## Versión 1.0.11

Fecha de lanzamiento: 4 de agosto de 2022

- ![icono de corrección](../../assets/fix.svg) **Se ha agregado compatibilidad con Symfony 5.4**—Correcciones de compatibilidad con Symfony 5.4.<!-- AC-3550 -->

## Versión 1.0.10

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Soporte PHP 8.1**—Se ha agregado compatibilidad con PHP 8.1 y se ha eliminado la compatibilidad con PHP 7.1.

## Versión 1.0.9

Fecha de la versión: 25 de octubre de 2021

- ![icono de corrección](../../assets/fix.svg) **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el `monolog` empaquetar en `^2.3`.<!-- ACMP-1263 -->

## Versión 1.0.8

Fecha de la versión: 29 de julio de 2021

- ![icono de corrección](../../assets/fix.svg) **Se han eliminado las barras finales de las direcciones URL generadas automáticamente**: se han eliminado las barras finales de las direcciones URL de página de categoría generadas durante el calentamiento de la caché.<!--MCLOUD-7192-->

## Versión 1.0.7

Fecha de la versión: 9 de septiembre de 2020

- ![nuevo icono](../../assets/new.svg) **Mejoras de registro**: permite reducir el tamaño del `cache.log` para mejorar el rendimiento.<!--MCLOUD-6859-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un error de tipo en los valores de configuración de caché que provocaba que `php bin/magento cache:evict` Comando CLI que falla.

## Versión 1.0.6

Fecha de lanzamiento: 5 de agosto de 2020

- ![nuevo icono](../../assets/new.svg) **Mejorar el rendimiento de Redis**—Añadido el `./bin/magento cache:evict` para eliminar las claves de Redis caducadas, lo que reduce el uso de memoria de Redis para mejorar el rendimiento.<!--MCLOUD-6023-->

- ![icono de corrección](../../assets/fix.svg) Se ha eliminado la compatibilidad con *Registros de New Relic en contexto* para solucionar un problema de rendimiento.<!--MCLOUD-6422-->

## Versión 1.0.5

Fecha de publicación: 25 de junio de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema introducido en la versión 1.0.4 de magento/magento-cloud-components que provocaba que la operación de vaciado de caché fallara durante la fase de implementación, interrumpiendo el proceso de implementación.

## Versión 1.0.4

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **Registros de New Relic implementados en contexto**: los registros de aplicación generados por Adobe Commerce ahora se muestran en seguimientos dentro de New Relic para mejorar las funciones de resolución de problemas.<!--MCLOUD-6029-->

- ![nuevo icono](../../assets/new.svg) **Registro mejorado**: se ha añadido el registro para rastrear los eventos de invalidación de caché y reindexación completa.<!--MCLOUD-6157-->

## Versión 1.0.3

Fecha de publicación: 27 de febrero de 2020

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de compatibilidad con `ece-tools` Versiones 2002.0.x que utilizan versiones anteriores de PHP.

## Versión 1.0.2

Fecha de publicación: 6 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) Se ha ampliado la funcionalidad del `WARM_UP_PAGES` variable de entorno para admitir la precarga de caché para páginas de producto específicas. Consulte la [variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages) tema para obtener una descripción detallada de la función.<!--MAGECLOUD-4444-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema en el cual una dirección URL de almacén no válida hacía que el vínculo posterior a la implementación fallara al usar `WARM_UP_PAGES` funcionalidad para rellenar la caché. Este problema solo se producía cuando se deshabilitaban las reescrituras de URL.<!-- MAGECLOUD-4094 -->

## Versión 1.0.1

Fecha de la versión: 23 de julio de 2019

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que afectaba a [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) que utiliza una URL de tienda predeterminada. Ahora, si la variable `config:show:default-url` El comando no puede recuperar una dirección URL base y se utiliza la dirección URL de la variable MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

## Versión 1.0.0

Fecha de publicación: 12 de junio de 2019

Esta es la primera versión de [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) paquete, que es una nueva dependencia para `ece-tools` versión del paquete 2002.0.20 y posterior.

- ![nuevo icono](../../assets/new.svg) Se ha añadido la capacidad de utilizar patrones de regex para configurar la variable **WARM_UP_PAGES** variable de entorno para almacenar en caché páginas únicas, varios dominios y varias páginas. Consulte [Variables posteriores a la implementación](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
