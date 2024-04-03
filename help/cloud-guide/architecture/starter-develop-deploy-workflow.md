---
title: Flujo de trabajo de proyecto inicial
description: Aprenda a utilizar los flujos de trabajo de desarrollo e implementación de Starter.
feature: Cloud, Paas
exl-id: f334047a-1e0d-45c7-bf96-5c2964741951
source-git-commit: 08f43a3b0a50cdb2a5e8a45bd2e2448bc6dbca2b
workflow-type: tm+mt
source-wordcount: '2103'
ht-degree: 0%

---

# Flujo de trabajo de proyecto inicial

Adobe Commerce en la nube incluye un único repositorio de Git con una `master` rama para el entorno de producción que se puede ramificar para crear un entorno de ensayo y varios entornos de integración para el trabajo de prueba y desarrollo. Puede tener hasta cuatro entornos activos, incluido un `master` para su servidor de producción. Consulte [Arquitectura de inicio](starter-architecture.md) para obtener una descripción general.

Para sus entornos, siga las [!UICONTROL Development > Staging > Production] flujo de trabajo para desarrollar e implementar el sitio.

- **Entorno de producción (sitio activo)**: proporciona un entorno de producción completo con todos los servicios creados e implementados a partir del código en `master` Rama.
- **Entorno de ensayo**: proporciona un entorno de ensayo completo que coincide con el entorno de producción con todos los servicios creados e implementados desde un `staging` rama que crea mediante clonación desde `master`.
- **Entornos de integración**: proporciona hasta dos entornos de desarrollo activos que se pueden crear a partir de `staging` Rama. El `integration` El entorno de no admite servicios de terceros como Fastly y New Relic.

Para sus ramas, puede seguir cualquier metodología de desarrollo. Por ejemplo, puede seguir una metodología Agile como scrum para crear ramas para cada sprint.

Desde cada sprint, puede crear ramas para cada historia de usuario. Todas las historias se vuelven comprobables. Puede combinar continuamente con la rama de sprint y validar esa rama de forma continua. Cuando termine el sprint, puede combinar la rama de sprint con `master` para implementar todos los cambios de sprint en producción sin tener que lidiar con un cuello de botella de prueba.

## Flujo de trabajo de desarrollo

El desarrollo y la implementación en planes Starter comienzan con su proyecto inicial. El proyecto se crea con el &quot;sitio en blanco&quot;, que es un repositorio de código de plantilla de Adobe Commerce en la infraestructura de la nube con una tienda completamente preparada. Esto crea un `master` con una copia del código del entorno de producción.

El flujo de trabajo de desarrollo incluye lo siguiente:

- [Clonar y bifurcar](#clone-and-branch) desde el `master` para crear `staging` y ramas de desarrollo
- [Desarrollar código](#develop-code) e instalar extensiones localmente en una rama de desarrollo, lo que incluye [!DNL Composer] actualizaciones
- [Configurar](#configure-store) su configuración de tienda y extensión
- [Generar configuración](#generate-configuration-management-files) archivos de administración
- [Código push](#push-code-and-test) Configuración de y para generar e implementar en `staging` y `production` entornos

![Desarrollo e implementación del flujo de trabajo](../../assets/starter/workflow.png)

También tiene algunos pasos opcionales para ayudarle a desarrollar y probar el código y los datos de la tienda:

- [Instalación de datos de ejemplo](#optional-install-sample-data) a su tienda
- [Extraer datos del almacén de producción](#optional-pull-production-data) hasta entornos

Este proceso supone que ha configurado su [espacio de trabajo del desarrollador local](../development/overview.md).

### Clonar y bifurcar

Para un nuevo proyecto de plan inicial, un `master` La rama de se clonó desde el repositorio Git de Adobe Commerce en la infraestructura de la nube. Para iniciar la bifurcación y el trabajo con el código, clone el `master` ramificar en el entorno local.

El formato del comando Git clone es el siguiente:

```bash
git fetch origin
```

```bash
git pull origin <environment-ID>
```

La primera vez que empiece a trabajar en ramas para el proyecto de inicio, cree un `staging` Rama. Esto crea una rama de código que coincide con el `master` que se implementa en un entorno de ensayo para probar los cambios de configuración y código antes de implementarlos en el entorno de producción.

A continuación, cree ramas desde `staging` para desarrollar código, añadir extensiones y configurar integraciones de terceros. Cada vez que desarrolle un código personalizado, añada extensiones, integre un servicio de terceros o trabaje en una rama de desarrollo creada a partir de `staging` Rama. Tiene cuatro entornos de integración activos disponibles. Al insertar una rama activa, uno de estos entornos de integración implementa automáticamente el código para probarlo.

El formato del comando de rama Git es el siguiente:

```bash
git checkout <branch-name>
```

El formato de la CLI de nube `branch` el comando es:

```bash
magento-cloud environment:branch <environment-name> <parent-environment-ID>
```

![Rama a partir de página maestra](../../assets/starter/branching.png)

### Desarrollar código

Con la rama base de Adobe Commerce en el código de infraestructura en la nube, puede empezar a instalar extensiones, desarrollar código personalizado, agregar temáticas y mucho más.

Utilice una estrategia de ramificación con el trabajo de desarrollo. El uso de una rama para realizar todo el trabajo a la vez puede dificultar las pruebas. Por ejemplo, puede seguir las metodologías de integración continua y sprint para funcionar:

- Añada algunas extensiones y configúrelas con la primera rama
- Inserte este código, pruebe y combine en Ensayo y luego en Producción
- Configure completamente sus servicios en `services.yaml` y añada una temática
- Inserte este código, pruebe y combine en Ensayo y luego en Producción
- Integración con un servicio de terceros
- Inserte este código, pruebe y combine en Ensayo y luego en Producción

Hasta que su tienda esté completamente creada, configurada y lista para iniciarse. Pero sigue leyendo, hay muchas opciones para tu tienda y configuración de código.

>[!NOTE]
>
>No complete todavía ninguna configuración en su estación de trabajo local.

![Código push de local](../../assets/starter/push-code.png)

### Configurar tienda

Cuando esté listo para configurar su tienda, inserte todo el código en el `integration` entorno. Configure los ajustes de la tienda desde el administrador para el entorno de integración, no en el entorno local. Puede encontrar la dirección URL haciendo clic en **Acceso al sitio** en el [!DNL Cloud Console]

Para obtener la mejor información sobre las configuraciones, consulte la documentación de Adobe Commerce y las extensiones instaladas. A continuación se muestran algunos vínculos e ideas que le ayudan a empezar:

- [Prácticas recomendadas para la configuración de tiendas](../store/best-practices.md) para conocer las prácticas recomendadas específicas en la nube
- [Configuración básica](https://docs.magento.com/user-guide/configuration/configuration-basic.html) para acceso de administrador de tienda, nombre, idiomas, monedas, marca, sitios, vistas de tienda y más
- [Tema](https://docs.magento.com/user-guide/design/design-theme.html) para mejorar el aspecto del sitio y de las tiendas, incluidos CSS y diseños
- [Configuración del sistema](https://docs.magento.com/user-guide/system/system.html) para funciones, herramientas, notificaciones y la clave de cifrado de la base de datos
- Configuración de extensión mediante su documentación

Más allá de la configuración de tienda, puede configurar varios sitios y tiendas, servicios configurados y mucho más. Consulte [Configurar la tienda](../store/overview.md).

### Generar archivos de administración de configuración

Si está familiarizado con Adobe Commerce, puede que le preocupe cómo obtener los ajustes de configuración de la base de datos en los entornos de desarrollo, ensayo y producción. Anteriormente, tenía que copiar todos los ajustes de configuración en papel o en un archivo y, a continuación, aplicarlos manualmente a otros entornos. O puede que haya volcado su base de datos y haya insertado esos datos en otro entorno.

Adobe Commerce en la infraestructura en la nube proporciona un conjunto de dos [Administración de configuración](../store/store-settings.md) comandos que exportan las opciones de configuración de su entorno a un archivo. Estos comandos solo están disponibles para **Adobe Commerce en la nube 2.2 y versiones posteriores**.

- `php .vendor/bin/ece-tools config:dump`: permite exportar sólo los valores de configuración introducidos o modificados desde valores por defecto a un fichero de configuración. _Recomendado_.
- `php bin/magento app:config:dump`: permite exportar todas las opciones de configuración, incluidas las modificadas y las por defecto, a un fichero de configuración.

El archivo generado es `app/etc/config.php`.

Genere el archivo en el entorno de integración donde configuró Adobe Commerce. Siga el proceso de generación del archivo, agregarlo a la rama e implementarlo.

**Notas importantes** en Administración de configuración:

- Cualquier configuración incluida en el archivo generada a partir de `app:config:dump` El comando está bloqueado para que no se pueda editar o solo lectura en el entorno implementado. Esta es una de las razones por las que Adobe recomienda utilizar `.vendor/bin/ece-tools config:dump` comando.

  Por ejemplo, puede instalar un módulo para Fastly en su entorno de desarrollo. Solo puede configurar este módulo en el entorno de ensayo y producción. Uso del `.vendor/bin/ece-tools config:dump` El comando mantiene editables esos campos predeterminados al implementar los cambios de desarrollo en el entorno de ensayo y producción.

- El archivo generado puede ser largo en función del tamaño de la implementación. El `.vendor/bin/ece-tools config:dump` genera un archivo más pequeño que el archivo generado por el comando `app:config:dump` comando.

Si utiliza la versión 2.2 o posterior de Adobe Commerce, los comandos de administración de configuración proporcionan una función adicional para proteger los datos confidenciales, como las credenciales de la zona protegida para un módulo de PayPal. Durante el proceso de exportación, cualquier valor que contenga datos confidenciales se exporta a un archivo de configuración independiente:`env.php` en el `app/etc/` directorio. Este archivo permanece en su entorno local y no se copia cuando inserta el código en otra rama. También puede crear variables de entorno con comandos CLI en todas las versiones de infraestructura en la nube de Adobe Commerce.

![Generar variables de entorno](../../assets/starter/env-variables.png)

Consulte [Administración de configuración](../store/store-settings.md).

### Código push y prueba

En este punto, debería tener una rama de código desarrollada con un archivo de configuración (`config.local.php` o `config.php`) listo para probar.

Cada vez que inserta código desde el entorno local, se ejecuta una serie de scripts de compilación e implementación. Estos scripts generan código nuevo e lo implementan en el entorno remoto. Por ejemplo, si está insertando una rama de desarrollo desde el entorno local a la rama remota, un entorno coincidente actualiza servicios, código y contenido estático.

Puede acceder directamente a este entorno con una URL de tienda, una URL de administrador y un SSH. Estos entornos incluyen un servidor web, una base de datos y servicios configurados. Cuando esté listo, puede empezar a implementar y probar en el entorno de ensayo.

Para obtener más información, consulte [Flujo de trabajo de implementación](#deployment-workflow).

### Opcional: Instalar datos de ejemplo

Si necesita datos de ejemplo al desarrollar su tienda, puede instalar datos de ejemplo. Estos datos simulan una tienda activa, incluidos clientes, productos y otros datos. Estos datos de ejemplo funcionan mejor con una instalación de Adobe Commerce de &quot;sitio en blanco&quot; en plantillas de infraestructura en la nube al crear el proyecto. Se recomienda eliminar los datos de ejemplo antes de ponerlos en marcha. Consulte [Instalar datos de ejemplo opcionales](../test/sample-data.md).

![Instalar datos de ejemplo opcionales](../../assets/starter/sample-data.png)

### Opcional: Extraer datos de producción

Añada todos sus productos, catálogos, contenido del sitio, etc. directamente al `production` entorno. Al agregar estos datos al entorno de producción, puede proporcionar precios, cupones, existencias de inventario, anuncios de ventas actualizados, información sobre ofertas futuras y mucho más para sus clientes. Estos datos no incluyen las configuraciones de extensión que se configuran en la rama de desarrollo local.

A medida que desarrolla funciones, agrega extensiones y diseña temas, es útil tener datos reales con los que trabajar. En cualquier momento, puede [crear un volcado de base de datos](../storage/database-dump.md) desde el entorno de producción e inserte eso en los entornos de ensayo e integración según sea necesario.

Para ayudar a exportar los datos de producción como datos de prueba para utilizarlos en entornos de ensayo e integración:

- [Ejecutar las utilidades de soporte](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) Comandos CLI (recomendados) al exportar una copia de seguridad protegida del cliente y almacenar datos mediante la clave de cifrado de Adobe Commerce

- [Recopilación de datos](https://docs.magento.com/user-guide/system/support-data-collector.html) herramienta para generar y exportar datos

Para migrar estos datos, consulte [Migración e implementación de datos y archivos estáticos](../deploy/staging-production.md#migrate-static-files).

![Extraer y sanear datos de producción](../../assets/starter/data-code-process.png)

>[!NOTE]
>
>Antes de transferir los datos a otro entorno, debe considerar la posibilidad de sanearlos. Tiene un par de opciones, incluidas [uso de utilidades de soporte](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) o el desarrollo de un script para borrar los datos de los clientes.

>[!WARNING]
>
>No inserte una base de datos desde un entorno de integración o ensayo a un entorno de producción. Si lo hace, los datos del entorno de integración o ensayo sobrescriben los datos de producción activos, incluidas las ventas, los pedidos, los clientes nuevos y actualizados, etc.

## Flujo de trabajo de implementación

Como se detalla en la información de arquitectura, Adobe Commerce en la infraestructura en la nube está impulsado por Git. La implementación de Adobe Commerce en la infraestructura de la nube forma parte de los procesos push de Git para las ramas.

Cuando inserta código ramificado desde el entorno local a la rama remota, comienza una serie de scripts de compilación e implementación.

Scripts de compilación:

- El sitio en el entorno de destino continúa ejecutándose durante la compilación

- Compruebe y ejecute Adobe Commerce en los parches y revisiones de la infraestructura en la nube

- Compile su código con un registro de compilación e implementación

- Compruebe la administración de la configuración si la implementación de contenido estático se produce durante esta fase

- Cree o utilice un slug de código sin cambiar para un proceso más rápido

- Aprovisionar todos los servicios y aplicaciones back-end

Implementar scripts:

- Coloca el sitio en el entorno de destino en modo de mantenimiento

- Implementa contenido estático si no se completa durante la generación

- Instala o actualiza Adobe Commerce en la infraestructura en la nube

- Configurar el enrutamiento para el tráfico

Una vez completada, la tienda vuelve a estar en línea y activa, con todo el código y las configuraciones actualizados.

Consulte [Proceso de implementación](../deploy/process.md).

### Insertar en Ensayo y probar

Inserte siempre el código en iteraciones en el `staging` entorno para pruebas completas. La primera vez que utilice este entorno, debe configurar algunos servicios, como [Rápido](/help/cloud-guide/cdn/fastly.md) y [New Relic](../monitor/new-relic-service.md). Además, configure puertas de enlace de pago, envíos, notificaciones y otros servicios vitales con credenciales de zona protegida o de prueba.

El ensayo es un entorno de preproducción que proporciona todos los servicios y configuraciones lo más cerca posible de la producción. Pruebe exhaustivamente cada servicio, verifique sus herramientas de prueba de rendimiento, realice pruebas de UAT como administrador y como cliente hasta que sienta que su tienda está lista para la producción.

Consulte [Implementar la tienda](../deploy/staging-production.md).

### Insertar en producción

Cuando se inserta el `master` sucursal, está empujando a la `production` entorno. Complete las actividades de configuración y prueba en el entorno de producción como lo hizo en el entorno de ensayo, con una diferencia importante. En el entorno de producción, utilice credenciales activas para la configuración y las pruebas. En el momento en que inicie el sitio, los clientes pueden completar las compras y los administradores pueden administrar la tienda activa.

Consulte [Implementar la tienda](../deploy/staging-production.md).

### Lanzamiento del sitio

Hay una guía clara para publicar en su sitio. Después de completar estos pasos, su tienda puede servir productos en su tema personalizado para la venta inmediatamente.

Consulte [Lanzamiento del sitio](../launch/overview.md).

## Integración continua

Siguiendo las metodologías de ramificación y desarrollo, puede desarrollar fácilmente nuevas funciones, configurar cambios y agregar extensiones para desarrollar e implementar actualizaciones de forma continua.

Todos los entornos de infraestructura en la nube admiten la integración continua para actualizaciones constantes. Este flujo de trabajo admite lanzamientos varias veces al día o en una programación establecida según las necesidades de la empresa.

- Crear ramas de desarrollo con funciones y cambios futuros

- Pruebe el código en su `integration` entorno

- Implementación y pruebas en `staging` entorno

- Implementación en `production` entorno
