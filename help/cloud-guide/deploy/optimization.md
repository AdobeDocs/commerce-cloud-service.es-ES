---
title: Optimizar implementación en la nube
description: Obtenga información acerca de las formas de optimizar el proceso de implementación de Adobe Commerce en proyectos de infraestructura en la nube, incluida la reducción del tiempo de inactividad, la implementación de contenido estático, la implementación basada en escenarios y los asistentes inteligentes.
feature: Cloud, Deploy, SCD
exl-id: 62e5eccb-6919-4a4b-9f50-6105f9d0f3af
source-git-commit: 5c47c8bf9c97fac70ef249f76ecc905df07e4437
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Optimizar implementación

El rendimiento del sitio puede verse afectado durante el proceso de implementación. El tiempo que un sitio está en modo de mantenimiento al implementarlo en un sitio de producción depende de muchos factores, como la configuración del entorno y la cantidad de contenido que contenga un sitio. La primera práctica recomendada para optimizar su implementación de Cloud es [actualizar para utilizar `ece-tools`](../dev-tools/install-package.md) para beneficiarse de las funciones del paquete, como los comandos para crear una copia de seguridad de la base de datos y verificar la configuración del entorno.

Los siguientes temas pueden ayudarle a comprender mejor cómo optimizar el proceso de implementación:

- [Proceso de implementación en la nube](process.md)
El proceso de implementación en la nube consta de tres fases y puede aprovechar los puntos fuertes y débiles de cada una de ellas.

- [Implementación sin tiempo de inactividad](reduce-downtime.md)
Comprenda qué sucede durante la implementación y cómo reducir el tiempo de inactividad que su tienda experimenta durante una actualización del entorno de producción.

- [Implementación de contenido estático](static-content.md)
La mejor manera de optimizar la implementación de Cloud es controlar cómo y cuándo generar contenido estático.

- [Asistentes inteligentes](smart-wizards.md)
El `ece-tools` proporciona los comandos del asistente inteligente para evaluar rápidamente la configuración del proyecto.

- [Seguimiento de implementaciones con New Relic](../monitor/track-deployments.md)
Utilice el servicio New Relic para monitorizar los eventos de implementación y analizar el impacto de la implementación en el rendimiento general.
