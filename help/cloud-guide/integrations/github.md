---
title: Integración de GitHub
description: Aprenda a integrar su proyecto de Adobe Commerce en la nube con GitHub.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
exl-id: 5305452f-4c8d-438c-ac78-e2e1ec2f8cd9
source-git-commit: 4d32fc596064f01eaefe3ee509a655837fa846de
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# Integración de GitHub

La integración de GitHub le permite administrar su Adobe Commerce en entornos de infraestructura en la nube directamente desde su repositorio de GitHub. La integración administra el contenido que ya se encuentra en GitHub y se sincroniza con su repositorio de código de Adobe Commerce en la infraestructura de la nube. Básicamente, el repositorio de código se convierte en un reflejo del repositorio de GitHub.

{{private-repository}}

Esta integración le permite:

- Crear un entorno al crear una rama
- Volver a implementar el entorno al combinar una solicitud de extracción
- Elimine el entorno cuando elimine la rama

Debe obtener un token de GitHub y un webhook para continuar con el proceso.

## Requisitos previos

- Acceso de administrador al proyecto de Adobe Commerce en la nube
- Repositorio de GitHub
- Token de acceso personal de GitHub

## Generar un token de GitHub

Cree un token de acceso personal clásico en la configuración de desarrollador de GitHub. Debe ser miembro de un grupo con acceso de escritura al repositorio de GitHub, para poder _push_ al repositorio. Incluya los siguientes ámbitos al crear el token:

- `admin:repo_hook`: permite crear enlaces web
- `repo`: integración con el repositorio
- `read:org`: integración con el repositorio de la organización.

Consulte [GitHub: Create](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## Preparación del repositorio

Clone su proyecto de Adobe Commerce en la nube desde un entorno existente y migre las ramas del proyecto a un nuevo repositorio de GitHub vacío, preservando los mismos nombres de rama. Lo es **crítico** para conservar un árbol Git idéntico, de modo que no se pierda ningún entorno o rama existente en su proyecto de Adobe Commerce en la nube.

1. Desde el terminal, inicie sesión en su proyecto de infraestructura de Adobe Commerce en la nube.

   ```bash
   magento-cloud login
   ```

1. Enumere sus proyectos y copie el ID del proyecto.

   ```bash
   magento-cloud project:list
   ```

1. Clone el proyecto en el entorno local.

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. Añada su repositorio de GitHub como remoto.

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   El nombre predeterminado de la conexión remota puede ser `origin` o `magento`. If `origin` existe, puede elegir un nombre diferente o puede cambiar el nombre de la referencia existente o eliminarla. Consulte [documentación remota de git](https://git-scm.com/docs/git-remote).

1. Compruebe que ha agregado correctamente el control remoto de GitHub.

   ```bash
   git remote -v
   ```

   Respuesta esperada:

   ```terminal
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. Inserte los archivos del proyecto en el nuevo repositorio de GitHub. Recuerde mantener todos los nombres de rama iguales.

   ```bash
   git push -u origin master
   ```

   Si está empezando con un nuevo repositorio de GitHub, es posible que tenga que utilizar el `-f` porque el repositorio remoto no coincide con su copia local.

1. Compruebe que el repositorio de GitHub contiene todos los archivos de proyecto.

## Habilitar la integración con GitHub

Antes de empezar, el código y los entornos del proyecto deben estar en el repositorio de GitHub. Después de habilitar la integración, el repositorio de GitHub se convierte en el origen del código. Si inserta cambios en el código original `magento` , la integración lo sobrescribe al insertar cambios de código en el repositorio de GitHub.

A continuación se habilita la integración de GitHub y se proporciona una URL de carga útil para utilizarla al crear un webhook.

>[!WARNING]
>
>El siguiente comando sobrescribe _todo_ en el proyecto de infraestructura en la nube de Adobe Commerce con código del repositorio de GitHub, que incluye todas las ramas, incluido el `production` Rama. Esta acción se produce de forma inmediata y no se puede deshacer. Como práctica recomendada, es importante clonar todas las ramas de su proyecto de infraestructura de Adobe Commerce en la nube e insertarlas en su repositorio de GitHub **antes** Añadir la integración de GitHub.

Puede elegir avanzar por las indicaciones de CLI utilizando `magento-cloud integration:add` o puede generar el comando integration con las siguientes opciones:

| Opción | ¿Requerido? | Descripción |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | Sí | La URL base de la instalación del servidor, que puede ser `https://github.com/` o una personalizada. Omita esta opción si el repositorio está alojado con Github público. |
| `--token` | Sí | El token de acceso personal que generó para GitHub |
| `--repository` | Sí | El nombre del repositorio: `owner-or-organisation/repository` |
| `--build-pull-requests` | Opcional | Indica a Adobe Commerce en la infraestructura de la nube que debe implementar después de combinar una solicitud de extracción (`true` de forma predeterminada) |
| `--fetch-branches` | Opcional | Hace que Adobe Commerce en la infraestructura de la nube rastree las ramas e implemente después de actualizar una rama (`true` de forma predeterminada) |
| `--prune-branches` | Opcional | Eliminar ramas que no existen en el sitio remoto (`true` de forma predeterminada) |

Hay muchas más opciones y puede verlas con la opción de ayuda:

```bash
magento-cloud integration:add --help
```

**Para habilitar la integración con GitHub**:

1. Habilite la integración.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **Ejemplo 1**: habilite la integración de GitHub para un repositorio personal y privado:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **Ejemplo 2**: habilite la integración de GitHub para un repositorio de la organización:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. Introduzca la información necesaria cuando se le solicite.

1. Copie el **URL de carga útil** se muestra en la salida de retorno.

   ```terminal
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## Añadir el webhook en GitHub

Para comunicar eventos, como una notificación push, con el servidor Git de Cloud, debe crear un webhook para el repositorio de GitHub:

1. En el repositorio de GitHub, haga clic en **Configuración** pestaña.

1. En la barra de navegación izquierda, haga clic en **Webhooks**.

1. En el _Webhooks_ , haga clic en **Añadir webhook**.

1. En el _Webhooks/Añadir webhook_ , edite los campos siguientes:

   - **URL de carga útil**: introduzca la URL devuelta al habilitar la integración de GitHub.
   - **Tipo de contenido**: elija **application/json** de la lista.
   - **Secreto**: introduzca un secreto de verificación.
   - **¿Qué eventos desea almacenar en déclencheur este webhook?**: Seleccionar **Enviarme todo**.
   - Seleccione el **Activo** casilla de verificación

1. Clic **Añadir webhook**.

## Prueba de la integración

Después de configurar la integración de GitHub, puede comprobar que la integración funciona con el complemento `magento-cloud` CLI:

```bash
magento-cloud integration:validate
```

O puede probarlo insertando un cambio simple en el repositorio de GitHub.

1. Cree un archivo de prueba.

   ```bash
   touch test.md
   ```

1. Confirme y envíe el cambio al repositorio de GitHub.

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. Inicie sesión en [[!DNL Cloud Console]](../project/overview.md) y compruebe que se muestra el mensaje de compromiso y que se implementa el proyecto.

## Eliminación de la integración

Puede quitar de forma segura la integración de GitHub del proyecto sin afectar al código.

**Para eliminar la integración con GitHub**:

1. Desde el terminal, inicie sesión en su proyecto de infraestructura de Adobe Commerce en la nube.

1. Enumere las integraciones. Necesita el ID de integración de GitHub para completar el siguiente paso.

   ```bash
   magento-cloud integration:list
   ```

1. Elimine la integración.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Además, puede eliminar la integración de GitHub iniciando sesión en su cuenta de GitHub y eliminando el enlace web en la _Webhooks_ pestaña del repositorio _Configuración_.
