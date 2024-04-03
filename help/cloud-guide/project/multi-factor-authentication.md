---
title: Habilitar la autenticación multifactor para el acceso SSH
description: Obtenga información sobre cómo administrar los requisitos de autenticación para el acceso SSH a Adobe Commerce en entornos de infraestructura en la nube.
feature: Cloud, Security
topic: Security
exl-id: 754b2c22-f197-49be-a699-fb3bedf053fc
source-git-commit: ec1e59c3aafae6452ad1590fdb9de37c68b94ed9
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Habilitar la autenticación multifactor para el acceso SSH

Para mayor seguridad, Adobe Commerce en la infraestructura en la nube proporciona aplicación de autenticación de varios factores (MFA) para administrar los requisitos de autenticación para el acceso SSH a los entornos en la nube.

Cuando MFA está habilitado en un proyecto, todas las cuentas de usuario con acceso SSH requieren un código de autenticación de doble factor (TFA) o un token de API y un certificado SSH para acceder al entorno.

>[!NOTE]
>
>MFA no está habilitado en los proyectos de Cloud de manera predeterminada. El propietario de la cuenta para el proyecto de Adobe Commerce en la nube debe [Enviar un ticket de asistencia de Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para habilitarlo. Cuando MFA está habilitado, todos los usuarios deben tener habilitada la autenticación de doble factor (TFA) en su cuenta de Adobe Commerce en la infraestructura de la nube para el acceso SSH a los entornos del proyecto.

## Certificados para acceso SSH

El MFA permite a los usuarios intercambiar un token de acceso de OAUTH con un certificado SSH de corta duración generado por la API de certificador de Adobe Cloud. Si el usuario tiene la función Administrador o Colaborador, una clave SSH válida y un código TFA o token de API válido, Adobe Commerce en la infraestructura en la nube utiliza estas credenciales para generar el certificado SSH temporal. La caducidad del certificado se establece en una hora, pero se actualiza automáticamente durante la sesión actual.

Después de iniciar sesión en un proyecto con MFA, los usuarios deben usar el `magento-cloud` CLI para generar el certificado SSH:

```bash
magento-cloud ssh-cert:load
```

El `ssh-cert:load` genera el certificado SSH y lo instala en el agente SSH del usuario local.

### Generar automáticamente un certificado al iniciar sesión

Puede configurar su entorno local para que genere el certificado SSH automáticamente al autenticarse en `magento-cloud` CLI.

**Para añadir la generación automática de certificados SSH a su `magento-cloud` Configuración de CLI**:

1. En la estación de trabajo local, cree un archivo llamado `config.yaml` en el `.magento-cloud` en su directorio personal si no existe.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Añada la siguiente configuración a `config.yaml` archivo.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Utilice el `magento-cloud` CLI para volver a autenticar:

   >Cerrar sesión:

   ```bash
   magento-cloud logout
   ```

   >Iniciar sesión:

   ```bash
   magento-cloud login
   ```

   >Siga la respuesta:

   ```terminal
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Conexión a un entorno mediante SSH con TFA

Cuando MFA está habilitado en un proyecto, debe tener TFA habilitado en su cuenta para poder conectarse a un entorno remoto mediante un SSH. Consulte [Habilitar TFA](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Requisitos previos:**

Para los proyectos habilitados con aplicación MFA, el acceso SSH requiere los siguientes permisos y configuración de cuenta:

- [Acceso de administrador o colaborador al entorno](user-access.md)
- [Clave de acceso SSH configurada en la cuenta](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA activado a cuenta](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**Para conectarse mediante SSH con credenciales de cuenta de usuario de TFA**:

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com).

1. En la estación de trabajo local, utilice el `magento-cloud` CLI para generar el certificado SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Respuesta de ejemplo:

   ```terminal
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Utilice un SSH para conectarse al entorno remoto.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```terminal
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Administración del código fuente mediante SSH con TFA

Al administrar el código fuente de Adobe Commerce en proyectos de infraestructura en la nube, se utiliza SSH para autenticarse en el repositorio Git del proyecto. Si el proyecto tiene habilitada la aplicación MFA, debe generar un certificado SSH para poder realizar operaciones de línea de comandos usando el repositorio Git.

**Para conectarse mediante SSH con credenciales de cuenta de usuario de TFA**:

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com) y autenticarse mediante TFA.

   >[!NOTE]
   >
   >Si no tiene TFA habilitado en su cuenta, debe habilitarlo. Consulte [Habilitar TFA en cuentas en la nube](user-access.md#enable-tfa-for-cloud-accounts).

1. En la estación de trabajo local, utilice el `magento-cloud` CLI para generar el certificado SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Respuesta de ejemplo:

   ```terminal
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Clone el repositorio Git para su entorno de proyecto:

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Respuesta de ejemplo:

   ```terminal
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Conexión a un entorno mediante SSH con un token de API

Cuando MFA está habilitado en un proyecto, los procesos automatizados que requieren acceso SSH a un entorno de nube requieren un token de API. Puede generar el token desde una cuenta de Adobe Commerce en la infraestructura de la nube con acceso de administrador o colaborador en el proyecto.

La autenticación con un token de API aún requiere la generación de un certificado SSH. Los procesos automatizados también deben automatizar la generación de un certificado SSH.

>[!BEGINSHADEBOX]

**Requisitos previos:**

- [Acceso de administrador o colaborador al entorno de Adobe Commerce en la nube](user-access.md)
- [Token de API válido disponible en la cuenta](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**Para conectarse mediante SSH con una credencial de token de API**:

1. Inicie sesión en el proyecto de Cloud mediante la autenticación de clave de API.

   ```bash
   magento-cloud auth:api-token
   ```

1. Cuando se le solicite, introduzca el valor de un token de API válido.

   ```terminal
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Ejemplo: script SSH automatizado

Existen dos opciones para almacenar el token de API.

>[!NOTE]
>
>Si se almacena un token de API, la variable `magento-cloud` CLI se autentica automáticamente y no es necesario realizar la `magento-cloud login` comando.

**Opción 1**: cree una variable de entorno para almacenar el token de API

Escriba el token en su bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Opción 2**: Añada el token a `config.yaml` archivo

1. En la estación de trabajo local, cree un archivo llamado `config.yaml` en el `.magento-cloud` en su directorio personal si no existe.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Añada la siguiente configuración a `config.yaml` archivo.

   ```yaml
   api:
      token: <your api token>
   ```

>Ejemplo de script bash

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Resolución de problemas

Utilice la siguiente información para resolver errores de solicitudes de conexión SSH debido a errores de autenticación como `access requires MFA` o `permission denied`.

### La solicitud no proporciona un certificado válido

Si la solicitud no proporciona un certificado válido, aparecerá un mensaje similar al siguiente:

```terminal
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Intente los siguientes procedimientos de solución de problemas para resolver el problema de conexión:

- Verifique la configuración de TFA de la cuenta
- Vuelva a autenticarse y vuelva a cargar el certificado

**Para verificar la configuración y autenticación de TFA**:

1. Iniciar sesión en [su cuenta](https://console.adobecommerce.com).

1. En el menú superior derecho de la cuenta, haga clic en **[!UICONTROL My Profile]**.

1. En el _Mi perfil_ , haga clic en **[!UICONTROL Security]** pestaña.

   Si TFA está habilitado, la sección Seguridad proporciona opciones para administrar la configuración de TFA.

1. Si TFA no está configurado, haga clic en **[!UICONTROL Set up application]** y siga las instrucciones para habilitarlo. Consulte [Habilitar TFA](user-access.md#enable-tfa-for-cloud-accounts).

1. Si TFA está configurado, intente autenticarse de nuevo.

**Para autenticar y volver a cargar el certificado SSH**:

1. Utilice el `magento-cloud` CLI para volver a autenticar:

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Vuelva a cargar el certificado SSH:

   ```bash
   magento-cloud ssh-cert:load
   ```

### Permiso denegado

Si falta la clave SSH o esta no es válida, la solicitud de conexión SSH devuelve un `Permission denied (publickey)` error.

```terminal
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Para solucionar el problema, añada la clave SSH a la sesión actual o actualice el archivo de configuración SSH para cargar las claves SSH automáticamente. Consulte [Añadir una clave SSH pública](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### No se puede acceder a los proyectos sin MFA

Si se autentica en un proyecto con la autenticación multifactor (MFA) habilitada, podría recibir el siguiente error al conectarse a otros proyectos que no requieran MFA:

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Respuesta de ejemplo:

```terminal
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Durante la generación del certificado SSH, la variable `magento-cloud` CLI añade una clave SSH adicional a su entorno local. Esa clave se utiliza de forma predeterminada si la configuración SSH local no incluye la clave SSH para el acceso al proyecto.

**Para añadir la clave SSH a la configuración local**:

1. Cree el `config` archivo si no existe.

   ```bash
   touch ~/.ssh/config
   ```

1. Añadir un `IdentityFile` configuración.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>Puede especificar varias claves SSH añadiendo varias `IdentityFile` Entradas de a la configuración.
