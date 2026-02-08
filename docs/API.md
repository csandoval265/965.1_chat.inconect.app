# 🔌 Documentación de API

Documentación completa de la API REST de la Plataforma de Gestión de WhatsApp Business.

## 📋 Tabla de Contenidos

- [Autenticación](#-autenticación)
- [Endpoints](#-endpoints)
  - [Autenticación](#autenticación)
  - [Conexiones WhatsApp](#conexiones-whatsapp)
  - [Sesiones de WhatsApp](#sesiones-de-whatsapp)
  - [Conexiones Telegram](#conexiones-telegram)
  - [Tickets](#tickets)
  - [Mensajes](#mensajes)
  - [Contactos](#contactos)
  - [Colas](#colas)
  - [Usuarios](#usuarios)
  - [Respuestas Rápidas](#respuestas-rápidas)
  - [Pagos](#pagos)
  - [Configuración](#configuración)
  - [Logs de Webhooks](#logs-de-webhooks)
  - [Respuestas Automáticas](#respuestas-automáticas-auto-replies)
  - [Horarios de Negocio](#horarios-de-negocio-business-hours)
  - [Configuración de Notificaciones](#configuración-de-notificaciones)
  - [Analytics](#analytics)
  - [Etiquetas](#etiquetas-tags)
  - [Campañas](#campañas)
  - [Mensajes Programados](#mensajes-programados)
  - [Plantillas de Mensajes](#plantillas-de-mensajes-message-templates)
  - [Configuración de Chatbot](#configuración-de-chatbot)
  - [Widgets](#widgets)
  - [Segmentos de Contactos](#segmentos-de-contactos)
  - [Contactos Duplicados](#contactos-duplicados)
  - [Listas de Difusión](#listas-de-difusión-broadcast-lists)
  - [API Externa](#api-externa-endpoints-de-api)
- [Códigos de Estado](#-códigos-de-estado)
- [Manejo de Errores](#-manejo-de-errores)
- [Ejemplos](#-ejemplos)
- [Webhooks](#-webhooks)

---

## 🔐 Autenticación

La API usa autenticación JWT (JSON Web Tokens). La mayoría de los endpoints requieren un token de autenticación.

### Obtener Token

```http
POST /auth/login
Content-Type: application/json

{
  "email": "usuario@ejemplo.com",
  "password": "contraseña"
}
```

**Respuesta exitosa (200):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "name": "Juan Pérez",
    "email": "usuario@ejemplo.com",
    "profile": "admin"
  }
}
```

### Usar el Token

Incluye el token en el header `Authorization`:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Refrescar Token

```http
POST /auth/refresh_token
Content-Type: application/json

{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Registrar Usuario

```http
POST /auth/signup
Content-Type: application/json

{
  "name": "Juan Pérez",
  "email": "usuario@ejemplo.com",
  "password": "contraseña_segura",
  "profile": "user"
}
```

### Cerrar Sesión

```http
DELETE /auth/logout
Authorization: Bearer {token}
```

---

## 📡 Endpoints

### Autenticación

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| POST | `/auth/login` | Iniciar sesión | No |
| POST | `/auth/signup` | Registrar usuario | No |
| POST | `/auth/refresh_token` | Refrescar token | No |
| DELETE | `/auth/logout` | Cerrar sesión | Sí |

### Conexiones WhatsApp

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/whatsapp` | Listar conexiones | Sí |
| POST | `/whatsapp` | Crear conexión | Sí |
| GET | `/whatsapp/:whatsappId` | Obtener conexión | Sí |
| PUT | `/whatsapp/:whatsappId` | Actualizar conexión | Sí |
| DELETE | `/whatsapp/:whatsappId` | Eliminar conexión | Sí |

### Sesiones de WhatsApp

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| POST | `/whatsappsession/:whatsappId` | Crear sesión de WhatsApp | Sí |
| PUT | `/whatsappsession/:whatsappId` | Actualizar sesión de WhatsApp | Sí |
| DELETE | `/whatsappsession/:whatsappId` | Eliminar sesión de WhatsApp | Sí |
| POST | `/whatsappsession/:whatsappId/request-pairing` | Solicitar código de emparejamiento | Sí |

**Crear sesión de WhatsApp:**
```http
POST /whatsappsession/:whatsappId
Authorization: Bearer {token}
```

**Actualizar sesión de WhatsApp (solicitar nuevo código QR):**
```http
PUT /whatsappsession/:whatsappId
Authorization: Bearer {token}
```

**Solicitar código de emparejamiento:**
```http
POST /whatsappsession/:whatsappId/request-pairing
Authorization: Bearer {token}
Content-Type: application/json

{
  "phoneNumber": "52998110259"
}
```

**Parámetros:**
- `whatsappId` (path): ID de la conexión de WhatsApp
- `phoneNumber` (body, requerido): Número de teléfono con código de país (sin +, solo dígitos)

**Ejemplo de respuesta exitosa (200):**
```json
{
  "message": "Pairing code requested successfully",
  "pairingCode": "ABC-DEF-GHI"
}
```

**Errores posibles:**
- `400`: Número de teléfono inválido o faltante
- `400`: La sesión ya está conectada (no se puede solicitar código cuando ya está emparejada)
- `400`: La sesión no está en estado QR
- `500`: Error al solicitar el código (sesión cerrada, método no disponible, etc.)

**Nota importante:** 
- Este endpoint debe llamarse **después** de iniciar la sesión con `POST /whatsappsession/:whatsappId`
- La sesión debe estar en estado **QR** (mostrando código QR), no conectada
- Si la sesión ya está conectada (`CONNECTED`), no se puede solicitar el código de emparejamiento
- El método `requestPairingCode` se llama directamente al cliente de WhatsApp cuando está en estado QR

### Conexiones Telegram

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/telegram/` | Listar conexiones de Telegram | Sí |
| POST | `/telegram/` | Crear conexión de Telegram | Sí |
| GET | `/telegram/:telegramId` | Obtener conexión de Telegram | Sí |
| PUT | `/telegram/:telegramId` | Actualizar conexión de Telegram | Sí |
| DELETE | `/telegram/:telegramId` | Eliminar conexión de Telegram | Sí |

**Listar conexiones de Telegram:**
```http
GET /telegram/?searchParam=bot&pageNumber=1&rowsPerPage=30
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `searchParam` (opcional): Búsqueda por nombre
- `pageNumber` (opcional): Número de página (default: "1")
- `rowsPerPage` (opcional): Resultados por página (default: "30")

**Respuesta:**
```json
{
  "telegrams": [
    {
      "id": 1,
      "name": "Bot Principal",
      "botToken": "123456789:ABCdefGHIjklMNOpqrsTUVwxyz",
      "status": "CONNECTED",
      "greetingMessage": "¡Hola! ¿En qué puedo ayudarte?",
      "farewellMessage": "¡Hasta luego!",
      "isDefault": true,
      "webhookUrl": "https://tu-servidor.com/webhook",
      "username": "mi_bot",
      "firstName": "Mi Bot",
      "lastMessageReceivedAt": "2024-01-15T10:30:00Z",
      "queues": [
        {
          "id": 1,
          "name": "Soporte"
        }
      ],
      "createdAt": "2024-01-01T00:00:00Z",
      "updatedAt": "2024-01-15T10:30:00Z"
    }
  ],
  "count": 1,
  "hasMore": false
}
```

**Crear conexión de Telegram:**
```http
POST /telegram/
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Bot Principal",
  "botToken": "123456789:ABCdefGHIjklMNOpqrsTUVwxyz",
  "status": "DISCONNECTED",
  "greetingMessage": "¡Hola! ¿En qué puedo ayudarte?",
  "farewellMessage": "¡Hasta luego!",
  "isDefault": true,
  "webhookUrl": "https://tu-servidor.com/webhook",
  "queueIds": [1, 2]
}
```

**Parámetros del body:**
- `name` (requerido): Nombre de la conexión
- `botToken` (requerido): Token del bot de Telegram (obtenido de @BotFather)
- `status` (opcional): Estado de la conexión (`DISCONNECTED`, `CONNECTED`, `PENDING`)
- `greetingMessage` (opcional): Mensaje de bienvenida
- `farewellMessage` (opcional): Mensaje de despedida
- `isDefault` (opcional): Si es la conexión por defecto (boolean)
- `webhookUrl` (opcional): URL del webhook para recibir eventos
- `queueIds` (opcional): Array de IDs de colas asociadas

**Obtener conexión de Telegram:**
```http
GET /telegram/:telegramId
Authorization: Bearer {token}
```

**Actualizar conexión de Telegram:**
```http
PUT /telegram/:telegramId
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Bot Actualizado",
  "status": "CONNECTED",
  "greetingMessage": "Nuevo mensaje de bienvenida",
  "webhookUrl": "https://nuevo-servidor.com/webhook",
  "queueIds": [1, 3]
}
```

**Notas importantes:**
- Al actualizar el `botToken`, la sesión de Telegram se reinicia automáticamente
- El campo `username` y `firstName` se actualizan automáticamente desde la API de Telegram
- El campo `lastMessageReceivedAt` se actualiza automáticamente cuando se recibe un mensaje
- Si se establece `isDefault: true`, la conexión anterior que tenía este flag se actualiza automáticamente

**Eliminar conexión de Telegram:**
```http
DELETE /telegram/:telegramId
Authorization: Bearer {token}
```

**Respuesta exitosa (200):**
```json
{
  "message": "Telegram deleted."
}
```

**Crear conexión:**
```http
POST /whatsapp
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Conexión Principal",
  "greetingMessage": "¡Hola! ¿En qué puedo ayudarte?",
  "farewellMessage": "¡Hasta luego!",
  "queueIds": [1, 2],
  "isDefault": true
}
```

### Tickets

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/tickets` | Listar tickets | Sí |
| GET | `/tickets/:ticketId` | Obtener ticket | Sí |
| GET | `/tickets/:ticketId/history` | Obtener historial del ticket | Sí |
| POST | `/tickets` | Crear ticket | Sí |
| PUT | `/tickets/:ticketId` | Actualizar ticket | Sí |
| DELETE | `/tickets/:ticketId` | Eliminar ticket | Sí |

**Listar tickets (con filtros):**
```http
GET /tickets?status=open&searchParam=cliente&pageNumber=1&limit=20
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `status`: `open`, `pending`, `closed`
- `searchParam`: Búsqueda por nombre/número
- `pageNumber`: Número de página
- `limit`: Resultados por página
- `showAll`: `true`/`false` (mostrar todos o solo asignados)
- `withUnreadMessages`: `true`/`false`
- `queueIds`: IDs de colas (JSON array)
- `tagIds`: IDs de etiquetas (JSON array o número único)
- `date`: Fecha para filtrar (formato ISO 8601)

**Actualizar ticket:**
```http
PUT /tickets/:id
Authorization: Bearer {token}
Content-Type: application/json

{
  "status": "closed",
  "userId": 2,
  "queueId": 1
}
```

### Mensajes

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/messages/:ticketId` | Obtener mensajes de un ticket | Sí |
| POST | `/messages/:ticketId` | Enviar mensaje | Sí |
| DELETE | `/messages/:messageId` | Eliminar mensaje | Sí |
| POST | `/messages/:ticketId/import` | Importar mensajes | Sí |
| POST | `/messages/:ticketId/import-group-contacts` | Importar contactos de grupo | Sí |
| POST | `/messages/:messageId/pin` | Fijar mensaje | Sí |
| POST | `/messages/:messageId/unpin` | Desfijar mensaje | Sí |

**Enviar mensaje:**
```http
POST /messages/:ticketId
Authorization: Bearer {token}
Content-Type: multipart/form-data

body: "Hola, ¿cómo estás?"
medias: [archivo1.jpg, archivo2.pdf] (opcional)
```

**Importar contactos de grupo:**
```http
POST /messages/:ticketId/import-group-contacts
Authorization: Bearer {token}
Content-Type: application/json

{
  "contacts": [
    {
      "number": "1234567890",
      "name": "Contacto 1"
    }
  ]
}
```

**Fijar mensaje:**
```http
POST /messages/:messageId/pin
Authorization: Bearer {token}
```

**Desfijar mensaje:**
```http
POST /messages/:messageId/unpin
Authorization: Bearer {token}
```

### Contactos

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/contacts` | Listar contactos | Sí |
| GET | `/contacts/:contactId` | Obtener contacto | Sí |
| POST | `/contacts` | Crear contacto | Sí |
| POST | `/contact` | Obtener contacto (alternativo) | Sí |
| PUT | `/contacts/:contactId` | Actualizar contacto | Sí |
| DELETE | `/contacts/:contactId` | Eliminar contacto | Sí |
| POST | `/contacts/import` | Importar contactos | Sí |
| GET | `/contacts/:contactId/groups/participants/export` | Exportar participantes de grupo | Sí |
| POST | `/contacts/:contactId/groups/participants/import` | Importar participantes de grupo | Sí |
| GET | `/contacts/:contactId/history` | Obtener historial completo del contacto | Sí |
| POST | `/contacts/:contactId/notes` | Crear nota del contacto | Sí |
| GET | `/contacts/:contactId/notes` | Listar notas del contacto | Sí |
| POST | `/contacts/:contactId/satisfaction` | Crear score de satisfacción | Sí |
| GET | `/contacts/:contactId/satisfaction` | Obtener score de satisfacción | Sí |

**Listar contactos:**
```http
GET /contacts?searchParam=nombre&pageNumber=1&rowsPerPage=30
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `searchParam` (opcional): Búsqueda por nombre o número
- `pageNumber` (opcional): Número de página (default: "1")
- `rowsPerPage` (opcional): Resultados por página (default: "30")

**Respuesta:**
```json
{
  "contacts": [
    {
      "id": 1,
      "name": "Juan Pérez",
      "number": "1234567890",
      "email": "juan@ejemplo.com",
      "tags": [
        {
          "id": 1,
          "name": "Cliente VIP",
          "color": "#FF5733"
        }
      ]
    }
  ],
  "count": 100,
  "hasMore": true
}
```

**Crear contacto:**
```http
POST /contacts
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Juan Pérez",
  "number": "1234567890",
  "email": "juan@ejemplo.com",
  "tagIds": [1, 2] (opcional)
}
```

**Campos:**
- `name` (requerido): Nombre del contacto
- `number` (requerido): Número de teléfono
- `email` (opcional): Email del contacto
- `tagIds` (opcional): Array de IDs de etiquetas a asociar

**Actualizar contacto:**
```http
PUT /contacts/:contactId
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Juan Pérez Actualizado",
  "number": "1234567890",
  "email": "nuevo@ejemplo.com",
  "tagIds": [1, 2, 3] (opcional)
}
```

**Exportar participantes de grupo:**
```http
GET /contacts/1/groups/participants/export
Authorization: Bearer {token}
```

**Importar participantes de grupo:**
```http
POST /contacts/1/groups/participants/import
Authorization: Bearer {token}
Content-Type: application/json

{
  "participants": [
    {
      "number": "1234567890",
      "name": "Participante 1"
    }
  ]
}
```

**Obtener historial completo del contacto:**
```http
GET /contacts/:contactId/history
Authorization: Bearer {token}
```

**Crear nota del contacto:**
```http
POST /contacts/:contactId/notes
Authorization: Bearer {token}
Content-Type: application/json

{
  "note": "Nota importante sobre el contacto"
}
```

**Listar notas del contacto:**
```http
GET /contacts/:contactId/notes
Authorization: Bearer {token}
```

**Crear score de satisfacción:**
```http
POST /contacts/:contactId/satisfaction
Authorization: Bearer {token}
Content-Type: application/json

{
  "score": 5,
  "comment": "Excelente servicio"
}
```

**Obtener score de satisfacción:**
```http
GET /contacts/:contactId/satisfaction
Authorization: Bearer {token}
```

### Segmentos de Contactos

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/contact-segments` | Listar segmentos de contactos | Sí |
| GET | `/contact-segments/:segmentId` | Obtener segmento | Sí |
| POST | `/contact-segments` | Crear segmento | Sí |
| PUT | `/contact-segments/:segmentId` | Actualizar segmento | Sí |
| POST | `/contact-segments/preview` | Vista previa del segmento | Sí |
| POST | `/contact-segments/:segmentId/refresh` | Refrescar segmento | Sí |

**Listar segmentos de contactos:**
```http
GET /contact-segments
Authorization: Bearer {token}
```

**Crear segmento:**
```http
POST /contact-segments
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Clientes VIP",
  "description": "Clientes con compras superiores a $1000",
  "conditions": {
    "operator": "AND",
    "rules": [
      {
        "field": "tags",
        "operator": "contains",
        "value": [1, 2]
      }
    ]
  }
}
```

**Vista previa del segmento:**
```http
POST /contact-segments/preview
Authorization: Bearer {token}
Content-Type: application/json

{
  "conditions": {
    "operator": "AND",
    "rules": [...]
  }
}
```

**Refrescar segmento:**
```http
POST /contact-segments/:segmentId/refresh
Authorization: Bearer {token}
```

### Contactos Duplicados

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/contacts/duplicates` | Buscar contactos duplicados | Sí |
| POST | `/contacts/merge` | Fusionar contactos duplicados | Sí |

**Buscar contactos duplicados:**
```http
GET /contacts/duplicates
Authorization: Bearer {token}
```

**Fusionar contactos duplicados:**
```http
POST /contacts/merge
Authorization: Bearer {token}
Content-Type: application/json

{
  "primaryContactId": 1,
  "duplicateContactIds": [2, 3]
}
```

### Listas de Difusión (Broadcast Lists)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/broadcast-list/` | Listar listas de difusión | Sí |
| POST | `/broadcast-list/` | Crear lista de difusión | Sí |
| GET | `/broadcast-list/:broadcastListId` | Obtener lista de difusión | Sí |
| PUT | `/broadcast-list/:broadcastListId` | Actualizar lista de difusión | Sí |
| DELETE | `/broadcast-list/:broadcastListId` | Eliminar lista de difusión | Sí |
| POST | `/broadcast-list/:broadcastListId/contacts` | Agregar contactos a la lista | Sí |
| DELETE | `/broadcast-list/:broadcastListId/contacts` | Eliminar contactos de la lista | Sí |

**Listar listas de difusión:**
```http
GET /broadcast-list/
Authorization: Bearer {token}
```

**Crear lista de difusión:**
```http
POST /broadcast-list/
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Lista de Clientes VIP",
  "description": "Lista para campañas especiales"
}
```

**Agregar contactos a la lista:**
```http
POST /broadcast-list/:broadcastListId/contacts
Authorization: Bearer {token}
Content-Type: application/json

{
  "contactIds": [1, 2, 3]
}
```

**Eliminar contactos de la lista:**
```http
DELETE /broadcast-list/:broadcastListId/contacts
Authorization: Bearer {token}
Content-Type: application/json

{
  "contactIds": [1, 2]
}
```

### Colas

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/queue` | Listar colas | Sí |
| GET | `/queue/:queueId` | Obtener cola | Sí |
| POST | `/queue` | Crear cola | Sí |
| PUT | `/queue/:queueId` | Actualizar cola | Sí |
| DELETE | `/queue/:queueId` | Eliminar cola | Sí |

**Crear cola:**
```http
POST /queue
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Soporte Técnico",
  "color": "#FF5733",
  "greetingMessage": "Bienvenido al soporte técnico"
}
```

### Usuarios

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/users` | Listar usuarios | Sí |
| GET | `/users/:id` | Obtener usuario | Sí |
| POST | `/users` | Crear usuario | Sí |
| PUT | `/users/:id` | Actualizar usuario | Sí |
| DELETE | `/users/:id` | Eliminar usuario | Sí |

### Respuestas Rápidas

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/quickAnswers` | Listar respuestas rápidas | Sí |
| GET | `/quickAnswers/:id` | Obtener respuesta rápida | Sí |
| POST | `/quickAnswers` | Crear respuesta rápida | Sí |
| PUT | `/quickAnswers/:id` | Actualizar respuesta rápida | Sí |
| DELETE | `/quickAnswers/:id` | Eliminar respuesta rápida | Sí |

**Crear respuesta rápida:**
```http
POST /quickAnswers
Authorization: Bearer {token}
Content-Type: application/json

{
  "shortcut": "saludo",
  "message": "¡Hola! ¿En qué puedo ayudarte hoy?"
}
```

### Pagos

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/payments` | Listar pagos | Sí |
| GET | `/payments/:paymentId` | Obtener pago | Sí |
| POST | `/payments` | Crear pago | Sí |
| PUT | `/payments/:paymentId` | Actualizar pago | Sí |
| GET | `/payments/stats` | Estadísticas de pagos | Sí |
| PUT | `/payments/:paymentId/status` | Actualizar estado del pago | Sí |
| DELETE | `/payments/:paymentId` | Eliminar pago | Sí |

**Crear pago:**
```http
POST /payments
Authorization: Bearer {token}
Content-Type: application/json

{
  "amount": 100.00,
  "description": "Pago de servicio",
  "provider": "stripe",
  "contactId": 1
}
```

**Actualizar pago:**
```http
PUT /payments/:paymentId
Authorization: Bearer {token}
Content-Type: application/json

{
  "amount": 150.00,
  "description": "Pago actualizado"
}
```

**Webhooks de pagos:**
- PayPal: `POST /webhooks/paypal`
- Stripe: `POST /webhooks/stripe`

**Proveedores de pago soportados:**
- **PayPal**: Pagos mediante enlaces de checkout
- **Stripe**: Pagos con tarjetas mediante sesiones de checkout

### Configuración

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/settings` | Listar configuraciones | Sí |
| PUT | `/settings/:key` | Actualizar configuración | Sí |

### Logs de Webhooks

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/webhook-logs` | Listar logs | Sí |
| GET | `/webhook-logs/:id` | Obtener log | Sí |
| GET | `/webhook-logs/stats` | Estadísticas | Sí |

### Respuestas Automáticas (Auto Replies)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/auto-replies` | Listar respuestas automáticas | Sí |
| POST | `/auto-replies` | Crear respuesta automática | Sí |
| PUT | `/auto-replies/:autoReplyId` | Actualizar respuesta automática | Sí |
| DELETE | `/auto-replies/:autoReplyId` | Eliminar respuesta automática | Sí |

**Listar respuestas automáticas:**
```http
GET /auto-replies?queueId=1
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `queueId`: ID de la cola (opcional) - Filtrar respuestas por cola

**Crear respuesta automática:**
```http
POST /auto-replies
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Saludo automático",
  "keywords": ["hola", "buenos días", "buenas tardes"],
  "message": "¡Hola! ¿En qué puedo ayudarte?",
  "isActive": true,
  "queueId": 1,
  "matchType": "contains"
}
```

**Campos:**
- `name` (requerido): Nombre de la respuesta automática
- `keywords` (requerido): Array de palabras clave o string JSON
- `message` (requerido): Mensaje a enviar cuando se detecten las palabras clave
- `isActive` (opcional): Si la respuesta está activa (default: true)
- `queueId` (opcional): ID de la cola asociada
- `matchType` (opcional): Tipo de coincidencia - `"exact"`, `"contains"`, `"starts_with"`, `"ends_with"` (default: "contains")

**Actualizar respuesta automática:**
```http
PUT /auto-replies/:autoReplyId
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Saludo actualizado",
  "keywords": ["hola", "hi"],
  "message": "¡Hola! ¿Cómo puedo ayudarte?",
  "isActive": false,
  "matchType": "exact"
}
```

### Horarios de Negocio (Business Hours)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/queues/:queueId/business-hours` | Listar horarios de una cola | Sí |
| POST | `/queues/:queueId/business-hours` | Crear horario de negocio | Sí |
| PUT | `/business-hours/:businessHoursId` | Actualizar horario de negocio | Sí |
| DELETE | `/business-hours/:businessHoursId` | Eliminar horario de negocio | Sí |
| GET | `/queues/:queueId/business-hours/check` | Verificar si está en horario | Sí |

**Listar horarios de negocio:**
```http
GET /queues/1/business-hours
Authorization: Bearer {token}
```

**Crear horario de negocio:**
```http
POST /queues/1/business-hours
Authorization: Bearer {token}
Content-Type: application/json

{
  "dayOfWeek": 1,
  "startTime": "09:00",
  "endTime": "18:00",
  "isOpen": true
}
```

**Campos:**
- `dayOfWeek` (requerido): Día de la semana (0 = Domingo, 1 = Lunes, ..., 6 = Sábado)
- `startTime` (requerido): Hora de inicio en formato HH:mm (ej: "09:00")
- `endTime` (requerido): Hora de fin en formato HH:mm (ej: "18:00")
- `isOpen` (opcional): Si el negocio está abierto ese día (default: true)

**Actualizar horario de negocio:**
```http
PUT /business-hours/1
Authorization: Bearer {token}
Content-Type: application/json

{
  "dayOfWeek": 1,
  "startTime": "08:00",
  "endTime": "17:00",
  "isOpen": true
}
```

**Verificar horario de negocio:**
```http
GET /queues/1/business-hours/check
Authorization: Bearer {token}
```

**Respuesta:**
```json
{
  "isOpen": true,
  "currentTime": "14:30",
  "nextOpenTime": "09:00",
  "nextCloseTime": "18:00"
}
```

### Configuración de Notificaciones

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/notification-settings` | Obtener configuración de notificaciones | Sí |
| PUT | `/notification-settings` | Actualizar configuración de notificaciones | Sí |

**Obtener configuración de notificaciones:**
```http
GET /notification-settings
Authorization: Bearer {token}
```

**Respuesta:**
```json
{
  "id": 1,
  "userId": 1,
  "emailEnabled": true,
  "pushEnabled": true,
  "ticketAssigned": true,
  "ticketTransferred": true,
  "ticketClosed": false,
  "newMessage": true,
  "slaWarning": true,
  "slaWarningMinutes": 30,
  "createdAt": "2024-01-01T00:00:00.000Z",
  "updatedAt": "2024-01-01T00:00:00.000Z"
}
```

**Actualizar configuración de notificaciones:**
```http
PUT /notification-settings
Authorization: Bearer {token}
Content-Type: application/json

{
  "emailEnabled": true,
  "pushEnabled": true,
  "ticketAssigned": true,
  "ticketTransferred": true,
  "ticketClosed": false,
  "newMessage": true,
  "slaWarning": true,
  "slaWarningMinutes": 30
}
```

**Campos (todos opcionales):**
- `emailEnabled`: Habilitar notificaciones por email
- `pushEnabled`: Habilitar notificaciones push
- `ticketAssigned`: Notificar cuando se asigne un ticket
- `ticketTransferred`: Notificar cuando se transfiera un ticket
- `ticketClosed`: Notificar cuando se cierre un ticket
- `newMessage`: Notificar cuando llegue un nuevo mensaje
- `slaWarning`: Habilitar alertas de SLA
- `slaWarningMinutes`: Minutos antes del SLA para alertar

### Analytics

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/analytics` | Obtener estadísticas y análisis | Sí |
| GET | `/analytics/export` | Exportar analytics | Sí |

**Obtener analytics:**
```http
GET /analytics
Authorization: Bearer {token}
```

**Exportar analytics:**
```http
GET /analytics/export
Authorization: Bearer {token}
```

**Parámetros de consulta (opcionales):**
- `startDate`: Fecha de inicio (formato ISO 8601)
- `endDate`: Fecha de fin (formato ISO 8601)
- `format`: Formato de exportación (`csv`, `xlsx`, `json`)

**Respuesta:** Archivo descargable con los datos de analytics en el formato especificado.

### Etiquetas (Tags)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/tags` | Listar etiquetas | Sí |
| POST | `/tags` | Crear etiqueta | Sí |
| PUT | `/tags/:tagId` | Actualizar etiqueta | Sí |
| DELETE | `/tags/:tagId` | Eliminar etiqueta | Sí |
| PUT | `/tags/tickets/` | Asociar etiquetas a múltiples tickets | Sí |
| PUT | `/tickets/:ticketId/tags` | Asociar etiquetas a ticket | Sí |
| PUT | `/contacts/:contactId/tags` | Asociar etiquetas a contacto | Sí |
| POST | `/tags/import` | Importar etiquetas | Sí |

**Asociar etiquetas a múltiples tickets:**
```http
PUT /tags/tickets/
Authorization: Bearer {token}
Content-Type: application/json

{
  "ticketIds": [1, 2, 3],
  "tagIds": [1, 2]
}
```

**Campos:**
- `ticketIds` (requerido): Array de IDs de tickets
- `tagIds` (requerido): Array de IDs de etiquetas

**Asociar etiquetas a ticket:**
```http
PUT /tickets/1/tags
Authorization: Bearer {token}
Content-Type: application/json

{
  "tagIds": [1, 2, 3]
}
```

**Asociar etiquetas a contacto:**
```http
PUT /contacts/1/tags
Authorization: Bearer {token}
Content-Type: application/json

{
  "tagIds": [1, 2]
}
```

### Pagos (Endpoints públicos)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/payment/:token` | Obtener pago por token | No |
| GET | `/payment/:token/provider-link` | Obtener link del proveedor | No |
| POST | `/payment/:token/clip-charge` | Procesar cargo Clip | No |
| POST | `/payment/:token/retry` | Reintentar pago | No |
| GET | `/payment/:token/success` | Manejar éxito de PayPal | No |
| POST | `/payments/webhooks/clip/test` | Probar webhook de Clip | Sí |

**Obtener pago por token (público):**
```http
GET /payment/abc123token
```

**Actualizar estado del pago:**
```http
PUT /payments/1/status
Authorization: Bearer {token}
Content-Type: application/json

{
  "status": "paid"
}
```

### Campañas

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/campaign/` | Listar campañas | Sí |
| POST | `/campaign/` | Crear campaña | Sí |
| GET | `/campaign/:campaignId` | Obtener campaña | Sí |
| GET | `/campaign/:campaignId/recipients` | Obtener destinatarios de campaña | Sí |
| PUT | `/campaign/:campaignId` | Actualizar campaña | Sí |
| DELETE | `/campaign/:campaignId` | Eliminar campaña | Sí |
| POST | `/campaign/:campaignId/process` | Procesar campaña | Sí |
| POST | `/campaign/:campaignId/update-metrics` | Actualizar métricas de campaña | Sí |
| POST | `/campaign/batch-update` | Actualizar campañas en lote | Sí |
| POST | `/campaign/batch-delete` | Eliminar campañas en lote | Sí |
| POST | `/campaign/batch-process` | Procesar campañas en lote | Sí |

**Listar campañas:**
```http
GET /campaign/?searchParam=nombre
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `searchParam`: Búsqueda por nombre (opcional)

**Crear campaña:**
```http
POST /campaign/
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Campaña de Navidad",
  "message": "¡Feliz Navidad! Mensaje personalizado: {{nombre}}",
  "whatsappId": 1,
  "scheduledAt": "2024-12-24T10:00:00Z",
  "dailyLimit": 100,
  "batchSize": 10,
  "pauseBetweenBatchesMin": 60,
  "pauseBetweenBatchesMax": 120,
  "delayBetweenMessagesMin": 5,
  "delayBetweenMessagesMax": 15,
  "useSmartPauses": true,
  "segmentType": "tags",
  "segmentData": {"tagIds": [1, 2]},
  "includeGroups": false,
  "variables": {"nombre": "Juan"},
  "notes": "Notas de la campaña",
  "mediaType": "image",
  "media": [archivo.jpg] (opcional),
  "mediaUrl": "ruta/al/archivo.jpg" (opcional)
}
```

**Campos:**
- `name` (requerido): Nombre de la campaña
- `message` (requerido): Mensaje a enviar (soporta variables con {{variable}})
- `whatsappId` (requerido): ID de la conexión WhatsApp
- `scheduledAt` (opcional): Fecha/hora programada (ISO 8601)
- `dailyLimit` (opcional): Límite diario de mensajes
- `batchSize` (opcional): Tamaño del lote de envío
- `pauseBetweenBatchesMin` (opcional): Pausa mínima entre lotes (segundos)
- `pauseBetweenBatchesMax` (opcional): Pausa máxima entre lotes (segundos)
- `delayBetweenMessagesMin` (opcional): Retraso mínimo entre mensajes (segundos)
- `delayBetweenMessagesMax` (opcional): Retraso máximo entre mensajes (segundos)
- `useSmartPauses` (opcional): Usar pausas inteligentes
- `segmentType` (opcional): Tipo de segmento (tags, all, custom)
- `segmentData` (opcional): Datos del segmento (JSON)
- `includeGroups` (opcional): Incluir grupos
- `variables` (opcional): Variables para personalización (JSON)
- `notes` (opcional): Notas adicionales
- `mediaType` (opcional): Tipo de medio (image, video, audio, document)
- `media` (opcional): Archivo multimedia a subir
- `mediaUrl` (opcional): URL o ruta de un archivo multimedia existente (útil cuando se crea desde una plantilla)

**Actualizar campaña:**
```http
PUT /campaign/:campaignId
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Campaña Actualizada",
  "dailyLimit": 200,
  "media": [nuevo-archivo.jpg] (opcional),
  "mediaUrl": "ruta/al/archivo.jpg" (opcional)
}
```

**Nota:** Puedes usar `media` para subir un nuevo archivo o `mediaUrl` para usar un archivo existente. Si envías `mediaUrl` como `null` o cadena vacía, se eliminará el archivo multimedia de la campaña.

**Procesar campaña:**
```http
POST /campaign/:campaignId/process
Authorization: Bearer {token}
```

**Actualizar métricas de campaña:**
```http
POST /campaign/:campaignId/update-metrics
Authorization: Bearer {token}
```

**Operaciones en lote:**
```http
POST /campaign/batch-update
Authorization: Bearer {token}
Content-Type: application/json

{
  "campaignIds": [1, 2, 3],
  "status": "paused"
}
```

**Campos:**
- `campaignIds` (requerido): Array de IDs de campañas
- `status` (requerido): Estado a aplicar - `"paused"` o `"running"`

```http
POST /campaign/batch-delete
Authorization: Bearer {token}
Content-Type: application/json

{
  "campaignIds": [1, 2, 3]
}
```

```http
POST /campaign/batch-process
Authorization: Bearer {token}
Content-Type: application/json

{
  "campaignIds": [1, 2, 3]
}
```

### Mensajes Programados

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/scheduled-messages` | Listar mensajes programados | Sí |
| GET | `/scheduled-messages/:scheduledMessageId` | Obtener mensaje programado | Sí |
| POST | `/scheduled-messages` | Crear mensaje programado | Sí |
| PUT | `/scheduled-messages/:scheduledMessageId` | Actualizar mensaje programado | Sí |
| DELETE | `/scheduled-messages/:scheduledMessageId` | Eliminar mensaje programado | Sí |
| POST | `/scheduled-messages/:scheduledMessageId/send-now` | Enviar mensaje ahora | Sí |
| POST | `/scheduled-messages/batch-send` | Enviar mensajes en lote | Sí |
| POST | `/scheduled-messages/batch-delete` | Eliminar mensajes en lote | Sí |
| POST | `/scheduled-messages/batch-cancel` | Cancelar mensajes en lote | Sí |

**Listar mensajes programados:**
```http
GET /scheduled-messages?ticketId=1&searchParam=texto
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `ticketId`: ID del ticket (opcional)
- `searchParam`: Búsqueda por texto (opcional)

**Crear mensaje programado:**
```http
POST /scheduled-messages
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "ticketId": 1,
  "body": "Mensaje programado",
  "scheduledFor": "2024-12-25T10:00:00Z",
  "quotedMsg": {...} (opcional),
  "signMessage": true,
  "medias": [archivo1.jpg, archivo2.pdf] (opcional)
}
```

**Campos:**
- `ticketId` (requerido): ID del ticket
- `body` (requerido): Contenido del mensaje
- `scheduledFor` (requerido): Fecha/hora programada (ISO 8601, debe ser futura)
- `quotedMsg` (opcional): Mensaje citado
- `signMessage` (opcional): Firmar mensaje (default: true)
- `medias` (opcional): Archivos multimedia

**Actualizar mensaje programado:**
```http
PUT /scheduled-messages/:scheduledMessageId
Authorization: Bearer {token}
Content-Type: application/json

{
  "body": "Mensaje actualizado",
  "scheduledFor": "2024-12-25T11:00:00Z"
}
```

**Enviar mensaje ahora:**
```http
POST /scheduled-messages/:scheduledMessageId/send-now
Authorization: Bearer {token}
```

**Operaciones en lote:**
```http
POST /scheduled-messages/batch-send
Authorization: Bearer {token}
Content-Type: application/json

{
  "scheduledMessageIds": [1, 2, 3]
}
```

```http
POST /scheduled-messages/batch-delete
Authorization: Bearer {token}
Content-Type: application/json

{
  "scheduledMessageIds": [1, 2, 3]
}
```

```http
POST /scheduled-messages/batch-cancel
Authorization: Bearer {token}
Content-Type: application/json

{
  "scheduledMessageIds": [1, 2, 3]
}
```

### Plantillas de Mensajes (Message Templates)

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/messageTemplates` | Listar plantillas de mensajes | Sí |
| GET | `/messageTemplates/:messageTemplateId` | Obtener plantilla de mensaje | Sí |
| POST | `/messageTemplates` | Crear plantilla de mensaje | Sí |
| PUT | `/messageTemplates/:messageTemplateId` | Actualizar plantilla de mensaje | Sí |
| DELETE | `/messageTemplates/:messageTemplateId` | Eliminar plantilla de mensaje | Sí |
| POST | `/messageTemplates/:messageTemplateId/submit-for-approval` | Enviar plantilla para aprobación | Sí |
| POST | `/messageTemplates/:messageTemplateId/approve` | Aprobar plantilla | Sí |
| POST | `/messageTemplates/:messageTemplateId/reject` | Rechazar plantilla | Sí |
| POST | `/messageTemplates/:messageTemplateId/render` | Renderizar plantilla con variables | Sí |

**Listar plantillas de mensajes:**
```http
GET /messageTemplates?searchParam=nombre&pageNumber=1&status=pending&category=marketing&createdById=1
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `searchParam`: Búsqueda por nombre (opcional)
- `pageNumber`: Número de página (opcional)
- `status`: Estado de la plantilla - `pending`, `approved`, `rejected` (opcional)
- `category`: Categoría de la plantilla (opcional)
- `createdById`: ID del usuario creador (opcional)

**Crear plantilla de mensaje:**
```http
POST /messageTemplates
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Plantilla de Bienvenida",
  "content": "¡Hola {{nombre}}! Bienvenido a nuestro servicio.",
  "category": "marketing",
  "variables": {"nombre": "string"},
  "description": "Plantilla para dar la bienvenida a nuevos clientes",
  "isActive": true,
  "media": [archivo.jpg] (opcional)
}
```

**Campos:**
- `name` (requerido): Nombre de la plantilla
- `content` (opcional): Contenido del mensaje (soporta variables con {{variable}}). Debe tener al menos contenido o imagen
- `category` (opcional): Categoría de la plantilla
- `variables` (opcional): Objeto JSON con las variables y sus tipos (ej: `{"nombre": "string", "edad": "number"}`)
- `description` (opcional): Descripción de la plantilla
- `isActive` (opcional): Si la plantilla está activa (default: true)
- `media` (opcional): Archivo multimedia (imagen, video, audio o documento)

**Nota:** La plantilla debe tener al menos contenido (`content`) o un archivo multimedia (`media`).

**Actualizar plantilla de mensaje:**
```http
PUT /messageTemplates/:messageTemplateId
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Plantilla Actualizada",
  "content": "Mensaje actualizado",
  "isActive": false,
  "media": [nuevo-archivo.jpg] (opcional)
}
```

**Enviar plantilla para aprobación:**
```http
POST /messageTemplates/:messageTemplateId/submit-for-approval
Authorization: Bearer {token}
```

**Aprobar plantilla:**
```http
POST /messageTemplates/:messageTemplateId/approve
Authorization: Bearer {token}
```

**Rechazar plantilla:**
```http
POST /messageTemplates/:messageTemplateId/reject
Authorization: Bearer {token}
Content-Type: application/json

{
  "rejectionReason": "El contenido no cumple con nuestras políticas"
}
```

**Campos:**
- `rejectionReason` (requerido): Razón del rechazo

**Renderizar plantilla con variables:**
```http
POST /messageTemplates/:messageTemplateId/render
Authorization: Bearer {token}
Content-Type: application/json

{
  "ticketId": 1,
  "customVariables": {
    "nombre": "Juan Pérez",
    "edad": 30
  }
}
```

**Campos:**
- `ticketId` (opcional): ID del ticket para obtener variables del contacto
- `customVariables` (opcional): Objeto JSON con variables personalizadas

**Respuesta:**
```json
{
  "renderedMessage": "¡Hola Juan Pérez! Bienvenido a nuestro servicio."
}
```

### Configuración de Chatbot

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/chatbot-configs` | Listar configuraciones de chatbot | Sí |
| GET | `/chatbot-configs/:chatbotConfigId` | Obtener configuración de chatbot | Sí |
| POST | `/chatbot-configs` | Crear configuración de chatbot | Sí |
| PUT | `/chatbot-configs/:chatbotConfigId` | Actualizar configuración de chatbot | Sí |
| DELETE | `/chatbot-configs/:chatbotConfigId` | Eliminar configuración de chatbot | Sí |
| POST | `/chatbot-configs/:chatbotConfigId/update-credit` | Actualizar créditos del chatbot | Sí |
| GET | `/chatbot-logs` | Listar logs del chatbot | Sí |

**Listar configuraciones de chatbot:**
```http
GET /chatbot-configs?queueId=1
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `queueId`: ID de la cola (opcional)

**Crear configuración de chatbot:**
```http
POST /chatbot-configs
Authorization: Bearer {token}
Content-Type: application/json

{
  "provider": "openai",
  "apiKey": "sk-...",
  "model": "gpt-3.5-turbo",
  "systemPrompt": "Eres un asistente virtual...",
  "maxTokens": 1000,
  "temperature": 0.7,
  "isActive": true,
  "useConversationHistory": true,
  "maxHistoryMessages": 10,
  "minCreditThreshold": 100,
  "priority": 1,
  "queueId": 1
}
```

**Campos:**
- `provider` (requerido): Proveedor del chatbot (openai, anthropic, etc.)
- `apiKey` (requerido): Clave API del proveedor
- `model` (requerido): Modelo a utilizar
- `systemPrompt` (opcional): Prompt del sistema
- `maxTokens` (opcional): Máximo de tokens
- `temperature` (opcional): Temperatura (0-1)
- `isActive` (opcional): Si está activo
- `useConversationHistory` (opcional): Usar historial de conversación
- `maxHistoryMessages` (opcional): Máximo de mensajes en historial
- `minCreditThreshold` (opcional): Umbral mínimo de créditos
- `priority` (opcional): Prioridad
- `queueId` (opcional): ID de la cola asociada

**Actualizar configuración de chatbot:**
```http
PUT /chatbot-configs/:chatbotConfigId
Authorization: Bearer {token}
Content-Type: application/json

{
  "isActive": false,
  "temperature": 0.8
}
```

**Actualizar créditos:**
```http
POST /chatbot-configs/:chatbotConfigId/update-credit
Authorization: Bearer {token}
Content-Type: application/json

{
  "credits": 500
}
```

**Listar logs del chatbot:**
```http
GET /chatbot-logs
Authorization: Bearer {token}
```

**Parámetros de consulta (opcionales):**
- `chatbotConfigId`: ID de la configuración del chatbot
- `ticketId`: ID del ticket
- `pageNumber`: Número de página
- `limit`: Resultados por página

### Widgets

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/api/widget/config/:widgetToken` | Obtener configuración del widget | No |
| GET | `/api/widget/session/:widgetToken` | Obtener sesión del widget | Token Widget |
| POST | `/api/widget/message` | Enviar mensaje desde widget | Token Widget |
| GET | `/api/widget/messages/:ticketId` | Obtener mensajes del widget | Token Widget |
| POST | `/api/widget/messages/:ticketId/read` | Marcar mensajes como leídos | Token Widget |
| GET | `/widgets` | Listar widgets | Sí |
| POST | `/widgets` | Crear widget | Sí |
| GET | `/widgets/:widgetId` | Obtener widget | Sí |
| PUT | `/widgets/:widgetId` | Actualizar widget | Sí |
| DELETE | `/widgets/:widgetId` | Eliminar widget | Sí |

**Obtener configuración del widget (público):**
```http
GET /api/widget/config/:widgetToken
```

**Respuesta:**
```json
{
  "name": "Widget de Soporte",
  "color": "#007bff",
  "greetingMessage": "¡Hola! ¿En qué puedo ayudarte?",
  "logo": "url-del-logo.png"
}
```

**Obtener sesión del widget:**
```http
GET /api/widget/session/:widgetToken
Authorization: Bearer {widget_token}
```

**Enviar mensaje desde widget:**
```http
POST /api/widget/message
Authorization: Bearer {widget_token}
Content-Type: multipart/form-data

{
  "body": "Mensaje desde el widget",
  "contact": {
    "name": "Juan Pérez",
    "email": "juan@ejemplo.com"
  },
  "medias": [archivo.jpg] (opcional)
}
```

**Obtener mensajes del widget:**
```http
GET /api/widget/messages/:ticketId
Authorization: Bearer {widget_token}
```

**Marcar mensajes como leídos:**
```http
POST /api/widget/messages/:ticketId/read
Authorization: Bearer {widget_token}
```

**Listar widgets:**
```http
GET /widgets
Authorization: Bearer {token}
```

**Crear widget:**
```http
POST /widgets
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Widget de Soporte",
  "color": "#007bff",
  "greetingMessage": "¡Hola! ¿En qué puedo ayudarte?",
  "logo": [archivo.png] (opcional)
}
```

**Campos:**
- `name` (requerido): Nombre del widget
- `color` (opcional): Color del widget (hex)
- `greetingMessage` (opcional): Mensaje de bienvenida
- `logo` (opcional): Logo del widget

**Actualizar widget:**
```http
PUT /widgets/:widgetId
Authorization: Bearer {token}
Content-Type: multipart/form-data

{
  "name": "Widget Actualizado",
  "color": "#28a745",
  "logo": [nuevo-logo.png] (opcional)
}
```

### Workflows

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/workflows` | Listar workflows | Sí |
| GET | `/workflows/:workflowId` | Obtener workflow | Sí |
| POST | `/workflows` | Crear workflow | Sí |
| PUT | `/workflows/:workflowId` | Actualizar workflow | Sí |
| DELETE | `/workflows/:workflowId` | Eliminar workflow | Sí |
| POST | `/workflows/:workflowId/execute` | Ejecutar workflow manualmente | Sí |
| GET | `/workflows/executions` | Listar ejecuciones de workflows | Sí |
| GET | `/workflows/:workflowId/executions` | Listar ejecuciones de un workflow | Sí |
| POST | `/workflows/:workflowId/validate` | Validar workflow | Sí |
| POST | `/workflows/validate` | Validar definición de workflow | Sí |

**Listar workflows:**
```http
GET /workflows?searchParam=nombre&pageNumber=1&rowsPerPage=30&isActive=true&triggerType=message_received&queueId=1
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `searchParam` (opcional): Búsqueda por nombre o descripción
- `pageNumber` (opcional): Número de página (default: "1")
- `rowsPerPage` (opcional): Resultados por página (default: "30")
- `isActive` (opcional): Filtrar por estado activo/inactivo
- `triggerType` (opcional): Filtrar por tipo de trigger
- `queueId` (opcional): Filtrar por cola

**Crear workflow:**
```http
POST /workflows
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Workflow de Bienvenida",
  "description": "Envía mensaje de bienvenida cuando se crea un ticket",
  "isActive": true,
  "triggerType": "ticket_created",
  "triggerConditions": {
    "queueId": 1
  },
  "workflowDefinition": {
    "nodes": [
      {
        "id": "trigger-1",
        "type": "trigger",
        "data": {},
        "position": { "x": 0, "y": 0 }
      },
      {
        "id": "action-1",
        "type": "action",
        "data": {
          "action": {
            "type": "send_message",
            "message": "¡Hola {{contact.name}}! Bienvenido a nuestro servicio."
          }
        },
        "position": { "x": 200, "y": 0 }
      }
    ],
    "edges": [
      {
        "id": "edge-1",
        "source": "trigger-1",
        "target": "action-1"
      }
    ]
  },
  "queueIds": [1, 2],
  "whatsappIds": [1]
}
```

**Campos:**
- `name` (requerido): Nombre del workflow
- `description` (opcional): Descripción del workflow
- `isActive` (opcional): Si el workflow está activo (default: true)
- `triggerType` (requerido): Tipo de trigger - `"message_received"`, `"ticket_created"`, `"ticket_closed"`, `"ticket_assigned"`, `"ticket_transferred"`
- `triggerConditions` (opcional): Condiciones adicionales del trigger (JSON)
- `workflowDefinition` (requerido): Definición del workflow con nodos y edges (JSON)
- `queueIds` (requerido): Array de IDs de colas (obligatorio, al menos una)
- `whatsappIds` (opcional): Array de IDs de conexiones WhatsApp (si no se proporciona, aplica a todas)

**Tipos de triggers:**
- `message_received`: Se activa cuando se recibe un mensaje
- `ticket_created`: Se activa cuando se crea un ticket
- `ticket_closed`: Se activa cuando se cierra un ticket
- `ticket_assigned`: Se activa cuando se asigna un ticket a un usuario
- `ticket_transferred`: Se activa cuando se transfiere un ticket a otra cola

**Tipos de nodos:**
- `trigger`: Nodo inicial del workflow (automático)
- `action`: Nodo de acción (enviar mensaje, asignar usuario/cola, agregar/remover etiquetas, cambiar estado)
- `condition`: Nodo de condición simple (true/false)
- `advanced_condition`: Nodo de condición avanzada con múltiples reglas (AND/OR)
- `delay`: Nodo de espera con tiempo configurable

**Actualizar workflow:**
```http
PUT /workflows/:workflowId
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "Workflow Actualizado",
  "isActive": false,
  "workflowDefinition": { ... }
}
```

**Ejecutar workflow manualmente:**
```http
POST /workflows/:workflowId/execute
Authorization: Bearer {token}
Content-Type: application/json

{
  "ticketId": 1,
  "contactId": 1,
  "contextData": {
    "customVariable": "valor"
  }
}
```

**Campos:**
- `ticketId` (opcional): ID del ticket para ejecutar el workflow
- `contactId` (opcional): ID del contacto
- `contextData` (opcional): Datos adicionales para el contexto del workflow

**Listar ejecuciones de workflows:**
```http
GET /workflows/executions?workflowId=1&ticketId=1&status=completed&limit=20&offset=0
Authorization: Bearer {token}
```

**Parámetros de consulta:**
- `workflowId` (opcional): Filtrar por workflow específico
- `ticketId` (opcional): Filtrar por ticket
- `contactId` (opcional): Filtrar por contacto
- `status` (opcional): Filtrar por estado (running, completed, failed)
- `limit` (opcional): Límite de resultados
- `offset` (opcional): Offset para paginación

**Validar workflow:**
```http
POST /workflows/:workflowId/validate
Authorization: Bearer {token}
Content-Type: application/json

{
  "workflowDefinition": { ... }
}
```

**O validar sin guardar:**
```http
POST /workflows/validate
Authorization: Bearer {token}
Content-Type: application/json

{
  "workflowDefinition": { ... }
}
```

**Respuesta de ejecución:**
```json
{
  "id": 1,
  "workflowId": 1,
  "ticketId": 1,
  "contactId": 1,
  "status": "completed",
  "contextData": "{\"customVariable\":\"valor\"}",
  "executionLogs": "[{\"timestamp\":\"2024-01-01T00:00:00Z\",\"nodeId\":\"trigger-1\",\"level\":\"info\",\"message\":\"Workflow iniciado\"}]",
  "result": "{\"success\":true}",
  "errorMessage": null,
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-01T00:00:00Z"
}
```

### Integración con Apptivo

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| GET | `/contacts/:contactId/apptivo/status` | Obtener estado de vinculación con Apptivo | Sí |
| POST | `/contacts/:contactId/apptivo/create` | Crear contacto en Apptivo y vincularlo | Sí |

**Obtener estado de vinculación con Apptivo:**
```http
GET /contacts/:contactId/apptivo/status
Authorization: Bearer {token}
```

**Crear contacto en Apptivo:**
```http
POST /contacts/:contactId/apptivo/create
Authorization: Bearer {token}
Content-Type: application/json

{
  "apptivoContactData": {
    "name": "Juan Pérez",
    "email": "juan@ejemplo.com",
    "phone": "1234567890"
  }
}
```

### API Externa (Endpoints de API)

Estos endpoints están diseñados para uso externo y utilizan **API Key** en lugar de JWT. Son ideales para integraciones con sistemas externos, webhooks, o aplicaciones de terceros.

**Autenticación:**
Los endpoints de API externa requieren un API Key que se genera en la configuración del sistema. El API Key se envía en el header `Authorization`:

```http
Authorization: Bearer {api_key}
```

| Método | Endpoint | Descripción | Auth |
|--------|----------|-------------|------|
| POST | `/api/messages/send` | Enviar mensaje por número | API Key |
| POST | `/api/messages/send-by-number` | Enviar mensaje por número (alternativo) | API Key |

**Enviar mensaje por API (sin ticket):**
```http
POST /api/messages/send
Authorization: Bearer {api_key}
Content-Type: multipart/form-data

{
  "number": "1234567890",
  "body": "Hola desde la API",
  "whatsappId": 1,
  "quotedMsg": {...} (opcional),
  "medias": [archivo1.jpg, archivo2.pdf] (opcional)
}
```

**Campos:**
- `number` (requerido): Número de teléfono del destinatario (solo dígitos, sin espacios ni guiones)
- `body` (opcional): Contenido del mensaje de texto
- `whatsappId` (opcional): ID de la conexión WhatsApp a usar. Si no se proporciona, se usa la conexión por defecto
- `quotedMsg` (opcional): Objeto del mensaje a citar
- `medias` (opcional): Archivos multimedia a enviar (imágenes, videos, documentos, audio)

**Nota:** Debe proporcionarse al menos `body` o `medias`. Si se proporcionan ambos, se enviará el mensaje de texto junto con los archivos multimedia.

**Enviar mensaje por número (API alternativa):**
```http
POST /api/messages/send-by-number
Authorization: Bearer {api_key}
Content-Type: multipart/form-data

{
  "number": "1234567890",
  "body": "Hola desde la API",
  "whatsappId": 1,
  "quotedMsg": {...} (opcional),
  "medias": [archivo1.jpg] (opcional)
}
```

**Campos:**
- `number` (requerido): Número de teléfono del destinatario (solo dígitos, sin espacios ni guiones)
- `body` (opcional): Contenido del mensaje de texto
- `whatsappId` (opcional): ID de la conexión WhatsApp a usar. Si no se proporciona, se usa la conexión por defecto
- `quotedMsg` (opcional): Objeto del mensaje a citar
- `medias` (opcional): Archivos multimedia a enviar

**Comportamiento:**
- Si el contacto no existe, se crea automáticamente
- Si no existe un ticket para ese contacto, se crea uno nuevo
- El mensaje se envía y se almacena en el ticket correspondiente
- Los mensajes enviados se marcan como leídos automáticamente

**Ejemplo de respuesta exitosa (200):**
```json
{
  "message": "Message sent successfully"
}
```

**Errores posibles:**
- `400`: Número de teléfono inválido o formato incorrecto
- `401`: API Key inválido o faltante
- `404`: Conexión WhatsApp no encontrada
- `500`: Error al enviar el mensaje

**Ejemplo con cURL:**
```bash
curl -X POST http://localhost:8080/api/messages/send \
  -H "Authorization: Bearer tu_api_key_aqui" \
  -F "number=1234567890" \
  -F "body=Hola desde la API" \
  -F "whatsappId=1" \
  -F "medias=@archivo.jpg"
```

**Ejemplo con JavaScript/Node.js:**
```javascript
const axios = require('axios');
const FormData = require('form-data');
const fs = require('fs');

const API_URL = 'http://localhost:8080';
const API_KEY = 'tu_api_key_aqui';

async function sendMessageViaAPI(number, body, whatsappId, mediaPath) {
  const formData = new FormData();
  formData.append('number', number);
  formData.append('body', body);
  formData.append('whatsappId', whatsappId);
  
  if (mediaPath) {
    formData.append('medias', fs.createReadStream(mediaPath));
  }

  const { data } = await axios.post(
    `${API_URL}/api/messages/send`,
    formData,
    {
      headers: {
        ...formData.getHeaders(),
        Authorization: `Bearer ${API_KEY}`
      }
    }
  );
  
  return data;
}

// Uso
sendMessageViaAPI('1234567890', 'Hola desde Node.js', 1, './imagen.jpg');
```

**Ejemplo con Python:**
```python
import requests

API_URL = 'http://localhost:8080'
API_KEY = 'tu_api_key_aqui'

def send_message_via_api(number, body, whatsapp_id, media_path=None):
    url = f'{API_URL}/api/messages/send'
    headers = {'Authorization': f'Bearer {API_KEY}'}
    
    data = {
        'number': number,
        'body': body,
        'whatsappId': whatsapp_id
    }
    
    files = {}
    if media_path:
        files['medias'] = open(media_path, 'rb')
    
    response = requests.post(url, headers=headers, data=data, files=files)
    return response.json()

# Uso
send_message_via_api('1234567890', 'Hola desde Python', 1, './imagen.jpg')
```

---

## 📊 Códigos de Estado

| Código | Descripción |
|--------|-------------|
| 200 | OK - Petición exitosa |
| 201 | Created - Recurso creado |
| 400 | Bad Request - Error en la petición |
| 401 | Unauthorized - No autenticado |
| 403 | Forbidden - Sin permisos |
| 404 | Not Found - Recurso no encontrado |
| 500 | Internal Server Error - Error del servidor |

---

## ⚠️ Manejo de Errores

Las respuestas de error siguen este formato:

```json
{
  "error": "Código del error",
  "message": "Descripción del error"
}
```

**Ejemplo:**
```json
{
  "error": "ERR_VALIDATION_ERROR",
  "message": "Validation failed"
}
```

### Códigos de Error Comunes

- `ERR_VALIDATION_ERROR`: Error de validación
- `ERR_NO_PERMISSION`: Sin permisos
- `ERR_NO_TICKET_FOUND`: Ticket no encontrado
- `ERR_NO_CONTACT_FOUND`: Contacto no encontrado
- `ERR_SENDING_WAPP_MSG`: Error al enviar mensaje
- `ERR_WAPP_NOT_INITIALIZED`: WhatsApp no inicializado

---

## 💡 Ejemplos

### Ejemplo Completo: Enviar Mensaje

```bash
# 1. Login
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "usuario@ejemplo.com",
    "password": "contraseña"
  }'

# Respuesta:
# {
#   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
#   "user": { ... }
# }

# 2. Obtener tickets
curl -X GET http://localhost:8080/tickets \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."

# 3. Enviar mensaje
curl -X POST http://localhost:8080/messages/1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -F "body=Hola desde la API"
```

### Ejemplo con JavaScript/Node.js

```javascript
const axios = require('axios');

const API_URL = 'http://localhost:8080';

// Login
async function login(email, password) {
  const { data } = await axios.post(`${API_URL}/auth/login`, {
    email,
    password
  });
  return data.token;
}

// Enviar mensaje
async function sendMessage(token, ticketId, message) {
  const { data } = await axios.post(
    `${API_URL}/messages/${ticketId}`,
    { body: message },
    {
      headers: {
        Authorization: `Bearer ${token}`
      }
    }
  );
  return data;
}

// Uso
(async () => {
  const token = await login('usuario@ejemplo.com', 'contraseña');
  await sendMessage(token, 1, 'Hola desde Node.js');
})();
```

### Ejemplo con Python

```python
import requests

API_URL = 'http://localhost:8080'

# Login
def login(email, password):
    response = requests.post(f'{API_URL}/auth/login', json={
        'email': email,
        'password': password
    })
    return response.json()['token']

# Enviar mensaje
def send_message(token, ticket_id, message):
    headers = {'Authorization': f'Bearer {token}'}
    response = requests.post(
        f'{API_URL}/messages/{ticket_id}',
        json={'body': message},
        headers=headers
    )
    return response.json()

# Uso
token = login('usuario@ejemplo.com', 'contraseña')
send_message(token, 1, 'Hola desde Python')
```

---

## 🔔 Webhooks

### Configurar Webhook

Configura el webhook en la conexión de WhatsApp:

```http
PUT /whatsapp/:id
Authorization: Bearer {token}
Content-Type: application/json

{
  "webhookUrl": "https://tu-servidor.com/webhook"
}
```

### Eventos Enviados

Cuando ocurre un evento, se envía un POST al webhook configurado:

```json
{
  "event": "message",
  "data": {
    "id": "message_id",
    "ticketId": 1,
    "contactId": 1,
    "body": "Mensaje recibido",
    "fromMe": false,
    "timestamp": "2024-01-01T12:00:00Z"
  }
}
```

**Tipos de eventos:**
- `message`: Nuevo mensaje
- `ticket`: Cambio en ticket
- `contact`: Cambio en contacto

---

## 📚 Vista de Endpoints (`/api-docs`)

La vista de endpoints es una página interactiva dentro de la aplicación que muestra todos los endpoints disponibles de la API. Para acceder:

```
https://tu-dominio.com/api-docs
```

**Nota**: Debes estar autenticado para acceder a la vista de endpoints. Está disponible en el menú lateral de la aplicación (opción "API Docs").

**La vista de endpoints permite:**
- Ver todos los endpoints disponibles organizados por categorías (Autenticación, Tickets, Contactos, Mensajes, etc.)
- Probar endpoints directamente desde la interfaz
- Ver esquemas de datos y validaciones para cada endpoint
- Ver ejemplos de requests y responses
- Configurar autenticación (JWT tokens) y probar llamadas en tiempo real
- Exportar documentación en múltiples formatos (OpenAPI, Postman, cURL)
- Buscar y filtrar endpoints por categoría o nombre
- Ver códigos de estado HTTP y manejo de errores

---

## 🔒 Autenticación API (API Key)

Para usar los endpoints de API externa (`/api/messages/*`) desde aplicaciones externas, debes usar **API Keys** en lugar de tokens JWT.

### Obtener API Key

1. Accede a la configuración del sistema
2. Genera un nuevo API Key
3. Guarda el API Key de forma segura (no se mostrará nuevamente)

### Usar API Key

Incluye el API Key en el header `Authorization`:

```http
Authorization: Bearer {api_key}
```

### Diferencias entre JWT y API Key

| Característica | JWT (Token) | API Key |
|---------------|-------------|---------|
| **Uso** | Endpoints internos de la plataforma | Endpoints de API externa (`/api/*`) |
| **Expiración** | Sí, tiene tiempo de expiración | No, permanente hasta que se revoque |
| **Renovación** | Requiere refresh token | No requiere renovación |
| **Alcance** | Acceso completo según permisos del usuario | Solo endpoints de API externa |
| **Recomendado para** | Aplicaciones web, frontend | Integraciones externas, webhooks, sistemas de terceros |

### Seguridad

- **Nunca compartas tu API Key** públicamente
- **Rota los API Keys** periódicamente si sospechas que han sido comprometidos
- **Usa HTTPS** siempre al enviar API Keys
- **Revoca API Keys** que ya no uses

---

¿Necesitas más información? Consulta la [documentación principal](../README.md) o la [guía de desarrollo](../DEVELOPMENT.md).

