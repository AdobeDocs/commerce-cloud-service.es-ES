---
title: Administrar espacio en disco
description: Obtenga información acerca de cómo administrar el espacio en disco mediante la interfaz de línea de comandos.
feature: Cloud, Storage
exl-id: 480cb33b-ac83-441d-946e-5b4de09ad84e
source-git-commit: 8b40397796ee865aefbf8a7948cc9a3aceb1d35c
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 0%

---

# Administrar espacio en disco

Puede encontrar la capacidad de almacenamiento total para su proyecto de Cloud en su contrato de infraestructura en la nube de Adobe Commerce y en su [página de cuenta](https://accounts.magento.cloud/user). Cada tarjeta de proyecto de su cuenta muestra la cantidad de _entornos_, el _almacenamiento_ capacidad en GB y el número de _usuarios_. Como alternativa, puede utilizar el siguiente comando de Cloud:

```bash
magento-cloud subscription:info | grep storage
```

Respuesta de ejemplo:

```terminal
| storage              | 51200
```

Cuando un entorno de ensayo o producción de Pro alcanza o supera el 95% de la capacidad de almacenamiento, la herramienta de monitorización de la infraestructura en la nube déclencheur una alerta de asistencia que le notifica de un aumento automático de la capacidad de almacenamiento.

Ejemplo de notificación:

>[!BEGINSHADEBOX]

_&quot;Nuestra monitorización ha detectado que el almacenamiento de archivos en su clúster (project-id-environment) está casi lleno. El uso del disco está actualmente en niveles de uso críticos con menos de 1 GiB restante. El volumen de almacenamiento compartido se está actualizando actualmente de 60 GiB a 70 GiB para mantener sus servicios en funcionamiento. Eche un vistazo al uso de los archivos de ensayo y producción para ver si puede liberar espacio.&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Se recomienda supervisar regularmente su capacidad de almacenamiento y mantenerla por debajo del 90 % para evitar estos aumentos automáticos. Una vez asignado, el aumento del almacenamiento para el ensayo y la producción de Pro no se puede revertir.

## Compruebe el entorno de integración

Puede comprobar el uso del espacio en disco para su entorno de integración utilizando `magento-cloud` CLI.

**Para comprobar el uso aproximado del espacio en disco**:

```bash
magento-cloud db:size
```

Respuesta de ejemplo:

```terminal
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Todos los montajes comparten un disco. Puede comprobar el uso del espacio en disco para los montajes mediante el `magento-cloud` CLI.

**Para comprobar el uso aproximado del espacio en disco para los montajes**:

```bash
magento-cloud mount:size
```

Respuesta de ejemplo:

```terminal
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## Comprobar clústeres dedicados

Para los entornos de ensayo y producción de Pro, puede comprobar el uso del espacio en disco en cada entorno utilizando `disk free` , que informa de la cantidad de espacio en disco utilizado por el sistema de archivos. Debe utilizar SSH para iniciar sesión en un entorno remoto.

```bash
df -h
```

El `-h` muestra el informe en un formato legible en lenguaje natural (KB, MB o GB).

En la siguiente respuesta de ejemplo, la variable `/mnt/shared` mount muestra el espacio en disco para los medios y `/data/mysql/` mount muestra espacio en disco para la base de datos:

```terminal
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

Puede limitar la respuesta especificando un directorio. Por ejemplo:

```bash
df -h var/
```

Respuesta de ejemplo:

```terminal
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Asignar espacio en disco

Dos [archivos de configuración](../environment/overview.md) Controle la asignación de espacio en disco en los entornos de nube: la `.magento.app.yaml` y el `.magento/services.yaml` archivo. Cada archivo contiene el `disk` , que define el valor del tamaño del disco en MB para la configuración correspondiente. Solo puede cambiar la asignación del espacio en disco en los entornos de integración de Pro y Starter.

>[!IMPORTANT]
>
>Para los entornos de ensayo y producción de Pro, debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para cambiar la asignación del espacio en disco. Solo se puede producir un aumento de tamaño de los entornos de ensayo y producción de Pro a determinados intervalos, por lo que, según el uso actual del espacio en disco, la asistencia técnica puede recomendar aumentar la asignación del espacio en disco en un mínimo de 10 GB. Una vez asignado, el aumento del almacenamiento para el ensayo y la producción de Pro no se puede revertir. El almacenamiento no se puede reasignar ni redistribuir entre los recursos. Para agregar más espacio de almacenamiento de archivos, reduzca el espacio en disco asignado para MySQL.

### Espacio en disco de aplicación

El `.magento.app.yaml` el archivo controla el [espacio en disco persistente](../application/properties.md#disk) disponible para la aplicación.

**Para aumentar el espacio en disco de la aplicación**:

1. En su entorno de desarrollo local, abra `.magento.app.yaml` archivo de configuración.

1. Defina un nuevo valor para `disk` propiedad (en MB).

   ```yaml
   disk: <value-mb>
   ```

1. Guarde los cambios en el archivo.

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   Los cambios surtirán efecto después de insertar el archivo YAML actualizado en el entorno remoto.

### Espacio en disco de servicio

El `.magento/services.yaml` controla el espacio en disco disponible para cada servicio, como MySQL y Redis.

**Para aumentar el espacio en disco de un servicio**:

1. En su entorno de desarrollo local, abra `.magento/services.yaml` archivo de configuración.

1. Añada o busque un servicio en el archivo. Consulte [más información sobre la configuración de servicios](../services/services-yaml.md).

1. Establezca un nuevo valor para la propiedad del disco (en MB).

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. Guarde los cambios en el archivo.

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   Los cambios surtirán efecto después de insertar el archivo YAML actualizado en el entorno remoto.

## Monitorización del espacio en disco

En entornos de Pro Production, puede supervisar el espacio en disco y otros indicadores de rendimiento mediante la directiva Alertas administradas para alertas de Adobe Commerce para New Relic. Para obtener más información, consulte [Monitorización del rendimiento con alertas administradas](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Para obtener más información, consulte [Prácticas recomendadas para resolver problemas de rendimiento de la base de datos](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html).

## No queda espacio

La caché de compilación puede crecer con el tiempo. Si recibe una advertencia que indique `No space left on device`, intente borrar la caché de la versión y volver a implementar:

```bash
magento-cloud project:clear-build-cache
```
