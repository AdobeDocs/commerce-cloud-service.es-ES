---
source-git-commit: 6d8c082d78259f8f7adb0fb7f11ff4fcdb234124
workflow-type: tm+mt
source-wordcount: '4030'
ht-degree: 0%

---
# ece-tools

<!-- The template to render with above values -->
**Versión**: 2002.1.18

Esta referencia contiene 34 comandos disponibles a través del `ece-tools` herramienta de línea de comandos.
La lista inicial se genera automáticamente utilizando `ece-tools list` en Adobe Commerce en la infraestructura en la nube.

>[!NOTE]
>
>Esta referencia se genera a partir del código base de la aplicación. Para cambiar el contenido, puede actualizar el código fuente para la implementación del comando correspondiente en la [código base](https://github.com/magento/magento-cloud-cli) y envíe los cambios para que se revisen. Otra forma es _Danos tu opinión_ (busque el vínculo en la parte superior derecha). Para ver las directrices de contribución, consulte [Contribuciones de código](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

## `_complete`

Comando interno para proporcionar sugerencias de finalización de shell

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

### `--shell`, `-s`

El tipo de contenedor (&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;)

- Requiere un valor

### `--input`, `-i`

Una matriz de tokens de entrada (por ejemplo, COMP_WORDS o argv)

- Predeterminado: `[]`
- Requiere un valor

### `--current`, `-c`

Índice de la matriz de &quot;entrada&quot; en la que se encuentra el cursor (p. ej. COMP_CWORD)

- Requiere un valor

### `--api-version`, `-a`

Versión de API del script de finalización

- Requiere un valor

### `--symfony`, `-S`

obsoleto

- Requiere un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `build`

Crea una aplicación.

```bash
ece-tools build
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `completion`

Volcar el script de finalización de shell

```bash
ece-tools completion [--debug] [--] [<shell>]
```


### `shell`

El tipo de shell (p. ej. &quot;bash&quot;), el valor de la variable env &quot;$SHELL&quot; se usará si no se proporciona


### `--debug`

Seguimiento del registro de depuración de finalización

- Predeterminado: `false`
- No acepta un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `db-dump`

Crea copias de seguridad de base de datos.

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```


### `databases`

Bases de datos para backup. Valores disponibles: [principales ventas de presupuesto]. Si no se especifica el valor del argumento, se crearán copias de seguridad de la base de datos con las credenciales almacenadas en el `MAGENTO_CLOUD_RELATIONSHIP` variable de entorno y/o la variable `stage.deploy.DATABASE_CONFIGURATION` propiedad del archivo de configuración .magento.env.yaml.

- Predeterminado: `[]`

- Matriz

### `--remove-definers`, `-d`

Quitar definidores del volcado de la base de datos

- Predeterminado: `false`
- No acepta un valor

### `--dump-directory`, `-a`

Usar directorio alternativo para guardar el volcado

- Requiere un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `deploy`

Implementa la aplicación.

```bash
ece-tools deploy
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `help`

Mostrar la ayuda de un comando

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```


### `command_name`

El nombre del comando

- Predeterminado: `help`


### `--format`

El formato de salida (txt, xml, json o md)

- Predeterminado: `txt`
- Requiere un valor

### `--raw`

Para generar la ayuda del comando raw

- Predeterminado: `false`
- No acepta un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `list`

Comandos de lista

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```


### `namespace`

El nombre del área de nombres


### `--raw`

Para generar la lista de comandos raw

- Predeterminado: `false`
- No acepta un valor

### `--format`

El formato de salida (txt, xml, json o md)

- Predeterminado: `txt`
- Requiere un valor

### `--short`

Para omitir la descripción de argumentos de comandos

- Predeterminado: `false`
- No acepta un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `patch`

Aplica parches personalizados.

```bash
ece-tools patch
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `post-deploy`

Realiza operaciones después de la implementación.

```bash
ece-tools post-deploy
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `run`

Ejecución de escenarios.

```bash
ece-tools run <scenario>...
```


### `scenario`

Escenario(s)

- Predeterminado: `[]`

- Requerido
- Matriz

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `backup:list`

Muestra la lista de archivos de copia de seguridad.

```bash
ece-tools backup:list
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `backup:restore`

Restaure los archivos de configuración importantes. Ejecutar copia de seguridad: lista para mostrar la lista de archivos de copia de seguridad.

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

### `--force`, `-f`

Sobrescribir archivos existentes al restaurar una copia de seguridad

- Predeterminado: `false`
- No acepta un valor

### `--file`

Una ruta de recuperación de archivos específica

- Acepta un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `build:generate`

Genera todos los archivos necesarios para la fase de compilación.

```bash
ece-tools build:generate
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `build:transfer`

Transfiere los archivos generados al directorio init.

```bash
ece-tools build:transfer
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cloud:config:create`

Crea un `.magento.env.yaml` con la configuración de variables de compilación, implementación y posterior a la implementación especificada. Sobrescribe los existentes `.magento,.env.yaml` archivo.

```bash
ece-tools cloud:config:create <configuration>
```


### `configuration`

Configuración en formato JSON

- Requerido

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cloud:config:update`

Actualiza el existente `.magento.env.yaml` con la configuración especificada. Crea `.magento.env.yaml` archivo si no existe.

```bash
ece-tools cloud:config:update <configuration>
```


### `configuration`

Configuración en formato JSON

- Requerido

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cloud:config:validate`

Valida `.magento.env.yaml` archivo de configuración

```bash
ece-tools cloud:config:validate
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `config:dump`

Configuración de volcado para la implementación de contenido estático.

```bash
ece-tools config:dump
```


```bash
ece-tools dump
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cron:disable`

Deshabilite todos los procesos cron del Magento y finalice todos los procesos en ejecución.

```bash
ece-tools cron:disable
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cron:enable`

Activa los procesos cron del Magento.

```bash
ece-tools cron:enable
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cron:kill`

Termina todos los procesos cron del Magento.

```bash
ece-tools cron:kill
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `cron:unlock`

Desbloquee los trabajos cron que se quedaron en estado &quot;en ejecución&quot;.

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

### `--job-code`

Código de trabajo Cron para desbloquear.

- Predeterminado: `[]`
- Acepta varios valores

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `dev:generate:schema-error`

Genera el archivo dist/error-codes.md a partir del archivo schema.error.yaml.

```bash
ece-tools dev:generate:schema-error
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `dev:git:update-composer`

Actualiza el Compositor para la implementación desde Git.

```bash
ece-tools dev:git:update-composer
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `env:config:show`

Muestre variables de entorno de configuración de nube codificadas.

```bash
ece-tools env:config:show [<variable>...]
```


### `variable`

Variables de entorno para mostrar, posibles opciones: servicios, rutas, variables

- Predeterminado: `[]`

- Matriz

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `error:show`

Muestra información sobre errores por ID de error o sobre todos los errores de la última implementación.

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```


### `error-code`

Código de error, si no se ha pasado el comando, se muestra información sobre todos los errores de la última implementación


### `--json`, `-j`

Se utiliza para obtener los resultados en formato JSON

- Predeterminado: `false`
- No acepta un valor

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `module:refresh`

Actualiza la configuración para habilitar los módulos recién añadidos.

```bash
ece-tools module:refresh
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `schema:generate`

Genera el archivo de esquema *.dist.

```bash
ece-tools schema:generate
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:ideal-state`

Comprueba el estado ideal de configuración.

```bash
ece-tools wizard:ideal-state
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:master-slave`

Comprueba la configuración maestro-esclavo.

```bash
ece-tools wizard:master-slave
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:scd-on-build`

Comprueba el SCD en la configuración de compilación.

```bash
ece-tools wizard:scd-on-build
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:scd-on-demand`

Comprueba la configuración de SCD bajo demanda.

```bash
ece-tools wizard:scd-on-demand
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:scd-on-deploy`

Comprueba el SCD en la configuración de implementación.

```bash
ece-tools wizard:scd-on-deploy
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor


## `wizard:split-db-state`

Comprueba la capacidad de dividir la base de datos y si ya se ha dividido o no.

```bash
ece-tools wizard:split-db-state
```

### `--help`, `-h`

Muestra la ayuda del comando especificado. Cuando no se proporciona ningún comando, se muestra la ayuda para el comando de lista

- Predeterminado: `false`
- No acepta un valor

### `--quiet`, `-q`

No generar ningún mensaje

- Predeterminado: `false`
- No acepta un valor

### `--verbose`, `-v|-vv|-vvv`

Aumente el nivel de detalle de los mensajes: 1 para un resultado normal, 2 para un resultado más detallado y 3 para una depuración

- Predeterminado: `false`
- No acepta un valor

### `--version`, `-V`

Mostrar esta versión de la aplicación

- Predeterminado: `false`
- No acepta un valor

### `--ansi`

Forzar (o deshabilitar —sin ansi) la salida ANSI

- No acepta un valor

### `--no-ansi`

Anule la opción &quot;—ansi&quot;

- Predeterminado: `false`
- No acepta un valor

### `--no-interaction`, `-n`

No haga ninguna pregunta interactiva

- Predeterminado: `false`
- No acepta un valor

