---
title: Propiedad Firewall
description: Consulte ejemplos sobre cómo configurar la propiedad firewall en el archivo de configuración de la aplicación Commerce.
feature: Cloud, Configuration, Security
exl-id: f169c008-c62a-41b7-a98d-cccd81c7291a
source-git-commit: 74d88560db3b65294673a1e1827f9cea098d707a
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Propiedad Firewall

>[!IMPORTANT]
>
>Solo proyectos iniciales

Para los proyectos iniciales, la variable `firewall` La propiedad agrega un _saliente_ firewall a la aplicación. Este cortafuegos no afecta a las solicitudes entrantes. Define qué `tcp` las solicitudes salientes pueden _salir_ un sitio de Adobe Commerce. Esto se denomina filtrado de salida. El cortafuegos de salida filtra lo que puede salir: salir o escapar del sitio. Limitar lo que puede escapar añade una potente herramienta de seguridad al servidor.

## Directivas de restricción predeterminadas

El cortafuegos proporciona dos directivas predeterminadas para controlar el tráfico saliente: `allow` y `deny`. El `allow` directiva _permite_ todo el tráfico saliente de forma predeterminada. Y el `deny` directiva _niega_ todo el tráfico saliente de forma predeterminada. Sin embargo, al agregar una regla, se anula la directiva predeterminada y el cortafuegos se bloquea **todo** tráfico saliente no permitido por la regla.

Para los planes de inicio, la directiva predeterminada se establece en `allow`. Esta configuración garantiza que todo el tráfico saliente actual permanezca desbloqueado hasta que agregue las reglas de filtrado de salida. La directiva predeterminada se puede establecer en `deny` bajo petición.

**Para comprobar la directiva predeterminada**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

A menos que usted lo haya solicitado `deny` para la directiva, el comando debe mostrar la directiva establecida en `allow`:

```terminal
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**Llave para llevar**: Cuando se agrega una regla de salida, se bloquea todo el tráfico saliente excepto los dominios, las direcciones IP o los puertos que se agregan a la regla. Por lo tanto, es importante tener una lista saliente completa definida y probada antes de agregarla al sitio de producción.

## Opciones de cortafuegos

El siguiente ejemplo de configuración en la `.magento.app.yaml` El archivo muestra todas las `firewall` opciones que puede utilizar para agregar reglas para el filtrado de salida.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## Reglas de filtrado de salida

Las configuraciones del cortafuegos saliente están formadas por reglas. Puede definir tantas reglas como necesite. Los requisitos para las reglas son los siguientes.

**Cada regla:**

- Debe comenzar con un guión (`-`). Añadir un comentario en la misma línea ayuda a documentar y separar visualmente una regla de la siguiente.
- Debe definir al menos una de las siguientes opciones: `domains`, `ips`, o `ports`.
- Debe utilizar el `tcp` protocolo. Como este es el protocolo predeterminado para todas las reglas, puede omitirlo de la regla.
- Puede definir `domains` o `ips`, pero no ambas en la misma regla.
- Puede incluir `yaml` comentarios (`#`) y saltos de línea para organizar los dominios, las direcciones IP y los puertos permitidos.

Cada regla utiliza las siguientes propiedades:

### `domains`

El `domains` permite una lista de nombres de dominio completos (FQDN).

Si una regla define `domains` pero no `ports`, el cortafuegos permite las solicitudes de dominio en cualquier puerto.

### `ips`

El `ips` permite una lista de direcciones IP en la notación CIDR. Puede especificar direcciones IP únicas o rangos de direcciones IP.

Para especificar una sola dirección IP, agregue `/32` Prefijo CIDR al final de su dirección IP:

```terminal
172.217.11.174/32  # google.com
```

Para especificar un intervalo de direcciones IP, utilice la variable [Rango de IP a CIDR](https://ipaddressguide.com/cidr) calculadora.

Si una regla define `ips` pero no `ports`, el cortafuegos permite las solicitudes de IP en cualquier puerto.

### `ports`

El `ports` permite una lista de puertos de 1 a 65535. Para la mayoría de las reglas del ejemplo, puertos `80` y `443` permitir solicitudes HTTP y HTTPS. Pero para New Relic, las reglas solo permiten el acceso a dominios y direcciones IP en el puerto `443`, tal como se recomienda en la documentación de New Relic sobre [Tráfico de red](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents).

Si una regla solo define `ports`, el cortafuegos permite el acceso a todos los dominios y direcciones IP de los puertos definidos.

>[!NOTE]
>
>Puerto `25`, el puerto SMTP para enviar correo electrónico, siempre se bloquea, sin excepción.

### `protocol`

Como se ha mencionado, TCP es el protocolo predeterminado y el único permitido para las reglas. UDP y sus puertos no están permitidos. Por este motivo, puede omitir la variable `protocol` opción de todas las reglas. Si desea incluirlo de todos modos, debe establecer el valor en `tcp`, como se muestra en la primera regla del ejemplo.

## Búsqueda de nombres de dominio para permitir

Para ayudarle a identificar los dominios que desea incluir en las reglas de filtrado de salida, utilice el siguiente comando para analizar el `dns.log` y mostrar una lista de todas las solicitudes DNS que ha registrado el sitio:

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

Este comando también muestra las solicitudes DNS realizadas pero bloqueadas por las reglas de filtrado de salida. La salida no muestra qué dominios se bloquearon, solo que se realizaron solicitudes. La salida no muestra las solicitudes realizadas mediante una dirección IP.

```terminal
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

A diferencia de las direcciones IP, los dominios suelen ser más específicos y seguros para el filtrado de salidas. Por ejemplo, si agrega una dirección IP para un servicio que utiliza una CDN, está permitiendo la dirección IP para la CDN, que puede ser utilizada por cientos o miles de otros dominios. Con una dirección IP, podría estar permitiendo el acceso saliente a miles de otros servidores.

## Prueba de reglas de filtrado de salida

Después de recopilar y configurar las reglas de acceso para los dominios y las direcciones IP que necesita su sitio, es hora de realizar notificaciones push y pruebas.

Para probar las reglas de filtrado de salida:

1. Crear un script de shell de `curl` para acceder a los dominios y las direcciones IP de las reglas. Incluya comandos que prueben el acceso a dominios y direcciones IP que deben bloquearse.

1. Configurar un `post_deploy` enlace en su `.magento.app.yaml` para ejecutar el script.

1. Empuje su `firewall` y el script de prueba a su `integration` Rama.

1. Examine la `post_deploy` salida de su `curl` comandos.

1. Refine su `firewall` reglas, actualizar el `curl` script, confirmación, inserción y repetición.

### `curl` ejemplo de script

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy` ejemplo

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
