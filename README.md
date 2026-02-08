# 🚀 Plataforma de Gestión de WhatsApp

<div align="center">

![Version](https://img.shields.io/badge/version-1.1.1-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Node](https://img.shields.io/badge/node-%3E%3D16.0.0-brightgreen.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-4.7-blue.svg)

**Solución completa para gestionar múltiples cuentas de WhatsApp Business con sistema de tickets, colas, pagos y más.**

[Características](#-características) • [Instalación](#-instalación-rápida) • [Documentación](#-documentación) • [Soporte](#-soporte)

</div>

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Características](#-características)
- [Tecnologías](#-tecnologías)
- [Requisitos del Sistema](#-requisitos-del-sistema)
- [Instalación Rápida](#-instalación-rápida)
- [Configuración](#-configuración)
- [Uso](#-uso)
- [Documentación](#-documentación)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [API](#-api)
- [Despliegue](#-despliegue)
- [Soporte](#-soporte)
- [Licencia](#-licencia)

---

## 🎯 Descripción

Plataforma empresarial completa para gestionar comunicaciones de WhatsApp Business. Permite conectar múltiples números de WhatsApp, gestionar conversaciones mediante un sistema de tickets, distribuir mensajes a través de colas, procesar pagos, enviar campañas masivas, automatizar respuestas con IA y mucho más.

**Características destacadas:**
- 🤖 Chatbot inteligente con IA para respuestas automáticas
- 📢 Sistema de campañas masivas con segmentación avanzada
- 💬 Widget integrable para convertir visitantes web a WhatsApp
- 💳 Procesamiento de pagos integrado
- 📊 Analytics y estadísticas en tiempo real

Ideal para:
- **Empresas** que necesitan gestionar atención al cliente por WhatsApp
- **Agencias** que manejan múltiples clientes
- **E-commerce** que requieren integración de pagos y automatización
- **Startups** que buscan automatizar su comunicación con IA
- **Negocios** que necesitan campañas masivas de marketing

---

## ✨ Características

### 🔌 Gestión de Conexiones
- ✅ Múltiples conexiones de WhatsApp simultáneas
- ✅ Conexión mediante QR Code
- ✅ Conexión mediante código de emparejamiento (pairing code)
- ✅ Estado de conexión en tiempo real
- ✅ Gestión de sesiones persistentes
- ✅ Reconexión automática
- ✅ Monitoreo de salud de conexiones
- ✅ Mensajes de bienvenida y despedida configurables

### 💬 Sistema de Tickets
- ✅ Conversaciones organizadas por tickets
- ✅ Estados: Abierto, Pendiente, Cerrado
- ✅ Asignación a usuarios/colas
- ✅ Historial completo de conversaciones
- ✅ Búsqueda avanzada de tickets
- ✅ Filtros y ordenamiento
- ✅ Mensajes fijados (pin/unpin) en conversaciones
- ✅ Importación de contactos de grupos desde conversaciones

### 👥 Gestión de Contactos
- ✅ Base de datos de contactos
- ✅ Importación masiva desde CSV
- ✅ Sincronización automática con WhatsApp
- ✅ Información de perfil (foto, nombre, estado)
- ✅ Historial completo de conversaciones por contacto
- ✅ Detección y fusión de contactos duplicados
- ✅ Notas personalizadas para contactos
- ✅ Score de satisfacción del cliente
- ✅ Segmentación avanzada de contactos
- ✅ Exportación e importación de participantes de grupos
- ✅ Gestión de grupos y participantes

### 📊 Colas de Atención
- ✅ Múltiples colas de atención
- ✅ Distribución automática de tickets
- ✅ Asignación manual de tickets
- ✅ Usuarios asignados a colas específicas
- ✅ Estadísticas por cola
- ✅ Mensajes de bienvenida y despedida configurables
- ✅ Horarios de negocio por cola
- ✅ Asignación de conexiones WhatsApp a colas

### 💳 Sistema de Pagos
- ✅ Integración con proveedores disponibles:
  - **PayPal** - Disponible y funcional
  - **Stripe** - Disponible y funcional
- ✅ Enlaces de pago públicos
- ✅ Webhooks para notificaciones
- ✅ Seguimiento de pagos
- ✅ Estadísticas de pagos
- ✅ Reintentos de pago
- ✅ Manejo de éxito y cancelación de pagos

### ⚡ Respuestas Rápidas
- ✅ Biblioteca de respuestas predefinidas
- ✅ Acceso rápido con comandos `/`
- ✅ Búsqueda de respuestas
- ✅ Personalización con variables

### 🤖 Chatbot con IA
- ✅ Chatbot inteligente con integración de IA
- ✅ Respuestas automáticas configurables
- ✅ Múltiples configuraciones simultáneas
- ✅ Sistema de prioridades
- ✅ Control de temperatura y tokens máximos
- ✅ Logs de interacciones del chatbot

### 📢 Campañas Masivas
- ✅ Sistema completo de envío masivo de mensajes
- ✅ Programación de envíos (fecha y hora)
- ✅ Plantillas de mensajes con variables dinámicas
- ✅ Segmentación avanzada (tags, colas, contactos, grupos, segmentos personalizados)
- ✅ Reportes de entrega y lectura en tiempo real
- ✅ Envío por lotes con pausas inteligentes
- ✅ Límites configurables (diarios, por lote)
- ✅ Soporte para multimedia (imágenes, videos, audio, documentos)
- ✅ Estados y seguimiento completo de campañas
- ✅ Operaciones en lote (actualizar, eliminar, procesar múltiples campañas)
- ✅ Procesamiento automático de campañas programadas
- ✅ Actualización de métricas en tiempo real
- ✅ Vista de destinatarios de campañas

### 🏷️ Sistema de Etiquetas (Tags)
- ✅ Etiquetas para tickets y contactos
- ✅ Organización y segmentación avanzada
- ✅ Importación masiva de etiquetas
- ✅ Asociación múltiple de etiquetas
- ✅ Asociación en lote a múltiples tickets

### 📝 Plantillas de Mensajes
- ✅ Biblioteca de plantillas reutilizables
- ✅ Variables dinámicas ({{nombre}}, {{fecha}}, etc.)
- ✅ Workflow de aprobación (borrador, pendiente, aprobado, rechazado)
- ✅ Categorización de plantillas
- ✅ Renderizado con variables del ticket/contacto
- ✅ Soporte para multimedia en plantillas
- ✅ Vista previa de plantillas antes de usar
- ✅ Renderizado dinámico con variables personalizadas

### ⚙️ Respuestas Automáticas (Auto-Replies)
- ✅ Respuestas automáticas por palabras clave
- ✅ Múltiples tipos de coincidencia (contiene, exacto, empieza con, termina con)
- ✅ Asignación por cola
- ✅ Activación/desactivación individual

### 🔄 Workflows Automatizados
- ✅ Sistema de automatización visual con editor de flujos
- ✅ Múltiples tipos de triggers (mensaje recibido, ticket creado, ticket cerrado, ticket asignado, ticket transferido)
- ✅ Nodos de acción (enviar mensaje, asignar usuario/cola, agregar/remover etiquetas, cambiar estado)
- ✅ Nodos de condición (simple y avanzada con lógica AND/OR)
- ✅ Nodos de espera (delay) con unidades configurables
- ✅ Filtros por colas y conexiones WhatsApp
- ✅ Ejecución en tiempo real con logs detallados
- ✅ Modo de simulación para probar workflows
- ✅ Validación de workflows antes de activar
- ✅ Historial completo de ejecuciones
- ✅ Prevención de ciclos infinitos y bucles

### 💬 Widget de Chat
- ✅ Widget integrable en sitios web
- ✅ Conversión de visitantes web a WhatsApp
- ✅ Personalización del widget (color, logo, mensajes)
- ✅ Conversaciones iniciadas desde el widget
- ✅ Estadísticas de conversiones desde el widget

### 🕐 Horarios de Negocio
- ✅ Configuración de horarios de atención
- ✅ Mensajes automáticos fuera de horario
- ✅ Días de la semana configurables
- ✅ Múltiples horarios por conexión

### 📅 Mensajes Programados
- ✅ Envío de mensajes programados
- ✅ Plantillas de mensajes reutilizables
- ✅ Gestión de mensajes pendientes
- ✅ Operaciones en lote (enviar ahora, eliminar, cancelar)
- ✅ Envío inmediato de mensajes programados

### 🔔 Webhooks
- ✅ Webhooks configurables por conexión
- ✅ Notificaciones de eventos en tiempo real
- ✅ Logs de webhooks con detalles completos
- ✅ Monitoreo de webhooks y estadísticas
- ✅ Reintentos automáticos en caso de fallo

### 📡 API REST
- ✅ API REST completa
- ✅ Documentación Swagger integrada
- ✅ Autenticación JWT con refresh tokens
- ✅ API Keys para integraciones externas
- ✅ Endpoints para todas las funcionalidades
- ✅ Más de 120 endpoints disponibles
- ✅ Endpoints de Workflows para automatización

### 🎨 Interfaz de Usuario
- ✅ Interfaz moderna y responsive
- ✅ Chat en tiempo real con Socket.IO
- ✅ Notificaciones de nuevos mensajes
- ✅ Soporte multi-idioma (ES/EN)
- ✅ Dashboard con estadísticas
- ✅ Tema personalizable
- ✅ Analytics y reportes exportables
- ✅ Configuración de notificaciones personalizables

### 🔒 Seguridad
- ✅ Autenticación JWT con refresh tokens
- ✅ Encriptación de mensajes
- ✅ Control de acceso basado en roles
- ✅ Validación de datos
- ✅ Protección CSRF

---

## 🛠 Tecnologías

### Backend
- **Node.js** + **TypeScript**
- **Express.js** - Framework web
- **Sequelize** - ORM para base de datos
- **Socket.IO** - Comunicación en tiempo real
- **whatsapp-web.js** - Integración con WhatsApp
- **JWT** - Autenticación
- **Jest** - Testing

### Frontend
- **React** - Biblioteca UI
- **Material-UI** - Componentes
- **Vite** - Build tool
- **Socket.IO Client** - Cliente WebSocket
- **i18next** - Internacionalización

### Base de Datos
- **MySQL/MariaDB** - Base de datos principal
- **PostgreSQL** - Soporte opcional

### Infraestructura
- **Docker** + **Docker Compose**
- **Nginx** - Reverse proxy
- **Certbot** - Certificados SSL

---

## 📦 Requisitos del Sistema

### Mínimos
- **CPU**: 2 cores
- **RAM**: 4 GB
- **Disco**: 20 GB libres
- **OS**: Linux (Ubuntu 20.04+ recomendado), macOS, Windows

### Recomendados
- **CPU**: 4+ cores
- **RAM**: 8+ GB
- **Disco**: 50+ GB SSD
- **OS**: Ubuntu 22.04 LTS

### Software Requerido
- **Docker** >= 20.10
- **Docker Compose** >= 2.0
- **Git**

---


### Conectar WhatsApp

1. Ve a **Conexiones** en el menú
2. Haz clic en **Nueva Conexión**
3. Escanea el código QR con tu WhatsApp
4. Espera a que se establezca la conexión

### Crear un Ticket

Los tickets se crean automáticamente cuando recibes un mensaje. También puedes:
- Enviar un mensaje a un número desde la API
- Importar conversaciones existentes

### Gestionar Colas

1. Ve a **Colas** en el menú
2. Crea una nueva cola
3. Asigna usuarios a la cola
4. Los tickets se distribuirán automáticamente

---

## 📚 Documentación


### 📖 Guías Principales


- **[API.md](docs/API.md)** - Documentación completa de la API REST
- **[MANUAL_DE_USUARIO.md](docs/MANUAL_DE_USUARIO.md)** - Manual completo de usuario

### ⚙️ Configuración

- **[VARIABLES_HORARIOS_NEGOCIO.md](docs/VARIABLES_HORARIOS_NEGOCIO.md)** - Configuración de horarios de negocio

### 🚀 Características Avanzadas
- **[CHATBOT_PROMPTS_EXAMPLES.md](docs/CHATBOT_PROMPTS_EXAMPLES.md)** - Ejemplos de prompts para chatbot con IA

### 📋 Planificación y Roadmap
- **[MEJORAS_Y_NUEVAS_FUNCIONES.md](docs/MEJORAS_Y_NUEVAS_FUNCIONES.md)** - 🚀 Mejoras recomendadas y nuevas funciones para aumentar el valor de la plataforma

### 🤝 Contribución y Seguridad
- **[SECURITY.md](SECURITY.md)** - Política de seguridad y reporte de vulnerabilidades
- **[CHANGELOG.md](docs/CHANGELOG.md)** - Historial de cambios y versiones
- **[PROJECT_SUMMARY.md](docs/PROJECT_SUMMARY.md)** - Resumen del proyecto


---


