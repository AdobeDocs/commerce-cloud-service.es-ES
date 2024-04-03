---
title: Configurar [!DNL Xdebug]
description: Obtenga información sobre cómo configurar la extensión Xdebug para depurar Adobe Commerce en el desarrollo de proyectos de infraestructura en la nube.
exl-id: bf2d32d8-fab7-439e-8df3-b039e53009d4
source-git-commit: 751456f50e7b017b47c2ff43e008c2d04a558d96
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 0%

---

# Configurar Xdebug

[!DNL Xdebug] es una extensión para depurar tu PHP. Aunque puede utilizar un IDE de su elección, a continuación se explica cómo configurar [!DNL Xdebug] y [!DNL PhpStorm] para depurar en el entorno local.

>[!NOTE]
>
>Puede configurar [!DNL Xdebug] para ejecutarse en el entorno de Cloud Docker para la depuración local sin cambiar la configuración del proyecto de Adobe Commerce en la infraestructura en la nube. Consulte [Configurar Xdebug para Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

Para habilitar [!DNL Xdebug], debe configurar un archivo en el repositorio de Git, configurar el IDE y configurar el reenvío de puertos. Puede configurar algunas opciones en la `magento.app.yaml` archivo. Después de editar, inserte los cambios de Git en todos los entornos de Starter y de integración de Pro para habilitar [!DNL Xdebug]. [!DNL Xdebug] ya está disponible en entornos de ensayo y producción Pro.

Una vez configurados, puede depurar los comandos de CLI, las solicitudes web y el código. Recuerde que todos los entornos de infraestructura en la nube son de solo lectura. Clone el código en el entorno de desarrollo local para realizar la depuración. Para los entornos de ensayo y producción de Pro, consulte [instrucciones adicionales](#debug-for-pro-staging-and-production) para [!DNL Xdebug].

## Requisitos

Para ejecutar y utilizar [!DNL Xdebug], necesita la URL SSH para el entorno. Puede localizar la información a través del [[!DNL Cloud Console]](../project/overview.md) o su [!DNL Cloud Onboarding UI].

## Configurar Xdebug

Para configurar [!DNL Xdebug], siga estos pasos:

- [Trabajar en una rama para insertar actualizaciones de archivos](#get-started-with-a-branch)
- [Activar [!DNL Xdebug] para entornos](#enable-xdebug-in-your-environment)
- [Configurar el IDE](#configure-phpstorm)
- [Configuración del reenvío de puertos](#set-up-port-forwarding)

### Introducción a una rama

Para agregar [!DNL Xdebug], el Adobe recomienda trabajar en [una rama de desarrollo](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### Habilitar Xdebug en su entorno

Puede activar [!DNL Xdebug] directamente a todos los entornos de Starter y entornos de integración de Pro. Este paso de configuración no es necesario para los entornos de ensayo y producción de Pro. Consulte [Depurar para ensayo y producción profesionales](#debug-for-pro-staging-and-production).

Para habilitar [!DNL Xdebug] para el proyecto, añada `xdebug` a la `runtime:extensions` de la sección `.magento.app.yaml` archivo.

**Para habilitar Xdebug**:

1. En su terminal local, abra el `.magento.app.yaml` en un editor de texto.

1. En el `runtime` sección, en `extensions`, agregue `xdebug`. Por ejemplo:

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. Guarde los cambios en `.magento.app.yaml` y salga del editor de texto.

1. Agregue, confirme e inserte los cambios para volver a implementar el entorno.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

Cuando se implementa en entornos Starter y entornos de integración Pro, [!DNL Xdebug] ya está disponible. Continúe configurando el IDE. Para PhpStorm, consulte [Configurar PhpStorm](#configure-phpstorm).

### Configurar PhpStorm

El [PhpStorm](https://www.jetbrains.com/phpstorm/) El IDE debe configurarse para que funcione correctamente con [!DNL Xdebug].

**Para configurar PhpStorm para que funcione con Xdebug**:

1. En el proyecto PhpStorm, abra el **Configuración** panel.

   - _macOS_—Seleccionar **PhpStorm** > **Preferencias**.
   - _Windows/Linux_—Seleccionar **Archivo** > **Configuración**.

1. En el _Configuración_ , expanda y busque el **Idiomas y marcos** > **PHP** > **Servidores** sección.

1. Haga clic en **+** para agregar una configuración de servidor. El nombre del proyecto aparece en gris en la parte superior.

1. [Opcional] Configure las siguientes opciones para la nueva configuración del servidor. Consulte [No hay ningún servidor de depuración configurado](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) en el _PHPStorm_ documentación.

   - **Nombre**: introduzca el mismo que el nombre de host. Este valor debe coincidir con el valor de `PHP_IDE_CONFIG` variable en [Depurar comandos CLI](#debug-cli-commands) para utilizar CLI para la depuración.
   - **Host**: introduzca el nombre de host.
   - **Puerto**: Entrar `443`.
   - **Depurador**—Seleccionar `Xdebug`.

1. Seleccionar **Usar asignaciones de ruta**. En el _Archivo/Directorio_ panel, la raíz del proyecto para el `serverName` muestra.

1. En el **Ruta absoluta en el servidor** Haga clic en la columna **Editar** y añada una configuración basada en el entorno.

   - Para todos los entornos de Starter y entornos de integración Pro, la ruta remota es `/app`.
   - Para entornos de ensayo y producción Pro:

      - Producción: `/app/<project_code>/`
      - Ensayo:  `/app/<project_code>_stg/`

1. Cambie el [!DNL Xdebug] puerto 9000 en el **Idiomas y marcos** > **PHP** > **Depurar** > **Xdebug** > **Puerto de depuración** panel.

1. Clic **Aplicar**.

### Configuración del reenvío de puertos

Asignar el `XDEBUG` del servidor al sistema local. Para realizar cualquier tipo de depuración, debe reenviar el puerto 9000 desde el servidor de Adobe Commerce en la nube a su equipo local. Consulte una de las siguientes secciones:

- [Reenvío de puertos en Mac o UNIX](#port-forwarding-on-mac-or-unix)
- [Reenvío de puertos en Windows](#port-forwarding-on-windows)

#### Reenvío de puertos en Mac o UNIX®

**Para configurar el reenvío de puertos en un entorno Mac o UNIX®**:

1. Abra un terminal.

1. Utilice SSH para establecer la conexión.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   Utilice el `-v` opción (detallada) para que cuando se conecte un socket al puerto que se está reenviando, se muestre en el terminal.

   Si se muestra el error &quot;no se puede conectar&quot; o &quot;no se pudo escuchar el puerto en remoto&quot;, podría haber otra sesión SSH activa en el servidor que está ocupando el puerto 9000. Si esa conexión no se está utilizando, puede terminarla.

**Para solucionar problemas de conexión**:

1. Utilice SSH para iniciar sesión en el entorno de integración, ensayo o producción remoto.

1. Ver una lista de sesiones SSH: `who`

1. Ver sesiones SSH existentes por usuario. ¡Tenga cuidado de no afectar a un usuario que no sea usted!

   - integración: los nombres de usuario son similares a `dd2q5ct7mhgus`
   - Ensayo: los nombres de usuario son similares a `dd2q5ct7mhgus_stg`
   - Producción: los nombres de usuario son similares a `dd2q5ct7mhgus`

1. Para una sesión de usuario anterior a la suya, busque el valor de pseudoterminal (PTS), como `pts/0`.

1. Elimine el ID de proceso (PID) correspondiente al valor PTS.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   Respuesta de ejemplo:

   ```terminal
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   Para finalizar la conexión, introduzca un comando kill con el ID de proceso (PID).

   ```bash
   kill 3664
   ```

#### Reenvío de puertos en Windows

Para configurar el reenvío de puertos (túnel SSH) en Windows, debe configurar la aplicación de terminal de Windows. En este ejemplo se explica cómo crear un túnel SSH utilizando [Masilla](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Puede utilizar otras aplicaciones como Cygwin. Para obtener más información sobre otras aplicaciones, consulte la documentación del proveedor proporcionada con esas aplicaciones.

**Para configurar un túnel SSH en Windows con Putty**:

1. Si aún no lo ha hecho, descargue [Masilla](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. Iniciar Masilla.

1. En el panel Categoría, haga clic en **Session**.

1. Introduzca la siguiente información:

   - **Nombre de host (o dirección IP)** field: introduzca la variable [URL SSH](../development/secure-connections.md#connect-to-a-remote-environment) para su servidor en la nube
   - **Puerto** campo: Intro `22`

   ![Configurar Masilla](../../assets/xdebug/putty-session.png)

1. En el _Categoría_ , haga clic en **Conexión** > **SSH** > **Túneles**.

1. Introduzca la siguiente información:

   - **Puerto de origen** campo: Intro `9000`
   - **Destino** campo: Intro `127.0.0.1:9000`
   - Clic **Remoto**

1. Clic **Añadir**.

   ![Creación de un túnel SSH en Putty](../../assets/xdebug/putty-tunnels.png)

1. En el _Categoría_ , haga clic en **Session**.

1. En el **Sesiones guardadas** , introduzca un nombre para este túnel SSH.

1. Clic **Guardar**.

   ![Guarde el túnel SSH.](../../assets/xdebug/putty-session-save.png)

1. Para probar el túnel SSH, haga clic en **Cargar**, luego haga clic en **Abrir**.

   Si aparece el error &quot;no se puede conectar&quot;, compruebe lo siguiente:

   - Todos los ajustes de Masilla son correctos
   - Está ejecutando Putty en el equipo en el que se encuentran las claves SSH de su Adobe Commerce privado en la infraestructura de la nube

## Acceso SSH a entornos Xdebug

Para iniciar la depuración, realizar la configuración y mucho más, necesita los comandos SSH para acceder a los entornos. Puede obtener esta información a través del [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) y la hoja de cálculo del proyecto.

Para entornos Starter y entornos de integración Pro, puede utilizar lo siguiente `magento-cloud` Comando CLI para SSH en esos entornos:

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

Para usar [!DNL Xdebug], SSH para el entorno de la siguiente manera:

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

Por ejemplo,

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Depurar para ensayo y producción profesionales

>[!NOTE]
>
>En entornos de ensayo y producción Pro, [!DNL Xdebug] siempre está disponible, ya que estos entornos tienen una configuración especial para [!DNL Xdebug]. Todas las solicitudes web normales se dirigen a un proceso PHP dedicado que no tiene [!DNL Xdebug]. Por lo tanto, estas solicitudes se procesan normalmente y no están sujetas a la degradación del rendimiento cuando [!DNL Xdebug] está cargado. Cuando se envía una solicitud web que tiene el [!DNL Xdebug] , se dirige a un proceso PHP separado que tiene [!DNL Xdebug] cargado.

Para usar [!DNL Xdebug] específicamente en el entorno de ensayo y producción de planificación profesional, puede crear un túnel SSH y una sesión web independientes a los que solo tiene acceso. Este uso difiere del acceso habitual, ya que solo proporciona acceso a usted y no a todos los usuarios.

Necesita lo siguiente:

- Comandos SSH para acceder a los entornos. Puede obtener esta información a través del [[!DNL Cloud Console]](../project/overview.md) o su [!DNL Cloud Onboarding UI].
- El `xdebug_key` valor establecido al configurar los entornos Staging y Pro.

  El `xdebug_key` se puede encontrar utilizando SSH para iniciar sesión en el nodo principal y ejecutar:

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**Para configurar un túnel SSH en un entorno de ensayo o producción**:

1. Abra un terminal.

1. Limpie todas las sesiones SSH de cada nodo web del clúster.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. Configure el túnel SSH para Xdebug para cada nodo web del clúster.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

**Para iniciar la depuración mediante la dirección URL del entorno**:

1. Habilitar la depuración remota; visite el sitio en el explorador y añada lo siguiente a la dirección URL donde `KEY` es el valor de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   Este paso establece la cookie que envía las solicitudes del explorador al déclencheur [!DNL Xdebug].

1. Complete la depuración con [!DNL Xdebug].

1. Cuando esté listo para finalizar la sesión, utilice el siguiente comando para quitar la cookie y finalizar la depuración a través del explorador donde `KEY` es el valor de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >El `XDEBUG_SESSION_START` pasado por `POST` no se admiten solicitudes de.

## Depurar comandos CLI

En esta sección se explican los comandos de CLI de depuración.

Para depurar comandos CLI:

1. SSH en el servidor que desea depurar mediante comandos CLI.

1. Cree las siguientes variables de entorno:

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   Estas variables se eliminan cuando finaliza la sesión SSH.

1. Comenzar depuración

   En entornos Starter y entornos de integración Pro, ejecute el comando CLI para depurar.
Puede añadir opciones de tiempo de ejecución, por ejemplo:

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   En los entornos de ensayo y producción de Pro, debe especificar la ruta al [!DNL Xdebug] Archivo de configuración de PHP al depurar comandos CLI, por ejemplo:

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## Depuración de solicitudes web

Los siguientes pasos le ayudan a depurar solicitudes web.

1. En el _Extensión_ , haga clic en **Depurar** para habilitar.

1. Haga clic con el botón secundario, seleccione el menú de opciones y establezca la tecla IDE en **PHPSTORM**.

1. Instale el [!DNL Xdebug] cliente en el explorador. Configúrelo y actívelo.

### Ejemplo: Configuración de Chrome

En esta sección se explica cómo utilizar [!DNL Xdebug] en Chrome con el complemento [!DNL Xdebug] Extensión del ayudante. Para obtener información acerca de [!DNL Xdebug] herramientas para otros exploradores, consulte la documentación del explorador.

**Para utilizar Xdebug Helper con Chrome**:

1. Crear un [Túnel SSH](#ssh-access-to-xdebug-environments) al servidor de Cloud.

1. Instale el [Extensión de ayuda de Xdebug](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) de la tienda de Chrome.

1. Habilite la extensión en Chrome como se muestra en la siguiente ilustración.

   ![Habilitar la extensión Xdebug en Chrome](../../assets/xdebug/enable-chrome-ext.png)

1. En Chrome, haga clic con el botón derecho en el icono de ayuda verde en la barra de herramientas de Chrome.

1. En el menú emergente, haga clic en **Opciones**.

1. Desde el _Clave IDE_ , haga clic en **PhpStorm**.

1. Clic **Guardar**.

   ![Opciones del asistente de Xdebug](../../assets/xdebug/helper-options.png)

1. Abra el proyecto PhpStorm.

1. En la barra de navegación superior, haga clic en **Empezar a escuchar** icono.

   Si no aparece la barra de navegación, haga clic en **Ver** > **Barra de navegación**.

1. En el panel de navegación de PhpStorm, haga doble clic en el archivo PHP que desea probar.

## Depuración del código local

Debido a los entornos de solo lectura, debe extraer el código de la estación de trabajo local desde un entorno o rama Git específica para realizar la depuración.

El método que elija depende de usted. Tiene las siguientes opciones:

- Compruebe el código de Git y ejecute `composer install`

  Este método funciona a menos que `composer.json` hace referencia a paquetes de repositorios privados a los que no tiene acceso. Este método resulta en obtener toda la base de código de Adobe Commerce.

- Copie el `vendor`, `app`, `pub`, `lib`, y `setup` directorios

  Este método hace que tenga todo el código que pueda probar. Según el número de recursos estáticos que tenga, podría resultar en una transferencia larga con un gran volumen de archivos.

- Copie el `vendor` solo directorio

  Debido a que la mayor parte del código se encuentra en `vendor` , es probable que este método dé como resultado buenas pruebas, aunque no están probando todo el código base.

**Para comprimir archivos y copiarlos en el equipo local**:

1. Utilice SSH para iniciar sesión en el entorno remoto.

1. Comprima los archivos.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   Por ejemplo, para comprimir `vendor` solo directorio:

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. En su entorno local, utilice PhpStorm para comprimir los archivos.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
