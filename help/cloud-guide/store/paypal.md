---
title: Configurar métodos de pago de PayPal
description: Configure los métodos de pago de PayPal para Adobe Commerce en la infraestructura en la nube.
feature: Cloud, Checkout, Payments
exl-id: e52fd719-f936-4e8b-8222-af133389d9e2
source-git-commit: aa1a334ca1383559194ca75247679c6fb5411802
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Configurar métodos de pago de PayPal

Adobe Commerce en la infraestructura en la nube proporciona una herramienta de incorporación para configurar las cuentas de Pago y envío de PayPal Express directamente a través del administrador. Esta herramienta está disponible para ECE 2.1.8 y versiones posteriores. Para facilitar la puesta en marcha y la prueba de los métodos de pago de PayPal, puedes activar y configurar tu cuenta PayPal Express Checkout para las cuentas de zona protegida o de producción.

Puede configurar la cuenta de zona protegida o de producción en cada entorno:

* Para los entornos de integración y ensayo, defina las credenciales de la zona protegida.
* Para el entorno de producción, establezca las credenciales de la zona protegida para la prueba inicial y reemplácelas por credenciales de producción en directo para una tienda iniciada.

## Cuenta PayPal

Aunque es mejor utilizar una cuenta comercial de PayPal preparada y configurada, puede crear una cuenta o actualizar una cuenta personal a través del administrador.

[!DNL PayPal onboarding] admite la conexión con las siguientes cuentas:

* Cuenta PayPal Business
* Cuenta personal de PayPal, convertir a una cuenta comercial. Si ya tienes una cuenta personal de PayPal, puedes iniciar sesión con esas credenciales y actualizar esta cuenta a una cuenta empresarial cuando completes la sincronización.

Si no tienes una cuenta PayPal, crea una. Escriba una dirección de correo electrónico para una cuenta nueva. Si no encuentra una cuenta PayPal coincidente, siga las indicaciones para crear una cuenta PayPal Business. O puede crear una cuenta directamente mediante [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### Limitaciones de PayPal

PayPal admite la conexión del Pago y envío de PayPal Express con países de todo el mundo, excepto por las siguientes limitaciones:

* India y Japón (las futuras actualizaciones de PayPal pueden admitir estas cuentas)
* Israel

Para Brasil, debes tener una cuenta comercial de PayPal para conectarte. No puedes convertir una cuenta personal de PayPal para Brasil durante este proceso. Si necesita una cuenta, [crear una cuenta PayPal empresarial](https://www.paypal.com/us/webapps/mpp/account-selection).

## Configurar Pago y envío de PayPal Express

Para configurar PayPal Express Checkout:

1. Acceda al administrador del entorno.
1. En la navegación del lado izquierdo, seleccione **Tiendas** > **Configuración**, luego seleccione **Ventas** > **Métodos de pago**.
1. En PayPal, seleccione **Configurar**. Los campos de configuración se muestran en secciones ampliables para las opciones Pago y envío exprés, Crédito de publicidad de PayPal y Básico y Avanzado.
1. Conecta tu cuenta PayPal. Hasta que la cuenta esté conectada, las opciones para habilitar están desactivadas. Para obtener más información sobre las cuentas disponibles y admitidas para conectarse y las limitaciones, consulte [Cuenta PayPal](#paypal-account).

   * Para conectar tu cuenta de PayPal Live, pulsa Conectar con PayPal y sigue las indicaciones. Cualquier cliente que compre con un PayPal activo completa y carga activamente a los clientes en una tienda en directo.
   * Para conectar la cuenta de zona protegida para realizar pruebas, haga clic en Credenciales de zona protegida y siga las indicaciones. Cualquier cliente que compre a través de una zona protegida con PayPal se completa sin cargar a los clientes de forma activa.

1. Configure los ajustes de Pago y envío exprés para autenticar y utilizar la API de PayPal:

   * **Correo electrónico asociado a la cuenta de PayPal Merchant** (Opcional) Introduce la dirección de correo electrónico asociada a tu cuenta comercial de PayPal. Este correo electrónico distingue entre mayúsculas y minúsculas.
   * **Métodos de autenticación de API** como una firma de API o un certificado de API.
   * Nombre de usuario, contraseña y firma de API capturados de su cuenta PayPal.
   * **Modo de zona protegida** seleccione Sí o No para indicar si las credenciales introducidas son para zona protegida. Si ha introducido credenciales de producción, seleccione No.
   * **La API utiliza proxy** seleccione Sí o No para establecer si el sistema utiliza un servidor proxy para establecer una conexión entre Adobe Commerce y el sistema de pago de PayPal. En caso afirmativo, introduzca el host y el puerto del proxy.

1. Para obtener información detallada y ver los pasos para configurar su cuenta, consulte [Pago y envío con PayPal Express](https://docs.magento.com/user-guide/payment/paypal-express-checkout.html) a partir del paso 2, complete la configuración necesaria.

Con la cuenta configurada y autenticada, puedes activar y desactivar las opciones de pago de PayPal en Configuración de PayPal requerida:

* **Habilitar esta solución** muestra el método de pago de PayPal a los clientes a través del sitio web.
* **Habilitar la experiencia de cierre de compra en contexto**
* **Activar crédito de PayPal** permite a los clientes obtener financiación mediante crédito PayPal sin costes adicionales. PayPal paga el pedido por adelantado y gestiona todos los reembolsos del crédito directamente con el cliente.

## Variables de PayPal

Cuando utilice la herramienta de incorporación de PayPal con Adobe Commerce en una infraestructura en la nube, añada la siguiente variable a `variables:env` de la sección `magento.app.yaml` archivo.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Si actualiza a la versión 2.2 desde la versión 2.1.8 o posterior, aún debe agregar esta variable.
