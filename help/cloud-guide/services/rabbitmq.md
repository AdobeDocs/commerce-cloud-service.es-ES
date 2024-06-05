---
title: Configurar el servicio de RabbitMQ
description: Obtenga información sobre cómo habilitar el servicio de RabbitMQ para administrar las colas de mensajes de Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Services
exl-id: 85794b8f-2260-4a6e-b5a6-a1b4c356594e
source-git-commit: adcfbb7217c70122a4003a66d1bec1a623fbf11a
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Configuración de [!DNL RabbitMQ] servicio

El [Marco de cola de mensajes (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) es un sistema de Adobe Commerce que permite [módulo](https://glossary.magento.com/module) para publicar mensajes en colas. También define los consumidores que reciben los mensajes de forma asincrónica.

El MQF utiliza [RabbitMQ](https://www.rabbitmq.com/) como agente de mensajería, que proporciona una plataforma escalable para enviar y recibir mensajes. También incluye un mecanismo para almacenar mensajes no enviados. [!DNL RabbitMQ] se basa en la especificación 0.9.1 del Protocolo avanzado de Message Queue Server (AMQP).

>[!WARNING]
>
>Si prefiere utilizar un servicio basado en AMQP existente, como [!DNL RabbitMQ], en lugar de depender de Adobe Commerce en la infraestructura de la nube para crearla, utilice el [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) para conectarla al sitio.

{{service-instruction}}

**Para habilitar RabbitMQ**:

1. Agregue el nombre, tipo y valor de disco necesarios (en MB) a `.magento/services.yaml` junto con la versión de RabbitMQ instalada.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Configure las relaciones en la variable `.magento.app.yaml` archivo.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verificar las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Conectarse a RabbitMQ para depurar

Para fines de depuración, es útil conectarse directamente a una instancia de servicio de una de las siguientes maneras:

- Conéctese desde su entorno de desarrollo local
- Conectar desde la aplicación
- Conéctese desde su aplicación PHP

### Conéctese desde su entorno de desarrollo local

1. Inicie sesión en `magento-cloud` CLI y proyecto:

   ```bash
   magento-cloud login
   ```

1. Compruebe el entorno con RabbitMQ instalado y configurado.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Utilice SSH para conectarse al entorno de la nube:

   ```bash
   magento-cloud ssh
   ```

1. Recupere los detalles de conexión de RabbitMQ y las credenciales de inicio de sesión desde el [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) variable:

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   En la respuesta, busque la información de RabbitMQ, por ejemplo:

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Habilite el reenvío de puertos locales a RabbitMQ (si su proyecto se encuentra en una región diferente, como la región US-3, EU-5 o AP-3, sustituya ``us-3``/``eu-5``/``ap-3`` para ``us``)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Ejemplo de acceso a la interfaz web de administración de RabbitMQ en `http://localhost:15672` es:

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. Mientras la sesión está abierta, puede iniciar un cliente de RabbitMQ de su elección desde la estación de trabajo local, configurada para conectarse a `localhost:<portnumber>` usando la información de número de puerto, nombre de usuario y contraseña de la variable MAGENTO_CLOUD_RELATIONSHIPS.

### Conectar desde la aplicación

Para conectarse a RabbitMQ que se ejecute en una aplicación, instale un cliente de, como [amqp-utils](https://github.com/dougbarth/amqp-utils), como dependencia del proyecto en su `.magento.app.yaml` archivo.

Por ejemplo,

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

Al iniciar sesión en su contenedor de PHP, debe introducir cualquier `amqp-` disponible para administrar las colas.

### Conéctese desde su aplicación PHP

Para conectarse a RabbitMQ usando su aplicación PHP, agregue un PHP [biblioteca](https://glossary.magento.com/library) al árbol de origen.
