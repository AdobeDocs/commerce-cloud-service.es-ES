---
title: Arquitectura de inicio
description: Obtenga información acerca de los entornos admitidos por la arquitectura de inicio.
feature: Cloud, Paas
exl-id: 03365d32-4eb4-42d4-82a7-771df5e7b3da
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Arquitectura de inicio

Su Adobe Commerce en la infraestructura de la nube La arquitectura inicial admite hasta **cuatro** entornos, incluido un `master` que contiene el código inicial del proyecto, el entorno de ensayo y hasta dos entornos de integración.

Todos los entornos están en contenedores PaaS (Platform as a service). Estos contenedores se implementan dentro de contenedores altamente restringidos en una cuadrícula de servidores. Estos entornos son de solo lectura, y aceptan cambios de código implementado desde ramas insertadas desde el espacio de trabajo local. Cada entorno proporciona una base de datos y un servidor web.

Puede utilizar cualquier metodología de desarrollo y ramificación que desee. Cuando obtenga acceso inicial a su proyecto, cree un `staging` entorno desde el `master` entorno. A continuación, cree el `integration` entorno mediante ramificación desde `staging`.

## Arquitectura del entorno de inicio

El diagrama siguiente muestra las relaciones jerárquicas de los entornos de Starter.

![Vista de alto nivel del proyecto de inicio](../../assets/starter/architecture.png)

## Entorno de producción

El entorno de producción proporciona el código fuente para implementar Adobe Commerce en la infraestructura de nube que ejecuta sus tiendas públicas de uno o varios sitios. El entorno de producción utiliza código de la variable `master` para configurar y habilitar el servidor web, la base de datos, los servicios configurados y el código de la aplicación.

Debido a que el `production` entorno es de solo lectura, utilice el `integration` para realizar cambios en el código, implemente en toda la arquitectura desde el `integration` hasta `staging`y, por último, al `production` entorno. Consulte [Implementar la tienda](../deploy/staging-production.md) y [Lanzamiento del sitio](../launch/overview.md).

El Adobe recomienda realizar pruebas completas en su `staging` antes de empujar a la `master` , que se implementa en `production` entorno.

## Entorno de ensayo

El Adobe recomienda crear una rama llamada `staging` de `master`. El `staging` branch implementa el código en el entorno de ensayo para proporcionar un entorno de preproducción que pruebe el código, los módulos y las extensiones, las puertas de enlace de pago, el envío, los datos de productos y mucho más. Este entorno proporciona la configuración para que todos los servicios coincidan con el entorno de producción, incluidos Fastly, New Relic APM y search.

En las secciones adicionales de esta guía se proporcionan instrucciones para las implementaciones de código finales y la prueba de interacciones a nivel de producción en un entorno de ensayo seguro. Para obtener el mejor rendimiento y las mejores pruebas de características, duplique la base de datos en el entorno de ensayo.

>[!WARNING]
>
>El Adobe recomienda probar la interacción de cada comerciante y cliente en el entorno de ensayo antes de implementarla en el entorno de producción. Consulte [Implementar la tienda](../deploy/staging-production.md) y [Probar implementación](../test/staging-and-production.md).

## Entorno de integración

Los desarrolladores utilizan el `integration` entorno para desarrollar, implementar y probar:

- código de aplicación Adobe Commerce

- Custom Code

- Extensiones

- Servicios

**Casos de uso recomendados:**

Los entornos de integración están diseñados para pruebas y desarrollo limitados. Por ejemplo, puede utilizar el entorno de integración para completar las siguientes tareas:

- Asegúrese de que los cambios en los procesos de integración continua (CI) sean compatibles con la nube

- Pruebe flujos de trabajo críticos en páginas clave como Inicio, Categoría, Página de detalles del producto (PDP), Cierre de compra y Administración.

Para obtener el mejor rendimiento en el entorno de integración, siga estas prácticas recomendadas:

- Restringir el tamaño del catálogo

- Limitar el uso a uno o dos usuarios simultáneos

- Deshabilite los trabajos cron y ejecute manualmente según sea necesario

Puede tener hasta **dos** entornos de integración activos. Puede crear un entorno de integración creando una rama desde el `staging` Rama. Cuando crea un entorno de integración, el nombre del entorno coincide con el nombre de la rama. Un entorno de integración incluye un servidor web y una base de datos. No incluye todos los servicios; por ejemplo, Fastly, CDN y New Relic no están disponibles.

Puede tener un número ilimitado de ramas inactivas para el almacenamiento de código. Para acceder, ver y probar una rama inactiva, debe activarla

{{enhanced-integration-envs}}

## Pila de tecnología de producción y ensayo

Los entornos de producción y ensayo incluyen las siguientes tecnologías. Puede modificar y configurar estas tecnologías mediante el [`.magento.app.yaml`](../application/configure-app-yaml.md) archivo.

- Rápidamente para almacenamiento en caché HTTP y CDN
- Nginx servidor web hablando con PHP-FPM, una instancia con varios trabajadores
- Servidor Redis
- Elasticsearch para la búsqueda en el catálogo de Adobe Commerce 2.2 a 2.4.3-p2
- AbraBuscar en el catálogo Adobe Commerce 2.3.7-p3, 2.4.3-p2 y 2.4.4 y posterior
- Filtrado de salidas (cortafuegos de salida)

### Servicios

Adobe Commerce en la infraestructura en la nube admite actualmente los siguientes servicios: PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce 2.2 a 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 y posterior), Redis y [!DNL RabbitMQ].

Cada servicio se ejecuta en un contenedor independiente y seguro. Los contenedores se administran juntos en el proyecto. Algunos servicios son estándar, como los siguientes:

- Enrutador HTTP (administra solicitudes entrantes, pero también almacena en caché y redirige)

- servidor de aplicaciones PHP

- Git

- Secure Shell (SSH)

### Versiones de software

Adobe Commerce en la infraestructura en la nube utiliza el sistema operativo Debian GNU/Linux y el servidor web NGINX. No puede actualizar este software, pero puede configurar versiones para lo siguiente:

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

En los entornos de ensayo y producción, se utiliza Fastly para la CDN y el almacenamiento en caché. La última versión de la extensión CDN de Fastly se instala durante el aprovisionamiento inicial del proyecto. Puede actualizar la extensión para obtener las últimas correcciones y mejoras de errores. Consulte [Módulo de CDN de Fastly para Magento 2](https://github.com/fastly/fastly-magento2). Además, tiene acceso a [New Relic](../monitor/account-management.md) para la monitorización del rendimiento.

Utilice los siguientes archivos para configurar las versiones de software que desea utilizar en la implementación.

- [`.magento.app.yaml`](../application/configure-app-yaml.md)

- [`routes.yaml`](../routes/routes-yaml.md)

- [&quot;services.yaml&quot;](../services/services-yaml.md)

### Backup y recuperación ante desastres

Puede crear una copia de seguridad de la base de datos y del sistema de archivos mediante el [!DNL Cloud Console] o la CLI. Consulte [Administración de backup](../storage/snapshots.md).

## Preparación para el desarrollo

El siguiente flujo de trabajo resume el proceso para bifurcar el código, desarrollar e implementar la tienda:

1. Configurar el entorno local

1. Clonar el `master` ramificar en el entorno local.

1. Crear un `staging` bifurcar desde `master`

1. Crear ramas para desarrollo desde `staging`

1. Código push para Git que crea e implementa en un entorno para realizar pruebas

Consulte las secciones siguientes para obtener instrucciones detalladas y tutoriales para desarrollar, probar e implementar su tienda:

- [Iniciar flujo de trabajo de desarrollo e implementación](starter-develop-deploy-workflow.md)

- [Desarrollo de Docker](../dev-tools/cloud-docker.md) (entorno de desarrollo local habilitado por Cloud Docker para Commerce)

- [Administrar ramas](../project/console-branches.md)

- [Implementar la tienda](../deploy/staging-production.md)

- [Lanzamiento del sitio](../launch/overview.md)
