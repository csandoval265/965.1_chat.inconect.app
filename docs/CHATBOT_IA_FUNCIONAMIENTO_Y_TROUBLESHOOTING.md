# 🤖 Chatbot de IA - Funcionamiento y Solución de Problemas

## 📋 Tabla de Contenidos

1. [¿Cómo Funciona el Chatbot de IA?](#cómo-funciona-el-chatbot-de-ia)
2. [Flujo de Procesamiento](#flujo-de-procesamiento)
3. [Proveedores Soportados](#proveedores-soportados)
4. [Problemas Comunes y Soluciones](#problemas-comunes-y-soluciones)
5. [Diagnóstico de Problemas](#diagnóstico-de-problemas)
6. [Verificación de Configuración](#verificación-de-configuración)
7. [Logs y Monitoreo](#logs-y-monitoreo)

---

## 🔄 ¿Cómo Funciona el Chatbot de IA?

El chatbot de IA es un sistema que utiliza modelos de lenguaje (LLM) para responder automáticamente a los mensajes de los clientes en WhatsApp o Telegram. Funciona de la siguiente manera:

### Componentes Principales

1. **Configuración del Chatbot** (`ChatbotConfig`): Define qué proveedor de IA usar, modelo, API key, y parámetros
2. **Servicio de Verificación** (`CheckAIChatbotService`): Procesa los mensajes y decide si debe responder
3. **Servicios de Proveedores**: Conectan con las APIs de OpenAI, Anthropic o Google
4. **Sistema de Logs** (`ChatbotLog`): Registra todas las interacciones para diagnóstico

### Orden de Prioridad

El sistema busca configuraciones en este orden:
1. **Configuración específica de cola** (si el ticket tiene una cola asignada)
2. **Configuración general** (sin cola específica)
3. **Mayor prioridad** (campo `priority`)
4. **ID más antiguo** (como desempate)

---

## 🔀 Flujo de Procesamiento

```
1. Cliente envía mensaje en WhatsApp/Telegram
   ↓
2. Sistema recibe el mensaje (wbotMessageListener/telegramMessageListener)
   ↓
3. Si un humano respondió → Cancela cualquier timer activo del chatbot
   ↓
4. Verifica si hay auto-reply configurado
   ↓
5. Si NO hay auto-reply → Programa respuesta del chatbot con tiempo de espera
   ↓
6. Espera el tiempo configurado (waitTimeBeforeResponse) después del último mensaje
   ↓
7. Si durante la espera:
   - Un humano responde → Cancela el timer
   - Pasa el tiempo máximo sin respuesta (maxIdleTime) → Cancela el timer
   - Se cumple el tiempo de espera → Ejecuta el chatbot
   ↓
8. CheckAIChatbotService:
   - Valida estado del ticket (debe estar en allowedStatuses)
   - Busca configuración activa (específica de cola o general)
   - Verifica crédito disponible
   - Construye mensajes (system prompt + historial + mensaje actual)
   - Llama al proveedor de IA (OpenAI/Anthropic/Google)
   ↓
9. Si hay respuesta exitosa → Envía mensaje al cliente
   ↓
10. Guarda log de la interacción
```

### Características del Sistema de Timers

El chatbot ahora utiliza un sistema de timers inteligente:

- **Tiempo de Espera Configurable**: Espera un tiempo definido (`waitTimeBeforeResponse`) después del último mensaje antes de responder
- **Detección de Respuesta Humana**: Si un humano responde (usuario asignado o mensaje del sistema), cancela automáticamente el timer
- **Timeout de Inactividad**: Si pasa el tiempo máximo sin respuesta (`maxIdleTime`), cancela el timer
- **Actualización de Tiempo**: Si el cliente envía otro mensaje durante la espera, actualiza el tiempo del último mensaje

### Condiciones para que NO Responda

El chatbot **NO responderá** si:

- ✅ El mensaje viene del sistema (`msg.fromMe === true` o `is_bot === true`)
- ✅ El mensaje está vacío o solo tiene espacios
- ✅ No hay configuración activa del chatbot
- ✅ El estado del ticket no está en `allowedStatuses` (por defecto: "pending,open")
- ✅ No hay crédito disponible
- ✅ El crédito está por debajo del umbral mínimo configurado
- ✅ Ya hay un auto-reply que respondió
- ✅ Un humano respondió durante el tiempo de espera
- ✅ Pasó el tiempo máximo de inactividad sin respuesta

---

## 🌐 Proveedores Soportados

### 1. OpenAI
- **Modelos**: `gpt-3.5-turbo`, `gpt-4`, `gpt-4-turbo`, etc.
- **Endpoint**: `https://api.openai.com/v1/chat/completions`
- **Autenticación**: Bearer token en header `Authorization`

### 2. Anthropic (Claude)
- **Modelos**: `claude-3-haiku-20240307`, `claude-3-sonnet-20240229`, `claude-3-opus-20240229`
- **Endpoint**: `https://api.anthropic.com/v1/messages`
- **Autenticación**: Header `x-api-key`

### 3. Google (Gemini)
- **Modelos**: `gemini-pro`, `gemini-pro-vision`
- **Endpoint**: `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`
- **Autenticación**: API key en query parameter

---

## ⚠️ Problemas Comunes y Soluciones

### 1. El chatbot no responde nada

**Posibles causas:**

#### a) No hay configuración activa
- **Síntoma**: No hay respuesta, no hay errores en logs
- **Solución**: 
  - Ve a la configuración del chatbot
  - Verifica que haya al menos una configuración con `isActive = true`
  - Si el ticket tiene una cola, asegúrate de que haya una configuración para esa cola o una general

#### b) API Key inválida o sin crédito
- **Síntoma**: Errores en logs con códigos 401, 403, o "insufficient_quota"
- **Solución**:
  - Verifica que la API key sea correcta
  - Revisa el balance de crédito en la cuenta del proveedor
  - Actualiza la API key si es necesario

#### c) Crédito por debajo del umbral mínimo
- **Síntoma**: Logs muestran "Crédito insuficiente"
- **Solución**:
  - Aumenta el crédito en la cuenta del proveedor
  - Reduce o elimina el `minCreditThreshold` en la configuración

#### d) Ticket tiene usuario asignado
- **Síntoma**: El chatbot no responde aunque esté configurado
- **Solución**: Esto es comportamiento esperado. El chatbot solo responde cuando NO hay usuario asignado

### 2. Errores de API

#### Error 401/403: API Key inválida
```
[OpenAI] Status Code: 401
[OpenAI] Error Message: Invalid API key
```
**Solución**: 
- Verifica que la API key sea correcta
- Asegúrate de que no tenga espacios extra
- Regenera la API key si es necesario

#### Error 429: Rate limit alcanzado
```
[OpenAI] Status Code: 429
[OpenAI] Error Message: Rate limit exceeded
```
**Solución**:
- Espera unos minutos antes de intentar de nuevo
- Considera usar un plan con mayor límite de rate
- Reduce la frecuencia de mensajes

#### Error 500: Error del servidor del proveedor
```
[OpenAI] Status Code: 500
```
**Solución**:
- Espera unos minutos (puede ser un problema temporal del proveedor)
- Verifica el estado del servicio del proveedor
- Intenta de nuevo más tarde

### 3. Respuestas vacías o sin sentido

#### a) System prompt mal configurado
- **Solución**: Revisa y mejora el system prompt. Ver ejemplos en `CHATBOT_PROMPTS_EXAMPLES.md`

#### b) Modelo no soportado
- **Solución**: Verifica que el modelo sea válido para el proveedor:
  - OpenAI: `gpt-3.5-turbo`, `gpt-4`, etc.
  - Anthropic: `claude-3-haiku-20240307`, etc.
  - Google: `gemini-pro`

#### c) Temperatura muy alta o muy baja
- **Solución**: Ajusta la temperatura (0.0-2.0):
  - 0.0-0.3: Más determinista y predecible
  - 0.7-1.0: Balance entre creatividad y coherencia
  - 1.5-2.0: Muy creativo pero puede ser incoherente

### 4. El chatbot responde cuando no debería

#### a) Auto-reply no configurado correctamente
- **Síntoma**: El chatbot responde incluso cuando hay auto-reply
- **Causa**: El auto-reply debe ejecutarse primero. Si no hay match, entonces se usa el chatbot
- **Solución**: Configura correctamente los auto-replies si quieres que tengan prioridad

#### b) Múltiples configuraciones activas
- **Síntoma**: Comportamiento inconsistente
- **Solución**: 
  - Revisa que no haya múltiples configuraciones activas para la misma cola
  - Usa el campo `priority` para controlar cuál se usa
  - Desactiva configuraciones que no necesites

---

## 🔍 Diagnóstico de Problemas

### Paso 1: Verificar Configuración

1. **Accede a la interfaz de configuración del chatbot**
2. **Verifica que haya al menos una configuración activa**:
   - `isActive = true`
   - API key configurada
   - Proveedor y modelo válidos

3. **Verifica la configuración específica de cola**:
   - Si el ticket tiene una cola asignada, busca una configuración con `queueId` = ID de la cola
   - Si no hay configuración específica, debe haber una general (`queueId = null`)

### Paso 2: Revisar Logs

Los logs se guardan en la tabla `ChatbotLogs` y también se imprimen en la consola del servidor.

#### En la Interfaz Web:
1. Ve a la configuración del chatbot
2. Haz clic en "Ver Logs" para la configuración que quieres revisar
3. Revisa los logs más recientes:
   - **Status = "success"**: La llamada fue exitosa
   - **Status = "error"**: Hubo un error, revisa `errorMessage` y `errorCode`

#### En la Consola del Servidor:
Busca líneas que empiecen con:
- `[AI Chatbot]` - Logs generales del servicio
- `[OpenAI]` - Logs específicos de OpenAI
- `[Anthropic]` - Logs específicos de Anthropic
- `[Google]` - Logs específicos de Google

### Paso 3: Verificar Crédito

El sistema verifica automáticamente el crédito cada 5 minutos. Para verificar manualmente:

1. **En la configuración del chatbot**, revisa:
   - `creditBalance`: Balance actual
   - `lastCreditCheck`: Última verificación
   - `minCreditThreshold`: Umbral mínimo configurado

2. **Si el crédito es 0 o null**, el chatbot no funcionará

3. **Verifica directamente en la cuenta del proveedor**:
   - OpenAI: https://platform.openai.com/account/usage
   - Anthropic: https://console.anthropic.com/settings/billing
   - Google: https://makersuite.google.com/app/apikey

### Paso 4: Probar Manualmente

Puedes probar el chatbot enviando un mensaje de prueba:

1. **Crea un ticket de prueba** sin usuario asignado
2. **Envía un mensaje** desde WhatsApp/Telegram
3. **Observa los logs** en tiempo real
4. **Verifica la respuesta** (o falta de respuesta)

---

## ✅ Verificación de Configuración

### Checklist de Configuración Correcta

- [ ] Al menos una configuración con `isActive = true`
- [ ] API key válida y con crédito disponible
- [ ] Proveedor correcto (`openai`, `anthropic`, o `google`)
- [ ] Modelo válido para el proveedor
- [ ] System prompt configurado (opcional pero recomendado)
- [ ] `maxTokens` configurado (recomendado: 200-1000)
- [ ] `temperature` configurado (recomendado: 0.7)
- [ ] `waitTimeBeforeResponse` configurado (tiempo en segundos, por defecto: 30)
- [ ] `maxIdleTime` configurado (tiempo máximo sin respuesta en segundos, por defecto: 300)
- [ ] `allowedStatuses` configurado (estados permitidos separados por coma, por defecto: "pending,open")
- [ ] Si hay colas, configuración específica o general disponible
- [ ] `minCreditThreshold` configurado apropiadamente (o 0 para desactivar)

### Nuevos Campos de Configuración

- **`waitTimeBeforeResponse`** (segundos, default: 30): Tiempo a esperar después del último mensaje del cliente antes de responder. Esto permite que un humano tenga tiempo de responder primero.

- **`maxIdleTime`** (segundos, default: 300): Tiempo máximo sin respuesta del cliente antes de cancelar el timer del chatbot. Si el cliente no responde en este tiempo, el chatbot no responderá.

- **`allowedStatuses`** (string, default: "pending,open"): Estados de ticket permitidos para que el chatbot responda. Separados por coma. Ejemplos:
  - `"pending,open"` - Solo tickets pendientes y abiertos
  - `"pending"` - Solo tickets pendientes
  - `"pending,open,preview"` - Tickets pendientes, abiertos y en preview

### Verificación de Prioridad

Si hay múltiples configuraciones activas:

1. **Configuración específica de cola** tiene prioridad sobre general
2. **Mayor `priority`** tiene prioridad sobre menor
3. **ID más antiguo** se usa como desempate

Ejemplo:
- Config 1: `queueId=1, priority=10` → Se usa para cola 1
- Config 2: `queueId=null, priority=5` → Se usa para otras colas
- Config 3: `queueId=1, priority=5` → NO se usa (Config 1 tiene mayor prioridad)

---

## 📊 Logs y Monitoreo

### Información Registrada en Logs

Cada interacción se registra con:

#### Logs Exitosos:
- `status`: "success"
- `responseTime`: Tiempo de respuesta en ms
- `promptTokens`: Tokens usados en el prompt
- `completionTokens`: Tokens usados en la respuesta
- `totalTokens`: Total de tokens
- `estimatedCost`: Costo estimado
- `userMessage`: Mensaje del usuario
- `aiResponse`: Respuesta generada

#### Logs de Error:
- `status`: "error"
- `errorMessage`: Mensaje de error
- `errorCode`: Código de error (si está disponible)
- `errorType`: Tipo de error
- `statusCode`: Código HTTP de respuesta
- `errorResponse`: Respuesta completa del error
- `responseTime`: Tiempo hasta el error

### Monitoreo Recomendado

1. **Revisa logs diariamente** para detectar problemas temprano
2. **Monitorea el crédito** para evitar quedarte sin saldo
3. **Revisa errores 429** (rate limits) para ajustar uso
4. **Analiza `responseTime`** para detectar problemas de rendimiento
5. **Revisa `estimatedCost`** para controlar gastos

---

## 🛠️ Comandos Útiles para Diagnóstico

### Ver logs del servidor en tiempo real:
```bash
# Si usas Docker
docker-compose logs -f backend

# Si ejecutas directamente
tail -f logs/app.log  # o donde estén tus logs
```

### Buscar errores específicos:
```bash
# Errores del chatbot
grep "\[AI Chatbot\]" logs/app.log | grep -i error

# Errores de OpenAI
grep "\[OpenAI\]" logs/app.log | grep -i error

# Errores de crédito
grep "Sin crédito" logs/app.log
```

---

## 📝 Notas Importantes

1. **El chatbot solo responde cuando NO hay usuario asignado al ticket**
2. **El auto-reply tiene prioridad sobre el chatbot de IA**
3. **El sistema verifica crédito automáticamente cada 5 minutos**
4. **Los logs se guardan en la base de datos y también se imprimen en consola**
5. **El historial de conversación es opcional** (configurado con `useConversationHistory`)

---

## 🆘 ¿Necesitas Más Ayuda?

Si después de seguir esta guía el problema persiste:

1. **Revisa los logs detallados** en la interfaz web o consola
2. **Verifica la documentación del proveedor** (OpenAI, Anthropic, Google)
3. **Consulta los ejemplos de prompts** en `CHATBOT_PROMPTS_EXAMPLES.md`
4. **Contacta al equipo de desarrollo** con los logs específicos del error

---

**Última actualización**: 2025-01-27
