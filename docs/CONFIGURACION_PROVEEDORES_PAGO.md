# Configuración de Proveedores de Pago

Este documento explica cómo configurar cada proveedor de pago disponible en el sistema.

## Proveedores Disponibles

| Proveedor | Estado | Disponibilidad |
|-----------|--------|----------------|
| **PayPal** | ✅ Disponible | Funcional y listo para producción |
| **Stripe** | ✅ Disponible | Funcional y listo para producción |

## Índice

- [PayPal](#paypal)
- [Stripe](#stripe)

---

## PayPal

### Credenciales Necesarias

Para configurar PayPal necesitas las siguientes credenciales:

- **Client ID**: Identificador público de tu aplicación PayPal
- **Client Secret**: Clave secreta de tu aplicación PayPal
- **Mode**: Modo de operación (`sandbox` o `live`)

### Cómo Obtener las Credenciales

1. Accede a [PayPal Developer](https://developer.paypal.com/)
2. Inicia sesión con tu cuenta PayPal (puede ser tu cuenta personal real)
3. Ve a **Dashboard** > **My Apps & Credentials**
4. Crea una nueva aplicación o selecciona una existente
5. Copia el **Client ID** y **Client Secret**
   - Para pruebas: usa las credenciales de **Sandbox**
   - Para producción: usa las credenciales de **Live**

### Cómo Crear una Cuenta de Prueba (Sandbox)

**⚠️ IMPORTANTE**: Para usar PayPal Sandbox, necesitas crear una cuenta de prueba. **NO puedes usar tu cuenta personal real de PayPal** en el modo Sandbox.

#### Pasos para crear una cuenta de prueba:

1. **Accede al Dashboard de PayPal Developer**
   - Ve a: https://developer.paypal.com/
   - Inicia sesión con tu cuenta PayPal (esta sí puede ser tu cuenta real)

2. **Ve a la sección de Accounts (Cuentas)**
   - En el menú lateral izquierdo, busca y haz clic en **"Accounts"** o **"Cuentas"**
   - O ve directamente a: https://developer.paypal.com/dashboard/accounts
   - Asegúrate de estar en la pestaña **"Sandbox"** (no "Live")

3. **Crea una nueva cuenta de prueba**
   - Haz clic en el botón **"Create Account"** o **"Crear Cuenta"**
   - Selecciona el tipo de cuenta:
     - **Personal**: Para simular un comprador (recomendado para pruebas)
     - **Business**: Para simular un vendedor
   - Completa el formulario:
     - **Email**: Usa un email de prueba (ej: `comprador@prueba.com` o `buyer@test.com`)
     - **Password**: Crea una contraseña de prueba (guárdala, la necesitarás)
     - **Country**: Selecciona tu país
   - Haz clic en **"Create Account"**

4. **Usa la cuenta de prueba**
   - Cuando vayas a pagar en tu aplicación, usa el **email y contraseña** de la cuenta de prueba que acabas de crear
   - **NO uses tu cuenta personal real** de PayPal

#### Información adicional sobre cuentas de prueba:

- Las cuentas de prueba de Sandbox son **completamente independientes** de tu cuenta real de PayPal
- Puedes crear **múltiples cuentas de prueba** (Personal y Business)
- Las transacciones en Sandbox **NO son reales** (no se cobra dinero real)
- Las cuentas de prueba tienen fondos ilimitados para pruebas
- Puedes ver las transacciones de prueba en el dashboard de PayPal Developer

### Configuración en el Sistema

Configura los siguientes settings en la base de datos:

| Key | Descripción | Ejemplo |
|-----|-------------|---------|
| `paypalClientId` | Client ID de PayPal | `AeA1QIZXiflr1_-...` |
| `paypalClientSecret` | Client Secret de PayPal | `ECYYWWtqg4...` |
| `paypalMode` | Modo: `sandbox` o `live` | `sandbox` |
| `paypalWebhookId` | Webhook ID de PayPal (para verificación) | `WH-...` |

### Configuración de Webhooks

1. En el Dashboard de PayPal Developer, ve a **Webhooks**
2. Crea un nuevo webhook con la siguiente URL:
   ```
   https://tu-dominio.com/webhooks/paypal
   ```
   **Nota**: Esta ruta está definida en el código (`backend/src/routes/paymentRoutes.ts`). Si necesitas cambiarla, modifica el archivo de rutas.
3. Selecciona los siguientes eventos:
   - `PAYMENT.CAPTURE.COMPLETED`
   - `CHECKOUT.ORDER.APPROVED`
4. Copia el **Webhook ID** y guárdalo en el setting `paypalWebhookId` (recomendado para verificación de origen)

### URL del Webhook

```
POST /webhooks/paypal
```

### Verificación de Origen

El sistema verifica automáticamente que los webhooks provengan de PayPal mediante:
- Verificación de headers de PayPal (`paypal-auth-algo`, `paypal-cert-url`, `paypal-transmission-id`, etc.)
- Verificación del Webhook ID si está configurado en `paypalWebhookId`

**Importante**: Si la verificación falla, el webhook será rechazado con un error 401.

### Cómo Verificar si PayPal Está Enviando Webhooks

Tu aplicación tiene un **Webhook Monitor** integrado que te permite ver en tiempo real todos los webhooks que recibe. Aquí te explicamos cómo usarlo:

#### 1. Usar el Webhook Monitor (Recomendado)

1. **Accede al Webhook Monitor** en tu aplicación:
   - Ve a la página "Webhook Monitor" o "Monitor de Webhooks" en el menú
   
2. **Inicia el monitoreo**:
   - Haz clic en el botón **"Iniciar Monitoreo"** (botón Play ▶️)
   - Verás un indicador que dice "Monitoreando" cuando esté activo

3. **Filtra por PayPal**:
   - En el filtro de "Proveedor", selecciona **"PayPal"**
   - Esto mostrará solo los webhooks de PayPal

4. **Observa los webhooks en tiempo real**:
   - Cuando PayPal envíe un webhook, aparecerá automáticamente en la lista
   - Cada webhook muestra:
     - **Estado**: `processed` (procesado), `failed` (fallido), `rejected` (rechazado)
     - **URL**: La ruta del webhook (`/webhooks/paypal`)
     - **IP**: La dirección IP de PayPal
     - **Body**: El contenido completo del webhook (JSON)
     - **Tiempo de procesamiento**: En milisegundos
     - **Errores**: Si hubo algún problema

#### 2. Verificar en el Dashboard de PayPal Developer

1. Ve a [PayPal Developer Dashboard](https://developer.paypal.com/dashboard/)
2. Inicia sesión con tu cuenta PayPal
3. Ve a **Webhooks** en el menú lateral
4. Selecciona tu webhook configurado
5. Revisa la sección **"Event notifications"** o **"Notificaciones de eventos"**
6. Verás:
   - Historial de eventos enviados
   - Respuestas del servidor (códigos HTTP 200, 401, 500, etc.)
   - Detalles de cada notificación

#### 3. Revisar los Logs del Servidor

Revisa la consola del backend. Cuando PayPal envía un webhook, verás mensajes como:
- `PayPal Webhook Error: ...` (si hay un error)
- `PayPal webhook verification failed` (si falla la verificación)
- Los webhooks exitosos también se registran en la base de datos

#### 4. Verificar la URL del Webhook

Asegúrate de que la URL del webhook en PayPal sea:
```
https://tu-dominio.com/webhooks/paypal
```

**Nota**: Reemplaza `tu-dominio.com` con tu dominio real. Si estás en desarrollo local, necesitarás usar un túnel como ngrok para que PayPal pueda alcanzar tu servidor.

##### Usar ngrok para Desarrollo Local

Si estás desarrollando en local (`localhost`), PayPal no puede alcanzar tu servidor directamente. Necesitas usar un túnel como **ngrok**:

1. **Instala ngrok**:
   ```bash
   # En macOS con Homebrew
   brew install ngrok
   
   # O descarga desde https://ngrok.com/download
   ```

2. **Inicia ngrok** apuntando a tu puerto del backend:
   ```bash
   ngrok http 8080
   # O el puerto que uses para tu backend (ej: 3000, 5000, etc.)
   ```

3. **Copia la URL HTTPS** que ngrok te da (ej: `https://abc123.ngrok.io`)

4. **Configura el webhook en PayPal** con la URL:
   ```
   https://abc123.ngrok.io/webhooks/paypal
   ```

5. **Importante**: Cada vez que reinicies ngrok, obtendrás una nueva URL. Tendrás que actualizar la URL del webhook en PayPal.

**Alternativa**: Si tienes ngrok con cuenta, puedes usar un dominio fijo:
```bash
ngrok http 8080 --domain=tu-dominio-fijo.ngrok.io
```

#### 5. Eventos que PayPal Envía

PayPal envía webhooks para los siguientes eventos (configurados en el dashboard):
- `PAYMENT.CAPTURE.COMPLETED`: Cuando un pago se completa exitosamente
- `CHECKOUT.ORDER.APPROVED`: Cuando una orden es aprobada

#### Solución de Problemas

##### Problema: El pago se completa pero no llega el webhook

**Pasos para diagnosticar:**

1. **Verifica en el Dashboard de PayPal Developer**:
   - Ve a [PayPal Developer Dashboard](https://developer.paypal.com/dashboard/)
   - Inicia sesión
   - Ve a **Webhooks** en el menú lateral
   - Selecciona tu webhook configurado
   - Revisa la sección **"Event notifications"** o **"Notificaciones de eventos"**
   - Busca intentos recientes de envío de webhooks
   - Verifica el código de respuesta HTTP:
     - **200**: El webhook se recibió correctamente
     - **401**: Error de verificación (revisa el `paypalWebhookId`)
     - **500**: Error en el servidor (revisa los logs)
     - **Sin intentos**: PayPal no está intentando enviar el webhook

2. **Si no tienes webhook configurado**:
   - Haz clic en **"Create Webhook"** o **"Crear Webhook"**
   - URL: `https://tu-dominio.com/webhooks/paypal`
   - Eventos a suscribir:
     - ✅ `PAYMENT.CAPTURE.COMPLETED`
     - ✅ `CHECKOUT.ORDER.APPROVED`
   - Guarda el **Webhook ID** (comienza con `WH-`) y guárdalo en el setting `paypalWebhookId`

3. **Si estás en desarrollo local**:
   - PayPal **NO puede alcanzar** `localhost` o `127.0.0.1`
   - Necesitas usar un túnel como **ngrok**:
     ```bash
     # Instala ngrok si no lo tienes
     # macOS: brew install ngrok
     # O descarga desde https://ngrok.com/download
     
     # Inicia ngrok apuntando a tu puerto del backend
     ngrok http 8080  # O el puerto que uses
     
     # Copia la URL HTTPS que te da (ej: https://abc123.ngrok.io)
     # Configura el webhook en PayPal con: https://abc123.ngrok.io/webhooks/paypal
     ```
   - **Importante**: Cada vez que reinicies ngrok, obtendrás una nueva URL. Actualiza la URL del webhook en PayPal.

4. **Verifica que la URL sea accesible**:
   - Prueba acceder a la URL del webhook desde tu navegador (debería dar un error 405 o similar, pero no un error de conexión)
   - Si no puedes acceder, PayPal tampoco podrá

5. **Revisa el Webhook Monitor**:
   - Ve al **Webhook Monitor** en tu aplicación
   - Inicia el monitoreo
   - Filtra por **PayPal**
   - Si ves webhooks con estado "rejected", revisa el `paypalWebhookId`

6. **Verifica los logs del servidor**:
   - Revisa la consola del backend
   - Busca mensajes como:
     - `PayPal webhook verification failed`
     - `PayPal Webhook Error: ...`
     - `PayPal webhook ID not configured`

**Si no ves webhooks en el monitor:**
1. Verifica que el webhook esté configurado correctamente en PayPal Developer
2. Verifica que la URL del webhook sea accesible desde internet
3. Revisa los logs del servidor para ver si hay errores
4. Verifica que el Webhook ID esté configurado correctamente en los settings
5. **Si estás en local, usa ngrok o similar**

**Si ves webhooks con estado "rejected":**
- Verifica que el `paypalWebhookId` esté configurado correctamente en los settings
- Revisa que la verificación del webhook esté funcionando correctamente
- El Webhook ID debe comenzar con `WH-` y coincidir exactamente con el del dashboard de PayPal

**Si ves webhooks con estado "failed":**
- Revisa el mensaje de error en el webhook
- Verifica los logs del servidor para más detalles
- Asegúrate de que el pago correspondiente exista en la base de datos
- Verifica que el `providerPaymentId` del pago coincida con el `orderId` del webhook

### Notas Importantes

- El modo `sandbox` es para pruebas y no procesa pagos reales
- Cambia a `live` solo cuando estés listo para producción
- Los webhooks son esenciales para actualizar el estado de los pagos automáticamente
- PayPal usa USD como moneda por defecto
- **Seguridad**: El sistema verifica automáticamente el origen de los webhooks para prevenir ataques

---

## Stripe

Esta guía te ayudará a configurar Stripe desde cero, incluyendo cómo obtener las credenciales, configurar los webhooks y agregar la URL del webhook en el dashboard de Stripe.

### Paso 1: Crear una Cuenta en Stripe

Si aún no tienes una cuenta:

1. Ve a [https://stripe.com/](https://stripe.com/)
2. Haz clic en **Sign in** o **Start now**
3. Crea tu cuenta o inicia sesión
4. Completa la verificación de tu cuenta (email, teléfono, etc.)

### Paso 2: Obtener las Credenciales de API

#### 2.1. Acceder al Dashboard de Stripe

1. Inicia sesión en [Stripe Dashboard](https://dashboard.stripe.com/)
2. Asegúrate de estar en el modo correcto:
   - **Test mode** (modo de prueba): Para desarrollo y pruebas. Las claves comienzan con `sk_test_` y `pk_test_`
   - **Live mode** (modo en vivo): Para producción. Las claves comienzan con `sk_live_` y `pk_live_`
   - Puedes cambiar entre modos usando el toggle en la parte superior izquierda del dashboard

#### 2.2. Obtener las API Keys

1. En el menú lateral izquierdo, haz clic en **Developers** (o ve directamente a [https://dashboard.stripe.com/apikeys](https://dashboard.stripe.com/apikeys))
2. En la sección **API keys**, verás dos claves:
   - **Publishable key** (comienza con `pk_test_` o `pk_live_`): Esta es la clave pública, opcional para el frontend
   - **Secret key** (comienza con `sk_test_` o `sk_live_`): Esta es la clave secreta, **REQUERIDA**
3. Haz clic en **Reveal test key** o **Reveal live key** para ver la clave secreta
4. **Copia ambas claves** y guárdalas de forma segura (las necesitarás más adelante)

**⚠️ Importante**: 
- La Secret Key solo se muestra una vez. Si la pierdes, deberás crear una nueva
- Nunca compartas tu Secret Key públicamente
- Usa claves de prueba (`sk_test_`) para desarrollo y claves de producción (`sk_live_`) solo cuando estés listo

### Paso 3: Configurar el Webhook en Stripe

Este es el paso más importante para recibir notificaciones cuando se completen los pagos. Los webhooks en Stripe se configuran directamente desde la sección de **Developers > Webhooks**, no desde Workflows.

#### 3.1. Acceder a la Sección de Webhooks

1. En el Dashboard de Stripe, ve a **Developers** (Desarrolladores) en el menú lateral izquierdo
2. Haz clic en **Webhooks** (o directamente a [https://dashboard.stripe.com/webhooks](https://dashboard.stripe.com/webhooks))
3. Verás una lista de endpoints de webhook existentes (si ya tienes alguno) o un mensaje indicando que no hay endpoints configurados

#### 3.2. Agregar un Nuevo Endpoint de Webhook

1. Haz clic en el botón **Add endpoint** o **+ Add endpoint** (o **Añadir endpoint** en español)
2. Se abrirá un formulario para configurar el nuevo webhook

#### 3.3. Configurar la URL del Webhook

En el campo **Endpoint URL**, ingresa la URL completa de tu webhook:

```
https://tu-dominio.com/webhooks/stripe
```

**Ejemplos de URLs válidas:**
- Producción: `https://miempresa.com/webhooks/stripe`
- Desarrollo local con ngrok: `https://abc123.ngrok.io/webhooks/stripe`
- Staging: `https://staging.miempresa.com/webhooks/stripe`

**⚠️ Importante**: 
- La URL debe ser accesible públicamente (Stripe no puede acceder a `localhost` o IPs privadas)
- Para desarrollo local, usa herramientas como [ngrok](https://ngrok.com/) o [Stripe CLI](https://stripe.com/docs/stripe-cli)
- La ruta `/webhooks/stripe` está definida en el código (`backend/src/routes/paymentRoutes.ts`). Si necesitas cambiarla, modifica el archivo de rutas primero

#### 3.4. Seleccionar los Eventos a Escuchar

En la sección **Events to send** (Eventos a enviar) o **Select events** (Seleccionar eventos):

1. Haz clic en **Select events** o **+ Select events** o **Seleccionar eventos**
2. Se abrirá un modal con una lista de eventos disponibles
3. Busca y selecciona el evento: **`checkout.session.completed`**
   - Puedes usar el buscador dentro del modal para encontrarlo más rápido
   - Este evento se dispara cuando un cliente completa exitosamente un pago en Stripe Checkout
4. Haz clic en **Add events** o **Agregar eventos** para confirmar la selección
5. Cierra el modal o haz clic en **Done** (Hecho)

**Eventos recomendados:**
- **`checkout.session.completed`**: **REQUERIDO** - Se dispara cuando un pago se completa exitosamente
- Opcionalmente puedes agregar `checkout.session.async_payment_succeeded` para pagos asíncronos

#### 3.5. Guardar el Endpoint

1. Revisa que la URL y los eventos estén correctos
2. Haz clic en **Add endpoint** o **Añadir endpoint** para crear el webhook
3. Stripe intentará enviar un evento de prueba a tu URL. Si tu servidor aún no está configurado, esto fallará, pero no te preocupes, puedes probarlo más tarde

#### 3.6. Obtener el Webhook Signing Secret

Después de crear el endpoint:

1. En la lista de webhooks, verás el endpoint que acabas de crear
2. Haz clic en el endpoint para ver sus detalles
3. En la página de detalles del webhook, busca la sección **Signing secret** (Secreto de firma)
4. Haz clic en **Reveal** o **Click to reveal** o **Revelar** para mostrar el secreto
5. **Copia el Signing secret** (comienza con `whsec_`). Este es el valor que necesitas guardar en `stripeWebhookSecret`

**⚠️ Importante**: 
- El Signing secret es diferente para cada endpoint
- Si creas un nuevo endpoint, obtendrás un nuevo Signing secret
- Este secreto se usa para verificar que los webhooks realmente provienen de Stripe
- **Guarda este secreto de forma segura**, ya que lo necesitarás para configurarlo en tu sistema

### Paso 4: Configurar las Credenciales en el Sistema

Ahora que tienes todas las credenciales, debes configurarlas en tu sistema.

#### 4.1. Configuración a través de la Interfaz de Administración

1. Accede a la página de **Settings** (Configuración) en tu aplicación
2. Busca la sección de **Stripe**
3. Completa los siguientes campos:

| Campo | Valor | Descripción |
|-------|-------|-------------|
| **Stripe Secret Key** | `sk_test_51...` o `sk_live_51...` | La Secret Key que copiaste en el Paso 2.2 |
| **Stripe Publishable Key** | `pk_test_51...` o `pk_live_51...` | La Publishable Key (opcional) |
| **Stripe Webhook Secret** | `whsec_...` | El Signing secret que copiaste en el Paso 3.6 |

4. Haz clic en **Guardar** o **Save**

#### 4.2. Configuración Directa en la Base de Datos

Si prefieres configurarlo directamente en la base de datos:

```sql
-- Configurar la Secret Key
INSERT INTO Settings (`key`, `value`, `createdAt`, `updatedAt`) 
VALUES ('stripeSecretKey', 'sk_test_51...', NOW(), NOW())
ON DUPLICATE KEY UPDATE `value` = 'sk_test_51...', `updatedAt` = NOW();

-- Configurar la Publishable Key (opcional)
INSERT INTO Settings (`key`, `value`, `createdAt`, `updatedAt`) 
VALUES ('stripePublishableKey', 'pk_test_51...', NOW(), NOW())
ON DUPLICATE KEY UPDATE `value` = 'pk_test_51...', `updatedAt` = NOW();

-- Configurar el Webhook Secret (altamente recomendado)
INSERT INTO Settings (`key`, `value`, `createdAt`, `updatedAt`) 
VALUES ('stripeWebhookSecret', 'whsec_...', NOW(), NOW())
ON DUPLICATE KEY UPDATE `value` = 'whsec_...', `updatedAt` = NOW();
```

### Paso 5: Verificar la Configuración

#### 5.1. Verificar que las Credenciales Estén Configuradas

1. Verifica que los tres settings estén configurados:
   - `stripeSecretKey` ✅
   - `stripeWebhookSecret` ✅ (recomendado)
   - `stripePublishableKey` (opcional)

#### 5.2. Probar el Webhook

Stripe proporciona herramientas para probar tus webhooks:

**Opción A: Usar el Dashboard de Stripe**
1. Ve a **Developers** > **Webhooks** en el Dashboard de Stripe
2. Haz clic en tu endpoint de webhook
3. Haz clic en **Send test webhook** (Enviar webhook de prueba)
4. Selecciona el evento `checkout.session.completed`
5. Haz clic en **Send test webhook**
6. Revisa los logs de tu servidor para verificar que recibiste el webhook
7. También puedes verificar en la pestaña **Events** (Eventos) del webhook para ver los eventos enviados

**Opción B: Usar Stripe CLI (Recomendado para desarrollo local)**

Stripe CLI es la mejor opción para probar webhooks en local porque:
- Genera automáticamente la firma correcta del webhook
- Permite usar eventos reales de Stripe
- No necesitas configurar un webhook en el dashboard para pruebas locales

**Instalación de Stripe CLI:**

```bash
# macOS
brew install stripe/stripe-cli/stripe

# Linux (Ubuntu/Debian)
# Descargar desde: https://github.com/stripe/stripe-cli/releases
# O usar snap:
sudo snap install stripe

# Windows
# Descargar desde: https://github.com/stripe/stripe-cli/releases
```

**Configuración para desarrollo local:**

1. **Iniciar sesión en Stripe CLI:**
```bash
stripe login
```
   - Esto abrirá tu navegador para autenticarte con tu cuenta de Stripe
   - Acepta la autorización

2. **Obtener el webhook secret para desarrollo local:**
```bash
stripe listen --forward-to localhost:3000/webhooks/stripe
```
   - Este comando:
     - Escucha todos los eventos de Stripe
     - Los reenvía automáticamente a tu servidor local
     - Muestra un **webhook signing secret** que comienza con `whsec_`
   - **IMPORTANTE**: Copia este `whsec_...` y configúralo en tu base de datos como `stripeWebhookSecret` para desarrollo local
   - Deja este comando corriendo en una terminal mientras desarrollas

3. **En otra terminal, disparar un evento de prueba:**
```bash
# Disparar un evento checkout.session.completed de prueba
stripe trigger checkout.session.completed
```

4. **O crear un pago real de prueba:**
   - Crea un pago desde tu aplicación (usando claves de prueba `sk_test_...`)
   - Completa el pago en Stripe Checkout
   - El webhook se enviará automáticamente a tu servidor local

**Nota importante sobre el webhook secret:**
- El webhook secret que obtienes con `stripe listen` es diferente al de producción
- Úsalo solo para desarrollo local
- Para producción, usa el webhook secret del endpoint configurado en el dashboard de Stripe

#### 5.3. Crear un Pago de Prueba

1. Crea un pago de prueba desde tu aplicación
2. Completa el pago en Stripe Checkout
3. Verifica que:
   - El pago se crea correctamente
   - El webhook se recibe (revisa los logs)
   - El estado del pago se actualiza automáticamente a "completed"

### Resumen de Configuración

| Setting | Dónde Obtenerlo | Requerido |
|---------|----------------|-----------|
| `stripeSecretKey` | Dashboard > Developers > API keys > Secret key | ✅ Sí |
| `stripePublishableKey` | Dashboard > Developers > API keys > Publishable key | ❌ No (opcional) |
| `stripeWebhookSecret` | Dashboard > Developers > Webhooks > [Tu endpoint] > Signing secret | ⚠️ Recomendado |

### URL del Webhook

La URL del webhook que debes configurar en Stripe es:

```
POST https://tu-dominio.com/webhooks/stripe
```

Esta ruta está definida en `backend/src/routes/paymentRoutes.ts`. Si necesitas cambiarla:
1. Modifica el archivo de rutas
2. Actualiza la URL en el endpoint de webhook de Stripe

### Verificación de Origen

El sistema verifica automáticamente que los webhooks provengan de Stripe mediante:
- Verificación de firma criptográfica usando el header `stripe-signature`
- Comparación con el secreto configurado en `stripeWebhookSecret`

**Importante**: 
- Si `stripeWebhookSecret` no está configurado, el sistema continuará funcionando pero **sin verificación** (menos seguro)
- Si la verificación falla, el webhook será rechazado con un error 400
- **Se recomienda encarecidamente** configurar el `stripeWebhookSecret` en producción

### Notas Importantes

- **Monto mínimo**: Stripe requiere un monto mínimo de $0.50 USD
- **Moneda**: Stripe usa USD como moneda por defecto en la configuración actual
- **Modo de prueba vs Producción**:
  - Claves de prueba: `sk_test_` y `pk_test_` - No procesan pagos reales
  - Claves de producción: `sk_live_` y `pk_live_` - Procesan pagos reales
  - **Nunca uses claves de producción en desarrollo**
- **Seguridad**: El sistema verifica automáticamente la firma del webhook si está configurado el `stripeWebhookSecret`
- **Webhooks locales**: Para desarrollo local, usa [Stripe CLI](https://stripe.com/docs/stripe-cli) o [ngrok](https://ngrok.com/) para exponer tu servidor local

### Troubleshooting

#### El webhook no se recibe

1. **Verifica que el endpoint esté activo**:
   - Ve a **Developers** > **Webhooks** en el Dashboard de Stripe
   - Asegúrate de que el endpoint esté habilitado (no deshabilitado)
   - Verifica que el estado del endpoint sea "Enabled" o "Habilitado"

2. **Verifica que la URL sea accesible públicamente**:
   - Stripe no puede acceder a `localhost` o IPs privadas
   - Usa ngrok o Stripe CLI para desarrollo local

3. **Verifica que la ruta sea correcta**:
   - La ruta debe ser exactamente `/webhooks/stripe`
   - Verifica que no haya espacios o caracteres extra en la URL configurada en el endpoint

4. **Verifica los eventos configurados**:
   - Asegúrate de que el evento `checkout.session.completed` esté seleccionado en el endpoint
   - Ve a los detalles del webhook y verifica la lista de eventos

5. **Revisa los logs del servidor**:
   - Busca errores relacionados con Stripe
   - Verifica que el servidor esté recibiendo las peticiones POST

6. **Verifica el Webhook Secret**:
   - Asegúrate de que el `stripeWebhookSecret` en tu base de datos coincida con el Signing secret del endpoint en Stripe
   - Ve a **Developers** > **Webhooks** > [Tu endpoint] > **Signing secret** para verificar
   - Si creaste un nuevo endpoint, obtén el nuevo Signing secret

7. **Revisa los eventos en Stripe**:
   - En los detalles del webhook, ve a la pestaña **Events** (Eventos)
   - Verifica si Stripe está intentando enviar eventos y si hay errores
   - Los eventos fallidos mostrarán el código de error HTTP

#### Error: "Webhook signature verification failed"

- Verifica que el `stripeWebhookSecret` esté correctamente configurado
- Asegúrate de que el servidor esté recibiendo el body raw (sin parsear) para la verificación de firma
- Verifica que el endpoint en Stripe esté activo

#### Los pagos no se completan automáticamente

- Verifica que el endpoint de webhook esté configurado y **habilitado** en Stripe
- Asegúrate de que el evento `checkout.session.completed` esté seleccionado en el endpoint
- Verifica que la URL del webhook sea correcta y accesible
- Revisa los logs del webhook en **Developers** > **Webhooks** > [Tu endpoint] > **Events** para ver si hay errores
- Revisa los logs de tu servidor para ver si hay errores al procesar el evento
- Verifica que el `stripeWebhookSecret` esté correctamente configurado para la verificación de firma

---

## Configuración General

### Seguridad de Webhooks

**IMPORTANTE**: Todos los webhooks están protegidos con verificación de origen para garantizar que las solicitudes provengan realmente del proveedor de pago y no de fuentes maliciosas.

#### Verificación Implementada

- **PayPal**: Verificación mediante headers de PayPal y Webhook ID
- **Stripe**: Verificación mediante firma criptográfica

#### Configuración de Verificación

Cada proveedor requiere configurar un secreto o ID específico:

| Proveedor | Setting Requerido | Dónde Obtenerlo |
|-----------|-------------------|-----------------|
| PayPal | `paypalWebhookId` | Dashboard de PayPal Developer > Webhooks |
| Stripe | `stripeWebhookSecret` | Dashboard de Stripe > Webhooks > Signing secret |

**Nota**: Si no configuras estos secrets, el sistema continuará funcionando pero sin verificación de origen (menos seguro). Se recomienda encarecidamente configurarlos en producción.

### Rutas de Webhooks

Las rutas de webhook están definidas en el código en `backend/src/routes/paymentRoutes.ts`:

- PayPal: `/webhooks/paypal`
- Stripe: `/webhooks/stripe`

**Importante**: Si necesitas cambiar estas rutas, debes modificar el archivo `paymentRoutes.ts` y actualizar la configuración en el dashboard de cada proveedor de pago.

### Variables de Entorno

Asegúrate de configurar la siguiente variable de entorno:

```env
FRONTEND_URL=https://tu-dominio.com
```

Esta URL se usa para generar las URLs de retorno (`returnUrl` y `cancelUrl`) después de completar o cancelar un pago.

### Estructura de URLs de Retorno

El sistema genera automáticamente las siguientes URLs:

- **Success URL**: `${FRONTEND_URL}/pay/${externalId}/success`
- **Cancel URL**: `${FRONTEND_URL}/pay/${externalId}/cancel`
- **Payment Link**: `${FRONTEND_URL}/pay/${externalId}`

### Configuración de Settings

Los settings se almacenan en la tabla `Settings` de la base de datos. Puedes configurarlos:

1. **A través de la interfaz de administración** (si está disponible)
2. **Directamente en la base de datos**:
   ```sql
   UPDATE Settings SET value = 'tu-valor' WHERE key = 'nombre-del-setting';
   ```
3. **A través de la API de settings** (si está disponible)

### Verificación de Configuración

Para verificar que un proveedor está correctamente configurado:

1. Revisa que todos los settings requeridos estén configurados
2. Verifica que los valores no estén vacíos
3. Prueba crear un pago de prueba con el proveedor
4. Verifica que los webhooks estén funcionando correctamente

### Troubleshooting

#### El proveedor no aparece en la lista de opciones

- Verifica que todas las credenciales requeridas estén configuradas
- Revisa que los valores de los settings no estén vacíos
- Consulta los logs del servidor para ver errores específicos

#### Los webhooks no funcionan

- Verifica que la URL del webhook sea accesible públicamente
- Asegúrate de que el servidor pueda recibir peticiones POST
- Revisa los logs del servidor para ver los errores de webhook
- **Verificación de origen**: Si recibes errores 401, verifica que los secrets de webhook estén correctamente configurados:
  - PayPal: `paypalWebhookId`
  - Stripe: `stripeWebhookSecret`

#### Los pagos no se completan

- Verifica que las credenciales sean correctas
- Asegúrate de estar usando las credenciales del ambiente correcto (sandbox/live)
- Revisa que los webhooks estén configurados y funcionando
- Consulta los logs del proveedor de pago para más detalles

---

## Soporte

Para más información sobre cada proveedor:

- **PayPal**: [Documentación de PayPal](https://developer.paypal.com/docs/)
- **Stripe**: [Documentación de Stripe](https://stripe.com/docs)

