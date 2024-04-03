---
title: Paquete Cloud Docker
description: Consulte la lista de las mejoras más recientes del paquete Cloud Docker.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2023-07-31T00:00:00Z
exl-id: 907d977f-2e9c-4553-a46b-000bc6a57b28
source-git-commit: 21754f2ee3df586cd03d57210741b36409ad2b36
workflow-type: tm+mt
source-wordcount: '3620'
ht-degree: 0%

---

# Paquete Cloud Docker

El [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) proporciona funcionalidad e imágenes de Docker para implementar Adobe Commerce en un entorno local de Cloud. Estas notas de la versión describen las mejoras más recientes realizadas en este paquete, que es un componente de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

El `magento/magento-cloud-docker` El paquete utiliza la siguiente secuencia de versiones: `<major>.<minor>.<patch>`

Las notas de la versión incluyen:

- ![nuevo icono](../../assets/new.svg) Nuevas funciones
- ![icono de corrección](../../assets/fix.svg) Correcciones y mejoras

<!--Add release notes below-->

## Versión 1.3.6 {#latest}

Fecha de la versión: 31 de julio de 2023

- ![nuevo icono](../../assets/new.svg) **Se ha añadido una nueva versión del servicio**: OpenSearch 2.5.
- ![nuevo icono](../../assets/new.svg) **Habilitar la caché del Compositor**: ahora puede ampliar la configuración de Docker para habilitar la eliminación de caché del Compositor al iniciar el contenedor de Docker. Consulte [Ampliación de la configuración de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) en el _Cloud Docker para Commerce_ guía.

## Versión 1.3.5

Fecha de la versión: 10 de marzo de 2023

- ![nuevo icono](../../assets/new.svg) **ionCube**—Se ha añadido la extensión ionCube para la imagen de PHP 8.1.
- ![nuevo icono](../../assets/new.svg) **Se agregaron nuevas versiones del servicio**—OpenSearch 2.3 y 2.4, PHP 8.2, Varnish 7.1.1.
- ![nuevo icono](../../assets/new.svg) **Compatibilidad mejorada con PHP 8.2**—Se corrigieron problemas de compatibilidad con ciertas versiones de PHP 8.2.x para admitir Commerce 2.4.6.
- ![icono de corrección](../../assets/fix.svg) **Problema del Compositor**: se han corregido problemas que se producían después de actualizar la versión del Compositor en los contenedores de Docker.

## Versión 1.3.4

Fecha de la versión: 27 de octubre de 2022

- ![nuevo icono](../../assets/new.svg) **Se agregaron nuevas imágenes de barniz**: se han añadido imágenes para Varnish 6.5, 7.0 y 7.1.<!-- MCLOUD-7879 -->

## Versión 1.3.3

Fecha de la versión: 13 de septiembre de 2022

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con Apple M1 (ARM64)**: se han añadido cambios en las imágenes de Docker para permitir la compatibilidad con la arquitectura Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![icono de corrección](../../assets/fix.svg) **Mailhog**: se ha corregido un problema por el que el servicio Mailhog no detectaba correos electrónicos en el modo de desarrollador.<!-- MCLOUD-8643 -->
- ![icono de corrección](../../assets/fix.svg) **init-docker.sh**: se ha corregido el validador de versiones de servicio en la variable `init-docker.sh` script.<!-- MCLOUD-8765 -->

## Versión 1.3.2

Fecha de la versión: 31 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Se ha añadido la imagen Elasticsearch 7.10**<!-- MCLOUD-8548 -->

## Versión 1.3.1

Fecha de la versión: 10 de marzo de 2022

- ![nuevo icono](../../assets/new.svg) **Soporte PHP 8.1**—Añadido soporte para PHP 8.1.
- ![nuevo icono](../../assets/new.svg) **OpenSearch**: se han añadido imágenes de las versiones 1.1 y 1.2 de OpenSearch.
- ![nuevo icono](../../assets/new.svg) **Composer 2.1**—Establecer composer 2.1.x de forma predeterminada en imágenes PHP 8.x.
- ![nuevo icono](../../assets/new.svg) **Mejoras en imágenes PHP**—

   - Se agregaron imágenes de PHP 8.1
   - Se ha actualizado xDebug versión 3.1.2
   - Se ha actualizado xmlrpc 1.0.0RC3

- ![icono de corrección](../../assets/fix.svg) **Mejoras de Elasticsearch y OpenSearch**: mejoras en los archivos Docker de Elasticsearch y OpenSearch; se ha eliminado la imagen de Elasticsearch 5.2.
- ![icono de corrección](../../assets/fix.svg) **Extensión de sodio**: se ha activado el `sodium` de forma predeterminada en todas las imágenes de PHP.
- ![icono de corrección](../../assets/fix.svg) **Volumen de caché del Compositor**: ruta fija para que el volumen de caché del Compositor tenga paquetes del Compositor en caché.
- ![icono de corrección](../../assets/fix.svg) **Limitación de memoria en nginx**: se ha corregido una limitación de memoria en la imagen NGINX.

## Versión 1.3.0

Fecha de la versión: 25 de octubre de 2021

- ![icono de corrección](../../assets/fix.svg) **Mejora del flujo de trabajo del modo Desarrollador**: anteriormente, era necesario especificar el modo en los pasos de compilación e implementación. Ahora, la `--mode` en la opción `build` Este paso determina el modo en la versión posterior `deploy` paso. Ya no es necesario configurar el modo después de la implementación. Consulte [Modo de desarrollador](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html).<!-- ACMP-1086 -->
- ![icono de corrección](../../assets/fix.svg) **Mejoras para el sistema de archivos de solo lectura**—<!-- ACMP-1106 -->
   - Se ha corregido un problema que iniciaba un contenedor de PHP para la configuración de correo.
   - Puede utilizar variables de entorno en archivos INI.
   - Asegúrese de que los puntos de entrada de PHP no necesitan permiso de escritura.
- ![icono de corrección](../../assets/fix.svg) **Actualizar nodo**—Actualice la versión del nodo agrupado; al instalar el nodo en imágenes PHP-CLI, ahora utiliza la versión actual de LTS.<!-- ACMP-1539 -->
- ![icono de corrección](../../assets/fix.svg) **Actualizar Symfony**: se han actualizado las dependencias de configuración de Symfony para que sean compatibles con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## Versión 1.2.4

Fecha de la versión: 29 de julio de 2021

- ![nuevo icono](../../assets/new.svg) **Nuevo `Zookeeper` contenedor**—Se ha añadido un [Contenedor de Zookeeper](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#zookeeper-container) para administrar la configuración del proveedor de bloqueos para proyectos que no se implementan en Adobe Commerce en la infraestructura de la nube.<!--MCLOUD-8000-->

- ![nuevo icono](../../assets/new.svg) **Se ha añadido compatibilidad con Composer 2.0.**—Se ha añadido la versión 2.0 del Compositor al fichero de configuración del Compositor para admitir las actualizaciones desde el Compositor 1.0, que está llegando al final de su vida útil.<!--MCLOUD-8003-->

## Versión 1.2.3

Fecha de publicación: 14 de junio de 2021

- ![nuevo icono](../../assets/new.svg) **Se agregó PHP 8.0**—Se ha actualizado PHP a la versión 8.0, lo que le permite aprovechar todas las nuevas funciones y optimizaciones que incluye PHP 8.0.<!--MCLOUD-7941-->
- ![nuevo icono](../../assets/new.svg) **Actualización a Varnish 6.6 y Elasticsearch 7.11.2**: los siguientes vínculos proporcionan información de versión sobre [Caché de barniz 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) y el Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![nuevo icono](../../assets/new.svg) **Añadido `ioncube` extensión para imagen PHP 7.4**: la `ioncube` La extensión ha sido re-añadida a la imagen PHP 7.4 después de haber sido inicialmente excluida de la actualización PHP 7.3 a PHP 7.4. *[Enviado por mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![nuevo icono](../../assets/new.svg) **Se ha añadido una opción de sincronización de archivos:`manual-native`**: la `manual-native` La opción de sincronización de archivos proporciona control manual sobre la sincronización, que proporciona el mejor rendimiento para los entornos de macOS y Windows. Obtenga más información sobre el uso de `manual-native` opción en [Modo de desarrollador](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) y [Sincronización de datos en un entorno para desarrolladores de Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html#file-synchronization-options).<!--MCLOUD-7977-->
- ![nuevo icono](../../assets/new.svg) **Eliminadas las eliminaciones de volumen de `up` y `down` comandos**: la `--volume` se ha eliminado de la `bin/magento-docker up` y `bin/magento-docker down` comandos, reemplazados por el nuevo `bin/magento-docker init` con una advertencia de pérdida de datos. Este cambio ayuda a evitar la pérdida accidental de datos. *[Enviado por joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![icono de corrección](../../assets/fix.svg) **Actualizado `CN` valor del certificado generado**: se han eliminado los codificados `CN` valor de Dockerfile. Este valor creó un error de certificado (`NET::ERR_CERT_INVALID`) que provocó el `--host` para la opción `ece-docker build:compose` comando que se ignorará.<!--MCLOUD-7934-->

## Versión 1.2.2

Fecha de publicación: 20 de abril de 2021

- ![nuevo icono](../../assets/new.svg) **Actualizado `host.docker.internal` para ser independiente de la plataforma**: ahora puede crear los mismos scripts Docker Compose para Ubuntu, Windows y macOS. El uso de Xdebug en Ubuntu ya no requiere una variable de entorno independiente. [Corrección enviada por Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![nuevo icono](../../assets/new.svg) **Se ha actualizado init-docker.sh**—Añadido el `mounts` objeto a `MAGENTO_CLOUD_APPLICATION` variable de entorno. [Corrección enviada por Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![nuevo icono](../../assets/new.svg) **Se ha actualizado init-docker.sh**: se ha actualizado el `init-docker.sh` con las versiones PHP 7.4 y Cloud Docker 1.2.1. [Corrección enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![nuevo icono](../../assets/new.svg) **Sodio activado de forma predeterminada**: se ha activado el `sodium` Extensión PHP de forma predeterminada dentro de imágenes PHP Docker.<!--MCLOUD-7548-->
- ![nuevo icono](../../assets/new.svg) **`custom-registry`opción**—Se ha añadido un `--custom-registry` opción para `php ./vendor/bin/ece-docker build:compose` para utilizar su propio registro de imágenes.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![nuevo icono](../../assets/new.svg) **Se eliminaron las versiones antiguas del Elasticsearch**: se han eliminado las versiones 1.7 y 2.4 del Elasticsearch de las imágenes del Elasticsearch.<!--MCLOUD-7504-->
- ![nuevo icono](../../assets/new.svg) **Generar automáticamente certificados NGINX**: permite eliminar los certificados existentes de la imagen NGINX. Los certificados NGINX ahora se generan automáticamente con cada nueva implementación para mejorar la seguridad.<!--MCLOUD-7396-->
- ![icono de corrección](../../assets/fix.svg) **Habilitado`opcache.validate_timestamps`**: se ha activado el `opcache.validate_timestamps` Configuración de PHP predeterminada en modo de desarrollador. Al habilitar esta configuración, se corrigió el problema en el cual los cambios en el sistema de archivos no se reconocían en Docker.<!--MCLOUD-7466-->
- ![icono de corrección](../../assets/fix.svg) **Fijo`build:custom:compose`**: se ha corregido `build:custom:compose` para generar un error cuando los archivos no se pueden sobrescribir durante el proceso de generación. Producir un error evita situaciones en las que `docker-compose up` podría estar utilizando los archivos incorrectos.<!--MCLOUD-7457-->
- ![icono de corrección](../../assets/fix.svg) **Fijo `--sync_engine="native"` opción**—Se ha corregido el problema en el que en el modo de producción (`--mode="production"`), el `--sync_engine="native"` no crearía ninguna entrada para carpetas locales en la `docker.composer.yml` archivo.<!--MCLOUD-7254-->
- ![icono de corrección](../../assets/fix.svg) **Se corrigieron errores de validación de versión del servicio**: versiones de servicio añadidas para [!DNL RabbitMQ], Elasticsearch y otros servicios a `type` propiedad en el `MAGENTO_CLOUD_RELATIONSHIP` variable. Adición de estas versiones a `relationships` corrigió los errores de validación que se produjeron durante la fase de implementación.<!--MCLOUD-7572-->

## Versión 1.2.1

Fecha de la versión: 21 de diciembre de 2020

- ![nuevo icono](../../assets/new.svg) **Opciones de comando NGINX**: se han añadido opciones de comando de compilación para cambiar el número de NGINX `worker_processes` y NGINX `worker_connections` para TLS y servicios web. El `worker_process` retiene la capacidad de establecer el valor en `auto`. Ejemplos: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![nuevo icono](../../assets/new.svg) **Opción de comando TLS**: se ha añadido la opción de comando de generación para crear una configuración sin el servicio TLS. Ejemplo: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![nuevo icono](../../assets/new.svg) **consumo de memoria NGINX**: se ha reducido la memoria consumida por el proceso NGINX para TLS y servicios web.<!--MCLOUD-7259-->

- ![nuevo icono](../../assets/new.svg) **Blackfire**: extensión PHP del Blackfire deshabilitada de forma predeterminada en la imagen Cloud Docker.

- ![icono de corrección](../../assets/fix.svg) **Contenedor de PHP-FPM**—Se corrigió la comprobación del estado del contenedor PHP-FPM al cambiar el `WEB_PORT` de `80` hasta `8080`.<!--MCLOUD-7232-->

- ![icono de corrección](../../assets/fix.svg) **Nomenclatura de volumen no válida**: se ha corregido un error con la asignación de nombres de volumen no válidos en el modo de desarrollador.<!--MCLOUD-7442-->

- ![icono de corrección](../../assets/fix.svg) **Puerto NGINX de subida**: se ha actualizado la imagen Docker NGINX 1.19 para utilizar el puerto 8080 y evitar así un bucle infinito. [Corrección enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## Versión 1.2.0

Fecha de la versión: 9 de noviembre de 2020

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedores—**

   - ![nuevo icono](../../assets/new.svg) **Contenedor de PHP-FPM**—Añadido soporte para la extensión gnupg PHP. [Corrección enviada por G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![icono de corrección](../../assets/fix.svg) **Contenedor de base de datos**: se ha corregido la comprobación de estado del contenedor de base de datos añadiendo la contraseña de base de datos necesaria al comando de comprobación de estado.<!--MCLOUD-7122-->

   - ![nuevo icono](../../assets/new.svg) **contenedor de Elasticsearch**

      - Se ha agregado compatibilidad con Elasticsearch 7.9 para la compatibilidad con próximas versiones de Adobe Commerce.<!--MCLOUD-7190-->

      - **Configuración del complemento de Elasticsearch**—Se ha agregado compatibilidad para utilizar la información de configuración del complemento Elasticsearch de la `services.yaml` para generar el `docker-compose.yaml` para un entorno de Cloud Docker para Commerce. Consulte [Complementos de Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Compatibilidad con complementos de Elasticsearch**—Se ha agregado compatibilidad con los siguientes complementos de Elasticsearch: `analysis-icu`, `analysis-phonetic`, `analysis-stempel`, y `analysis-nori`. El `analysis-icu` y `analysis-phonetic` Los complementos de están instalados de forma predeterminada. Puede agregar o quitar el `analysis-stempel` y `analysis-nori` complementos según sea necesario.<!--MCLOUD-2789-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor CLI**

      - **Ejecutar comandos dentro de contenedores Docker PHP**—Ahora puede usar la CLI de Cloud Docker para ejecutar comandos dentro de contenedores de PHP en su entorno de Docker sin tener que instalar PHP en el host. Por ejemplo, el siguiente comando crea la configuración:  `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Consulte [CLI de Cloud Docker](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli). [Corrección enviada por G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Se ha agregado el cliente OpenSSH a los contenedores CLI de PHP. Ahora, puede utilizar el reenvío ssh-agent para Composer si la variable `composer.json` contiene repositorios de Git privados que requieren que un cliente ssh use comandos del Compositor.<!--MCLOUD-6008-->

   - ![icono de corrección](../../assets/fix.svg) **Contenedor TLS**: ahora, la variable [Contenedor TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) se basa en `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Imagen Docker en lugar de la imagen CentOS. Este cambio corrige los problemas que ocasionaban errores al enviar solicitudes HTTPS entre contenedores en el entorno de Cloud Docker.<!--MCLOUD-6469-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor de prueba**: se ha añadido un contenedor de prueba para la prueba de aplicaciones y se ha añadido el `--with-test` al Docker. `build:compose` para crear el contenedor solo cuando se realice la prueba en el entorno Docker. Consulte [pruebas de aplicación](https://devdocs.magento.com/cloud/docker/docker-test-app-mftf.html).<!--MCLOUD-6394-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor FPM-XDEBUG**

      - ![nuevo icono](../../assets/new.svg) **Configuración de Xdebug en Linux**—Añadido el `--set-docker-host` a la opción `ece-docker build:compose` para configurar el `host.docker.internal` en el contenedor Xdebug. Esta opción es necesaria para utilizar Xdebug en sistemas Linux. Consulte [Configurar Xdebug para Docker](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-6430-->

      - ![icono de corrección](../../assets/fix.svg) Se ha corregido la configuración de la variable Xdebug para que Docker ENTRYPOINT se resolviera `uninitialized "with_xdebug" variable` errores en los registros. [Corrección enviada por Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![nuevo icono](../../assets/new.svg) **Cambios de configuración de Docker**

   - **Configuración de MailHog**: ahora se puede utilizar lo siguiente `ece-docker build:compose` opciones de comando para deshabilitar MailHog y especificar puertos: `--no-mailhog`, `--mailhog-http-port`, y `--mailhog-smtp-port`. Consulte [Configurar correo electrónico](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Para Cloud Docker para Commerce 1.2.0 y versiones posteriores, Adobe ahora proporciona imágenes de Docker para cada versión del parche y el generador de configuración de Docker crea la configuración de Docker con una versión de parche especificada en lugar de utilizar la última. Anteriormente, el generador de configuración de Docker creaba la configuración con la última versión de parche que podía interrumpir el Docker de Cloud para entornos de Commerce creados con una versión anterior.<!--MCLOUD-7093-->

   - **Especificar imágenes y versiones personalizadas en la configuración personalizada de Cloud Docker**: se ha actualizado el `build:custom:compose` comando con opciones para especificar imágenes y versiones personalizadas al generar un fichero de configuración de composición Docker personalizado (`docker-compose.yaml`). Consulte [Crear una configuración personalizada de Docker Compose](https://devdocs.magento.com/cloud/docker/docker-config-sources.html#build-a-custom-docker-compose-configuration). <!--MCLOUD-7089-->

   - Se ha actualizado la configuración del host Docker para exponer el puerto 443 y habilitar el acceso a Adobe Commerce (`https://magento2.docker`) de todos los contenedores CLI. Puede cambiar el puerto predeterminado agregando el `--tls-port` al generar el archivo de configuración de Docker.<!--MCLOUD-6806-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que la compilación de Cloud Docker para Commerce fallara si la variable `app/etc/env.php` el archivo existe.<!--MCLOUD-6732-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración de compilación para reemplazar los volúmenes con nombre por volúmenes regulares a fin de evitar problemas al implementar Cloud Docker para Commerce en Linux o el subsistema de Windows para Linux (WSL2).<!--MCLOUD-6732-->

- ![icono de corrección](../../assets/fix.svg) Se han actualizado las pruebas funcionales de Cloud Docker para Commerce con el fin de admitir Composer 2.0.<!--MCLOUD-7183-->

## Versión 1.1.2

Fecha de la versión: 9 de septiembre de 2020

- ![nuevo icono](../../assets/new.svg) Se ha agregado compatibilidad con Elasticsearch 7.7<!--MCLOUD-6219-->

## Versión 1.1.1

Fecha de lanzamiento: 5 de agosto de 2020

- ![icono de corrección](../../assets/fix.svg) **Configuración de correo electrónico actualizada**: se ha actualizado la configuración predeterminada de Cloud Docker para Commerce con el fin de admitir el servicio MailHog en lugar de utilizar SendMail. Consulte [Configurar correo electrónico](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-5624-->

- ![icono de corrección](../../assets/fix.svg) Se ha restaurado la biblioteca PS en la configuración del entorno de Cloud Docker para corregirla. `ps:  command not found` errores.<!--MCLOUD-6621-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración predeterminada de Cloud Docker para Commerce a fin de eliminar el montaje automático de los volúmenes de punto de entrada de la base de datos y MariaDB para corregirlos `Cannot create container for service db` errores que se pueden producir al iniciar el entorno de Cloud Docker.

  Ahora puede configurar el entorno de Cloud Docker para montar los directorios de la base de datos agregando las siguientes opciones a la variable `ece-docker build:compose` comando: `--with-entry-point` y `with-mariadb-conf`. Consulte [Opciones de configuración del servicio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-6424-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de comandos de CLI**

| Acción | Comando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Agregar un punto de entrada al contenedor de base de datos para restaurar la base de datos desde la copia de seguridad | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Agregar un volumen de configuración de MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## Versión 1.1.0

Fecha de publicación: 25 de junio de 2020

- ![nuevo icono](../../assets/new.svg) **Compatibilidad añadida para la solución de rendimiento de bases de datos divididas**: ahora puede configurar e implementar un almacén mediante la solución de rendimiento de la base de datos dividida en el entorno de Cloud Docker.<!--MCLOUD-3740-->

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con la implementación de Adobe Commerce y Magento Open Source**: ahora puede utilizar Cloud Docker para Commerce con el fin de implementar un entorno de desarrollo local para proyectos que no estén alojados en Adobe Commerce en infraestructuras en la nube.<!--MCLOUD-5667-->

- ![nuevo icono](../../assets/new.svg) **Compatibilidad con Blackfire.io**—Se ha añadido compatibilidad para utilizar el [Extensión de Blackfire.io](https://devdocs.magento.com/cloud/docker/docker-config-blackfire-io.html) para pruebas de rendimiento automatizadas. [Corrección enviada por Adarsh Manickam de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedores**

   - **Barniz**: ahora Varnish es la memoria caché predeterminada al implementar Adobe Commerce en un entorno de Cloud Docker mediante una versión compatible de la plantilla de aplicación de Cloud. Consulte [Contenedor de barniz](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container).<!--MCLOUD-2634-->

   - Se ha añadido la `--no-varnish` Opción para omitir la instalación del servicio Varnish al generar el archivo de configuración de Cloud Docker.<!--MCLOUD-2634-->

   - ![nuevo icono](../../assets/new.svg) **Base de datos**

      - Se ha añadido la compatibilidad con la base de datos MySQL. Ahora puede configurar el entorno de Cloud Docker con MariaDB o MySQL. Consulte [Opciones de configuración del servicio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-5691-->

      - Se ha añadido la capacidad de definir la configuración de incremento y desplazamiento para la replicación de bases de datos al generar el archivo de composición Docker. Consulte [Contenedores de servicio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers).<!--MCLOUD-5735-->

   - ![nuevo icono](../../assets/new.svg) **PHP-FPM**

      - Se ha agregado compatibilidad con PHP 7.4. [Corrección enviada por Mohanela Murugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Se ha añadido la capacidad de copiar `php.ini` en el directorio raíz del proyecto al entorno Cloud Docker y aplique la configuración personalizada de PHP a los contenedores PHP-FPM y CLI. Consulte [Personalizar la configuración de PHP](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#customize-php-settings). [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - Se ha añadido una comprobación de estado del contenedor. [Corrección enviada por Visanth Sampath de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![icono de corrección](../../assets/fix.svg) **Node.js**: se ha actualizado la versión predeterminada de Node.js de la versión 8 a la versión 10 para mejorar la seguridad. La versión 8 de Node.js está obsoleta y ya no se actualiza con correcciones de errores ni parches de seguridad. [Corrección enviada por Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![nuevo icono](../../assets/new.svg) **Elasticsearch**

      - Se ha agregado compatibilidad con Elasticsearch 6.8, 7.2, 7.5 y 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Se ha añadido la capacidad de personalizar [Configuración del contenedor del Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) cuando genere el fichero de configuración de composición Docker.<!--MCLOUD-3059-->

      - Se ha añadido la `--no-es` a las opciones de configuración del servicio para generar el fichero de configuración de Docker Compose. Utilice esta opción para omitir la instalación del contenedor de Elasticsearch y utilice la búsqueda MySQL en su lugar. Esta opción solo es compatible con las versiones de Adobe Commerce 2.3.5 y anteriores.<!--MCLOUD-3766-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor FPM-XDEBUG**—Se ha añadido una opción de configuración de servicio para instalar y configurar Xdebug para depurar PHP en su entorno Cloud Docker. Consulte [Configurar Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-4098-->

- ![nuevo icono](../../assets/new.svg) **Cambios de configuración de Docker**

   - Se han agregado comprobaciones de estado para los contenedores de servicio PHP-FPM, Redis, Elasticsearch y MySQL Docker.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Se ha cambiado el modo de sincronización de archivos predeterminado a `native` en el modo de desarrollador.<!--MCLOUD-3890 -->

   - Se ha añadido información de versión a la imagen genérica del contenedor del servicio Docker al generar el `docker-compose.yml` archivo.<!--MCLOUD-3878-->

   - Se ha mejorado la capacidad de gestionar grandes respuestas desde el contenedor de PHP-FPM ascendente mediante el aumento de la `fastcgi_buffers` para el servidor Nginx.<!--MCLOUD-5980-->

   - Se ha mejorado el rendimiento de la sincronización de archivos mutágenos añadiendo una segunda sesión de sincronización para sincronizar archivos en `vendor` directorio. Este cambio evita que el mutágeno se bloquee durante el proceso de sincronización de archivos. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![nuevo icono](../../assets/new.svg) **Actualizaciones de comandos de CLI**

| Acción | Comando |
| -------- | --------------- |
| Borrar caché de Redis | `bin/magento-docker flush-redis` |
| Borrar caché de barniz | `bin/magento-docker flush-varnish` |
| Omitir instalación predeterminada de barniz | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personalizar opciones del Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Quitar configuración del Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configurar el contenedor de base de datos con MySQL versión 5.6 o 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Especificar una URL base personalizada | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Agregar contenedor para la configuración Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![icono de corrección](../../assets/fix.svg) Se ha corregido la configuración de la sincronización de archivos de mutágeno para evitar que el mutágeno cree sesiones antiguas. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema de configuración que provocaba errores de sintaxis en el registro de composición de Docker al iniciar el contenedor PHP-FPM. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![icono de corrección](../../assets/fix.svg) Se corrigieron errores de conflictos de volumen que a veces se producían al utilizar varios entornos Docker. [Corrección enviada por G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![icono de corrección](../../assets/fix.svg) Se ha corregido un problema que provocaba que `ece-docker build:compose` error del comando si la configuración incluía Blackfire.io. [Corrección enviada por G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![icono de corrección](../../assets/fix.svg) Se ha actualizado la configuración de la imagen CLI de PHP para evitar errores de memoria insuficiente que se produjeron al instalar varios paquetes mediante Cloud Docker para Commerce. [Corrección enviada por Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![icono de corrección](../../assets/fix.svg) Se ha agregado compatibilidad con varios usuarios de MySQL en el entorno Cloud Docker. En versiones anteriores de, la variable `build:compose` error de la operación si el `magento.app.yaml` el archivo especificó varios usuarios de base de datos. [Corrección enviada por G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![icono de corrección](../../assets/fix.svg) Eliminado `rsyslog` desde los contenedores de Cloud Docker para Commerce PHP para resolver los problemas de compatibilidad que causaban notificaciones de advertencia durante la implementación. Cloud Docker no utiliza la utilidad rsyslog.<!--MCLOUD-6173-->

## Versión 1.0.0

Fecha de la versión: 5 de febrero de 2020

- ![nuevo icono](../../assets/new.svg) **Se ha creado un paquete independiente para enviar`Cloud Docker for Commerce`**: se ha movido el código fuente para ofrecer Cloud Docker para Commerce desde el `ece-tools` repositorio a la [nuevo `magento-cloud-docker` repositorio](https://github.com/magento/magento-cloud-docker) para mantener la calidad del código y proporcionar versiones independientes. El nuevo paquete es una dependencia para ECE-Tools v2002.1.0 y posterior.

  Al actualizar ece-tools, también se actualiza el `magento/magento-cloud-docker` a la versión 1.0.0. Si ha utilizado Cloud Docker para Commerce con una `ece-tools` versión (2002.0.x), revise la [incompatibilidades con versiones anteriores](backward-incompatible-changes.md) y actualice el proyecto como secuencias de comandos, comandos y procesos según sea necesario.

- ![nuevo icono](../../assets/new.svg) **Se ha añadido el control de versiones a las imágenes Docker**: ahora se debe actualizar el `magento/magento-cloud-docker` para obtener las imágenes actualizadas.<!--MAGECLOUD-4737-->

- ![nuevo icono](../../assets/new.svg) **Actualizaciones de contenedores**—

   - ![nuevo icono](../../assets/new.svg) **Contenedor de PHP-FPM**—

      - ![nuevo icono](../../assets/new.svg) **Se ha añadido compatibilidad con Node.js**: se ha actualizado la imagen PHP-FPM para admitir las funciones node, npm y grunt-cli dentro del contenedor de PHP.<!--MAGECLOUD-3953-->

      - ![nuevo icono](../../assets/new.svg) **Se ha añadido la compatibilidad con [ionCube](https://www.ioncube.com/)**: se ha actualizado la configuración por defecto de Docker para admitir ionCube en el entorno de desarrollo local de Docker.<!--MAGECLOUD-4354-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor web**—

      - ![nuevo icono](../../assets/new.svg) **Personalizar la configuración de NGINX**—Se ha añadido la capacidad de montar un personalizado `nginx.conf` en el entorno de Cloud Docker para Commerce. Consulte [Contenedor web](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#web-container).<!--MAGECLOUD-4204-->

      - ![nuevo icono](../../assets/new.svg) **Certificados NGINX generados automáticamente**: el fichero de configuración de Docker incluye ahora la configuración para generar automáticamente certificados NGINX para el contenedor web.<!--MAGECLOUD-4258-->

   - ![nuevo icono](../../assets/new.svg) **Nuevo contenedor de Selenium**—Se ha añadido un [Contenedor de selenio](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#selenium-container) para admitir la prueba de aplicaciones de Adobe Commerce mediante el Marco de prueba funcional de Magento (MFTF).<!--MAGECLOUD-4040-->

   - ![nuevo icono](../../assets/new.svg) **[!DNL RabbitMQ]compatibilidad con versiones de**: se ha actualizado el [!DNL RabbitMQ] configuración de contenedor para admitir [!DNL RabbitMQ] versión 3.8.<!--MAGECLOUD-4674-->

   - ![icono de corrección](../../assets/fix.svg) **Contenedor de base de datos persistente**: la `magento-db: /var/lib/mysql` el volumen de la base de datos ahora persiste después de detener y quitar la configuración de Docker y se restaura al reiniciar la configuración de Docker. Ahora debe eliminar manualmente el volumen de la base de datos. Consulte [Contenedores de base de datos].<!--MAGECLOUD-3978-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor TLS**—

      - ![nuevo icono](../../assets/new.svg) **Se ha actualizado la imagen base del contenedor para utilizar la imagen oficial**: la [Contenedor TLS de nube](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) La imagen ahora se basa en el `debian:jessie` Imagen de Docker.—<!--MAGECLOUD-4163-->

      - ![nuevo icono](../../assets/new.svg) **Se ha agregado compatibilidad con [Proxy de terminación TLS de libras]**: la [Archivo de configuración de almohadilla](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) agrega las siguientes variables ENV para personalizar la configuración de Docker para el contenedor TLS:

         - **`TimeOut`**: permite definir el valor de tiempo de espera Tiempo en primer byte (TTFB). El valor predeterminado es 300 segundos.

         - **`RewriteLocation`**: permite determinar si el proxy Pound reescribe la ubicación en la URL de solicitud de forma predeterminada. El valor predeterminado es `0` para evitar que la reescritura interrumpa las redirecciones a sitios web externos como un sitio de SSO externo. [Corrección enviada por Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![nuevo icono](../../assets/new.svg) Se ha aumentado el valor de tiempo de espera en la configuración del contenedor TLS de 15 a 300 segundos. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor de barniz**—

      - ![nuevo icono](../../assets/new.svg) **Se ha actualizado la imagen base del contenedor para utilizar la imagen oficial**: la [Contenedor de barniz de nube](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) ahora se basa en el `centos` Imagen de Docker.<!--MAGECLOUD-4163-->

      - ![nuevo icono](../../assets/new.svg) **Configuración de tiempo de espera predeterminada mejorada**-Añadido `.first_byte_timeout` y `.between_bytes_timeout` configuración en el contenedor de barniz. Ambos valores de tiempo de espera predeterminados son `300s` (5 minutos). [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![icono de corrección](../../assets/fix.svg) **Omitir barniz durante sesiones de Xdebug**: se ha actualizado la configuración del contenedor de barniz para que devuelva `pass` en solicitudes recibidas cuando Xdebug está habilitado. En versiones anteriores, no se podía utilizar Xdebug si el entorno Docker incluía Varnish. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![nuevo icono](../../assets/new.svg) **Cambios de configuración de Docker**—

   - ![nuevo icono](../../assets/new.svg) **Administrar montajes y volúmenes para el proyecto**: se ha añadido la capacidad de administrar montajes y volúmenes al iniciar un entorno Docker para el desarrollo local. Consulte [Compartir datos de proyectos].<!--MAGECLOUD-3248-->

   - ![nuevo icono](../../assets/new.svg) **Compatibilidad con el modo de puente de red**: se ha añadido compatibilidad con el modo de puente de red para habilitar conexiones entre contenedores de Docker a través de la red local.<!--MAGECLOUD-4165-->

   - ![nuevo icono](../../assets/new.svg) **Contenedor Cron deshabilitado de forma predeterminada**: para mejorar el rendimiento, el contenedor de Cron ya no está configurado de forma predeterminada al crear el entorno de Docker. Puede usar el complemento `--with-cron` opción del comando Docker build para agregar un contenedor Cron a su entorno. Consulte [Administración de trabajos cron](https://devdocs.magento.com/cloud/docker/docker-manage-cron-jobs.html).<!--MAGECLOUD-5181-->

   - ![nuevo icono](../../assets/new.svg) **Detener la sincronización de archivos de copia de seguridad grandes**—Se han añadido volcados de base de datos y archivos de almacenamiento (ZIP, SQL, GZ y BZ2) a la lista de exclusión de `dist/docker-sync.yml` y `dist/mutagen.sh` archivos. La sincronización de archivos grandes (>1 GB) puede provocar un periodo de inactividad y los archivos de copia de seguridad no suelen requerir sincronización, ya que se pueden regenerar.<!--MAGECLOUD-3979-->

- ![nuevo icono](../../assets/new.svg) **Cambios de comando**—

   - ![icono de corrección](../../assets/fix.svg) Se ha cambiado el nombre `./bin/docker` archivo a `./bin/magento-docker` para solucionar un problema que provocaba que algunos entornos de Docker se dañaran porque la variable `./bin/docker` sobrescribe los archivos binarios existentes de Docker. Este es un [cambio incompatible con versiones anteriores](backward-incompatible-changes.md) que requiere actualizaciones de los scripts y comandos.<!-- MAGECLOUD-4038 -->

   - ![nuevo icono](../../assets/new.svg) **Se ha añadido una opción de configuración de servicio para exponer el puerto de base de datos al host**: utilice el `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` opción para exponer el puerto de la base de datos al host al crear el `docker-compose.yml` archivo: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![nuevo icono](../../assets/new.svg) **Nuevo comando posterior a la implementación**: anteriormente, los vínculos posteriores a la implementación definidos en la variable `.magento.app.yaml` se ejecutó automáticamente después de implementar Adobe Commerce en un contenedor de Cloud Docker mediante el `cloud-deploy` comando. Ahora, debe emitir un `cloud-post-deploy` para ejecutar los vínculos posteriores a la implementación después de la implementación. Consulte las instrucciones de lanzamiento actualizadas para [promotor](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) y [producción](https://devdocs.magento.com/cloud/docker/docker-mode-production.html) modo.<!--MAGECLOUD-3996-->

   - ![nuevo icono](../../assets/new.svg) Se ha añadido la `--rm` opción para `./bin/magento-docker` comandos para los contenedores build e deploy. Esto elimina el contenedor una vez completada la tarea.<!--MAGECLOUD-4205-->

   - ![nuevo icono](../../assets/new.svg) **Actualizaciones de `build:compose` mando**—

      - ![nuevo icono](../../assets/new.svg) Se ha añadido la `--sync-engine="native"` a la opción `docker-build` para desactivar la sincronización de archivos cuando genere el archivo de configuración Docker Compose en modo de desarrollador. Utilice esta opción cuando desarrolle en sistemas Linux, que no requieren sincronización de archivos para el desarrollo local de Docker. Consulte [Sincronización de datos en el entorno de Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![nuevo icono](../../assets/new.svg) Se ha cambiado la configuración predeterminada de sincronización de archivos de `docker-sync` hasta `native`. [Corrección enviada por Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![nuevo icono](../../assets/new.svg) **Mejoras de validación**—

   - ![nuevo icono](../../assets/new.svg) Se ha agregado validación al proceso de implementación para los entornos de desarrollo local de Docker para verificar que la configuración del entorno de nube incluye la clave de cifrado necesaria para descifrar la base de datos. Ahora, aparece un mensaje de error en el registro si la configuración del entorno no especifica un valor para la clave de cifrado.<!--MAGECLOUD-4423-->

   - ![nuevo icono](../../assets/new.svg) Se ha agregado una comprobación de estado del contenedor al servicio Elasticsearch para garantizar que el servicio esté listo antes de continuar con el procesamiento de la compilación y la implementación. Si la comprobación de estado devuelve un error, el contenedor se reinicia automáticamente.<!--MAGECLOUD-4456-->
