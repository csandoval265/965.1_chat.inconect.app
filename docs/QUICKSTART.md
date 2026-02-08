# ⚡ Guía de Inicio Rápido

Guía rápida para tener la plataforma funcionando.

**⚠️ IMPORTANTE**: Este proyecto requiere configuración de dominio y certificados SSL. No funciona por puertos de desarrollo.

## 🚀 Instalación Rápida (Docker)

### Paso 1: Configurar Dominio y SSL

**Antes de iniciar**, necesitas:

1. **Dominio configurado** apuntando a tu servidor
2. **Certificados SSL** (Let's Encrypt recomendado)

```bash
# Obtener certificados con Let's Encrypt
sudo certbot certonly --standalone -d tu-dominio.com -d whatsapp-api.tu-dominio.com
```

### Paso 2: Clonar y Configurar

```bash
# Clonar repositorio
git clone <repository-url>
cd WhatsApp

# Copiar y configurar variables de entorno
cp env.example .env
nano .env  # Edita dominios, contraseñas y APP_ENV
```

**Variables críticas en .env:**
```env
BACKEND_URL=https://whatsapp-api.tu-dominio.com
FRONTEND_URL=https://tu-dominio.com
APP_ENV=develop  # o tu entorno
FRONTEND_SERVER_NAME=whatsapp.tu-dominio.com
BACKEND_SERVER_NAME=whatsapp-api.tu-dominio.com
```

### Paso 3: Configurar Nginx

Edita los archivos de configuración en `docker/nginx/conf/${APP_ENV}/` con tus dominios.

### Paso 4: Iniciar Servicios

```bash
# Iniciar todos los servicios
docker-compose up -d

# Ver logs
docker-compose logs -f
```

### Paso 5: Acceder a la Aplicación

1. Abre tu navegador en `https://tu-dominio.com` (usa HTTPS, NO localhost)
2. Crea tu primer usuario (será administrador)
3. Ve a **Conexiones** y conecta tu WhatsApp

## 📱 Conectar WhatsApp

1. Ve a **Conexiones** → **Nueva Conexión**
2. Ingresa un nombre (ej: "Mi WhatsApp")
3. Escanea el código QR con tu teléfono:
   - Abre WhatsApp
   - Configuración → Dispositivos vinculados
   - Vincular un dispositivo
   - Escanea el código
   
   **Alternativa:** También puedes usar el código de emparejamiento (pairing code) si está disponible

## ✅ Verificar que Todo Funciona

```bash
# Ver estado de contenedores
docker-compose ps

# Todos deben estar "Up" y "healthy"

# Verificar que Nginx está funcionando
curl -I https://tu-dominio.com

# Verificar certificado SSL
openssl s_client -connect tu-dominio.com:443 -servername tu-dominio.com
```

## 🎯 Primeros Pasos

### 1. Crear una Cola

1. Ve a **Colas** → **Nueva Cola**
2. Nombre: "Atención al Cliente"
3. Guarda

### 2. Crear Respuestas Rápidas

1. Ve a **Respuestas Rápidas** → **Nueva**
2. Atajo: `saludo`
3. Mensaje: `¡Hola! ¿En qué puedo ayudarte?`
4. Guarda

### 3. Enviar un Mensaje de Prueba

1. Ve a **Tickets**
2. Crea un nuevo ticket o selecciona uno existente
3. Escribe un mensaje y envía

### 4. Configurar Respuestas Automáticas (Opcional)

1. Ve a **Respuestas Automáticas** → **Nueva**
2. Configura palabras clave y el mensaje a enviar
3. Asigna a una cola si es necesario

### 5. Configurar Horarios de Negocio (Opcional)

1. Ve a **Colas** → Selecciona una cola → **Horarios**
2. Configura los horarios de atención por día de la semana
3. Configura mensajes para fuera de horario

## 🔧 Configuración Básica

### Variables Mínimas Requeridas

En tu archivo `.env`, asegúrate de tener:

```env
# Base de datos (cambia las contraseñas)
MYSQL_ROOT_PASSWORD=tu_contraseña_segura
MYSQL_PASSWORD=tu_contraseña_segura

# JWT (genera valores aleatorios)
JWT_SECRET=$(openssl rand -base64 32)
JWT_REFRESH_SECRET=$(openssl rand -base64 32)

# Encriptación
MESSAGE_ENCRYPTION_KEY=$(openssl rand -base64 24)
FILE_ENCRYPTION_KEY=$(openssl rand -base64 24)
```

## 🆘 Problemas Comunes

### Los contenedores no inician

```bash
# Ver qué está pasando
docker-compose logs

# Reiniciar
docker-compose restart
```

### No puedo acceder por el dominio

1. Verifica que el dominio apunta correctamente al servidor:
   ```bash
   dig tu-dominio.com
   ```

2. Verifica que los certificados SSL están configurados:
   ```bash
   ls -la /etc/letsencrypt/live/tu-dominio.com/
   ```

3. Verifica la configuración de Nginx:
   ```bash
   docker-compose exec nginx nginx -t
   ```

4. Revisa los logs de Nginx:
   ```bash
   docker-compose logs nginx
   ```

### No puedo conectar WhatsApp

1. Verifica que el contenedor tenga recursos (al menos 2GB RAM)
2. Genera un nuevo código QR
3. Revisa los logs: `docker-compose logs backend`

### Error de base de datos

```bash
# Verificar que MySQL esté corriendo
docker-compose ps mysql

# Ver logs
docker-compose logs mysql
```

## 📚 Próximos Pasos

Una vez que tengas todo funcionando:

1. **Configura pagos** - Ver [CONFIGURACION_PROVEEDORES_PAGO.md](CONFIGURACION_PROVEEDORES_PAGO.md)
2. **Personaliza** - Configura colas, respuestas rápidas, etc.
3. **Lee la documentación** - [README.md](README.md) para más detalles
4. **Configura producción** - [INSTALLATION.md](INSTALLATION.md) para despliegue

## 🎉 ¡Listo!

Tu plataforma está funcionando. Ahora puedes:
- Gestionar conversaciones de WhatsApp
- Organizar tickets con etiquetas
- Procesar pagos (PayPal, Stripe, Mercado Pago, Clip)
- Enviar campañas masivas
- Configurar chatbot con IA
- Usar plantillas de mensajes
- Programar mensajes
- Y mucho más...

¿Necesitas ayuda? Consulta el [FAQ](FAQ.md) o la [documentación completa](README.md).

