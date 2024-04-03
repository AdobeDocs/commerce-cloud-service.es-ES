---
title: Trabajadores
description: Obtenga información sobre cómo configurar la propiedad de trabajadores en [!DNL Commerce] archivo de configuración de la aplicación.
feature: Cloud, Configuration
exl-id: d6816925-5912-45ca-8255-6c307e58542d
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Propiedad Workers

Puede definir un trabajador para que se ejecute de forma independiente de la instancia web sin una instancia de Nginx en ejecución; sin embargo, el trabajador utiliza el mismo almacenamiento de red utilizado por la instancia [!DNL Commerce] aplicación. No es necesario configurar un servidor web en la instancia de trabajo (mediante Node.js o Go) porque el enrutador no puede dirigir solicitudes públicas al trabajador. Esto hace que la instancia de trabajo sea ideal para tareas en segundo plano o para ejecutar continuamente tareas que corren el riesgo de bloquear una implementación.

## Configuración de un trabajador

Los trabajadores solo están disponibles para su uso con los entornos de ensayo y producción de Pro. Los entornos de integración de Pro y Starter pueden optar por utilizar el [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) variable.

Para configurar un trabajador en Ensayo o Producción Pro, [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) e incluir la siguiente información:

- Identificador de proyecto
- ID de entorno
- Nombre del trabajador
- Comandos de inicio

Puede configurar un proceso por trabajador. Una configuración de trabajo básica y común en `.magento.app.yaml` podría tener el siguiente aspecto:

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

En este ejemplo se define un solo trabajador denominado `queue`, con un nivel de asignación de recursos pequeño (tamaño S), y ejecuta el `php ./bin/magento` al inicio. El trabajador `queue` a continuación, se ejecuta en cada nodo como un proceso de trabajo. Si el comando se cierra, se reinicia automáticamente.

## Comandos y anulaciones

El `commands.start` es necesaria para iniciar comandos con la aplicación de trabajo. Puede utilizar cualquier comando shell válido, aunque es ideal utilizar el lenguaje de su aplicación. Si el comando especificado por `start` La clave termina y se reinicia automáticamente.

>[!IMPORTANT]
>
>El `deploy` y `post_deploy` ganchos y `crons` los comandos solo se ejecutan en el contenedor web, no en las instancias de trabajo.

### Herencia

Definiciones de la variable `size`, `relationships`, `access`, `disk` y `mount`, y `variables` las propiedades las hereda un trabajador, a menos que se anulen explícitamente.

Las siguientes propiedades son las más utilizadas para anular [configuración de nivel superior](properties.md):

- `size`: asigne menos recursos a un único proceso en segundo plano.
- `variables`: permite indicar a la aplicación que se ejecute de forma diferente

### Intervalos y colas

Aunque cada trabajador se pone en cola detrás de otro, la siguiente configuración produce una separación coherente de dos segundos en las marcas de tiempo en el `var/time.txt` , independientemente del tiempo de espera de ocho segundos dentro del código PHP:

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
