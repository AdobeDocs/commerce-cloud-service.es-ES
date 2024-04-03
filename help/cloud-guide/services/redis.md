---
title: Configurar el servicio Redis
description: Aprenda a configurar y optimizar Redis como solución de caché back-end para Adobe Commerce en infraestructuras en la nube.
feature: Cloud, Cache, Services
exl-id: d6971875-d302-495a-ad10-a81c507c2bc9
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Configurar el servicio Redis

[Redis](https://redis.io) es una solución de caché back-end opcional que reemplaza a Zend Framework Zend_Cache_Backend_File, que Adobe Commerce utiliza de forma predeterminada.

Consulte [Configurar Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html) en el _Guía de configuración_.

{{service-instruction}}

**Para habilitar Redis**:

1. Añada el nombre y el tipo necesarios a `.magento/services.yaml` archivo.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Para proporcionar su propia configuración de Redis, agregue una `core_config` clave en su `.magento/services.yaml` archivo:

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Configure las relaciones en la variable `.magento.app.yaml` archivo.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Verificar las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Uso de la CLI de Redis

Suponiendo que la relación de Redis tenga el nombre `redis`, puede acceder a él mediante el complemento `redis-cli` herramienta.

1. Utilice SSH para conectarse al entorno de integración con Redis instalado y configurado.

1. Abra un túnel SSH a un host.

   ```bash
   redis-cli -h redis.internal
   ```

## Obtener la versión de Redis instalada

Utilice el siguiente comando para obtener la versión de Redis instalada en un entorno de integración:

```bash
redis-cli -h redis.internal info | grep version
```

Respuesta de ejemplo:

```terminal
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis en ensayo y producción Pro

Para obtener la versión de Redis instalada en un entorno de ensayo o producción, utilice el `redis-server` comando:

```bash
redis-server -v
```

```terminal
Redis server v=7.0.5 ...
```

Utilice el siguiente comando para instalar la configuración de Redis en un entorno de ensayo o producción de Pro:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Respuesta de ejemplo:

```terminal
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Solución de problemas de Redis

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda sobre la resolución de problemas de Redis:

- [Retrasar el retraso del inicio de sesión o cierre de compra del administrador](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [Implementación de caché de Redis extendida en Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [MDVA-30102: La caché de Redis se está llenando](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/patches/v1-0-6/mdva-30102-magento-patch-redis-cache-getting-full.html)
- [Alertas administradas en Adobe Commerce: alerta de advertencia de memoria Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Alertas administradas en Adobe Commerce: leer alerta crítica de memoria](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
- [Solucionador de problemas Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-troubleshooter.html)
