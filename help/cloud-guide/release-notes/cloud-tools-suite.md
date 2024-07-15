---
title: Notas de la versión de Cloud Tools Suite
description: Obtenga información sobre las últimas mejoras del conjunto de herramientas de la nube para Adobe Commerce.
feature: Cloud, Release Notes
exl-id: 6a652e93-46a2-4590-97fc-fb5d114ece9a
source-git-commit: e04a9de8f0e31098f0cc2e47112f206c11a0e23b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Notas de la versión de Commerce Cloud Tools Suite

Esta información de la versión detalla las mejoras más recientes de Cloud Tools Suite para Commerce, paquetes diseñados para implementar y administrar instalaciones y actualizaciones de Adobe Commerce en la plataforma en la nube.

| Notas de la versión | Versión | Descripción | Source |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` paquete](ece-tools-package.md) | 2002.1.19 | Un conjunto de scripts y herramientas diseñadas para administrar e implementar proyectos en la nube | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.1) |
| [Parches de nube para Commerce](cloud-patches.md) | 1.0.27 | Un conjunto de parches que mejoran la integración de todas las versiones de Adobe Commerce con los entornos en la nube. Este paquete incluye revisiones de Adobe Commerce y revisiones disponibles que se aplican cuando se usa `ece-tools` para implementar | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.0.1) |
| [Cloud Docker para Commerce](cloud-docker.md) | 1.3.7 | Archivos de funcionalidad y configuración para imágenes de Docker para implementar Adobe Commerce en un entorno de nube local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.0) |
| [Componentes de nube de Commerce](cloud-components.md) | 1.0.14 | Funcionalidad principal extendida de Adobe Commerce para sitios implementados en la infraestructura en la nube | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.0.2) |

Al actualizar a ECE-Tools 2002.1.0 o posterior, se actualiza automáticamente a las versiones más recientes de los otros paquetes, que son dependencias del paquete `ece-tools`. Consulte [Metapackage de nube](../development/overview.md#cloud-metapackage) para obtener una lista de dependencias.
