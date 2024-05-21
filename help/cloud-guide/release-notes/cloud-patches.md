---
title: Parches de nube para Commerce
description: Consulte la lista de las mejoras más recientes del paquete Parches en la nube.
recommendations: noDisplay, catalog
last-substantial-update: 2024-05-21T00:00:00Z
exl-id: ae6b511b-a37d-4776-9a5e-ad7d9f9f6611
source-git-commit: 61c42a1bd1d5a28f90b8756032ee6f45be4565b2
workflow-type: tm+mt
source-wordcount: '2208'
ht-degree: 0%

---

# Parches de nube para Commerce

El [Parches de nube](https://github.com/magento/magento-cloud-patches) proporciona un conjunto de parches necesarios que mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube y admite la entrega rápida de correcciones críticas.

El paquete Parches de nube para Commerce es una dependencia para el paquete ECE-Tools y se instala y actualiza al instalar o actualizar el paquete ECE-Tools. También puede utilizar y administrar Parches de nube para Commerce como paquete independiente para aplicar parches a un proyecto de Adobe Commerce que no esté en la plataforma en la nube. Estas notas de la versión describen las mejoras más recientes realizadas en este paquete.

>[!TIP]
>
>Para asegurarse de que el proyecto tiene todos los parches necesarios, actualice a la [última versión de ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>Consulte [Aplicar parches](../development/apply-patches.md) para obtener instrucciones sobre cómo aplicar parches a los proyectos.

El `magento/magento-cloud-patches` El paquete utiliza la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## Versión 1.0.27 {#latest}

Fecha de la versión: 21 de mayo de 2024

- **Compatibilidad con PHP 8.3**—Este parche resuelve errores de compatibilidad entre php 8.3 y la versión del paquete del compositor.

## Versión 1.0.26

Fecha de publicación: 8 de abril de 2024

- ![nuevo icono](../../assets/new.svg) **PHP** — Añadido soporte para PHP 8.3.

## Versión 1.0.25

Fecha de la versión: 16 de enero de 2024

- **Mejoras de caché**-Este parche mejora la eficacia de la caché de diseño, reduciendo significativamente el uso de memoria, para las versiones 2.4.4 y posteriores de Adobe Commerce.<!-- MCLOUD-11514 -->
- **Mejoras en CRON Jobs**-Este parche corrige el problema en el que los trabajos perdidos esperan innecesariamente bloqueos de trabajo cron para las versiones 2.4.4 y posteriores de Adobe Commerce.<!-- MCLOUD-11329 -->

## Versión 1.0.24

Fecha de la versión: 15 de septiembre de 2023

- **Mejora del rendimiento**-Este parche corrige un problema que afecta al rendimiento al reducir el número de veces que se cargan las mismas configuraciones de implementación para Adobe Commerce 2.4.6 a 2.4.6-p1<!-- MCLOUD-10604 -->

## Versión 1.0.23

Fecha de la versión: 31 de julio de 2023

- **Se ha eliminado el parche MCLOUD-10604**- Este parche se ha trasladado a QPT.<!-- MCLOUD-10736 -->

## Versión 1.0.22

Fecha de publicación: 19 de junio de 2023

- **Asistente/salida de CLI de QPT mejorado**—Se ha añadido una advertencia al asistente/salida de CLI de QPT que le recuerda que debe verificar los detalles y requisitos del parche si hay dependencias.<!-- ACP2E-1963 -->
- **Parches añadidos para Commerce 2.4.6:**
   - Se ha corregido el `regexp cache tag` validación.<!-- MCLOUD-10226 -->
   - Rendimiento mejorado al reducir el número de veces que se cargan las mismas configuraciones de implementación.<!-- MCLOUD-10604 -->
- **Se han añadido parches para Commerce 2.3.7 a 2.4.6**: se ha corregido un problema que provocaba un incremento de un valor aleatorio en lugar de un incremento de 1 para el `catalog_product_entity_*` tablas.<!-- MCLOUD-10032 -->
- **Se han añadido parches para Commerce 2.4.0 a 2.4.6**—Se ha corregido un error que indicaba que `The file can't be deleted. Warning!unlink: No such file or directory`, que se producía al vaciar la caché de JS/CSS del administrador.<!-- MCLOUD-10279 -->

## Versión 1.0.21

Fecha de la versión: 10 de marzo de 2023

- **Compatibilidad mejorada con PHP 8.2**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.

## Versión 1.0.20

Fecha de la versión: 27 de octubre de 2022

- **Se agregó el parche de mejoras de caché L2**: este parche corrige un problema que se producía al vaciar la caché L2 local para las versiones 2.4.0 y 2.4.1 de Commerce.<!-- MCLOUD-7845 -->

## Versión 1.0.19

Fecha de la versión: 13 de septiembre de 2022

- **Compatibilidad mejorada con PHP 8.1**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.1.x.

## Versión 1.0.18

Fecha de lanzamiento: 11 de agosto de 2022

Parche crítico para Adobe Commerce 2.4.5:

- **Emisión de pedidos mediante pagos de Braintree**: este parche resuelve un problema crítico que impide que los administradores realicen nuevos pedidos o repedidos.<!-- MCLOUD-9137 -->

Consulte [El administrador no puede crear un pedido/repedido cuando el pago del Braintree está activado](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## Versión 1.0.17

Fecha de la versión: 24 de mayo de 2022

Se han corregido restricciones para los parches de seguridad en `patches.json` archivo.

## Versión 1.0.16

Fecha de la versión: 31 de marzo de 2022

Parche crítico para Adobe Commerce 2.3.3-p1 y versiones posteriores:

Se han actualizado los parches para resolver un problema **crítico** vulnerabilidad que provoca la ejecución de código remoto no autenticado.<!-- MCLOUD-8479 -->

Consulte [Boletín de seguridad del Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## Versión 1.0.15

Fecha de la versión: 10 de marzo de 2022

- **Soporte PHP 8.1**—Se ha agregado compatibilidad con PHP 8.1 y se ha eliminado la compatibilidad con PHP 7.0 y 7.1.
- **Se ha añadido el parche para Adobe Commerce 2.3.3**: divisa fija que se muestra en la página del producto.

## Versión 1.0.14

Fecha de publicación: 13 de febrero de 2022

Parche crítico para Adobe Commerce 2.3.3-p1 y versiones posteriores:

Se ha añadido un parche para resolver un problema **crítico** vulnerabilidad que provoca la ejecución de código remoto no autenticado.<!-- MCLOUD-8461 -->

Consulte [Boletín de seguridad del Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## Versión 1.0.13

Fecha de la versión: 25 de octubre de 2021

- **Actualizar monólogo**: se ha actualizado la versión mínima requerida para el `monolog` empaquetar en `^2.3`.<!-- ACMP-1263 -->
- **Método PHP incompatible**—Se ha corregido un método PHP incompatible para las versiones 2.4.3 y 2.3.7-p1 de Adobe Commerce.<!-- AC-384 -->
- **Error de PHP**: se ha corregido un `PHP error 'Undefined variable: errorMessage' ...` error que se produjo al intentar aplicar un parche.<!-- ACP2E-138 -->

## Versión 1.0.12

Fecha de lanzamiento: 12 de agosto de 2021

Parche crítico para Adobe Commerce 2.4.3 y 2.3.7-p1:

- **Problema con la limitación de velocidad de API**: este parche corrige un límite de velocidad por defecto que impedía que las API web procesaran solicitudes con más de 20 elementos en una matriz. Este parche aumenta el valor predeterminado del límite de velocidad. Consulte Adobe Commerce [Notas de la versión 2.4.3](https://devdocs.magento.com/guides/v2.4/release-notes/commerce-2-4-3.html#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting) y el [Notas de la versión 2.3.7](https://devdocs.magento.com/guides/v2.3/release-notes/2-3-7-p1.html#apply-mc-43048__set_rate_limits__237-p1patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## Versión 1.0.11

Fecha de la versión: 29 de julio de 2021

- **Se ha corregido un problema que se producía al aplicar el parche de navegación por capas B2B**: para los clientes que han aplicado el parche de navegación por capas B2B, esta corrección resuelve un problema `Undefined offset` error que se muestra en la página Buscar después de cambiar a la vista Tienda.<!--MCLOUD-5287-->

- **Parche de pago y envío de PayPal**: corrige un problema de Adobe Commerce 2.3.7 con PayPal Express en el que se muestra el precio de pedido colocado anteriormente.<!--MC-42674-->

- **Compatibilidad con categoría de parche**: se ha añadido compatibilidad con las categorías de parches de procesamiento y los orígenes de origen asignados a los parches de calidad. Las categorías permiten a los clientes utilizar filtros y ordenar para buscar parches más rápidamente al utilizar [Herramienta Parches de calidad](https://github.com/magento/quality-patches) y la herramienta de análisis de todo el sitio (SWAT). <!--MC-38577-->

## Versión 1.0.10

Fecha de la versión: 10 de mayo de 2021

- **Compatibilidad con Adobe Commerce 2.3.7**: se ha resuelto un conflicto de dependencias del compositor para la instalación en Adobe Commerce 2.3.7.<!--MC-42131-->
- **Se ha corregido un problema que se producía al aplicar un parche empaquetado varias veces**: la aplicación de un parche empaquetado (uno que incluya otros parches obsoletos) más de una vez podría revertir los paquetes obsoletos incluidos. Ahora todos los parches se aplican solo una vez. Al intentar aplicar el mismo paquete de nuevo, aparece un mensaje que indica que el parche ya se ha aplicado.<!--MC-41912-->
- **Parche de navegación por capas B2B**: se ha corregido otro problema que impedía que la navegación por capas mostrara todas las opciones de producto cuando el usuario habilitaba el catálogo compartido B2B.<!--MCLOUD-7742-->

## Versión 1.0.9

Fecha de publicación: 1 de febrero de 2021

- **Parche de navegación por capas B2B**: se ha corregido el problema que impedía que la navegación por capas mostrara todas las opciones de producto cuando el catálogo compartido B2B estaba activado.<!--MCLOUD-6923-->
- **Compatibilidad con PHP 7.4**—Se ha corregido un problema de compatibilidad de parches en la nube con PHP 7.4.<!--MCLOUD-7367-->
- **Los parches obsoletos se vuelven visibles**: se ha corregido un problema de parches en la nube en el que los parches obsoletos se volvían visibles en la tabla de parches después de aplicar un parche de sustitución que contenía todo el contenido del parche obsoleto. Esto puede suceder si aplica un parche que combina varios parches más.<!--MC-40626-->
- **Errores silenciosos al aplicar parches**: se ha corregido un problema de parches de nube en el que la variable `git apply` El comando no pudo aplicar parches en algunos entornos.<!--MC-40529-->

## Versión 1.0.8

Fecha de la versión: 14 de octubre de 2020

- **Actualizaciones de compatibilidad para parches en la nube de magento/magento**: se ha actualizado el `symfony` y `semver` restricciones de versión en `composer.json` para comprobar la compatibilidad con Adobe Commerce 2.4.1 y versiones posteriores.<!--MCLOUD-7111-->

## Versión 1.0.7

Fecha de la versión: 14 de octubre de 2020

- **Revisiones de Redis para Adobe Commerce 2.3.0 a 2.3.5, 2.4.0**: se han actualizado los parches de Redis para admitir la adición de productos a una categoría al implementar una caché de Nivel 2. <!--MCLOUD-6659-->

- **parche de Braintree VBE**: permite corregir un problema que generaba un error cuando un administrador intentaba ver un informe de liquidación de Braintree. <!--MCLOUD-6684-->

- Ahora, la `ece-patches apply` utiliza el comando Unix `patch` para aplicar parches si Git no está disponible en el sistema host. <!--MCLOUD-7069-->

## Versión 1.0.6

Fecha de lanzamiento:

- **Revisiones de Redis para Adobe Commerce 2.3.0 - 2.3.4**—Optimizar la comunicación y mejorar el rendimiento
   - Reducción del tamaño de las transferencias de red entre Redis y Adobe Commerce
   - Corrección de condiciones de carrera en operaciones de carga y escritura de Redis
   - Reescribir el adaptador de caché base para controlar los errores al guardar
   - Disminuir consumo de CPU de Redis<!--MCLOUD-6139-->

- **Revisiones de Redis para Adobe Commerce 2.3.0 - 2.3.5**—Mejorar el rendimiento y corregir errores
   - Corregir la implementación de bloqueo de caché para evitar bloqueos infinitos
   - Mejora del mecanismo de bloqueo actual
   - Implementar bloqueos firmados para evitar el desbloqueo de solicitudes en paralelo
   - Corrija el siguiente error que se produce en la operación de escritura de Redis: `OOM command not allowed when used memory > maxmemory`
   - Corregir el procesamiento para limpiar la caché al `cat_p` etiqueta que se ejecuta durante las actualizaciones de productos<!--MCLOUD-6110-->

- Se ha corregido un problema que provocaba un error al aplicar el requerido `amzn/amazon-pay-module` parche de Adobe Commerce en proyectos de infraestructura en la nube con Adobe Commerce v2.2.6 o 2.3.5, que no incluyen este módulo. Ahora, el proceso de aplicación de parches omite el `amzn/amazon-pay-module` parche si el módulo no está instalado.<!--MCLOUD-6588-->

## Versión 1.0.5

Fecha de publicación: 26 de junio de 2020

- **Mejoras de rendimiento de Redis**: añade funciones de optimización de Redis a las versiones de Adobe Commerce 2.3.3 y 2.3.4. Estas correcciones se incluyeron en la versión 2.3.5 de Adobe Commerce. Consulte [Mejoras de rendimiento](https://devdocs.magento.com/guides/v2.3/release-notes/release-notes-2-3-5-commerce.html#performance-boosts) en el _Notas de la versión de Adobe Commerce 2.3.5_.<!--MCLOUD-5771-->

- **Enriquecimiento del registro de New Relic**: añade la Monolog ProcessorInterface necesaria para admitir mejoras en las funciones de registro de New Relic introducidas en Cloud Components de Commerce versión 1.0.4. Este parche es necesario para implementar Adobe Commerce 2.1.x. Si no se aplica el parche, la compilación falla durante el `di:compile` proceso.<!--MCLOUD-6029-->

## Versión 1.0.4

Fecha de la versión: 12 de mayo de 2020

- **Amazon Pay checkout**: corrige un problema con el widget de pago de Amazon Pay que impedía a los clientes cambiar el método de pago en la _Revisión y pagos_ paso durante el proceso de cierre de compra.<!--MCLOUD-5930-->

- **Visualización del producto en la página Categoría**: corrige un problema que impedía que los productos se mostraran en la página de categoría de _Mostrar todas las páginas_ vista.<!--MCLOUD-5684-->

- **Carga de imagen del Page Builder**: corrige un problema en la interfaz del Page Builder que, en ocasiones, provocaba el siguiente error al cargar imágenes en la galería de imágenes: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Suprimir advertencias innecesarias de generación de mapas del sitio**: añade un intento de reintento cuando se producen errores durante la generación del mapa del sitio y omite la notificación por correo electrónico del cliente en casos en los que los errores se pueden recuperar automáticamente.<!--MCLOUD-3025-->

- **Mejora del rendimiento del sitio**: corrige un problema de rendimiento con el `Magento\Framework\App\DeploymentConfig\Reader::load` función, que periódicamente experimentaba largos tiempos de carga que afectaban el rendimiento del sitio. <!--MCLOUD-5650-->

- Se ha actualizado la asignación de parches para los parches de método de pago con el fin de que se dirijan a los módulos de pago en lugar del paquete base de Magento (base de magento/magento2), de modo que los parches de pago se apliquen únicamente si existen los módulos de pago.<!--MCLOUD-5666-->

- Se han actualizado los parches de compatibilidad con el Magento Open Source.<!--MCLOUD-5701-->

## Versión 1.0.3

Fecha de publicación: 28 de abril de 2020

- Se ha agregado una corrección para el parche &quot;FPC se está deshabilitando durante las implementaciones&quot; para admitir Adobe Commerce 2.3.5.

## Versión 1.0.2

Fecha de publicación: 27 de febrero de 2020

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Actualizaciones de compatibilidad para parches en la nube de magento/magento**

   - Se ha actualizado el `symfony` y `semver` restricciones de versión en `composer.json` para comprobar la compatibilidad con Adobe Commerce 2.4 y versiones posteriores.<!--MAGECLOUD-5127-->

   - Restricciones actualizadas en `composer.json` para la compatibilidad con `ece-tools` Versiones 2002.0.22 y posteriores de 2002.0.x.

- **Pago y envío con PayPal Express**—Publicado el 12 de febrero de 2020, este parche resuelve un problema que afecta a los pedidos realizados con Pago y envío mediante PayPal Express, en los que la dirección de envío del pedido especifica una región de país que se ha introducido manualmente en el campo de texto en lugar de seleccionarse en el menú desplegable de la página Envío. Consulte la descripción completa del parche en la página de descarga del parche.

- **Corrección de implementación de aplicación**—Se ha añadido un parche para solucionar un problema que deshabilitaba la caché de página completa durante el proceso de implementación. Este parche se aplica a Adobe Commerce 2.3.2 y versiones posteriores.

- **Parámetro de ámbito para API asíncrona/masiva**—Se ha actualizado este parche para corregir un error de sintaxis en el `composer.json` archivo. Este parche se aplica al Magento Open Source 2.3.1 y 2.3.2. Consulte la descripción completa del parche en la página de descarga del parche.

## Versión 1.0.1

Fecha de publicación: 6 de febrero de 2020

Hemos incluido todos los parches de Magento Open Source 2.x desde la página de descargas de software en la versión 1.0.1 de magento/magento-cloud-patch. Si ha copiado parches en el proyecto anteriormente, elimínelos para evitar conflictos.

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Corregir interbloqueos de cron y mejorar el bloqueo de cron**—

   - Corrige un problema con algunos trabajos cron que no se ejecutaban debido a un valor de estado incorrecto en la variable `cron_schedule` tabla. Ahora, utilizamos el marco de bloqueo de Adobe Commerce para comprobar y actualizar el estado del trabajo cron en lugar de utilizar el `cron_schedule` tabla. Los trabajos cron que han finalizado con un estado de error se vuelven a intentar durante la siguiente ejecución cron en lugar de esperar 24 horas.

   - Agrega un _volver a intentar_ para evitar interbloqueos durante las actualizaciones de los datos en el `cron_schedule` tabla.

- **Actualizado `magento/magento-cloud-patches` para incluir todos los parches disponibles para Magento Open Source 2.x**: se ha actualizado el paquete magento/magento-cloud-patch para incluir todos los parches de Magento Open Source 2.x disponibles en la página de descargas de software. Si anteriormente ha copiado parches de Magento Open Source en su proyecto de Adobe Commerce en la nube, elimínelos para evitar conflictos.<!--MAGECLOUD-4606-->

- **Corrección de paginación del catálogo de Elasticsearch** : se ha reemplazado el parche de paginación del catálogo de Elasticsearch entregado en magento/magento-cloud-patch v1.0 con una corrección más eficaz.<!--MAGECLOUD-4847-->

- **Parches de Page Builder**: en Cloud Patches para Commerce 1.0.0, hemos incorporado parches de Page Builder para resolver una vulnerabilidad conocida de ejecución de código remoto (RCE) de Page Builder, con la corrección inicial basada en Adobe Commerce 2.3.3. Hemos actualizado estos parches con una implementación más estable basada en Adobe Commerce 2.3.4., que incluye varias optimizaciones para solucionar el problema.<!--MAGECLOUD-4884-->

  Si tiene el paquete magento/magento-cloud-patch 1.0.0, aún estará protegido de los problemas de vulnerabilidad RCE de Page Builder. Si actualiza a 1.0.1 o posterior, tendrá una mejor implementación de la misma corrección.

## Versión 1.0.0

Fecha de la versión: 14 de noviembre de 2019

Esta es la primera versión de [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) paquete, que es una nueva dependencia para el `ece-tools` versión del paquete 2002.0.22 o versiones posteriores.

Esta versión de incluye las siguientes revisiones y correcciones críticas:

- **Revisiones de seguridad de Page Builder para las versiones 2.3.1.x y 2.3.2.x**: corrige un problema en la vista previa de Page Builder que permite a los usuarios no autenticados acceder a algunos métodos de creación de plantillas que se pueden utilizar para almacenar en déclencheur la ejecución de código arbitrario a través de la red (RCE), lo que provoca fugas de información global. Este problema se puede producir cuando se utilizan versiones no compatibles de Page Builder con Adobe Commerce 2.3.1 y 2.3.2.<!--MAGECLOUD-4649-->

- **Revisiones MSI**: soluciona los problemas que causaban errores de indexación y problemas de rendimiento al utilizar la configuración de inventario por defecto para la gestión de stock.<!--MAGECLOUD-4428-->

- **Compatibilidad con versiones anteriores de nuevas interfaces de correo**-Corrige un problema de incompatibilidad con versiones anteriores causado por el `Magento\Framework\Mail\EmailMessageInterface` Interfaz de PHP incluida en Adobe Commerce v2.3.3. En el ámbito de este parche, la nueva `EmailMessageInterface` hereda del antiguo `MessageInterface`, y los módulos principales de Adobe Commerce se revierten para depender de `MessageInterface`.<!--MAGECLOUD-4422-->

- **La paginación del catálogo no funciona en Elasticsearch 6.x**: corrige un problema crítico con la paginación de resultados de búsqueda que afecta a los clientes que utilizan el Elasticsearch 6.x como motor de búsqueda de catálogos.<!--MAGECLOUD-4448-->
