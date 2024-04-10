---
title: Información general de desarrollo
description: Prepárese para el desarrollo local con un proyecto de Adobe Commerce en la nube.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: d4452d7d-d3dc-4f8d-8bd7-76f05d89f545
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Información general de desarrollo

Los entornos remotos de Adobe Commerce en la infraestructura en la nube son **Solo lectura**, incluidos todos los entornos de inicio y todos los entornos de integración, ensayo y producción de Pro. En un entorno de desarrollo local, puede escribir y probar el código antes de insertarlo en un entorno de integración para realizar más pruebas e implementaciones en Ensayo y producción.

Antes de preparar el espacio de trabajo local, asegúrese de que dispone de lo siguiente [credenciales](../../get-started/prepare-workspace.md). El desarrollo local requiere la instalación de PHP y Composer a menos que opte por utilizar [Cloud Docker para Commerce](#docker-environment).

## Paquetes necesarios

Adobe Commerce en la infraestructura en la nube usa Composer para administrar las dependencias y actualizaciones de los proyectos. Para el desarrollo local, debe instalar las versiones de PHP y Composer compatibles con su proyecto en la nube. Por ejemplo, si está utilizando la variable [!DNL Commerce] 2.4.7, puede ver que la variable [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.7/.magento.app.yaml) el archivo de configuración utiliza **PHP 8.3** y **Compositor 2.7.2**.

Compositor instala las bibliotecas y dependencias requeridas para su proyecto en la `vendor` directorio. Los siguientes ficheros de composición requeridos se encuentran en el directorio raíz del proyecto:

- `composer.json`: utilice el `composer.json` para administrar instalaciones y actualizaciones de productos.
- `composer.lock`: la `composer.lock` El archivo almacena un conjunto de dependencias de versión exactas que satisfacen las restricciones de versión de cada requisito para cada paquete en el árbol de dependencias del proyecto.

**Comandos comunes:**

| Comando | Descripción |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Actualizaciones de las versiones más recientes de las dependencias reflejadas en la `composer.json` archivo. Esto actualiza el `composer.lock` archivo. |
| `composer install` | Lee la `composer.lock` para descargar dependencias. Se recomienda mantener una copia actualizada de `composer.lock` en el repositorio del proyecto. |

{style="table-layout:auto"}

Una vez que agregue, confirme e inserte el código actualizado, el proceso de implementación ejecuta automáticamente el `composer install` durante la [fase de compilación](../deploy/process.md#build-phase-build-phase).

### Metapaquete de nube

Adobe Commerce en la infraestructura en la nube utiliza un metapaquete que requiere lo siguiente `magento/product-enterprise-edition`. Para obtener las últimas actualizaciones de la última versión de Commerce, utilice la siguiente sintaxis de restricción:

```text
>=current_version <next_version
```

Por ejemplo, para utilizar la última versión de Adobe Commerce 2.4.7, establezca `2.4.7` como la versión &quot;actual&quot; y `2.4.8` como la versión &quot;siguiente&quot; en el `composer.json` archivo:

```text
"magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
```

Los paquetes principales de este metapaquete son los siguientes:

- **supplier/magento/ece-tools**: la `ece-tools` es compatible con la versión 2.1.4 y posterior de Adobe Commerce para proporcionar un completo conjunto de funciones que puede utilizar para administrar su proyecto de infraestructura de Adobe Commerce en la nube. Contiene scripts y comandos de infraestructura de Adobe Commerce en la nube diseñados para ayudarle a administrar su código y generar e implementar automáticamente sus proyectos. Consulte la [`ece-tools` información general del paquete](../dev-tools/package-overview.md).
- **seller/magento/product-enterprise-edition**: este metapaquete requiere componentes de aplicación, incluidos módulos, marcos, temas y mucho más.
- **supplier/fastly2/magento2**: este módulo gestiona la CDN y los servicios de Fastly para los entornos de ensayo profesional y producción y producción inicial. Consulte [Servicios rápidos](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **proveedor/magento/module-paypal-on-boarding**: este módulo proporciona un pago de puerta de enlace de pago mediante la conexión a su cuenta de comerciante de PayPal. Consulte [Herramienta de incorporación de PayPal](../store/paypal.md).

>[!TIP]
>
>Consulte [Paquetes de nube para Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) en el _Notas de la versión de Commerce_ para obtener una lista de dependencias y licencias de terceros.

## Entorno Docker

Puede utilizar la herramienta Cloud Docker para Commerce para emular Adobe Commerce en entornos de producción y desarrollo de infraestructura en la nube para el desarrollo local. Cloud Docker para Commerce no requiere que PHP y Composer se instalen localmente.

- [Desarrollo local con Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) en el sitio de Adobe Developer
- [Arquitectura de Docker y comandos comunes](../dev-tools/cloud-docker.md)
- [Notas de la versión de Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Para obtener información sobre el uso de servicios de alojamiento basados en Git con Adobe Commerce en la infraestructura en la nube, consulte [Integraciones](../integrations/overview.md).
