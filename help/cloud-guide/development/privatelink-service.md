---
title: Servicio PrivateLink
description: Aprenda a utilizar el servicio PrivateLink para establecer una conexión segura entre una nube privada y la plataforma de nube de Adobe Commerce en la misma región.
feature: Cloud, Iaas, Security
exl-id: b25548b8-312b-4a74-b242-f4e2ac6cf945
source-git-commit: 5b52157c6c623bb4c765a2d545919df79d81f1da
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# Servicio PrivateLink

Adobe Commerce en la nube admite la integración con [AWS PrivateLink](https://aws.amazon.com/privatelink/) o [Vínculo privado de Azure](https://learn.microsoft.com/en-us/azure/private-link/) servicio. Puede utilizar PrivateLink para establecer una comunicación privada y segura entre Adobe Commerce en entornos de infraestructura en la nube con servicios y aplicaciones alojados en sistemas externos. Tanto la aplicación de Adobe Commerce como los sistemas externos deben ser accesibles a través de los puntos de conexión de Virtual Private Cloud (VPC) configurados en la misma plataforma de nube (AWS o Azure) dentro de la misma región de nube.

>[!TIP]
>
>PrivateLink se utiliza mejor para proteger conexiones para integraciones que no son HTTP(S), como transferencias de archivos o bases de datos. Si planea integrar su aplicación con las API de Adobe Commerce, consulte cómo crear una [Malla de API de Adobe](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) in _Malla de API para el Generador de aplicaciones de Adobe Developer_.

## Funciones y asistencia

La integración del servicio PrivateLink para Adobe Commerce en proyectos de infraestructura en la nube incluye las siguientes funciones y compatibilidad:

- Una conexión segura entre un cliente de Virtual Private Cloud (VPC) y el VPC de Adobe en la misma plataforma de nube (AWS o Azure) dentro de la misma región de nube.
- Compatibilidad con la comunicación unidireccional o bidireccional entre los servicios de puntos de conexión disponibles en el Adobe y los VPC del cliente.
- Habilitación del servicio:

   - Abra los puertos necesarios en el entorno de Adobe Commerce en la nube
   - Establecer la conexión inicial entre el cliente y los VPC de Adobe
   - Solucionar problemas de conexión durante la activación

## Limitaciones

- La compatibilidad con PrivateLink solo está disponible en los entornos de producción y ensayo de Pro. No está disponible en entornos locales o de integración, ni en proyectos iniciales.
- No puede establecer conexiones SSH mediante PrivateLink. Consulte [Habilitar las claves SSH](secure-connections.md).
- La compatibilidad con Adobe Commerce no cubre la resolución de problemas de AWS PrivateLink más allá de la activación inicial.
- Los clientes son responsables de los costes asociados con la gestión de su propio VPC.
- No puede utilizar el protocolo HTTPS (puerto 443) para conectarse a Adobe Commerce en la infraestructura de la nube a través de Azure Private Link debido a [Revestimiento de origen rápido](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html). Esta limitación no se aplica a AWS PrivateLink.
- PrivateDNS no está disponible.

## Tipos de conexión de PrivateLink

Existen dos tipos de conexión PrivateLink disponibles, que se muestran en el siguiente diagrama de red, para establecer una comunicación segura entre el almacén y los sistemas externos alojados fuera del entorno de Cloud.

![Diagrama de red de PrivateLink](../../assets/privatelink-architecture-diagram.png)

Elija uno de los tipos de conexión PrivateLink más adecuados para su Adobe Commerce en entornos de infraestructura en la nube:

- **PrivateLink unidireccional**-Elija esta configuración para recuperar datos de forma segura desde un almacén de infraestructura en la nube de Adobe Commerce.
- **PrivateLink bidireccional**: elija esta configuración para establecer conexiones seguras desde y hacia sistemas fuera de Adobe Commerce en el entorno de la infraestructura en la nube. La opción bidireccional requiere dos conexiones:

   - Una conexión entre el VPC del cliente y el VPC de Adobe
   - Una conexión entre el VPC de Adobe y el VPC del cliente

>[!TIP]
>
>Póngase en contacto con el administrador de red o con el proveedor de Cloud Platform para obtener ayuda con la selección del tipo de conexión PrivateLink o para la configuración y administración de VPC. Consulte la documentación de PrivateLink de Cloud Platform: [AWS PrivateLink](https://aws.amazon.com/privatelink/) o [Vínculo privado de Azure](https://learn.microsoft.com/en-us/azure/private-link/).

## Solicitar habilitación de PrivateLink

>[!WARNING]
>
>La activación de PrivateLink puede llevar hasta _cinco_ días laborables. Proporcionar información incompleta o inexacta puede retrasar el proceso.

### Requisitos previos

![check](../../assets/fix.svg) Una cuenta de Cloud (AWS o Azure) en la misma región que la instancia de Adobe Commerce en la nube.

![check](../../assets/fix.svg) Un VPC en el entorno del cliente que aloja los servicios para conectarse a través de PrivateLink. Consulte la documentación de AWS o Azure para obtener ayuda con la configuración de VPC o póngase en contacto con el administrador de red.

![check](../../assets/fix.svg) Para las conexiones PrivateLink bidireccionales, debe crear la configuración del servicio de punto final para su aplicación o servicio y crear un punto final en su entorno VPC antes de solicitar la habilitación de PrivateLink. Consulte [Configuración para conexiones PrivateLink bidireccionales](#set-up-for-bidirectional-privatelink-connections).

Recopile los siguientes datos necesarios para la habilitación de PrivateLink:

- **Número de cuenta de Customer Cloud** (AWS o Azure): debe estar en la misma región que la instancia de Adobe Commerce en la nube
- **Región de nube**: Proporcione la región de Cloud donde se aloja la cuenta para fines de verificación
- **Puertos de servicios y comunicaciones**: el Adobe debe abrir los puertos para habilitar la comunicación de servicio entre los VPC, por ejemplo, el puerto SQL 3306 o el puerto SFTP 2222
- **Identificador de proyecto**: proporcione el ID del proyecto Pro de Adobe Commerce en la infraestructura en la nube. Puede obtener el identificador de proyecto y otra información sobre el proyecto mediante los siguientes métodos [CLI de nube](../dev-tools/cloud-cli-overview.md) comando: `magento-cloud project:info`
- **Tipo de conexión**: permite especificar unidireccional o bidireccional para el tipo de conexión
- **Servicio de extremo**: para conexiones PrivateLink bidireccionales, proporcione la URL DNS para el servicio de extremo VPC al que debe conectarse el Adobe, por ejemplo: `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **Acceso al servicio de extremo concedido**: para conectarse al servicio externo, permita al servicio de punto de conexión acceder al siguiente principal de cuenta de AWS: `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >Si no se proporciona acceso al servicio de punto de conexión, la conexión PrivateLink bidireccional al servicio en su VPC es **no** se ha añadido, lo que retrasa la configuración.

#### Requisitos previos adicionales específicos de la habilitación de vínculos privados de Azure

- Proporcione el ID del clúster; con SSH, inicie sesión en el remoto y use el comando: `cat /etc/platform_cluster`
- Para que un servicio externo se conecte al clúster de Adobe Commerce Pro, necesita lo siguiente:

   - Lista de puertos del clúster de Pro que se expondrán al nuevo extremo privado externo
   - Una lista de ID de suscripción de Azure para las conexiones de extremo privado

- Para conectar el clúster de Adobe Commerce Pro a un servicio externo, necesita:

   - Una lista de ID de recursos para los servicios de destino. Los ID de servicio de vínculo privado externo tienen un aspecto similar al siguiente:

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### Flujo de trabajo de habilitación

El siguiente flujo de trabajo describe el proceso de activación de la integración de PrivateLink con Adobe Commerce en la infraestructura en la nube.

1. **Cliente** envía un ticket de asistencia solicitando la habilitación de PrivateLink con la línea de asunto `PrivateLink support for <company>`. Incluya el [datos necesarios para la habilitación](#prerequisites) en el ticket. El Adobe de utiliza el ticket de asistencia para coordinar la comunicación durante el proceso de activación.

1. **Adobe** permite el acceso de la cuenta del cliente al servicio de punto final en el VPC de Adobe.

   - Actualice la configuración del servicio de extremo de Adobe para aceptar solicitudes iniciadas desde la cuenta de cliente de AWS o Azure.
   - Actualice el ticket de asistencia técnica para proporcionar el nombre de servicio del extremo VPC de Adobe al que se va a conectar, por ejemplo `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. **Cliente** agrega el servicio extremo de Adobe a su cuenta de Cloud (AWS o Azure), que almacena en déclencheur una solicitud de conexión para el Adobe. Consulte la documentación de la plataforma en la nube para obtener instrucciones:

   - Para AWS, consulte [Aceptar y rechazar solicitudes de conexión de extremo de interfaz].
   - Para Azure, consulte [Administrar solicitudes de conexión].

1. **Adobe** aprueba la solicitud de conexión.

1. Después de la aprobación de la solicitud de conexión, **el cliente** [verifica la conexión](#test-vpc-endpoint-service-connection) entre su VPC y el VPC de Adobe.

1. Pasos adicionales para habilitar conexiones bidireccionales:

   - **Adobe** proporciona la entidad de seguridad de la cuenta de Adobe (usuario raíz para la cuenta de AWS o Azure) y solicita acceso al servicio de extremo de VPC del cliente.
   - **Cliente** habilita el acceso de Adobe al servicio extremo en el VPC del cliente. Esto supone que el principal de la cuenta Adobe tiene acceso a `arn:aws:iam::402592597372:root`, tal como se describe anteriormente en la **Acceso al servicio de extremo concedido** requisito previo.

      - Actualice la configuración del servicio de punto de conexión del cliente para aceptar solicitudes iniciadas desde la cuenta de Adobe. Consulte la documentación de la plataforma en la nube para obtener instrucciones:

         - Para AWS, consulte [Agregar y quitar permisos para el servicio de punto de conexión].
         - Para Azure, consulte [Administrar una conexión de extremo privado]

      - Proporcione el Adobe con el nombre del servicio de punto final para el VPC del cliente.

   - **Adobe** agrega el servicio extremo de cliente a la cuenta de la plataforma de Adobe (AWS o Azure), que almacena en déclencheur una solicitud de conexión al VPC del cliente.
   - **Cliente** aprueba la solicitud de conexión del Adobe para completar la configuración.
   - **Cliente** [verifica la conexión](#test-vpc-endpoint-service-connection) del Adobe VPC.

## Probar la conexión del servicio de extremo VPC

Puede utilizar la aplicación Telnet para probar la conexión con el servicio de extremo de VPC.

**Para probar la conexión al servicio de extremo de VPC**:

1. Desde el directorio raíz del proyecto, **check out** el entorno de ensayo o producción configurado para acceder al servicio de extremo PrivateLink.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Ejecute el siguiente comando CURL:

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   Ejemplo:

   ```terminal
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   Respuesta correcta de muestra:

   ```terminal
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   Respuesta fallida de muestra:

   ```terminal
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. Compruebe que el servicio está escuchando en la VM.

   ```bash
   netstat -na | grep <port>
   ```

1. Compruebe el flujo de paquetes.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   Compruebe la siguiente configuración interna para asegurarse de que es válida:

   - Configuración de servicios de extremo y extremo
   - Configuración del Equilibrador de carga de red (NLB)
   - Los grupos de destino en NLB y compruebe que están en buen estado
   - La URL del extremo netcat/curl de cada VM (enumerada anteriormente)

   Consulte los siguientes artículos para obtener ayuda sobre la resolución de problemas de conexión:

   - [AWS: resolución de problemas de conexiones de servicio de extremos]
   - [Amazon: Solución de problemas de conectividad de Azure Private Link]

   Si no puede resolver los errores, actualice el ticket de asistencia de Adobe Commerce para solicitar ayuda para establecer la conexión.

## Cambiar la configuración de PrivateLink

[Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para cambiar una configuración de PrivateLink existente. Por ejemplo, puede solicitar cambios como los siguientes:

- Elimine la conexión PrivateLink del entorno de ensayo o producción de Adobe Commerce en la infraestructura en la nube Pro.
- Cambie el número de cuenta de la plataforma de customer Cloud para acceder al servicio de extremo de Adobe.
- Agregar o quitar conexiones PrivateLink del VPC de Adobe a otros servicios de punto final disponibles en el entorno VPC del cliente.

## Configuración para conexiones PrivateLink bidireccionales

El VPC del cliente debe tener los siguientes recursos disponibles para admitir conexiones PrivateLink bidireccionales:

- Un equilibrador de carga de red (NLB)
- Una configuración de servicio de punto final que permite el acceso a una aplicación o servicio desde el VPC del cliente
- Un [extremo de interfaz] (AWS) o [extremo privado] (Azure) que permite que el Adobe se conecte a servicios de extremo alojados en su VPC

Si estos recursos no están disponibles en el VPC del cliente, debe iniciar sesión en su cuenta de Cloud Platform para agregar la configuración.

- Consola VPC de Amazon- `https://console.aws.amazon.com/vpc/`
- Azure Portal- `https://portal.azure.com`

Consulte la documentación de la plataforma de Cloud para ver las instrucciones de configuración de PrivateLink:

- **Documentación de AWS PrivateLink**
   - [Crear un equilibrador de carga de red]
   - [Crear una configuración de servicio de extremo]
   - [Crear un extremo de interfaz]
   - [Ciclo de extremo de interfaz]

- **Documentación de Azure PrivateLink**
   - [Crear un equilibrador de carga]
   - [Flujo de trabajo de vínculo privado de Azure]

<!--Link definitions-->

[Aceptar y rechazar solicitudes de conexión de extremo de interfaz]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Agregar y quitar permisos para el servicio de punto de conexión]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon: Solución de problemas de conectividad de Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS: resolución de problemas de conexiones de servicio de extremos]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Flujo de trabajo de vínculo privado de Azure]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[Crear un equilibrador de carga]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[Crear un equilibrador de carga de red]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[Crear una configuración de servicio de extremo]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[Crear un extremo de interfaz]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[extremo de interfaz]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[Administrar una conexión de extremo privado]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[Administrar solicitudes de conexión]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[extremo privado]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
