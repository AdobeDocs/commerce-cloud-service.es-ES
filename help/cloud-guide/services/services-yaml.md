---
title: Configurar servicios
description: Obtenga información sobre cómo configurar los servicios que utiliza Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Configuration, Services
exl-id: 48091c10-c53f-4aad-afbe-b4516653bda1
source-git-commit: 4d790bff2ba5d02ef10de5c36a2f0d140e31a407
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---

# Configurar servicios

El `services.yaml` define los servicios compatibles y utilizados por Adobe Commerce en la infraestructura de la nube, como MySQL, Redis y Elasticsearch o OpenSearch. No es necesario suscribirse a proveedores de servicios externos. Este archivo se encuentra en `.magento` del proyecto.

El script de implementación utiliza los archivos de configuración de `.magento` para aprovisionar el entorno con los servicios configurados. Un servicio está disponible para su aplicación si está incluido en el [`relationships`](../application/properties.md#relationships) propiedad del `.magento.app.yaml` archivo. El `services.yaml` el archivo contiene _type_ y _disco_ valores. El tipo de servicio define el servicio _name_ y _version_.

Al cambiar una configuración de servicio, una implementación aprovisiona el entorno con los servicios actualizados, lo que afecta a los siguientes entornos:

- Todos los entornos iniciales, incluida la producción `master`
- Entornos de integración Pro

{{pro-update-service}}

## Servicios predeterminados y admitidos

La infraestructura en la nube admite e implementa los siguientes servicios:

- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

Puede ver las versiones y los valores de disco predeterminados en el [predeterminado `services.yaml` archivo](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). El siguiente ejemplo muestra el `mysql`, `redis`, `opensearch` o `elasticsearch`, y `rabbitmq` servicios definidos en la `services.yaml` archivo de configuración:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## Valores de servicio

Debe proporcionar el ID de servicio y la configuración del tipo de servicio `type: <name>:<version>`. Si el servicio utiliza almacenamiento persistente, debe proporcionar un valor de disco.

Utilice el siguiente formato:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

El `service-id` identifica el servicio en el proyecto. Solo puede utilizar caracteres alfanuméricos en minúsculas: `a` hasta `z` y `0` hasta `9`, como `redis`.

Esta _service-id_ se utiliza en el [`relationships`](../application/properties.md#relationships) propiedad del `.magento.app.yaml` archivo de configuración:

```yaml
relationships:
    redis: "<name>:redis"
```

Puede asignar nombres a varias instancias de cada tipo de servicio. Por ejemplo, puede utilizar varias instancias de Redis, una para la sesión y otra para la caché.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

Cambiar el nombre de un servicio en `services.yaml` archivo **elimina permanentemente** lo siguiente:

- El servicio existente antes de crear un servicio con el nuevo nombre especificado.
- Se eliminarán todos los datos existentes del servicio. El Adobe recomienda encarecidamente que [realizar una copia de seguridad del entorno Starter](../storage/snapshots.md) antes de cambiar el nombre de un servicio existente.

### `type`

El `type` valor especifica el nombre y la versión del servicio. Por ejemplo:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

El `disk` El valor especifica el tamaño del almacenamiento en disco persistente (en MB) que se va a asignar al servicio. Los servicios que utilizan almacenamiento persistente, como MySQL, deben proporcionar un valor de disco. Los servicios que utilizan memoria en lugar de almacenamiento persistente, como Redis, no requieren un valor de disco.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

La cantidad de almacenamiento predeterminada actual por proyecto es de 5 GB o 512 0 MB. Puede distribuir esta cantidad entre su aplicación y cada uno de sus servicios.

## Relaciones de servicio

En Adobe Commerce sobre proyectos de infraestructura en la nube, [relaciones](../application/properties.md#relationships) configurado en la `.magento.app.yaml` determina qué servicios están disponibles para su aplicación.

Puede recuperar los datos de configuración de todas las relaciones de servicio desde el [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md) variable de entorno. Los datos de configuración incluyen el nombre, el tipo y la versión del servicio junto con los detalles de conexión necesarios, como el número de puerto y las credenciales de inicio de sesión.

**Para comprobar las relaciones en el entorno local**:

1. En el entorno local, muestre las relaciones del entorno activo.

   ```bash
   magento-cloud relationships
   ```

1. Confirme el `service` y `type` de la respuesta. La respuesta proporciona información de conexión, como la dirección IP y el número de puerto.

   >Respuesta de muestra abreviada

   ```terminal
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**Para comprobar las relaciones en entornos remotos**:

1. Utilice SSH para iniciar sesión en el entorno remoto.

1. Enumerar los datos de configuración de relaciones para todos los servicios configurados en el entorno.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o bien, utilice lo siguiente `ece-tools` comando para ver las relaciones:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Confirme el `service` y `type` de la respuesta. La respuesta proporciona información de conexión, como la dirección IP y el número de puerto, así como las credenciales de nombre de usuario y contraseña necesarias.

## Versiones de servicio

La compatibilidad y la versión del servicio para Adobe Commerce en la infraestructura en la nube están determinadas por las versiones implementadas y probadas en la infraestructura en la nube, y a veces difieren de las versiones admitidas por las implementaciones locales de Adobe Commerce. Consulte [Requisitos del sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) en el _Instalación_ para obtener una lista de las dependencias de software de terceros que Adobe ha probado con versiones específicas de Adobe Commerce y Magento Open Source.

### Comprobaciones de EOL de software

Durante el proceso de implementación, la variable `ece-tools` El paquete comprueba las versiones de servicio instaladas con las fechas de fin de vida útil (EOL) de cada servicio.

- Si la versión de un servicio se encuentra en los tres meses siguientes a la fecha límite, se muestra una notificación en el registro de implementación.
- Si la fecha límite se sitúa en el pasado, aparece una notificación de advertencia.

Para mantener la seguridad de la tienda, actualice las versiones de software instaladas antes de que lleguen a EOL. Puede revisar las fechas límite en la [ece-tools&#39; `eol.yaml` archivo](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migrar a OpenSearch

{{elasticsearch-support}}

Para la versión de Adobe Commerce 2.4.4 y posterior, consulte [Configuración del servicio OpenSearch](opensearch.md).

## Cambiar la versión del servicio

Puede actualizar la versión del servicio instalado para que sea compatible con la versión de Adobe Commerce implementada en su entorno de nube.

No puede actualizar directamente la versión del servicio para un servicio instalado. Sin embargo, puede crear un servicio con la versión requerida. Consulte [Versión de servicio de downgrade](#downgrade-version).

### Actualizar la versión del servicio instalado

Puede actualizar la versión del servicio instalado actualizando la configuración del servicio en el `services.yaml` archivo.

1. Cambie el [`type`](#type) valor para el servicio en `.magento/services.yaml` archivo:

   > Definición del servicio original

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Definición de servicio actualizada

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Versión de downgrade

No puede reducir un servicio instalado directamente. Tiene dos opciones:

1. Cambie el nombre de un servicio existente con la nueva versión, que elimina el servicio y los datos existentes, y agrega uno nuevo.

1. Cree un servicio de y guarde los datos del servicio existente.

Al cambiar la versión del servicio, debe actualizar la configuración del servicio en la `services.yaml` y actualice las relaciones en el archivo `.magento.app.yaml` archivo.

**Para reducir una versión de servicio cambiando el nombre de un servicio existente**:

1. Cambie el nombre del servicio existente en la `.magento/services.yaml` y cambie la versión.

   >[!WARNING]
   >
   >Al cambiar el nombre de un servicio existente, se sustituye y se eliminan todos los datos. Si necesita conservar los datos, cree un servicio en lugar de cambiar el nombre del servicio existente.

   Por ejemplo, para reducir la versión de MariaDB para _mysql_ servicio de la versión 10.4 a la 10.3, cambie el existente _service-id_ y _type_ configuración.

   > Original `services.yaml` definición

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Nuevo `services.yaml` definición

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Actualice las relaciones en la `.magento.app.yaml` archivo.

   > Original `.magento.app.yaml` configuración

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Actualizado `.magento.app.yaml` configuración

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.

**Para reducir la categoría de un servicio creando un servicio**:

1. Agregue una definición de servicio a `services.yaml` para el proyecto con la especificación de versión degradada. Consulte _mysql2_ en el ejemplo siguiente:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Cambie la configuración de relaciones en la `.magento.app.yaml` para utilizar el nuevo servicio.

   > Original `.magento.app.yaml` configuración

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Nuevo `.magento.app.yaml` configuración

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.
