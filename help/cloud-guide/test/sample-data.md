---
title: Datos de muestra
description: Aprenda a instalar datos de ejemplo con Adobe Commerce en la infraestructura en la nube.
exl-id: 517e549f-15ba-47b3-9236-a1c4d24c917d
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Datos de muestra

Si necesita datos de ejemplo al desarrollar su tienda, puede instalar datos de ejemplo. Estos datos simulan una tienda Adobe Commerce activa con clientes, productos y otros datos. Estos datos de ejemplo funcionan mejor con una nueva instalación de Adobe Commerce en plantillas de infraestructura en la nube.

Como práctica recomendada, instale datos de ejemplo en entornos de desarrollo e integración. Si usa datos de ejemplo en Ensayo o Producción, debe [eliminar](#reset-or-uninstall-sample-data) la información y los productos antes de lanzarlos.

## Instalación de datos de ejemplo

Para instalar datos de ejemplo:

1. En la estación de trabajo local, cambie al directorio del proyecto.

1. En la raíz, introduzca el siguiente comando para agregar datos de ejemplo:

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. Espere a que se actualicen los componentes.

1. Confirme e inserte los cambios:

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Espere a que se implemente el proyecto.

1. Compruebe que la instalación se haya realizado correctamente desde la página de la tienda en el entorno de integración. Puede encontrar el vínculo de URL a la tienda a través de [!DNL Cloud Console].

1. Tome una instantánea de su entorno:

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

Puede comenzar a probar el desarrollo con datos en directo.

## Restablecer o desinstalar datos de ejemplo

Puede restablecer o quitar los datos de ejemplo siguiendo el mismo procedimiento utilizado para instalarlos:

- Eliminar datos de ejemplo: `./bin/magento sampledata:remove`
- Restablecer datos de ejemplo: `./bin/magento sampledata:reset`
