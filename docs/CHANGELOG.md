# 📝 Changelog

Todos los cambios notables en este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [1.1.1] - 2024-12-XX

### ✨ Agregado
- Mejoras en la gestión de contactos duplicados
- Sistema de notas y score de satisfacción para contactos
- Segmentación avanzada de contactos con condiciones personalizadas
- Exportación e importación de participantes de grupos
- Historial completo de contactos
- Mensajes fijados (pin/unpin) en conversaciones
- Importación de contactos de grupos desde conversaciones
- Mejoras en el sistema de campañas masivas
- Operaciones en lote para campañas (actualizar, eliminar, procesar)
- Actualización de métricas de campañas
- Soporte mejorado para multimedia en campañas
- Sistema de listas de difusión (broadcast lists)
- Gestión de participantes en grupos
- Mejoras en el sistema de workflows automatizados
- Validación mejorada de workflows
- Ejecución manual de workflows con contexto personalizado
- Historial detallado de ejecuciones de workflows
- Mejoras en el sistema de pagos con Clip
- Procesamiento de cargos directos con tarjetas
- Webhooks mejorados para proveedores de pago
- Sistema de configuración de notificaciones personalizables
- Alertas de SLA configurables
- Mejoras en analytics y reportes exportables
- Soporte mejorado para múltiples idiomas

### 🔧 Mejorado
- Optimización de rendimiento en consultas de base de datos
- Mejoras en la gestión de sesiones de WhatsApp
- Sistema de reconexión automática mejorado
- Validación mejorada de datos en endpoints de API
- Manejo de errores más robusto
- Logs más detallados y estructurados
- Mejoras en la interfaz de usuario
- Optimización de carga de datos en el frontend
- Mejoras en la experiencia de usuario del chat

### 🐛 Corregido
- Correcciones en el sistema de envío de mensajes
- Mejoras en la gestión de estados de conexión WhatsApp
- Correcciones en el procesamiento de webhooks
- Mejoras en la validación de workflows
- Correcciones menores en la interfaz de usuario

### 📚 Documentación
- Actualización completa de la documentación de API
- Mejoras en las guías de instalación
- Documentación actualizada de nuevas funcionalidades
- Ejemplos mejorados de uso de la API

---

## [1.0.0] - 2024-01-XX

### ✨ Agregado
- Sistema completo de gestión de WhatsApp Business
- Múltiples conexiones de WhatsApp simultáneas
- Conexión mediante QR Code y código de emparejamiento (pairing code)
- Sistema de tickets para organizar conversaciones con SLA
- Sistema de etiquetas (tags) para tickets y contactos
- Gestión de contactos con importación masiva y campos personalizados
- Sistema de colas para distribución de tickets con horarios de negocio
- Respuestas rápidas con comandos `/`
- Respuestas automáticas (auto-replies) por palabras clave
- Sistema de pagos integrado (PayPal, Stripe)
- Webhooks configurables por conexión con logs y estadísticas
- API REST completa con más de 120 endpoints
- Documentación Swagger integrada
- Interfaz de usuario moderna y responsive
- Dashboard con estadísticas y analytics exportables
- Soporte multi-idioma (ES/EN)
- Autenticación JWT con refresh tokens
- API Keys para integraciones externas
- Encriptación de mensajes
- Control de acceso basado en roles
- Chatbot con IA (OpenAI, Anthropic, Google)
- Sistema de créditos para chatbot
- Campañas masivas con segmentación avanzada
- Plantillas de mensajes con workflow de aprobación
- Mensajes programados con operaciones en lote
- Widget de chat integrable
- Horarios de negocio configurables por cola
- Configuración de notificaciones personalizables
- Monitoreo de webhooks y logs
- Sistema de Workflows automatizados con editor visual
- Múltiples tipos de triggers para workflows (mensaje recibido, ticket creado/cerrado/asignado/transferido)
- Nodos de acción (enviar mensaje, asignar usuario/cola, etiquetas, cambiar estado)
- Nodos de condición (simple y avanzada con lógica AND/OR)
- Nodos de espera (delay) con unidades configurables
- Ejecución de workflows con logs detallados
- Modo de simulación para probar workflows
- Validación de workflows antes de activar
- Historial completo de ejecuciones
- Documentación completa (README, INSTALLATION, API, etc.)

### 🔧 Configuración
- Docker Compose para desarrollo y producción
- Configuración de Nginx como reverse proxy
- Soporte para MySQL/MariaDB y PostgreSQL
- Variables de entorno configurables
- Scripts de migración de base de datos

### 📚 Documentación
- README.md completo y profesional
- Guía de instalación detallada (INSTALLATION.md)
- Guía de desarrollo (DEVELOPMENT.md)
- Documentación de API (API.md)
- Guía de contribución (CONTRIBUTING.md)
- Documentación de configuración de pagos
- Guías de widget y chatbot

---

## Tipos de Cambios

- **Agregado** para nuevas funcionalidades
- **Modificado** para cambios en funcionalidades existentes
- **Deprecado** para funcionalidades que serán eliminadas
- **Eliminado** para funcionalidades eliminadas
- **Corregido** para correcciones de bugs
- **Seguridad** para vulnerabilidades

---

## [Sin Versión] - Próximas Versiones

### 🎯 Planeado
- ✅ Sistema de analytics avanzado (IMPLEMENTADO)
- ✅ Chatbot con IA integrado (IMPLEMENTADO - OpenAI, Anthropic, Google)
- ✅ Campañas masivas de mensajería (IMPLEMENTADO)
- ✅ Sistema de etiquetas para tickets (IMPLEMENTADO)
- ✅ Horarios de atención automáticos (IMPLEMENTADO)
- ✅ Plantillas de mensajes (IMPLEMENTADO)
- ✅ Mensajes programados (IMPLEMENTADO)
- ✅ Sistema de Workflows automatizados (IMPLEMENTADO)
- Integraciones con CRM populares
- App móvil (React Native)
- Sistema de suscripciones y planes
- White-label completo
- Notificaciones push avanzadas
- Exportación/importación mejorada
- Autenticación 2FA
- Logs de auditoría completos
- Más proveedores de IA
- Integraciones con más plataformas de pago

---

## Notas de Versión

### Versión 1.1.1
Versión con mejoras significativas en gestión de contactos, campañas y workflows. Incluye nuevas funcionalidades de segmentación, notas de contactos y operaciones en lote.

**Mejoras principales:**
- Sistema completo de gestión de contactos duplicados
- Notas y score de satisfacción para contactos
- Segmentación avanzada de contactos
- Operaciones en lote para campañas
- Mejoras en workflows automatizados
- Sistema de listas de difusión
- Mejoras en procesamiento de pagos

**Requisitos mínimos:**
- Node.js >= 16.0.0
- Docker >= 20.10
- 4GB RAM (8GB recomendado)

### Versión 1.0.0
Primera versión estable del producto. Incluye todas las funcionalidades core para gestión de WhatsApp Business.

**Características principales:**
- Gestión completa de conversaciones
- Sistema de tickets profesional con SLA y etiquetas
- Integración de pagos (PayPal, Stripe)
- Chatbot con IA (3 proveedores: OpenAI, Anthropic, Google)
- Campañas masivas con segmentación avanzada
- Plantillas de mensajes con workflow de aprobación
- Mensajes programados con operaciones en lote
- Sistema de Workflows automatizados
- API REST completa (120+ endpoints)
- Interfaz moderna con analytics exportables

**Requisitos mínimos:**
- Node.js >= 16.0.0
- Docker >= 20.10
- 4GB RAM (8GB recomendado)

---

Para más información sobre cambios específicos, consulta los commits en el repositorio.
