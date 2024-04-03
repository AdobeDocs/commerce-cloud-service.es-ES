---
title: Configurar el servicio MySQL
description: Aprenda a administrar el servicio MySQL para el almacenamiento de datos persistentes con Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Services, Storage
exl-id: 70820d00-8b82-4b60-87e4-ea98fd7ffcb2
source-git-commit: 9be8d1e062ab3dba86bc4d9c9ee8b8ece33d5b75
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Configurar el servicio MySQL

El `mysql` El servicio proporciona almacenamiento de datos persistente basado en [MariaDB](https://mariadb.com/) versiones 10.2 a 10.4, compatibles con [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) y funciones reimplementadas de MySQL 5.6 y 5.7.

La reindexación en MariaDB 10.4 tarda más tiempo en comparación con otras versiones de MariaDB o MySQL. Consulte [Indexadores](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) en el _Prácticas recomendadas de rendimiento_ guía.

>[!WARNING]
>
>Tenga cuidado al actualizar MariaDB de la versión 10.1 a la 10.2. MariaDB 10.1 es la última versión que admite _XtraDB_ como motor de almacenamiento. MariaDB 10.2 utiliza _InnoDB_ para el motor de almacenamiento. Después de actualizar de 10.1 a 10.2, no podrá revertir el cambio. Adobe Commerce admite ambos motores de almacenamiento; sin embargo, debe comprobar las extensiones y otros sistemas utilizados por el proyecto para asegurarse de que son compatibles con MariaDB 10.2. Consulte [Cambios incompatibles entre 10.1 y 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Para habilitar MySQL**:

1. Agregue el nombre, tipo y valor de disco necesarios (en MB) a `.magento/services.yaml` archivo.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Errores de MySQL, como `PDO Exception: MySQL server has gone away`, puede producirse como resultado de un espacio en disco insuficiente. Compruebe que ha asignado suficiente espacio en disco al servicio en la [`.magento/services.yaml`](services-yaml.md#disk) archivo.

1. Configure las relaciones en la variable `.magento.app.yaml` archivo.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Agregue, confirme e inserte los cambios de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Verificar las relaciones de servicio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configurar la base de datos MySQL

Dispone de las siguientes opciones al configurar la base de datos MySQL:

- **`schemas`**: un esquema define una base de datos. El esquema predeterminado es `main` base de datos.
- **`endpoints`**: cada punto final representa una credencial con privilegios específicos. El extremo predeterminado es `mysql`, que tiene `admin` acceso a la `main` base de datos.
- **`properties`**: las propiedades se utilizan para definir configuraciones de base de datos adicionales.

El siguiente es un ejemplo básico de configuración en `.magento/services.yaml` archivo:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

El `properties` en el ejemplo anterior, modifica el valor predeterminado `optimizer` configuración como [recomendado en la guía Prácticas recomendadas de rendimiento](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**Opciones de configuración de MariaDB**:

| Opciones | Descripción | Valor predeterminado |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Conjunto de caracteres predeterminado. | utf8mb4 |
| `default_collation` | Intercalación predeterminada. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Tamaño máximo de los paquetes, en MB. Intervalo `1` hasta `100`. | 16 |
| `optimizer_switch` | Establezca valores para el optimizador de consultas. Consulte [Documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Seleccione las estadísticas utilizadas por el optimizador. Intervalo `1` hasta `5`. Consulte [Documentación de MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 para 10.4 y versiones posteriores |

### Configurar varios usuarios de base de datos

De forma opcional, puede configurar varios usuarios con diferentes permisos para acceder a `main` base de datos.

De forma predeterminada, hay un extremo denominado `mysql` que tiene acceso de administrador a la base de datos. Para configurar varios usuarios de la base de datos, debe definir varios extremos en la variable `services.yaml` y declare las relaciones en el `.magento.app.yaml` archivo. Para entornos de ensayo y producción Pro, [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar al usuario adicional.

Utilice una matriz anidada para definir los puntos finales para el acceso específico del usuario. Cada extremo puede designar el acceso a uno o más esquemas (bases de datos) y diferentes niveles de permisos en cada uno.

Los niveles de permiso válidos son:

- `ro`: solo se permiten consultas SELECT.
- `rw`: se permiten consultas SELECT y consultas INSERT, UPDATE y consultas de DELETE.
- `admin`: se permiten todas las consultas, incluidas las consultas DDL (CREATE TABLE, DROP TABLE, etc.).

Por ejemplo:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

En el ejemplo anterior, la variable `admin` El punto de conexión proporciona acceso de administrador a `main` base de datos, la `reporter` El extremo proporciona acceso de solo lectura y la variable `importer` el extremo proporciona acceso de lectura y escritura, lo que significa que:

- El `admin` el usuario tiene control total de la base de datos.
- El `reporter` el usuario solo tiene privilegios SELECT.
- El `importer` el usuario tiene privilegios SELECT, INSERT, UPDATE y DELETE.

Agregue los puntos finales definidos en el ejemplo anterior a `relationships` propiedad del `.magento.app.yaml` archivo. Por ejemplo:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Si configura un usuario MySQL, no puede utilizar el [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) mecanismo de control de acceso para procedimientos y vistas almacenados.

## Conexión a la base de datos

El acceso a la base de datos MariaDB requiere directamente que utilice un SSH para iniciar sesión en el entorno remoto de la nube y conectarse a la base de datos.

1. Utilice SSH para iniciar sesión en el entorno remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere las credenciales de inicio de sesión de MySQL de la `database` y `type` propiedades en la [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) variable.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   En la respuesta, busque la información de MySQL. Por ejemplo:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Conéctese a la base de datos.

   - Para empezar, utilice el siguiente comando:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Para Pro, utilice el siguiente comando con el nombre de host, el número de puerto, el nombre de usuario y la contraseña recuperados del `$MAGENTO_CLOUD_RELATIONSHIPS` variable.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Puede usar el complemento `magento-cloud db:sql` para conectarse a la base de datos remota y ejecutar comandos SQL.

## Conectar con base de datos secundaria

>[!IMPORTANT]
>
>Esta función solo está disponible en los clústeres de ensayo y producción profesional.

A veces, debe conectarse a la base de datos secundaria para mejorar el rendimiento de la base de datos o resolver los problemas de bloqueo de la base de datos. Si esta configuración es necesaria, utilice `"port" : 3304` para establecer la conexión. Consulte la [Práctica recomendada para configurar la conexión esclava MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) tema en la _Prácticas recomendadas de implementación_ guía.

## Resolución de problemas

Consulte los siguientes artículos de soporte de Adobe Commerce para obtener ayuda con la resolución de problemas de MySQL:

- [Comprobación de consultas y procesos lentos en MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [Crear volcado de base de datos en la nube](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [Herramienta de migración de datos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Actualización de Adobe Commerce: tablas compactas a dinámicas 2.2.x, 2.3.x a 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
