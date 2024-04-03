---
title: Implementar en ensayo y producción
description: Aprenda a implementar su código de infraestructura de Adobe Commerce en la nube en los entornos de ensayo y producción para realizar pruebas adicionales.
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 4b82289f-ee04-4b14-a0ed-7a8a19fc6a6a
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1289'
ht-degree: 0%

---

# Implementar en ensayo y producción

El proceso de implementación y lanzamiento comienza con el desarrollo, continúa con el ensayo y termina con el lanzamiento en producción. Adobe proporciona una solución de entorno integral para garantizar configuraciones coherentes. Todos los entornos admiten el acceso directo por URL a la tienda y el acceso Admin y SSH para los comandos CLI.

Cuando esté listo para implementar su tienda, debe completar la implementación y las pruebas en el entorno de ensayo antes de implementarlo en producción. Esta sección proporciona instrucciones e información detalladas sobre el proceso de generación e implementación, la migración de datos y contenido, y las pruebas.

>[!TIP]
>
>El Adobe recomienda crear un [reserva](../storage/snapshots.md) del entorno antes de las implementaciones.

Además, puede habilitar [Seguimiento de implementaciones con New Relic](../monitor/track-deployments.md) para supervisar los eventos de implementación y ayudarle a analizar el rendimiento entre implementaciones.

## Flujo de implementación de inicio

El Adobe recomienda crear un `staging` rama del `master` para apoyar mejor el desarrollo y la implementación de su plan de inicio. A continuación, tiene preparados dos de los cuatro entornos activos: `master` para producción y `staging` para Ensayo.

Para obtener información detallada del proceso, consulte [Iniciar desarrollo e implementación del flujo de trabajo](../architecture/starter-develop-deploy-workflow.md).

## Flujo de implementación de Pro

Pro viene con un entorno de integración grande con dos ramas activas, una global `master` ramas, Ensayo y Producción. Al crear el proyecto, el código está listo para ramificarse, desarrollarse e insertarse para crear e implementar el sitio. Aunque el entorno de integración puede tener muchas ramas, Ensayo y Producción solo tienen una rama para cada entorno.

Para obtener información detallada del proceso, consulte [Flujo de trabajo de desarrollo e implementación de Pro](../architecture/pro-develop-deploy-workflow.md).

## Implementar código en ensayo

El entorno de ensayo proporciona un entorno casi de producción que incluye una base de datos, un servidor web y todos los servicios, incluidos Fastly y New Relic. Puede insertar, combinar e implementar completamente a través del [[!DNL Cloud Console]](../project/overview.md) o [Comandos CLI de Cloud](../dev-tools/cloud-cli-overview.md) mediante una aplicación de terminal.

### Implementación de código con [!DNL Cloud Console]

El [!DNL Cloud Console] proporciona funciones para crear, administrar e implementar código en entornos de integración, ensayo y producción para planes Starter y Pro.

**En los proyectos Pro, implemente la rama de integración en el entorno de ensayo**:

1. [Iniciar sesión](https://accounts.magento.cloud) a su proyecto.
1. Seleccione el `integration` Rama.
1. Seleccione el **Combinar** opción para implementarla en Ensayo.

   ![Combinar](../../assets/button-merge.png){width="150"}

1. Completar todo [prueba](../test/staging-and-production.md) en el entorno de ensayo.
1. Cuando el ensayo esté listo, seleccione la opción **Combinar** para implementarlo en el entorno de producción.

**Para empezar, implemente la rama de desarrollo en ensayo**:

1. [Iniciar sesión](https://accounts.magento.cloud) a su proyecto.
1. Seleccione la rama de código preparada.
1. Seleccione el **Combinar** opción para implementarla en Ensayo.

   ![Combinar](../../assets/button-merge.png){width="150"}

1. Completar todo [prueba](../test/staging-and-production.md) en el entorno de ensayo.
1. Cuando el ensayo esté listo, seleccione la opción **Combinar** Opción de implementación en producción (`master`).

### Implementar código con la línea de comandos

La CLI de la nube proporciona comandos para implementar código. Necesita SSH y acceso Git a su proyecto.

#### Paso 1: Implementación y prueba del entorno de integración

1. Después de iniciar sesión en el proyecto, compruebe el entorno de integración:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronice el entorno de integración local con el entorno remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Cree una instantánea del entorno como copia de seguridad:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Actualice el código en la rama local según sea necesario.

1. Agregar, confirmar e insertar cambios en el entorno.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Prueba completa del sitio.

#### Paso 2: Combinar cambios en Ensayo e implementación

1. Consulte el entorno de ensayo:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronice el entorno de ensayo local con el entorno remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Cree una instantánea del entorno como copia de seguridad:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Combine el entorno de integración en Ensayo para implementar:

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Prueba completa del sitio.

#### Paso 3: Implementación en producción

1. Consulte, sincronice y cree una instantánea de su entorno de producción local.

1. Combine el entorno de ensayo con el de producción para implementar:

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Prueba completa del sitio.

## Migrar archivos estáticos

[Archivos estáticos](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) se almacenan en `mounts`. Existen dos métodos para migrar archivos desde una ubicación de montaje de origen, como el entorno local, a una ubicación de montaje de destino. Ambos métodos utilizan el `rsync` , pero Adobe recomienda utilizar la variable `magento-cloud` CLI para mover archivos entre el entorno local y remoto. Y el Adobe recomienda utilizar la variable `rsync` al mover archivos desde un origen remoto a una ubicación remota diferente.

### Migración de archivos mediante la CLI

Puede usar el complemento `mount:upload` y `mount:download` Comandos CLI para migrar archivos entre el entorno local y remoto. Ambos comandos utilizan el `rsync` , pero los comandos CLI proporcionan opciones y mensajes adaptados al entorno de Adobe Commerce en la infraestructura en la nube. Por ejemplo, si utiliza el comando simple sin opciones, la CLI le pide que seleccione el montaje o los montajes que desea cargar o descargar.

```bash
magento-cloud mount:download
```

Respuesta de ejemplo:

```terminal
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**Para cargar archivos desde una carpeta local `pub/media/` carpeta al remoto `pub/media/` carpeta para el entorno actual**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Respuesta de ejemplo:

```terminal
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Utilice el `--help` para la opción `mount:upload` y `mount:download` para ver más opciones. Por ejemplo, existe un `--delete` para eliminar archivos superfluos durante la migración.

### Migrar archivos mediante rsync

Como alternativa, puede utilizar la variable `rsync` para migrar archivos.

```bash
rsync -azvP <source> <destination>
```

Este comando utiliza las siguientes opciones:

- `a`-archive
- `z`Comprimir archivos durante la migración
- `v`-verboso
- `P`-progreso parcial

Consulte [rsync](https://linux.die.net/man/1/rsync) ayuda.

>[!NOTE]
>
>Para transferir medios directamente de entornos remotos a remotos, debe activar el reenvío de agentes SSH, consulte [Guía de GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Para migrar archivos estáticos de entornos remotos a remotos directamente (enfoque rápido)**:

1. Utilice SSH para iniciar sesión en el entorno de origen. No use el `magento-cloud` CLI. Uso del `-A` es importante porque permite el reenvío de la conexión del agente de autenticación.

   >[!TIP]
   >
   >Para encontrar la **Acceso SSH** vínculo en su [!DNL Cloud Console], seleccione el entorno y haga clic en **Acceso al sitio**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Utilice el `rsync` para copiar el `pub/media` desde el entorno de origen a un entorno remoto diferente.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Inicie sesión en el otro entorno remoto para comprobar que los archivos se migraron correctamente.

## Migración de la base de datos

>[!WARNING]
>
>Las operaciones de importación y exportación de bases de datos pueden tardar mucho tiempo, lo que puede afectar al rendimiento y la disponibilidad del sitio. Programe operaciones de importación y exportación durante las horas de menor actividad para evitar interrupciones o bajo rendimiento en los sitios de producción.

>[!BEGINSHADEBOX]

**Requisito previo:** Un volcado de base de datos (consulte el paso 3) debe incluir déclencheur de base de datos. Para descargarlos, confirme que tiene el [privilegio de déclencheur](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>La base de datos del entorno de integración se utiliza estrictamente para pruebas de desarrollo y puede incluir datos que no desea migrar a Ensayo y Producción.

Para implementaciones de integración continua, haga clic en Adobe **no recomienda** migrar datos de integración a ensayo y producción. Puede pasar datos de prueba o sobrescribir datos importantes. Todas las configuraciones vitales se pasan mediante el [archivo de configuración](../store/store-settings.md) y `setup:upgrade` durante la compilación y la implementación.

>[!ENDSHADEBOX]

Adobe **recomienda** migrar datos de producción a ensayo para probar por completo el sitio y almacenarlo en un entorno casi de producción con todos los servicios y configuraciones.

>[!NOTE]
>
>Para transferir medios directamente de entornos remotos a remotos, debe habilitar el reenvío del agente ssh, consulte [Guía de GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Realizar copia de seguridad de la base de datos

Se recomienda crear una copia de seguridad de la base de datos. El siguiente procedimiento utiliza las directrices de [Realizar copia de seguridad de la base de datos](../storage/database-dump.md).

**Para volcar la base de datos**:

1. [Utilizar SSH para iniciar sesión en el entorno remoto](../development/secure-connections.md#use-an-ssh-command) que contiene la base de datos que desea copiar.

1. Enumere las relaciones de entorno y anote la información de inicio de sesión de la base de datos.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Para Ensayo y Producción Pro, el nombre de la base de datos se encuentra en la variable `MAGENTO_CLOUD_RELATIONSHIPS` (normalmente es la misma que el nombre de la aplicación y el nombre de usuario).

1. Cree una copia de seguridad de la base de datos. Para elegir un directorio de destino para el volcado de la base de datos, utilice el `--dump-directory` opción.

   Para entornos Starter y entornos de integración Pro, utilice `main` como nombre de la base de datos:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Opciones de volcado:
   - `--dump-directory=<dir>`: permite elegir un directorio de destino para el volcado de la base de datos
   - `--remove-definers`: permite quitar sentencias DEFINER del volcado de la base de datos

1. Aunque se prefiere el método ECE-Tools, otro método es crear un archivo de volcado de base de datos utilizando MySQL nativo en formato GZIP.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Si ha configurado la autenticación de doble factor en el entorno de destino, es mejor excluir las tablas 2FA relacionadas para evitar volver a configurarla después de la migración de la base de datos:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Tipo `logout` para finalizar la conexión SSH.

### Soltar y volver a crear la base de datos

Al importar datos, debe soltar y crear una base de datos.

**Para soltar y volver a crear la base de datos**:

1. Establezca un [Túnel SSH](../development/secure-connections.md#ssh-tunneling) al entorno remoto.

1. Conéctese al servicio de base de datos.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. En el `MariaDB [main]>` , suelte la base de datos.

   Para la integración de Starter y Pro:

   ```shell
   drop database main;
   ```

   Para la producción:

   ```shell
   drop database <cluster-id>;
   ```

   Para Ensayo:

   ```shell
   drop database <cluster-ID_stg>;
   ```

1. Vuelva a crear la base de datos.

   Para la integración de Starter y Pro:

   ```shell
   create database main;
   ```

   Importar para producción:

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Importar para ensayo:

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```
