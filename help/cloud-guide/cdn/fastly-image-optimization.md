---
title: Optimización rápida de imágenes
description: Aprenda a optimizar la entrega de imágenes y a simplificar la administración de imágenes para el sitio de Adobe Commerce habilitando y configurando la Optimización rápida de imágenes.
feature: Cloud, Configuration, Media
exl-id: 20f5c5c3-7c43-493b-b651-82b023cf5db5
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Optimización rápida de imágenes

La optimización rápida de imágenes (Fastly IO) proporciona manipulación y optimización de imágenes en tiempo real para acelerar la entrega de imágenes y simplificar el mantenimiento de los conjuntos de fuentes de imágenes para aplicaciones web adaptables. Una vez configurado Fastly IO proporciona las siguientes funciones de optimización de imágenes:

- Forzar conversión con pérdidas
- Optimización de imagen profunda
- Proporciones de píxeles adaptables
- Compatibilidad con formatos de imagen comunes: PNG, JPEG, GIF y WebP

Antes de activar y configurar la opción Fastly IO, debe configurar el servicio Fastly y configurar el blindaje de origen.

En función de los ajustes de configuración, el fragmento Fastly Image Optimization (Fastly IO) inserta el código VCL para realizar la optimización de imágenes que acelera la entrega de imágenes de producto en la tienda. Existen tres pasos para configurar Fastly IO: Habilitar, Configurar y Verificar.

## Activar Fastly IO

Habilite la Optimización rápida de imágenes (Fastly IO) desde el panel de administración al cargar el fragmento de VCL de Fastly IO. El fragmento de código proporciona las instrucciones de configuración de Fastly para procesar todas las imágenes a través de optimizadores de imágenes, utilizando configuraciones predeterminadas.

**Requisitos previos:**

- Instale o actualice a la versión 1.2.62 o posterior del módulo Fastly
- [Configurar el escudo de origen rápido y el back-end](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**Para habilitar Fastly IO**:

1. Inicie sesión en su [Administrador](../../get-started/onboarding.md#access-your-admin-panel) panel como administrador.

1. Seleccionar **Tiendas** > **Configuración** > **Configuración** > **Avanzadas** > **Sistema**.

1. En el panel derecho, expanda **Caché de página completa**.

1. Seleccionar **Configuración rápida** > **Optimización de imagen** para especificar las opciones de configuración.

1. En el _Fragmento de E/S rápido_ , seleccione **Habilitar/deshabilitar**.

1. Cargue el fragmento de Fastly IO:

   - Seleccionar **Opciones de configuración de IO predeterminadas** para abrir la página Opciones de configuración predeterminadas de optimización de la imagen.
   - Seleccionar **Cargar** para cargar el fragmento de VCL en el servidor.

## Configuración de Fastly IO

Revise y actualice los ajustes de configuración de E/S predeterminados para la optimización de imágenes según sea necesario. Por ejemplo, es posible que desee cambiar WebP y los niveles de calidad de JPEG para formatos con pérdidas, o cambiar el formato para ofrecer imágenes de JPEG a _Progresivo_ o _Línea base_. Además, puede utilizar Fastly IO para funciones de optimización de imágenes más granulares, como:

- Forzar conversión con pérdidas
- Optimización de imagen profunda
- Proporciones de píxeles adaptables

**Para actualizar Fastly IO**:

1. En el _Configuración rápida_ página en la _Opciones de configuración de IO predeterminadas_ , seleccione **Configurar**.

   ![Ver los ajustes de configuración de Fastly IO](../../assets/cdn/fastly-io-default-config.png)

1. Revise y actualice los ajustes de configuración de Fastly IO en la _Opciones de configuración predeterminadas de optimización de imágenes_ página:

   ![Revisar la configuración de Fastly IO](../../assets/cdn/fastly-io-config-options.png)

   - **¿Auto WebP?**: permite mantener la configuración por defecto (`Yes`) para convertir imágenes al formato WebP en exploradores compatibles. Si cambia la configuración a **No**, utiliza rápidamente el tipo de archivo de imagen en lugar de convertir la imagen al formato WebP.

   - **Calidad WebP predeterminada (con pérdida)**: permite mantener la configuración por defecto (`85`) o escriba el nivel de compresión de las imágenes con formato de archivo con pérdida. Puede especificar cualquier número entero entre 1 y 100.

   - **Controles de formato de JPEG predeterminados** — mantener la configuración predeterminada (`Auto`) o seleccione el tipo de JPEG que se utilizará para mostrar una imagen. Si el valor se establece en _Automático_, ofrece rápidamente imágenes con el tipo de salida que coincide con el tipo de entrada. Seleccionar _Línea base_ para mostrar imágenes línea a línea empezando desde arriba a la izquierda y yendo hacia abajo a la derecha. Seleccionar _Progresivo_ para mostrar una imagen borrosa que se vuelve clara a medida que se carga.

   - **Calidad de JPEG predeterminada**: permite mantener la configuración por defecto (`85`) o escriba el nivel de compresión de la calidad de los formatos de archivo con pérdidas. Especifique cualquier número entero entre 1 y 100.

   - **¿Permitir ampliación?**—dejar el valor predeterminado (`No`) o seleccione `Yes` para devolver imágenes más grandes que el archivo de origen original para que se ajusten a las dimensiones solicitadas.

   - **Redimensionar filtro**: permite mantener la configuración por defecto (`Lancsoz3`) o seleccione una alternativa. Esta configuración especifica el filtro utilizado para enviar una imagen cuyo tamaño se ha cambiado. Según el filtro seleccionado, la imagen cuyo tamaño se haya cambiado puede tener un número de píxeles mayor o menor.

      - `Lanczos3` (por defecto): ofrece la mejor calidad de imagen. Aumenta la capacidad de detectar bordes y funciones lineales dentro de una imagen y utiliza _[!DNL sinc]_remuestreo para proporcionar la mejor reconstrucción posible.
      - `Lanczos2`: utiliza el mismo filtro que `Lancsoz3` pero con una aproximación menos precisa de la _[!DNL sinc]_función de remuestreo.
      - `Bicubic`: tiene un efecto de enfoque natural al reducir el tamaño de una imagen.
      - `Bilinear`: tiene un efecto de suavizado natural al ampliar una imagen.
      - `Nearest`: tiene un efecto de pixelación natural al cambiar el tamaño de las imágenes en píxeles.

1. Después de especificar los ajustes de configuración de E/S para el servicio de Fastly, seleccione **Cancelar** para volver a los ajustes de configuración de Fastly.

1. En la configuración de Optimización de imagen _Activar optimización de imagen profunda_ , seleccione **Sí** para activar la optimización de imágenes en profundidad.

   ![Activar optimización de imagen profunda de Fastly IO](../../assets/cdn/fastly-io-deep-image-config.png)

   La optimización profunda de imágenes está desactivada de forma predeterminada. Cuando esta característica está habilitada, la característica de cambio de tamaño integrada de Adobe Commerce se desactiva y el trabajo de cambio de tamaño se descarga en el servicio de Fastly IO. La optimización de imágenes solo se aplica a imágenes de productos. No se cambia el tamaño de las imágenes de CMS. Consulte la [Documentación de Fastly](#deep-image-optimization).

1. Después de activar la optimización de imágenes en profundidad, active las opciones [proporciones de píxeles adaptables](#adaptive-pixel-ratios) para generar imágenes optimizadas para su uso en sitios web adaptables.

   ![Activar proporción de píxeles adaptable de Fastly IO](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - En el _Habilitar proporciones de píxeles de dispositivo adaptable_ , seleccione **Sí**.
   - En el _Proporciones de píxeles del dispositivo_ , acepte la configuración predeterminada o seleccione el campo **Entrada del sistema** para quitar la configuración. A continuación, seleccione la proporción que desee. Un valor mayor de proporción de píxeles del dispositivo proporciona imágenes más grandes.

1. Seleccionar **Guardar configuración**.

### Forzar conversión con pérdidas

De forma predeterminada, el servicio Fastly IO fuerza la conversión de formatos sin pérdida como PNG, BMP o WEBP a formato JPEG/WEBP.

La ventaja de forzar la conversión con pérdida es que se proporcionan imágenes más pequeñas.
Por ejemplo, si utiliza el formato JPEG o WEBp en lugar de PNG, el tamaño se puede reducir de un 60 a un 70 por ciento en función del nivel de calidad especificado en la configuración de Fastly IO.

Según el nivel de calidad seleccionado para la optimización de imágenes, es posible que perciba diferencias visuales en las imágenes. Por ejemplo, las transparencias o el canal del Alpha se eliminan y se sustituyen por un fondo blanco, a menos que utilice la optimización de imágenes profundas, que utiliza el color de fondo de la temática.

Si desactiva la conversión con pérdidas (`WebP Auto? = No`), Fastly IO solo cambia las imágenes del JPEG al formato WEBP para exploradores compatibles. No se cambian otros tipos de imagen. Por ejemplo, si la imagen original es PNG, la salida del servicio Fastly IO es PNG.

### Optimización de imagen profunda

La optimización profunda de imágenes está desactivada de forma predeterminada. Al habilitar esta opción, se desactiva el cambio de tamaño integrado de Adobe Commerce y se descarga completamente en el servicio Fastly IO.
Esta función solo cambia de tamaño _producto_ imágenes. No se cambia el tamaño de las imágenes de CMS.

Al habilitar la optimización de imágenes profundas, se agrega una definición de color de fondo a cada imagen según se define en la temática. Como resultado, las imágenes WebP cambian de sin pérdidas WebP a con pérdidas WebP. Una de las principales diferencias entre las imágenes sin pérdidas y con pérdida es que la pérdida elimina el canal alfa de las imágenes PNG, lo que proporciona imágenes mucho más pequeñas. Sin embargo, las imágenes con transparencias pueden tener un aspecto extraño en las páginas de productos y campañas que utilizan un fondo diferente.

Por ejemplo, el siguiente código representa el origen de una imagen del tema de Luma:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Cuando la función de optimización de imagen profunda de Fastly IO está habilitada, el código fuente original de la imagen se vuelve a escribir como se muestra en el siguiente ejemplo:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### Proporciones de píxeles adaptables

La función de proporción de píxeles adaptable es útil para optimizar imágenes para aplicaciones web progresivas. Permite enviar varios tamaños y resoluciones de imagen desde un archivo de fuente de imagen agregando una `srcset` para cada imagen de producto.

Cuando la función de proporción de píxeles adaptable está activada, el servicio Fastly IO ofrece una imagen de ancho fijo que se puede adaptar a variables `device-pixel-ratios`.
Por ejemplo, el servicio modifica la definición de la imagen del producto como se muestra en el siguiente ejemplo:

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Consulte `srcset` [compatibilidad con exploradores](https://caniuse.com/#feat=srcset) y [especificación](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset).

## Validar Fastly IO

Después de habilitar y configurar Fastly IO, valide la configuración realizando pruebas de velocidad de página web con y sin Fastly IO habilitado. Además, revise las imágenes de su tienda para comprobar el tamaño y la apariencia de las imágenes en busca de problemas.
