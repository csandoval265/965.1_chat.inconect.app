# 📦 Guía de Instalación Completa

Esta guía te ayudará a instalar y configurar la Plataforma de Gestión de WhatsApp Business paso a paso.

## 📋 Tabla de Contenidos

- [Requisitos Previos](#-requisitos-previos)
- [Instalación con Docker (Recomendado)](#-instalación-con-docker-recomendado)
- [Instalación Manual](#-instalación-manual)
- [Configuración Inicial](#-configuración-inicial)
- [Configuración de Producción](#-configuración-de-producción)
- [Verificación](#-verificación)
- [Solución de Problemas](#-solución-de-problemas)

---

## 🔧 Requisitos Previos

### Software Necesario

1. **Docker** (versión 20.10 o superior)
   ```bash
   # Ubuntu/Debian
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   
   # Verificar instalación
   docker --version
   docker-compose --version
   ```

2. **Docker Compose** (versión 2.0 o superior)
   - Generalmente incluido con Docker Desktop
   - Para Linux, puede requerir instalación separada

3. **Git**
   ```bash
   sudo apt-get update
   sudo apt-get install git
   ```

4. **Editor de texto** (nano, vim, o tu preferido)

### Requisitos del Sistema

- **RAM**: Mínimo 4GB (8GB recomendado para producción)
- **Disco**: Mínimo 20GB libres (50GB+ recomendado para producción)
- **CPU**: Mínimo 2 cores (4+ cores recomendado para producción)
- **OS**: Linux (Ubuntu 20.04+ recomendado), macOS, o Windows con WSL2
- **Node.js**: >= 16.0.0 (si instalación manual)
- **MySQL/MariaDB**: >= 10.6 (si instalación manual)

---

## 🐳 Instalación con Docker (Recomendado)

Esta es la forma más sencilla y recomendada de instalar la plataforma.

**⚠️ IMPORTANTE**: Este proyecto usa Nginx como reverse proxy y **requiere configuración de dominio y certificados SSL**. No funciona directamente por puertos de desarrollo (localhost:puerto).

### Paso 1: Clonar el Repositorio

```bash
git clone <repository-url>
cd WhatsApp
```

### Paso 2: Configurar Dominio y Certificados SSL

Antes de iniciar los servicios, debes:

1. **Configurar tu dominio DNS**:
   - Apunta tu dominio (ej: `whatsapp.tu-dominio.com`) al IP de tu servidor
   - Apunta el subdominio de API (ej: `whatsapp-api.tu-dominio.com`) al mismo IP

2. **Obtener certificados SSL**:

   **Opción A: Let's Encrypt (Recomendado)**
   ```bash
   # Instalar Certbot
   sudo apt-get update
   sudo apt-get install certbot
   
   # Obtener certificados
   sudo certbot certonly --standalone -d tu-dominio.com -d www.tu-dominio.com
   
   # Los certificados estarán en /etc/letsencrypt/live/tu-dominio.com/
   ```

   **Opción B: Certificados propios**
   ```bash
   # Coloca tus certificados en:
   docker/nginx/certs/tu-dominio.pem
   docker/nginx/certs/tu-dominio-key.pem
   ```

3. **Configurar Nginx**:
   - Edita `docker/nginx/conf/${APP_ENV}/whatsapp.conf` con tu dominio
   - Edita `docker/nginx/conf/${APP_ENV}/whatsapp-api.conf` con tu dominio API
   - O crea una nueva configuración en `docker/nginx/conf/tu-entorno/`

### Paso 3: Configurar Variables de Entorno

```bash
# Copiar archivo de ejemplo
cp env.example .env

# Editar el archivo .env
nano .env
```

### Paso 3: Configurar Variables Básicas

Edita el archivo `.env` con los siguientes valores mínimos:

```env
# Base de Datos
MYSQL_ROOT_PASSWORD=tu_contraseña_root_segura
MYSQL_DATABASE=whatsapp
MYSQL_USER=whatsapp_user
MYSQL_PASSWORD=tu_contraseña_usuario_segura
MYSQL_PORT=3307

# JWT Secrets (genera valores aleatorios seguros)
JWT_SECRET=$(openssl rand -base64 32)
JWT_REFRESH_SECRET=$(openssl rand -base64 32)

# Encriptación de Mensajes (32 caracteres)
MESSAGE_ENCRYPTION_KEY=$(openssl rand -base64 24)

# Encriptación de Archivos (32 caracteres)
FILE_ENCRYPTION_KEY=$(openssl rand -base64 24)

# URLs (usar HTTPS con tu dominio, NO localhost:puerto)
BACKEND_URL=https://whatsapp-api.tu-dominio.com
FRONTEND_URL=https://whatsapp.tu-dominio.com

# Nginx - Configuración de dominios
APP_ENV=develop  # o el nombre de tu entorno (debe coincidir con carpeta en docker/nginx/conf/)
FRONTEND_SERVER_NAME=whatsapp.tu-dominio.com
BACKEND_SERVER_NAME=whatsapp-api.tu-dominio.com

# Nginx
NGINX_HTTP_PORT=80
NGINX_HTTPS_PORT=443
```

**⚠️ IMPORTANTE**: 
- Genera valores aleatorios y seguros para los secrets
- En producción, usa HTTPS y URLs reales
- Guarda estos valores de forma segura

### Paso 4: Crear Directorios Necesarios

```bash
# Crear directorios para volúmenes
mkdir -p backend/public
mkdir -p backend/.wwebjs_auth
mkdir -p docker/mysql/backup
mkdir -p docker/nginx/certs

# Si usas certificados propios, colócalos aquí:
# docker/nginx/certs/tu-dominio.pem
# docker/nginx/certs/tu-dominio-key.pem
```

### Paso 5: Verificar Configuración de Nginx

Antes de iniciar, verifica que:
- Los archivos de configuración en `docker/nginx/conf/${APP_ENV}/` tienen tus dominios correctos
- Los certificados SSL están en `/etc/letsencrypt` (Let's Encrypt) o `docker/nginx/certs` (propios)
- Las variables `FRONTEND_SERVER_NAME` y `BACKEND_SERVER_NAME` coinciden con tus dominios

### Paso 6: Iniciar los Servicios

```bash
# Construir e iniciar todos los servicios
docker-compose up -d

# Ver el estado de los contenedores
docker-compose ps

# Ver logs en tiempo real
docker-compose logs -f
```

### Paso 7: Esperar a que los Servicios Estén Listos

Espera aproximadamente 1-2 minutos para que:
- MySQL se inicialice
- Las migraciones de base de datos se ejecuten
- El backend compile y se inicie
- El frontend se construya

Puedes verificar el estado con:

```bash
# Ver logs del backend
docker-compose logs backend

# Ver logs de MySQL
docker-compose logs mysql

# Verificar que MySQL esté listo
docker-compose exec mysql mysqladmin ping -h localhost
```

---

## 💻 Instalación Manual

Si prefieres instalar sin Docker, sigue estos pasos:

### Paso 1: Instalar Node.js

```bash
# Instalar Node.js 18+ (usando nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 18
nvm use 18
```

### Paso 2: Instalar MySQL

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install mysql-server

# Iniciar MySQL
sudo systemctl start mysql
sudo systemctl enable mysql

# Configurar MySQL
sudo mysql_secure_installation
```

### Paso 3: Crear Base de Datos

```bash
mysql -u root -p

# En MySQL
CREATE DATABASE whatsapp CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'whatsapp_user'@'localhost' IDENTIFIED BY 'tu_contraseña';
GRANT ALL PRIVILEGES ON whatsapp.* TO 'whatsapp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Paso 4: Configurar Backend

```bash
cd backend

# Instalar dependencias
npm install --legacy-peer-deps

# Crear archivo .env
cp env.example .env
nano .env
```

Configura el `.env` del backend:

```env
DB_HOST=localhost
DB_USER=whatsapp_user
DB_PASS=tu_contraseña
DB_NAME=whatsapp
DB_DIALECT=mysql

JWT_SECRET=tu_secret_jwt
JWT_REFRESH_SECRET=tu_refresh_secret
MESSAGE_ENCRYPTION_KEY=tu_clave_32_caracteres
FILE_ENCRYPTION_KEY=tu_clave_archivos_32_caracteres

BACKEND_URL=http://localhost:8080
FRONTEND_URL=http://localhost:3000
```

### Paso 5: Ejecutar Migraciones

```bash
# Ejecutar migraciones
npm run db:migrate

# (Opcional) Ejecutar seeds
npm run db:seed
```

### Paso 6: Compilar y Ejecutar Backend

```bash
# Compilar TypeScript
npm run build

# Ejecutar en desarrollo
npm run dev

# O en producción
npm start
```

### Paso 7: Configurar Frontend

```bash
cd ../frontend

# Instalar dependencias
npm install

# Crear archivo .env
echo "VITE_BACKEND_URL=http://localhost:8080" > .env

# Ejecutar en desarrollo
npm run dev

# O construir para producción
npm run build
```

---

## ⚙️ Configuración Inicial

### Paso 1: Acceder a la Aplicación

1. Abre tu navegador
2. Ve a `https://tu-dominio.com` (usa HTTPS, NO localhost:puerto)
3. Deberías ver la página de login

### Paso 2: Crear Usuario Administrador

Si es la primera vez que accedes, crea tu usuario:

1. Haz clic en **Registrarse** o **Sign Up**
2. Completa el formulario:
   - Email
   - Nombre
   - Contraseña
3. El primer usuario será automáticamente administrador

### Paso 3: Conectar WhatsApp

1. Ve a **Conexiones** en el menú lateral
2. Haz clic en **Nueva Conexión**
3. Ingresa un nombre para la conexión
4. Escanea el código QR con tu WhatsApp:
   - Abre WhatsApp en tu teléfono
   - Ve a Configuración > Dispositivos vinculados
   - Toca "Vincular un dispositivo"
   - Escanea el código QR
5. Espera a que se establezca la conexión (verás el estado cambiar a "CONNECTED")

### Paso 4: Configurar Colas (Opcional)

1. Ve a **Colas** en el menú
2. Crea una nueva cola
3. Asigna usuarios a la cola
4. Los tickets se distribuirán automáticamente

---

## 🚀 Configuración de Producción

### Paso 1: Configurar Dominio y SSL

**⚠️ CRÍTICO**: Sin dominio y certificados SSL configurados, la aplicación NO funcionará.

#### Opción A: Con Certbot (Let's Encrypt) - Recomendado

```bash
# 1. Instalar Certbot
sudo apt-get update
sudo apt-get install certbot

# 2. Detener Nginx temporalmente si está corriendo
docker-compose stop nginx

# 3. Obtener certificados
sudo certbot certonly --standalone \
  -d tu-dominio.com \
  -d www.tu-dominio.com \
  -d whatsapp-api.tu-dominio.com

# 4. Los certificados estarán en:
# /etc/letsencrypt/live/tu-dominio.com/fullchain.pem
# /etc/letsencrypt/live/tu-dominio.com/privkey.pem

# 5. Configurar renovación automática
sudo certbot renew --dry-run
```

#### Opción B: Con Certificados Existentes

1. Coloca tus certificados SSL en `docker/nginx/certs/`:
   ```bash
   docker/nginx/certs/tu-dominio.pem
   docker/nginx/certs/tu-dominio-key.pem
   ```

2. Actualiza la configuración de Nginx en `docker/nginx/conf/${APP_ENV}/whatsapp.conf`:
   ```nginx
   ssl_certificate     /etc/nginx/certs/tu-dominio.pem;
   ssl_certificate_key /etc/nginx/certs/tu-dominio-key.pem;
   ```

### Paso 2: Variables de Entorno de Producción

Asegúrate de configurar en `.env`:

```env
# URLs de producción (usar HTTPS con dominios reales)
BACKEND_URL=https://whatsapp-api.tudominio.com
FRONTEND_URL=https://whatsapp.tudominio.com

# Nginx - Configuración de dominios
APP_ENV=produccion  # o el nombre de tu entorno
FRONTEND_SERVER_NAME=whatsapp.tudominio.com
BACKEND_SERVER_NAME=whatsapp-api.tudominio.com

# Secrets seguros (genera nuevos)
JWT_SECRET=secret_produccion_muy_seguro
JWT_REFRESH_SECRET=refresh_secret_produccion_muy_seguro
MESSAGE_ENCRYPTION_KEY=clave_32_caracteres_produccion
FILE_ENCRYPTION_KEY=clave_archivos_32_caracteres_produccion

# Base de datos
MYSQL_ROOT_PASSWORD=contraseña_root_muy_segura
MYSQL_PASSWORD=contraseña_usuario_muy_segura

# Configuración de producción
NODE_ENV=production
```

### Paso 3: Configurar Firewall

```bash
# Permitir puertos necesarios
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp  # SSH
sudo ufw enable
```

### Paso 4: Configurar Backup Automático

```bash
# Crear script de backup
cat > backup.sh << 'EOF'
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
docker-compose exec -T mysql mysqldump -u root -p$MYSQL_ROOT_PASSWORD whatsapp > backup_$DATE.sql
EOF

chmod +x backup.sh

# Agregar a crontab (backup diario a las 2 AM)
(crontab -l 2>/dev/null; echo "0 2 * * * /ruta/al/backup.sh") | crontab -
```

### Paso 5: Monitoreo (Opcional)

Considera configurar:
- **PM2** para gestión de procesos
- **Nginx logs** para monitoreo
- **Health checks** para los servicios

---

## ✅ Verificación

### Verificar que Todo Funciona

1. **Backend API**
   ```bash
   curl https://whatsapp-api.tu-dominio.com/health
   # Debería responder con estado OK
   ```

2. **Frontend**
   - Abre `https://tu-dominio.com` (usa HTTPS)
   - Deberías ver la página de login
   - Verifica que el certificado SSL es válido

3. **Base de Datos**
   ```bash
   docker-compose exec mysql mysql -u root -p -e "SHOW DATABASES;"
   # Deberías ver la base de datos 'whatsapp'
   ```

4. **Vista de Endpoints (`/api-docs`)**
   - Accede a la aplicación web en `https://tu-dominio.com`
   - Inicia sesión con tu usuario
   - Ve a **API Docs** en el menú lateral o accede directamente a `https://tu-dominio.com/api-docs`
   - Deberías ver la vista de endpoints con todos los endpoints disponibles organizados por categorías

5. **Conexión WhatsApp**
   - Ve a Conexiones
   - El estado debería ser "CONNECTED"
   - Envía un mensaje de prueba

---

## 🔧 Solución de Problemas

### Problema: Los contenedores no inician

**Solución:**
```bash
# Ver logs detallados
docker-compose logs

# Verificar recursos del sistema
docker stats

# Reiniciar servicios
docker-compose down
docker-compose up -d
```

### Problema: Error de conexión a MySQL

**Solución:**
```bash
# Verificar que MySQL esté corriendo
docker-compose ps mysql

# Ver logs de MySQL
docker-compose logs mysql

# Verificar credenciales en .env
cat .env | grep MYSQL
```

### Problema: WhatsApp no se conecta

**Solución:**
```bash
# Ver logs del backend
docker-compose logs backend | grep -i whatsapp

# Verificar que Chrome esté instalado en el contenedor
docker-compose exec backend which google-chrome-stable

# Reiniciar la conexión
# Ve a Conexiones > Editar > Desconectar > Conectar
```

### Problema: Frontend no carga

**Solución:**
```bash
# Ver logs del frontend
docker-compose logs frontend

# Verificar que el build se completó
docker-compose exec frontend ls -la /usr/src/app/dist

# Reconstruir frontend
docker-compose up -d --build frontend
```

### Problema: Error 502 Bad Gateway

**Solución:**
```bash
# Verificar que Nginx esté corriendo
docker-compose ps nginx

# Ver logs de Nginx
docker-compose logs nginx

# Verificar configuración de Nginx
docker-compose exec nginx nginx -t

# Verificar que los dominios están configurados correctamente
docker-compose exec nginx cat /etc/nginx/sites-available/whatsapp.conf | grep server_name

# Verificar que los certificados SSL existen
ls -la /etc/letsencrypt/live/tu-dominio.com/
# o
ls -la docker/nginx/certs/
```

### Problema: No puedo acceder por el dominio

**Solución:**
```bash
# 1. Verificar DNS
dig tu-dominio.com
nslookup tu-dominio.com

# 2. Verificar que el dominio apunta al servidor
ping tu-dominio.com

# 3. Verificar certificados SSL
openssl s_client -connect tu-dominio.com:443 -servername tu-dominio.com

# 4. Verificar configuración de Nginx
docker-compose exec nginx nginx -T | grep -A 5 "server_name"

# 5. Verificar que APP_ENV coincide con la carpeta de configuración
echo $APP_ENV
ls docker/nginx/conf/
```

### Problema: Migraciones fallan

**Solución:**
```bash
# Ejecutar migraciones manualmente
docker-compose exec backend npm run db:migrate

# Si hay errores, verificar la conexión a la BD
docker-compose exec backend npm run db:migrate:status
```

### Problema: Puerto ya en uso

**Solución:**
```bash
# Ver qué está usando el puerto
sudo lsof -i :3000
sudo lsof -i :8080
sudo lsof -i :3307

# Cambiar puertos en .env
NGINX_HTTP_PORT=8080
NGINX_HTTPS_PORT=8443
MYSQL_PORT=3308
```

---

## 📝 Comandos Útiles

```bash
# Ver estado de contenedores
docker-compose ps

# Ver logs
docker-compose logs -f [servicio]

# Reiniciar un servicio
docker-compose restart [servicio]

# Detener todos los servicios
docker-compose down

# Detener y eliminar volúmenes (⚠️ elimina datos)
docker-compose down -v

# Reconstruir un servicio
docker-compose up -d --build [servicio]

# Ejecutar comandos en un contenedor
docker-compose exec [servicio] [comando]

# Ver uso de recursos
docker stats

# Limpiar sistema Docker
docker system prune -a
```

---

## 🎉 ¡Listo!

Si has seguido todos los pasos, tu plataforma debería estar funcionando correctamente. 

**Próximos pasos:**
1. Configura tus proveedores de pago (ver [CONFIGURACION_PROVEEDORES_PAGO.md](CONFIGURACION_PROVEEDORES_PAGO.md))
2. Crea tus colas de atención
3. Configura respuestas rápidas
4. Personaliza la plataforma según tus necesidades

**¿Necesitas ayuda?** Consulta la sección de [Soporte](../README.md#-soporte) en el README principal.

