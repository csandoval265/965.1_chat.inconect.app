# Manual de Usuario - Plataforma de Gestión de WhatsApp

## Índice

1. [Introducción](#introducción)
2. [Autenticación](#autenticación)
   - [Inicio de Sesión](#inicio-de-sesión)
   - [Registro de Usuario](#registro-de-usuario)
3. [Página Principal - Analytics](#página-principal---analytics)
4. [Gestión de Tickets](#gestión-de-tickets)
5. [Conexiones de WhatsApp](#conexiones-de-whatsapp)
6. [Contactos](#contactos)
7. [Usuarios](#usuarios)
8. [Respuestas Rápidas](#respuestas-rápidas)
9. [Plantillas de Mensajes](#plantillas-de-mensajes)
10. [Colas (Queues)](#colas-queues)
11. [Pagos](#pagos)
12. [Etiquetas (Tags)](#etiquetas-tags)
13. [Respuestas Automáticas](#respuestas-automáticas)
14. [Configuración de Chatbot](#configuración-de-chatbot)
15. [Campañas](#campañas)
16. [Mensajes Programados](#mensajes-programados)
17. [Widgets de Chat](#widgets-de-chat)
18. [Workflows Automatizados](#workflows-automatizados)
19. [Configuración General](#configuración-general)
20. [Configuración de Notificaciones](#configuración-de-notificaciones)
21. [Documentación de API](#documentación-de-api)
21. [Monitor de Socket.IO](#monitor-de-socketio)
22. [Monitor de Webhooks](#monitor-de-webhooks)
23. [Páginas de Pago Públicas](#páginas-de-pago-públicas)

---

## Introducción

Este manual cubre todas las funcionalidades de la plataforma de gestión de WhatsApp. La plataforma permite gestionar conversaciones, contactos, campañas, pagos y mucho más a través de una interfaz web intuitiva.

### Características Principales

- **Gestión de Conversaciones**: Administra tickets y conversaciones de WhatsApp
- **Gestión de Contactos**: Importa, exporta y gestiona tu base de contactos
- **Campañas Masivas**: Crea y gestiona campañas de mensajería masiva
- **Automatización**: Respuestas automáticas y chatbots con IA
- **Analytics**: Métricas y estadísticas en tiempo real
- **Pagos**: Integración con múltiples pasarelas de pago
- **Widgets**: Integra chat en tu sitio web

---

## Autenticación

### Inicio de Sesión

**Ruta**: `/login`

#### Funcionalidades

1. **Campos de Entrada**:
   - **Email**: Dirección de correo electrónico del usuario
   - **Contraseña**: Contraseña de acceso
   - **Mostrar/Ocultar Contraseña**: Botón para alternar la visibilidad de la contraseña

2. **Acciones Disponibles**:
   - **Iniciar Sesión**: Autentica al usuario y redirige al dashboard
   - **Registrarse**: Enlace para crear una nueva cuenta

3. **Validaciones**:
   - El email debe tener un formato válido
   - La contraseña es obligatoria
   - Se muestran mensajes de error si las credenciales son incorrectas

4. **Comportamiento**:
   - Si el usuario ya está autenticado, se redirige automáticamente al dashboard
   - Los errores de autenticación se muestran mediante notificaciones toast

---

### Registro de Usuario

**Ruta**: `/signup`

#### Funcionalidades

1. **Campos de Registro**:
   - **Nombre**: Nombre completo del usuario
   - **Email**: Dirección de correo electrónico (debe ser único)
   - **Contraseña**: Contraseña de acceso (mínimo 6 caracteres)
   - **Confirmar Contraseña**: Verificación de la contraseña
   - **Mostrar/Ocultar Contraseña**: Botón para alternar la visibilidad

2. **Validaciones**:
   - El nombre es obligatorio
   - El email debe tener formato válido y ser único
   - La contraseña debe tener al menos 6 caracteres
   - Las contraseñas deben coincidir

3. **Acciones**:
   - **Registrarse**: Crea la cuenta y redirige al login
   - **Iniciar Sesión**: Enlace para usuarios existentes

4. **Mensajes de Error**:
   - Email ya registrado
   - Contraseñas no coinciden
   - Campos obligatorios faltantes

---

## Página Principal - Analytics

**Ruta**: `/` o `/analytics`

### Descripción General

La página de Analytics proporciona una visión completa del rendimiento de la plataforma con métricas, gráficos y estadísticas en tiempo real.

### Funcionalidades Principales

#### 1. Filtros de Datos

- **Rango de Fechas**: 
  - Últimos 7 días (por defecto)
  - Últimos 15 días
  - Últimos 30 días
  - Últimos 90 días
  - Personalizado (fecha inicio y fin)

- **Filtro por Cola**:
  - Todas las colas
  - Cola específica seleccionada

- **Aplicar Filtros**: Botón para aplicar los filtros seleccionados
- **Limpiar Filtros**: Restablece los filtros a valores por defecto

#### 2. Métricas Principales (Overview)

- **Total de Tickets**: Número total de tickets en el período
- **Tickets Abiertos**: Tickets actualmente abiertos
- **Tickets Cerrados**: Tickets cerrados en el período
- **Tasa de Resolución**: Porcentaje de tickets resueltos
- **Tiempo Promedio de Resolución**: Tiempo promedio para cerrar tickets
- **Tiempo de Respuesta Promedio**: Tiempo promedio de primera respuesta
- **Mensajes Enviados**: Total de mensajes enviados
- **Mensajes Recibidos**: Total de mensajes recibidos

#### 3. Gráficos de Rendimiento

- **Gráfico de Tickets por Día**: Línea temporal mostrando la evolución de tickets
- **Gráfico de Mensajes por Día**: Línea temporal de mensajes enviados/recibidos
- **Gráfico de Rendimiento de Agentes**: Comparación de rendimiento entre agentes
- **Distribución de Tickets por Estado**: Gráfico de pastel con estados (abierto, pendiente, cerrado)
- **Distribución de Tickets por Cola**: Gráfico de barras por cola
- **Distribución de Tickets por Etiquetas**: Gráfico de barras por etiquetas

#### 4. Estadísticas de Mensajes

- **Mensajes por Tipo**: Texto, imagen, audio, video, documento
- **Mensajes por Hora del Día**: Distribución horaria
- **Mensajes por Día de la Semana**: Distribución semanal

#### 5. Métricas de SLA (Service Level Agreement)

- **Tickets dentro del SLA**: Porcentaje de tickets resueltos dentro del tiempo acordado
- **Tiempo de Respuesta dentro del SLA**: Porcentaje de respuestas dentro del SLA
- **Tickets fuera del SLA**: Número y porcentaje de tickets fuera del SLA

#### 6. Acciones Disponibles

- **Actualizar Datos**: Botón de refresh para recargar los datos
- **Exportar Reporte**: Exporta los datos en formato CSV/Excel (próximamente)
- **Filtros Avanzados**: Modal con opciones de filtrado detalladas

#### 7. Visualización

- Todos los gráficos son interactivos
- Tooltips al pasar el mouse sobre los datos
- Gráficos responsivos que se adaptan al tamaño de pantalla
- Colores diferenciados para mejor visualización

---

## Gestión de Tickets

**Ruta**: `/tickets` o `/tickets/:ticketId`

### Descripción General

La página de Tickets es el centro de comunicación de la plataforma. Permite gestionar todas las conversaciones de WhatsApp organizadas como tickets.

### Funcionalidades Principales

#### 1. Lista de Tickets (TicketsManager)

**Panel Lateral Izquierdo**:

- **Búsqueda de Tickets**: 
  - Campo de búsqueda para filtrar tickets por contacto, número, etiquetas
  - Búsqueda en tiempo real

- **Filtros de Tickets**:
  - **Estado**: Todos, Abiertos, Pendientes, Cerrados
  - **Cola**: Filtrar por cola específica
  - **Usuario**: Filtrar por usuario asignado
  - **Etiquetas**: Filtrar por etiquetas
  - **Fecha**: Rango de fechas

- **Ordenamiento**:
  - Por fecha (más recientes primero)
  - Por estado
  - Por prioridad

- **Información por Ticket**:
  - Avatar del contacto
  - Nombre del contacto
  - Último mensaje (preview)
  - Hora del último mensaje
  - Badge de mensajes no leídos
  - Estado del ticket (color indicador)
  - Etiquetas asociadas

- **Indicadores Visuales**:
  - Tickets no leídos destacados
  - Tickets pendientes con indicador especial
  - Tickets fuera del SLA resaltados

#### 2. Vista de Conversación (Ticket)

**Panel Principal Derecho**:

- **Encabezado del Ticket**:
  - Nombre y número del contacto
  - Estado del ticket (abierto, pendiente, cerrado)
  - Cola asignada
  - Usuario asignado
  - Etiquetas
  - Botones de acción rápida

- **Área de Mensajes**:
  - **Historial de Conversación**:
    - Mensajes ordenados cronológicamente
    - Diferenciación visual entre mensajes enviados y recibidos
    - Timestamp de cada mensaje
    - Estado de entrega (enviado, entregado, leído)
  
  - **Tipos de Mensajes Soportados**:
    - Texto plano
    - Imágenes (con preview)
    - Videos (con preview)
    - Audios (reproductor integrado)
    - Documentos (con nombre y tamaño)
    - Ubicaciones (mapa integrado)
    - Contactos
    - Stickers

- **Composer de Mensajes**:
  - **Campo de Texto**: Editor de mensajes con soporte para:
    - Emojis
    - Formato de texto (negrita, cursiva)
    - Variables dinámicas
  
  - **Adjuntos**:
    - Botón para adjuntar imágenes
    - Botón para adjuntar documentos
    - Botón para enviar ubicación
    - Botón para enviar contacto
  
  - **Respuestas Rápidas**: Acceso rápido a respuestas predefinidas
  - **Plantillas**: Selección de plantillas de mensajes aprobadas
  - **Enviar**: Botón para enviar el mensaje

- **Acciones del Ticket**:
  - **Asignar Usuario**: Asignar ticket a un agente específico
  - **Transferir a Cola**: Mover ticket a otra cola
  - **Agregar Etiquetas**: Asociar etiquetas al ticket
  - **Marcar como Pendiente**: Pausar el ticket
  - **Cerrar Ticket**: Finalizar la conversación
  - **Reabrir Ticket**: Reabrir un ticket cerrado
  - **Ver Historial**: Historial completo de cambios del ticket

#### 3. Funcionalidades Avanzadas

- **Notificaciones en Tiempo Real**:
  - Nuevos mensajes aparecen automáticamente
  - Indicador de escritura cuando el contacto está escribiendo
  - Actualización de estado de entrega en tiempo real

- **Búsqueda en Conversación**:
  - Buscar mensajes específicos dentro de la conversación
  - Navegación entre resultados

- **Información del Contacto**:
  - Panel lateral con información del contacto
  - Historial de tickets anteriores
  - Notas y observaciones

- **Atajos de Teclado**:
  - `Enter`: Enviar mensaje
  - `Shift + Enter`: Nueva línea
  - `Ctrl/Cmd + K`: Buscar tickets
  - `Esc`: Cerrar modales

#### 4. Responsive Design

- En dispositivos móviles, la lista de tickets se oculta cuando se selecciona un ticket
- Navegación táctil optimizada
- Interfaz adaptativa según el tamaño de pantalla

---

## Conexiones de WhatsApp

**Ruta**: `/connections`

### Descripción General

Esta página permite gestionar las conexiones de WhatsApp Business API. Puedes conectar múltiples números de WhatsApp y gestionar sus sesiones.

### Funcionalidades Principales

#### 1. Lista de Conexiones

**Tabla de Conexiones**:

- **Columnas Visibles**:
  - **Nombre**: Nombre identificador de la conexión
  - **Número**: Número de teléfono asociado
  - **Estado**: Estado de la conexión (conectado, desconectado, QR pendiente, etc.)
  - **Perfil**: Nombre del perfil de WhatsApp
  - **Colas Asociadas**: Colas vinculadas a esta conexión
  - **Última Actualización**: Fecha y hora de última actualización
  - **Acciones**: Botones de acción

- **Estados de Conexión**:
  - 🟢 **CONNECTED**: Conectado y funcionando
  - 🔴 **DISCONNECTED**: Desconectado
  - 🟡 **QRCODE**: Esperando escaneo de QR
  - 🟡 **PAIRING**: En proceso de emparejamiento
  - 🟡 **OPENING**: Abriendo sesión
  - 🔴 **TIMEOUT**: Tiempo de espera agotado

#### 2. Acciones Disponibles

- **Agregar Nueva Conexión**:
  - Modal con formulario para crear nueva conexión
  - Campos: Nombre, número de teléfono, colas asociadas
  - Validación de número de teléfono

- **Editar Conexión**:
  - Modificar nombre
  - Cambiar colas asociadas
  - Actualizar configuración

- **Eliminar Conexión**:
  - Confirmación antes de eliminar
  - Eliminación en cascada de relaciones

- **Iniciar Sesión**:
  - Inicia una nueva sesión de WhatsApp
  - Genera código QR para escanear

- **Detener Sesión**:
  - Cierra la sesión activa
  - Desconecta el número

- **Solicitar Nuevo QR**:
  - Regenera el código QR si expiró
  - Útil cuando el QR anterior no se escaneó a tiempo

- **Ver QR Code**:
  - Muestra el código QR actual
  - Opción para descargar la imagen
  - Instrucciones de escaneo

#### 3. Funcionalidades Avanzadas

- **Filtros de Tabla**:
  - Filtrar por estado
  - Filtrar por cola
  - Búsqueda por nombre o número

- **Ordenamiento**:
  - Por nombre
  - Por estado
  - Por fecha de actualización

- **Selección Múltiple**:
  - Seleccionar múltiples conexiones
  - Acciones en lote (eliminar múltiples)

- **Actualización en Tiempo Real**:
  - Los cambios de estado se reflejan automáticamente
  - Notificaciones cuando una conexión se desconecta

- **Indicadores Visuales**:
  - Badge de advertencia en el menú si hay conexiones desconectadas
  - Colores según el estado de la conexión

#### 4. Configuración de Colas

- Asignar múltiples colas a una conexión
- Las colas determinan cómo se distribuyen los tickets
- Gestión desde el modal de edición

---

## Contactos

**Ruta**: `/contacts`

### Descripción General

Gestiona tu base de contactos, incluyendo información de clientes, grupos de WhatsApp y participantes de grupos.

### Funcionalidades Principales

#### 1. Lista de Contactos

**Tabla de Contactos**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre del contacto
  - **Número**: Número de teléfono (formato internacional)
  - **Email**: Dirección de correo electrónico
  - **WhatsApp**: Número de WhatsApp asociado
  - **Etiquetas**: Etiquetas asociadas al contacto
  - **Cola**: Cola asignada por defecto
  - **Última Interacción**: Fecha de último mensaje
  - **Acciones**: Botones de acción

- **Información Adicional**:
  - Avatar del contacto (si está disponible)
  - Estado de WhatsApp (si está conectado)
  - Número de tickets asociados

#### 2. Acciones Disponibles

- **Agregar Contacto**:
  - Modal con formulario
  - Campos: Nombre, número, email, WhatsApp, cola, etiquetas
  - Validación de formato de número y email

- **Editar Contacto**:
  - Modificar información del contacto
  - Actualizar etiquetas y cola asignada
  - Guardar cambios

- **Eliminar Contacto**:
  - Confirmación antes de eliminar
  - Eliminación de relaciones asociadas

- **Importar Contactos**:
  - **Desde Archivo CSV**:
    - Formato: nombre, número, email, etiquetas
    - Validación de datos
    - Preview antes de importar
    - Opción de actualizar contactos existentes
  
  - **Desde WhatsApp**:
    - Importar contactos desde conexión activa
    - Requiere WhatsApp Business conectado
    - Sincronización automática

- **Exportar Contactos**:
  - Exportar a CSV
  - Incluir todos los campos o seleccionar específicos
  - Filtros aplicados se reflejan en la exportación

- **Importar Participantes de Grupo**:
  - Importar miembros de grupos de WhatsApp
  - Requiere conexión activa y cuenta Business
  - Crea contactos automáticamente para cada participante

#### 3. Funcionalidades Avanzadas

- **Filtros Avanzados**:
  - Por nombre o número
  - Por etiquetas
  - Por cola
  - Por rango de fechas (última interacción)
  - Por email
  - Combinación de múltiples filtros

- **Búsqueda**:
  - Búsqueda en tiempo real
  - Búsqueda por nombre, número, email
  - Resaltado de resultados

- **Selección Múltiple**:
  - Seleccionar múltiples contactos
  - Acciones en lote:
    - Eliminar múltiples
    - Asignar etiquetas en lote
    - Cambiar cola en lote
    - Exportar seleccionados

- **Vista de Detalles**:
  - Panel lateral con información completa
  - Historial de tickets del contacto
  - Historial de mensajes
  - Notas y observaciones

- **Actualización en Tiempo Real**:
  - Nuevos contactos aparecen automáticamente
  - Cambios se reflejan sin recargar

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Reordenar columnas
  - Preferencias guardadas en localStorage

#### 4. Gestión de Etiquetas

- Asignar múltiples etiquetas a un contacto
- Filtrar contactos por etiquetas
- Gestión desde el modal de edición

---

## Usuarios

**Ruta**: `/users`

### Descripción General

Gestiona los usuarios del sistema, sus roles, permisos y asignaciones. Solo accesible para administradores.

### Funcionalidades Principales

#### 1. Lista de Usuarios

**Tabla de Usuarios**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre completo del usuario
  - **Email**: Dirección de correo electrónico
  - **Perfil**: Rol del usuario (admin, user, etc.)
  - **WhatsApp Asociado**: Conexión de WhatsApp asignada
  - **Colas**: Colas asignadas al usuario
  - **Estado**: Activo/Inactivo
  - **Último Acceso**: Fecha de último inicio de sesión
  - **Acciones**: Botones de acción

#### 2. Acciones Disponibles

- **Agregar Usuario**:
  - Modal con formulario completo
  - Campos:
    - Nombre completo
    - Email (único)
    - Contraseña (mínimo 6 caracteres)
    - Perfil/Rol (admin, user)
    - WhatsApp asociado (opcional)
    - Colas asignadas (múltiples)
  - Validación de email único
  - Validación de contraseña segura

- **Editar Usuario**:
  - Modificar información del usuario
  - Cambiar perfil/rol
  - Actualizar WhatsApp asociado
  - Modificar colas asignadas
  - Cambiar contraseña (opcional)

- **Eliminar Usuario**:
  - Confirmación antes de eliminar
  - Validación de que no sea el último administrador
  - Eliminación de relaciones asociadas

#### 3. Perfiles y Permisos

- **Administrador (admin)**:
  - Acceso completo a todas las funcionalidades
  - Gestión de usuarios
  - Configuración del sistema
  - Acceso a analytics completos

- **Usuario (user)**:
  - Acceso a tickets asignados
  - Gestión de contactos
  - Uso de respuestas rápidas
  - Acceso limitado a configuración

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Por perfil/rol
  - Por cola asignada
  - Por WhatsApp asociado
  - Por estado (activo/inactivo)
  - Búsqueda por nombre o email

- **Selección Múltiple**:
  - Seleccionar múltiples usuarios
  - Acciones en lote:
    - Eliminar múltiples
    - Cambiar perfil en lote
    - Asignar colas en lote

- **Asignación de Colas**:
  - Cada usuario puede estar asignado a múltiples colas
  - Los tickets de esas colas estarán disponibles para el usuario
  - Gestión desde el modal de edición

- **Actualización en Tiempo Real**:
  - Nuevos usuarios aparecen automáticamente
  - Cambios se reflejan sin recargar

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Preferencias guardadas

---

## Respuestas Rápidas

**Ruta**: `/quickAnswers`

### Descripción General

Gestiona respuestas predefinidas que los agentes pueden usar rápidamente durante las conversaciones para agilizar la atención al cliente.

### Funcionalidades Principales

#### 1. Lista de Respuestas Rápidas

**Tabla de Respuestas Rápidas**:

- **Columnas Disponibles**:
  - **Atajo**: Comando o palabra clave para invocar la respuesta
  - **Mensaje**: Contenido de la respuesta rápida
  - **Creado por**: Usuario que creó la respuesta
  - **Fecha de Creación**: Fecha y hora de creación
  - **Acciones**: Botones de acción

#### 2. Acciones Disponibles

- **Agregar Respuesta Rápida**:
  - Modal con formulario
  - Campos:
    - **Atajo**: Palabra clave única (ej: `/saludo`, `/despedida`)
    - **Mensaje**: Texto de la respuesta (soporta variables)
  - Validación de atajo único
  - Preview del mensaje

- **Editar Respuesta Rápida**:
  - Modificar atajo y mensaje
  - Actualizar contenido

- **Eliminar Respuesta Rápida**:
  - Confirmación antes de eliminar
  - Eliminación permanente

#### 3. Uso de Respuestas Rápidas

- **En el Composer de Mensajes**:
  - Escribir el atajo (ej: `/saludo`)
  - Autocompletado con sugerencias
  - Reemplazo automático del atajo por el mensaje completo

- **Variables Disponibles**:
  - `{contact.name}`: Nombre del contacto
  - `{contact.number}`: Número del contacto
  - `{user.name}`: Nombre del agente
  - `{ticket.id}`: ID del ticket
  - Variables personalizadas

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Búsqueda por atajo
  - Búsqueda por contenido del mensaje
  - Filtrar por creador

- **Selección Múltiple**:
  - Seleccionar múltiples respuestas
  - Eliminar múltiples en lote

- **Actualización en Tiempo Real**:
  - Nuevas respuestas disponibles inmediatamente
  - Cambios se reflejan sin recargar

- **Categorización** (futuro):
  - Organizar por categorías
  - Filtros por categoría

---

## Plantillas de Mensajes

**Ruta**: `/messageTemplates`

### Descripción General

Gestiona plantillas de mensajes oficiales de WhatsApp Business. Las plantillas deben ser aprobadas por WhatsApp antes de poder usarse.

### Funcionalidades Principales

#### 1. Lista de Plantillas

**Tabla de Plantillas**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre de la plantilla
  - **Categoría**: Categoría de WhatsApp (MARKETING, UTILITY, AUTHENTICATION)
  - **Estado**: Estado de aprobación (PENDING, APPROVED, REJECTED)
  - **Idioma**: Idioma de la plantilla
  - **Descripción**: Descripción breve
  - **Creado por**: Usuario que creó la plantilla
  - **Fecha de Creación**: Fecha y hora
  - **Acciones**: Botones de acción

#### 2. Estados de Plantillas

- **PENDING**: Esperando aprobación de WhatsApp
- **APPROVED**: Aprobada y lista para usar
- **REJECTED**: Rechazada por WhatsApp (con motivo)

#### 3. Acciones Disponibles

- **Crear Plantilla**:
  - Modal con formulario completo
  - Campos:
    - **Nombre**: Nombre único de la plantilla
    - **Categoría**: MARKETING, UTILITY, o AUTHENTICATION
    - **Idioma**: Código de idioma (es, en, etc.)
    - **Cuerpo del Mensaje**: Texto principal
    - **Encabezado** (opcional): Texto o media
    - **Pie de Página** (opcional): Texto
    - **Botones** (opcional): Hasta 3 botones
      - Botones de URL
      - Botones de llamada
      - Botones rápidos
    - **Variables**: Usar `{{1}}`, `{{2}}`, etc. para variables
  - Validación según reglas de WhatsApp
  - Preview de la plantilla

- **Editar Plantilla**:
  - Solo plantillas en estado PENDING pueden editarse
  - Modificar contenido antes de enviar a aprobación

- **Eliminar Plantilla**:
  - Confirmación antes de eliminar
  - Solo plantillas no aprobadas pueden eliminarse

- **Enviar a Aprobación**:
  - Envía la plantilla a WhatsApp para revisión
  - Cambia estado a PENDING
  - WhatsApp revisa y aprueba/rechaza

- **Aprobar** (solo admin):
  - Marcar como aprobada manualmente
  - Útil para testing

- **Rechazar** (solo admin):
  - Marcar como rechazada
  - Agregar motivo del rechazo

#### 4. Tipos de Plantillas

- **MARKETING**: 
  - Promociones, ofertas, anuncios
  - Requiere consentimiento del usuario
  - Horarios restringidos

- **UTILITY**: 
  - Confirmaciones, actualizaciones, recordatorios
  - No requiere consentimiento explícito
  - Disponible 24/7

- **AUTHENTICATION**: 
  - Códigos OTP, verificación
  - Solo para autenticación
  - Formato muy específico

#### 5. Funcionalidades Avanzadas

- **Filtros**:
  - Por estado
  - Por categoría
  - Por idioma
  - Búsqueda por nombre

- **Selección Múltiple**:
  - Seleccionar múltiples plantillas
  - Eliminar múltiples en lote

- **Preview**:
  - Vista previa de cómo se verá la plantilla
  - Simulación con variables de ejemplo

- **Historial de Cambios**:
  - Ver historial de modificaciones
  - Ver motivo de rechazo si fue rechazada

- **Actualización en Tiempo Real**:
  - Cambios de estado se reflejan automáticamente
  - Notificaciones cuando se aprueba/rechaza

---

## Colas (Queues)

**Ruta**: `/Queues`

### Descripción General

Las colas organizan y distribuyen los tickets entre los agentes. Permiten segmentar la atención por departamentos, productos o tipos de consulta.

### Funcionalidades Principales

#### 1. Lista de Colas

**Tabla de Colas**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre de la cola
  - **Color**: Color identificador (badge visual)
  - **Mensaje de Bienvenida**: Mensaje automático al asignar ticket
  - **Usuarios Asignados**: Número de usuarios en la cola
  - **Tickets Abiertos**: Número de tickets actualmente abiertos
  - **Acciones**: Botones de acción

#### 2. Acciones Disponibles

- **Agregar Cola**:
  - Modal con formulario
  - Campos:
    - **Nombre**: Nombre único de la cola
    - **Color**: Selector de color (hex o paleta)
    - **Mensaje de Bienvenida**: Mensaje que se envía automáticamente cuando se asigna un ticket de esta cola
    - **Usuarios**: Selección múltiple de usuarios asignados
    - **WhatsApp**: Conexiones de WhatsApp asociadas
  - Validación de nombre único

- **Editar Cola**:
  - Modificar nombre, color y mensaje
  - Agregar/remover usuarios
  - Cambiar conexiones de WhatsApp

- **Eliminar Cola**:
  - Confirmación antes de eliminar
  - Validación de que no tenga tickets abiertos
  - Opción de transferir tickets a otra cola antes de eliminar

#### 3. Funcionalidades de Colas

- **Distribución Automática**:
  - Los tickets se asignan automáticamente a usuarios de la cola
  - Distribución por round-robin o carga de trabajo

- **Mensaje de Bienvenida**:
  - Se envía automáticamente al asignar ticket
  - Soporta variables dinámicas
  - Personalizable por cola

- **Filtrado de Tickets**:
  - Filtrar tickets por cola
  - Ver solo tickets de colas asignadas (para usuarios)

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Búsqueda por nombre
  - Filtrar por usuarios asignados

- **Selección Múltiple**:
  - Seleccionar múltiples colas
  - Eliminar múltiples en lote

- **Estadísticas**:
  - Ver número de tickets por cola
  - Métricas de rendimiento por cola

- **Actualización en Tiempo Real**:
  - Cambios se reflejan automáticamente
  - Actualización de contadores de tickets

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Preferencias guardadas

---

## Pagos

**Ruta**: `/payments`

### Descripción General

Gestiona pagos y transacciones a través de múltiples pasarelas de pago integradas (PayPal, Stripe, Mercado Pago, Clip).

### Funcionalidades Principales

#### 1. Lista de Pagos

**Tabla de Pagos**:

- **Columnas Disponibles**:
  - **ID**: Identificador único del pago
  - **Contacto**: Nombre del contacto/cliente
  - **Monto**: Cantidad a pagar
  - **Moneda**: Moneda del pago (USD, MXN, etc.)
  - **ID de Transacción**: ID de la transacción en la pasarela
  - **Proveedor**: Pasarela de pago (PayPal, Stripe, etc.)
  - **Estado**: Estado del pago (pending, paid, canceled, failed)
  - **Fecha de Creación**: Fecha y hora
  - **Fecha de Pago**: Fecha cuando se completó
  - **Acciones**: Botones de acción

#### 2. Estados de Pago

- **PENDING**: Pago pendiente, esperando confirmación
- **PAID**: Pago completado exitosamente
- **CANCELED**: Pago cancelado por el usuario
- **FAILED**: Pago fallido (error en procesamiento)

#### 3. Acciones Disponibles

- **Crear Pago**:
  - Modal con formulario completo
  - Campos:
    - **Contacto**: Seleccionar contacto existente o crear nuevo
    - **Monto**: Cantidad a cobrar
    - **Moneda**: Seleccionar moneda
    - **Proveedor**: Seleccionar pasarela de pago
    - **Descripción**: Descripción del pago
    - **Fecha de Vencimiento** (opcional): Fecha límite para pagar
  - Genera link de pago único
  - Token de seguridad para el pago

- **Editar Pago**:
  - Modificar información del pago
  - Solo pagos en estado PENDING pueden editarse
  - Actualizar monto, descripción, etc.

- **Eliminar Pago**:
  - Confirmación antes de eliminar
  - Solo pagos cancelados o fallidos pueden eliminarse

- **Copiar Link de Pago**:
  - Copia el link público del pago al portapapeles
  - Link formato: `/pay/:token`

- **Abrir Link de Pago**:
  - Abre el link de pago en nueva pestaña
  - Útil para testing o compartir manualmente

- **Ver Detalles**:
  - Información completa del pago
  - Historial de intentos
  - Logs de la pasarela

#### 4. Integraciones de Pago

- **PayPal**:
  - Integración con PayPal Checkout
  - Soporte para PayPal y tarjetas
  - Webhooks para actualización de estado

- **Stripe**:
  - Integración con Stripe Checkout
  - Soporte para múltiples métodos de pago
  - Webhooks para actualización de estado

- **Mercado Pago**:
  - Integración con Mercado Pago Checkout
  - Popular en Latinoamérica
  - Múltiples métodos de pago

- **Clip**:
  - Integración con Clip (México)
  - Checkout integrado en la página
  - Soporte para tarjetas y efectivo

#### 5. Funcionalidades Avanzadas

- **Filtros**:
  - Por estado
  - Por proveedor
  - Por contacto
  - Por rango de fechas
  - Por monto (mínimo/máximo)
  - Búsqueda por ID o transacción

- **Selección Múltiple**:
  - Seleccionar múltiples pagos
  - Exportar seleccionados
  - Eliminar múltiples en lote

- **Exportar**:
  - Exportar a CSV/Excel
  - Incluir todos los campos
  - Filtros aplicados se reflejan

- **Actualización en Tiempo Real**:
  - Cambios de estado se reflejan automáticamente
  - Notificaciones cuando se completa un pago

- **Estadísticas**:
  - Total de pagos por estado
  - Monto total recaudado
  - Gráficos de pagos por proveedor

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Preferencias guardadas

---

## Etiquetas (Tags)

**Ruta**: `/tags`

### Descripción General

Gestiona etiquetas para organizar y categorizar tickets y contactos. Las etiquetas permiten filtrar y segmentar información.

### Funcionalidades Principales

#### 1. Lista de Etiquetas

**Tabla de Etiquetas**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre de la etiqueta
  - **Color**: Color identificador (badge visual)
  - **Usos**: Número de tickets/contactos que usan esta etiqueta
  - **Fecha de Creación**: Fecha y hora
  - **Acciones**: Botones de acción

#### 2. Acciones Disponibles

- **Crear Etiqueta**:
  - Modal con formulario
  - Campos:
    - **Nombre**: Nombre único de la etiqueta
    - **Color**: Selector de color (hex o paleta)
  - Validación de nombre único
  - Preview del color

- **Editar Etiqueta**:
  - Modificar nombre y color
  - Actualizar información

- **Eliminar Etiqueta**:
  - Confirmación antes de eliminar
  - Se remueve de todos los tickets/contactos asociados

- **Importar Etiquetas**:
  - Importar desde archivo CSV
  - Formato: nombre, color (hex)
  - Validación de datos
  - Opción de actualizar existentes

#### 3. Uso de Etiquetas

- **En Tickets**:
  - Asignar múltiples etiquetas a un ticket
  - Filtrar tickets por etiquetas
  - Búsqueda por etiquetas

- **En Contactos**:
  - Asignar etiquetas a contactos
  - Segmentar contactos por etiquetas
  - Filtrar en campañas

- **En Campañas**:
  - Filtrar destinatarios por etiquetas
  - Segmentación avanzada

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Búsqueda por nombre
  - Filtrar por color
  - Ordenar por usos

- **Selección Múltiple**:
  - Seleccionar múltiples etiquetas
  - Eliminar múltiples en lote

- **Estadísticas**:
  - Ver número de usos por etiqueta
  - Etiquetas más utilizadas

- **Actualización en Tiempo Real**:
  - Nuevas etiquetas disponibles inmediatamente
  - Contadores de usos actualizados

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Preferencias guardadas

---

## Respuestas Automáticas

**Ruta**: `/auto-replies`

### Descripción General

Configura respuestas automáticas basadas en palabras clave. El sistema detecta palabras clave en los mensajes entrantes y responde automáticamente.

### Funcionalidades Principales

#### 1. Lista de Respuestas Automáticas

**Tabla de Respuestas Automáticas**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre identificador de la respuesta automática
  - **Palabras Clave**: Palabras que activan la respuesta
  - **Cola**: Cola asociada (opcional, si está vacío aplica a todas)
  - **Tipo de Coincidencia**: Exacta, contiene, comienza con, termina con
  - **Activo**: Estado activo/inactivo
  - **Mensaje**: Mensaje de respuesta
  - **Acciones**: Botones de acción

#### 2. Tipos de Coincidencia

- **Exacta**: El mensaje debe coincidir exactamente con la palabra clave
- **Contiene**: El mensaje debe contener la palabra clave
- **Comienza con**: El mensaje debe comenzar con la palabra clave
- **Termina con**: El mensaje debe terminar con la palabra clave

#### 3. Acciones Disponibles

- **Crear Respuesta Automática**:
  - Modal con formulario completo
  - Campos:
    - **Nombre**: Nombre identificador
    - **Palabras Clave**: Lista de palabras separadas por comas
    - **Cola**: Seleccionar cola específica (opcional)
    - **Tipo de Coincidencia**: Seleccionar tipo
    - **Activo**: Checkbox para activar/desactivar
    - **Mensaje**: Mensaje de respuesta automática
      - Soporta variables dinámicas
      - Soporta formato de texto
  - Validación de palabras clave
  - Preview del mensaje

- **Editar Respuesta Automática**:
  - Modificar todos los campos
  - Activar/desactivar respuesta
  - Actualizar palabras clave

- **Eliminar Respuesta Automática**:
  - Confirmación antes de eliminar
  - Eliminación permanente

#### 4. Funcionalidades Avanzadas

- **Prioridad**:
  - Si múltiples respuestas coinciden, se usa la primera
  - Orden configurable

- **Variables en Mensajes**:
  - `{contact.name}`: Nombre del contacto
  - `{contact.number}`: Número del contacto
  - `{message}`: Mensaje original del contacto
  - Variables personalizadas

- **Filtros**:
  - Por cola
  - Por estado (activo/inactivo)
  - Búsqueda por nombre o palabras clave

- **Selección Múltiple**:
  - Seleccionar múltiples respuestas
  - Activar/desactivar múltiples en lote
  - Eliminar múltiples en lote

- **Testing**:
  - Probar respuesta con mensaje de ejemplo
  - Verificar coincidencias

- **Actualización en Tiempo Real**:
  - Cambios se aplican inmediatamente
  - Nuevas respuestas activas automáticamente

- **Logs**:
  - Ver historial de activaciones
  - Estadísticas de uso

---

## Configuración de Chatbot

**Ruta**: `/chatbot-config`

### Descripción General

Configura chatbots con inteligencia artificial para automatizar respuestas. Soporta múltiples proveedores de IA (OpenAI, Anthropic, Google).

### Funcionalidades Principales

#### 1. Lista de Configuraciones de Chatbot

**Tabla de Configuraciones**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre de la configuración
  - **Proveedor**: Proveedor de IA (OpenAI, Anthropic, Google)
  - **Modelo**: Modelo de IA utilizado
  - **Cola**: Cola asociada
  - **Créditos**: Saldo de créditos disponible
  - **Activo**: Estado activo/inactivo
  - **Acciones**: Botones de acción

#### 2. Proveedores de IA Soportados

- **OpenAI**:
  - Modelos: GPT-3.5, GPT-4, GPT-4 Turbo
  - API Key requerida
  - Créditos basados en tokens

- **Anthropic (Claude)**:
  - Modelos: Claude 3 Opus, Claude 3 Sonnet, Claude 3 Haiku
  - API Key requerida
  - Créditos basados en tokens

- **Google (Gemini)**:
  - Modelos: Gemini Pro, Gemini Ultra
  - API Key requerida
  - Créditos basados en tokens

#### 3. Acciones Disponibles

- **Crear Configuración de Chatbot**:
  - Modal con formulario completo
  - Campos:
    - **Nombre**: Nombre identificador
    - **Proveedor**: Seleccionar proveedor
    - **API Key**: Clave API del proveedor
    - **Modelo**: Seleccionar modelo disponible
    - **Cola**: Cola asociada (opcional)
    - **Prompt del Sistema**: Instrucciones para el chatbot
    - **Temperatura**: Control de creatividad (0-1)
    - **Max Tokens**: Límite de tokens por respuesta
    - **Activo**: Activar/desactivar
  - Validación de API Key
  - Test de conexión

- **Editar Configuración**:
  - Modificar todos los campos
  - Actualizar API Key
  - Cambiar modelo
  - Ajustar parámetros

- **Eliminar Configuración**:
  - Confirmación antes de eliminar
  - Eliminación permanente

- **Actualizar Créditos**:
  - Actualizar saldo manualmente
  - Sincronizar con proveedor
  - Ver historial de uso

- **Ver Logs**:
  - Historial de interacciones del chatbot
  - Mensajes enviados/recibidos
  - Tokens utilizados
  - Costos por interacción
  - Filtrar por fecha, cola, contacto

#### 4. Funcionalidades Avanzadas

- **Prompt del Sistema**:
  - Instrucciones personalizadas para el chatbot
  - Contexto sobre la empresa/producto
  - Estilo de respuesta deseado
  - Variables disponibles: `{contact.name}`, `{ticket.id}`, etc.

- **Parámetros de IA**:
  - **Temperatura**: Controla la creatividad (0 = determinista, 1 = creativo)
  - **Max Tokens**: Límite de longitud de respuesta
  - **Top P**: Control de diversidad
  - **Frequency Penalty**: Penaliza repeticiones

- **Filtros**:
  - Por proveedor
  - Por cola
  - Por estado (activo/inactivo)
  - Búsqueda por nombre

- **Selección Múltiple**:
  - Seleccionar múltiples configuraciones
  - Activar/desactivar múltiples en lote
  - Eliminar múltiples en lote

- **Monitoreo**:
  - Uso de créditos en tiempo real
  - Alertas cuando los créditos son bajos
  - Estadísticas de uso por día/semana/mes

- **Integración con Tickets**:
  - El chatbot responde automáticamente cuando:
    - No hay agentes disponibles
    - El ticket está en una cola específica
    - Se activa manualmente
  - Transición suave a agente humano

- **Actualización en Tiempo Real**:
  - Cambios se aplican inmediatamente
  - Créditos actualizados automáticamente

---

## Campañas

**Ruta**: `/campaigns`

### Descripción General

Crea y gestiona campañas de mensajería masiva para enviar mensajes a múltiples contactos de forma automatizada y programada.

### Funcionalidades Principales

#### 1. Lista de Campañas

**Tabla de Campañas**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre de la campaña
  - **Estado**: Estado (draft, scheduled, processing, paused, completed, canceled)
  - **Progreso**: Porcentaje de completado
  - **Destinatarios**: Número total de destinatarios
  - **Enviados**: Mensajes enviados
  - **Entregados**: Mensajes entregados
  - **Fallidos**: Mensajes fallidos
  - **Tasa de Entrega**: Porcentaje de entrega
  - **Tasa de Lectura**: Porcentaje de lectura
  - **Próximo Mensaje**: Tiempo hasta el próximo envío
  - **Acciones**: Botones de acción

#### 2. Estados de Campaña

- **DRAFT**: Borrador, no iniciada
- **SCHEDULED**: Programada para iniciar
- **PROCESSING**: En proceso de envío
- **PAUSED**: Pausada temporalmente
- **COMPLETED**: Completada exitosamente
- **CANCELED**: Cancelada

#### 3. Acciones Disponibles

- **Crear Campaña**:
  - Modal con formulario completo (múltiples pasos)
  - **Paso 1 - Información Básica**:
    - Nombre de la campaña
    - Descripción (opcional)
    - Cola asociada
    - Conexión de WhatsApp
  
  - **Paso 2 - Mensaje**:
    - Seleccionar plantilla de mensaje (opcional)
    - O escribir mensaje personalizado
    - Adjuntar media (imagen, video, documento)
    - Variables dinámicas disponibles
    - Preview del mensaje
  
  - **Paso 3 - Destinatarios**:
    - Seleccionar contactos individuales
    - O filtrar por:
      - Etiquetas
      - Cola
      - Rango de fechas (última interacción)
      - Campos personalizados
    - Preview de destinatarios
    - Validación de destinatarios válidos
  
  - **Paso 4 - Configuración**:
    - **Programación**:
      - Iniciar inmediatamente
      - O programar fecha/hora de inicio
    - **Límites Diarios**:
      - Número máximo de mensajes por día
      - Horario de envío (ej: 9 AM - 6 PM)
    - **Intervalo entre Mensajes**:
      - Tiempo mínimo entre envíos (evitar spam)
    - **Pausa Inteligente**:
      - Pausar si hay muchos fallos
      - Pausar si hay límites de WhatsApp

- **Editar Campaña**:
  - Solo campañas en estado DRAFT pueden editarse completamente
  - Campañas en otros estados tienen edición limitada

- **Eliminar Campaña**:
  - Solo campañas en estado DRAFT o CANCELED pueden eliminarse
  - Confirmación antes de eliminar

- **Iniciar/Procesar Campaña**:
  - Inicia el envío de mensajes
  - Cambia estado a PROCESSING
  - Comienza a enviar según configuración

- **Pausar Campaña**:
  - Pausa temporalmente el envío
  - Estado cambia a PAUSED
  - Puede reanudarse después

- **Reanudar Campaña**:
  - Continúa el envío desde donde se pausó
  - Estado cambia a PROCESSING

- **Cancelar Campaña**:
  - Detiene permanentemente la campaña
  - Estado cambia a CANCELED
  - No puede reanudarse

- **Actualizar Métricas**:
  - Fuerza actualización de estadísticas
  - Útil para verificar progreso

- **Ver Historial de Destinatarios**:
  - Modal con lista de destinatarios
  - Estado de cada envío (pendiente, enviado, entregado, leído, fallido)
  - Fecha/hora de envío
  - Fecha/hora de entrega/lectura
  - Motivo de fallo (si aplica)
  - Filtrar por estado
  - Exportar historial

#### 4. Funcionalidades Avanzadas

- **Segmentación Avanzada**:
  - Combinar múltiples filtros
  - Excluir contactos específicos
  - Incluir solo contactos con etiquetas específicas

- **Personalización de Mensajes**:
  - Variables dinámicas por contacto:
    - `{contact.name}`: Nombre del contacto
    - `{contact.number}`: Número
    - `{contact.email}`: Email
    - Variables personalizadas
  - Cada mensaje se personaliza automáticamente

- **Gestión de Límites**:
  - Respetar límites de WhatsApp Business
  - Evitar bloqueos por spam
  - Distribución inteligente de envíos

- **Manejo de Errores**:
  - Reintentos automáticos para fallos temporales
  - Exclusión automática de números inválidos
  - Logs detallados de errores

- **Filtros**:
  - Por estado
  - Por cola
  - Por fecha de creación
  - Búsqueda por nombre

- **Selección Múltiple**:
  - Seleccionar múltiples campañas
  - Pausar/reanudar múltiples en lote
  - Cancelar múltiples en lote

- **Estadísticas en Tiempo Real**:
  - Contadores actualizados en tiempo real
  - Gráficos de progreso
  - Tasa de entrega y lectura
  - Tiempo estimado de finalización

- **Countdown de Próximo Mensaje**:
  - Muestra tiempo hasta el próximo envío
  - Actualizado cada segundo
  - Útil para monitorear campañas activas

- **Actualización en Tiempo Real**:
  - Métricas actualizadas automáticamente
  - Cambios de estado reflejados inmediatamente

- **Exportar Resultados**:
  - Exportar historial completo de destinatarios
  - Incluir todos los estados y fechas
  - Formato CSV/Excel

---

## Mensajes Programados

**Ruta**: `/scheduled-messages`

### Descripción General

Gestiona mensajes programados para enviarse en una fecha y hora específica. Útil para recordatorios, seguimientos y comunicaciones planificadas.

### Funcionalidades Principales

#### 1. Lista de Mensajes Programados

**Tabla de Mensajes Programados**:

- **Columnas Disponibles**:
  - **Contacto**: Nombre del contacto destinatario
  - **Mensaje**: Preview del mensaje (primeros caracteres)
  - **Programado para**: Fecha y hora programada
  - **Estado**: Estado (scheduled, sent, canceled, failed)
  - **Firma**: Usuario que creó el mensaje
  - **Fecha de Creación**: Fecha y hora de creación
  - **Acciones**: Botones de acción

#### 2. Estados de Mensaje Programado

- **SCHEDULED**: Programado, esperando fecha/hora
- **SENT**: Enviado exitosamente
- **CANCELED**: Cancelado antes de enviar
- **FAILED**: Falló al enviar

#### 3. Acciones Disponibles

- **Crear Mensaje Programado**:
  - Modal con formulario
  - Campos:
    - **Contacto**: Seleccionar contacto
    - **Mensaje**: Texto del mensaje
      - Soporta variables dinámicas
      - Adjuntar media (opcional)
    - **Fecha y Hora**: Selector de fecha/hora
      - Validación de fecha futura
    - **Cola**: Cola asociada (opcional)
    - **WhatsApp**: Conexión de WhatsApp
  - Preview del mensaje
  - Validación de contacto válido

- **Enviar Ahora**:
  - Envía el mensaje inmediatamente
  - No espera la fecha programada
  - Cambia estado a SENT

- **Cancelar**:
  - Cancela el envío programado
  - Estado cambia a CANCELED
  - No puede reanudarse

- **Eliminar**:
  - Elimina el mensaje programado
  - Solo mensajes cancelados o fallidos pueden eliminarse
  - Confirmación antes de eliminar

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Por estado
  - Por contacto
  - Por rango de fechas (programado para)
  - Por usuario creador
  - Búsqueda por mensaje

- **Selección Múltiple**:
  - Seleccionar múltiples mensajes
  - Enviar ahora múltiples en lote
  - Cancelar múltiples en lote
  - Eliminar múltiples en lote

- **Vista de Calendario** (futuro):
  - Ver mensajes programados en vista de calendario
  - Filtrar por día/semana/mes

- **Notificaciones**:
  - Notificación cuando se envía un mensaje programado
  - Alerta si falla el envío

- **Actualización en Tiempo Real**:
  - Cambios de estado se reflejan automáticamente
  - Mensajes enviados se actualizan en tiempo real

- **Personalización de Columnas**:
  - Mostrar/ocultar columnas
  - Preferencias guardadas

- **Reenvío Automático**:
  - Reintento automático si falla el envío
  - Configurable número de reintentos

---

## Widgets de Chat

**Ruta**: `/widgets`

### Descripción General

Crea y gestiona widgets de chat para integrar en tu sitio web. Los widgets permiten que los visitantes inicien conversaciones de WhatsApp directamente desde tu sitio.

### Funcionalidades Principales

#### 1. Lista de Widgets

**Tabla de Widgets**:

- **Columnas Disponibles**:
  - **Nombre de la Empresa**: Nombre identificador
  - **WhatsApp**: Conexión de WhatsApp asociada
  - **Cola**: Cola asignada para tickets del widget
  - **Token**: Token único del widget
  - **Estado**: Estado (active, inactive)
  - **URL del Sitio**: Sitio web donde está integrado
  - **Acciones**: Botones de acción

#### 2. Acciones Disponibles

- **Crear Widget**:
  - Modal con formulario
  - Campos:
    - **Nombre de la Empresa**: Nombre identificador
    - **WhatsApp**: Seleccionar conexión de WhatsApp
    - **Cola**: Seleccionar cola para tickets
    - **Color Primario**: Color del widget (opcional)
    - **Color Secundario**: Color secundario (opcional)
    - **Mensaje de Bienvenida**: Mensaje inicial (opcional)
    - **URL del Sitio**: URL donde se integrará (opcional, para validación)
  - Genera token único automáticamente
  - Preview del widget

- **Editar Widget**:
  - Modificar todos los campos
  - Actualizar configuración
  - Cambiar cola o WhatsApp

- **Eliminar Widget**:
  - Confirmación antes de eliminar
  - Eliminación permanente

- **Ver Código de Integración**:
  - Modal con código JavaScript
  - Código listo para copiar y pegar
  - Instrucciones de integración
  - Opciones de personalización:
    - Posición del widget (bottom-right, bottom-left, etc.)
    - Tamaño del botón
    - Texto del botón
    - Mostrar/ocultar en móviles

- **Probar Widget Localmente**:
  - Abre página de prueba local
  - Simula el widget en acción
  - Útil para testing antes de integrar

#### 3. Funcionalidades del Widget

- **Botón Flotante**:
  - Botón flotante en la esquina de la página
  - Clickable para abrir chat
  - Personalizable en color y posición

- **Ventana de Chat**:
  - Ventana emergente con chat
  - Interfaz similar a WhatsApp Web
  - Envío y recepción de mensajes en tiempo real
  - Soporte para texto, imágenes, documentos

- **Creación Automática de Tickets**:
  - Cada conversación del widget crea un ticket
  - Asignado a la cola configurada
  - Contacto creado automáticamente si no existe

- **Integración Simple**:
  - Solo requiere agregar un script
  - No requiere configuración adicional
  - Funciona en cualquier sitio web

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Por estado
  - Por WhatsApp
  - Por cola
  - Búsqueda por nombre

- **Selección Múltiple**:
  - Seleccionar múltiples widgets
  - Eliminar múltiples en lote

- **Estadísticas**:
  - Número de conversaciones iniciadas
  - Tickets creados desde el widget
  - Tasa de conversión

- **Personalización Avanzada**:
  - CSS personalizado
  - Temas predefinidos
  - Logo de la empresa

- **Actualización en Tiempo Real**:
  - Cambios se reflejan automáticamente
  - Widgets actualizados sin recargar

- **Seguridad**:
  - Token único por widget
  - Validación de origen (opcional)
  - Protección contra spam

- **Multi-idioma**:
  - Soporte para múltiples idiomas
  - Textos personalizables

---

## Workflows Automatizados

**Ruta**: `/workflows`

### Descripción General

Los Workflows permiten automatizar procesos y acciones basadas en eventos específicos. Puedes crear flujos visuales con nodos de trigger, condición, acción, delay y webhook para automatizar tareas repetitivas y mejorar la eficiencia.

### Funcionalidades Principales

#### 1. Lista de Workflows

**Tabla de Workflows**:

- **Columnas Disponibles**:
  - **Nombre**: Nombre del workflow
  - **Descripción**: Descripción breve del workflow
  - **Trigger**: Tipo de evento que activa el workflow
  - **Estado**: Activo/Inactivo
  - **Colas**: Colas asociadas
  - **Conexiones**: Conexiones WhatsApp asociadas
  - **Última Ejecución**: Fecha de última ejecución
  - **Ejecuciones**: Número de ejecuciones totales
  - **Acciones**: Botones de acción

#### 2. Tipos de Triggers

- **Mensaje Recibido**: Se activa cuando se recibe un mensaje nuevo
- **Ticket Creado**: Se activa cuando se crea un nuevo ticket
- **Ticket Cerrado**: Se activa cuando se cierra un ticket
- **Ticket Asignado**: Se activa cuando se asigna un ticket a un usuario
- **Ticket Transferido**: Se activa cuando se transfiere un ticket a otra cola

#### 3. Tipos de Nodos

**Nodo Trigger**:
- Nodo inicial del workflow
- Se activa automáticamente según el tipo de trigger configurado
- No requiere configuración adicional

**Nodo de Acción**:
- **Enviar Mensaje**: Envía un mensaje al contacto
  - Soporta variables dinámicas: `{{contact.name}}`, `{{ticket.id}}`, etc.
- **Asignar Usuario**: Asigna el ticket a un usuario específico
- **Asignar Cola**: Transfiere el ticket a otra cola
- **Agregar Etiquetas**: Agrega etiquetas al ticket o contacto
- **Remover Etiquetas**: Remueve etiquetas del ticket o contacto
- **Remover Todas las Etiquetas**: Limpia todas las etiquetas
- **Cambiar Estado**: Cambia el estado del ticket (abierto, pendiente, cerrado)

**Nodo de Condición**:
- **Condición Simple**: Evalúa una condición (true/false)
  - Campos disponibles: `ticket.status`, `ticket.queueId`, `ticket.userId`, `contact.name`, `contact.number`, `message.body`, `ticket.createdAt`, `ticket.lastMessage`, `ticket.tags`
  - Operadores: `equals`, `not_equals`, `contains`, `not_contains`, `starts_with`, `ends_with`, `greater_than`, `less_than`, `is_empty`, `is_not_empty`, `has_tag`, `has_any_tag`, `has_none_of_tags`
- **Condición Avanzada**: Múltiples reglas con lógica AND/OR
  - Permite combinar múltiples condiciones
  - Lógica: AND (todas deben cumplirse) u OR (al menos una debe cumplirse)

**Nodo de Espera (Delay)**:
- Espera un tiempo configurable antes de continuar
- Unidades: segundos, minutos, horas
- Útil para programar acciones con retraso

#### 4. Acciones Disponibles

- **Crear Workflow**:
  - Modal con editor visual de workflows
  - **Paso 1 - Información Básica**:
    - Nombre del workflow
    - Descripción
    - Tipo de trigger
    - Condiciones del trigger (opcional)
    - Colas asociadas (obligatorio)
    - Conexiones WhatsApp (opcional)
    - Estado (activo/inactivo)
  
  - **Paso 2 - Editor Visual**:
    - Arrastrar y soltar nodos
    - Conectar nodos con edges
    - Configurar cada nodo
    - Validación en tiempo real
    - Preview del flujo

- **Editar Workflow**:
  - Modificar información y definición
  - Actualizar nodos y conexiones
  - Cambiar triggers y condiciones

- **Eliminar Workflow**:
  - Confirmación antes de eliminar
  - Eliminación permanente

- **Activar/Desactivar Workflow**:
  - Toggle para activar/desactivar
  - Los workflows inactivos no se ejecutan

- **Ejecutar Manualmente**:
  - Ejecutar workflow para un ticket específico
  - Útil para testing o ejecución manual
  - Opción de proporcionar datos de contexto

- **Validar Workflow**:
  - Validar estructura del workflow
  - Verificar que no tenga errores
  - Comprobar que todos los nodos estén conectados correctamente

- **Ver Ejecuciones**:
  - Historial completo de ejecuciones
  - Logs detallados de cada ejecución
  - Estado de cada ejecución (running, completed, failed)
  - Filtros por fecha, ticket, contacto, estado

- **Modo Simulación**:
  - Probar workflow sin ejecutar acciones reales
  - Ver qué acciones se ejecutarían
  - Útil para debugging y testing

#### 5. Funcionalidades Avanzadas

- **Filtros**:
  - Por nombre o descripción
  - Por tipo de trigger
  - Por estado (activo/inactivo)
  - Por cola
  - Por conexión WhatsApp

- **Variables Dinámicas**:
  - En mensajes: `{{contact.name}}`, `{{contact.number}}`, `{{ticket.id}}`
  - En webhooks: `{{contact.name}}`, `{{contact.number}}`, `{{contact.id}}`, `{{ticket.id}}`, `{{ticket.status}}`
  - Se reemplazan automáticamente con valores reales

- **Prevención de Ciclos**:
  - El sistema previene ciclos infinitos
  - Límite de profundidad máxima
  - Detección de nodos visitados

- **Logs de Ejecución**:
  - Logs detallados de cada nodo ejecutado
  - Timestamps de cada acción
  - Niveles: info, success, warn, error
  - Datos de contexto en cada log

- **Historial de Ejecuciones**:
  - Ver todas las ejecuciones de un workflow
  - Filtrar por ticket, contacto, estado
  - Ver logs completos de cada ejecución
  - Exportar historial

- **Validación Automática**:
  - Validación al guardar workflow
  - Verificación de estructura
  - Comprobación de conexiones
  - Mensajes de error claros

- **Selección Múltiple**:
  - Seleccionar múltiples workflows
  - Activar/desactivar múltiples en lote
  - Eliminar múltiples en lote

- **Actualización en Tiempo Real**:
  - Cambios se reflejan automáticamente
  - Ejecuciones aparecen en tiempo real
  - Logs actualizados en vivo

#### 6. Ejemplos de Uso

**Workflow de Bienvenida**:
1. Trigger: Ticket Creado
2. Condición: Cola = "Soporte"
3. Acción: Enviar Mensaje "¡Hola {{contact.name}}! Bienvenido a nuestro servicio de soporte."

**Workflow de Escalación**:
1. Trigger: Mensaje Recibido
2. Condición Avanzada: Ticket sin asignar Y Mensaje contiene "urgente"
3. Acción: Asignar a Usuario (Supervisor)
4. Acción: Agregar Etiqueta "Urgente"
5. Acción: Enviar Mensaje "Hemos escalado tu consulta a nuestro supervisor."

**Workflow de Seguimiento**:
1. Trigger: Ticket Cerrado
2. Acción: Esperar 24 horas
3. Acción: Enviar Mensaje "¿Cómo fue tu experiencia? Nos encantaría conocer tu opinión."

**Workflow de Notificación**:
1. Trigger: Ticket Asignado
2. Acción: Enviar Mensaje "Tu ticket ha sido asignado a un agente."
3. Acción: Agregar Etiqueta "Asignado"

---

## Configuración General

**Ruta**: `/Settings`

### Descripción General

Gestiona la configuración general de la plataforma, incluyendo integraciones de pasarelas de pago y configuraciones del sistema.

### Funcionalidades Principales

#### 1. Configuración General del Sistema

- **Configuraciones Disponibles**:
  - **Nombre de la Empresa**: Nombre de la organización
  - **Email de Contacto**: Email para notificaciones
  - **Idioma por Defecto**: Idioma de la interfaz
  - **Zona Horaria**: Zona horaria del sistema
  - **Formato de Fecha**: Formato de visualización de fechas
  - **Límite de Archivos**: Tamaño máximo de archivos adjuntos
  - **Tiempo de SLA**: Tiempo objetivo para resolución de tickets

#### 2. Integraciones de Pasarelas de Pago

- **PayPal**:
  - **Habilitar PayPal**: Checkbox para activar/desactivar
  - **Client ID**: ID de cliente de PayPal
  - **Client Secret**: Secreto de cliente de PayPal
  - **Modo**: Sandbox (pruebas) o Producción
  - **Guardar**: Botón para guardar configuración
  - Test de conexión disponible

- **Stripe**:
  - **Habilitar Stripe**: Checkbox para activar/desactivar
  - **Publishable Key**: Clave pública de Stripe
  - **Secret Key**: Clave secreta de Stripe
  - **Modo**: Test o Live
  - **Guardar**: Botón para guardar configuración
  - Test de conexión disponible

- **Mercado Pago**:
  - **Habilitar Mercado Pago**: Checkbox para activar/desactivar
  - **Access Token**: Token de acceso
  - **Public Key**: Clave pública
  - **Modo**: Sandbox o Producción
  - **Guardar**: Botón para guardar configuración

- **Clip**:
  - **Habilitar Clip**: Checkbox para activar/desactivar
  - **API Key**: Clave API de Clip
  - **Merchant ID**: ID del comercio
  - **Modo**: Pruebas o Producción
  - **Guardar**: Botón para guardar configuración

#### 3. Otras Configuraciones

- **Configuración de Email** (si está disponible):
  - SMTP Server
  - Puerto
  - Usuario
  - Contraseña
  - Encriptación (TLS/SSL)

- **Configuración de Notificaciones**:
  - Habilitar notificaciones push
  - Configurar webhooks
  - URLs de callback

- **Configuración de Seguridad**:
  - Política de contraseñas
  - Autenticación de dos factores (si está disponible)
  - Sesiones simultáneas

#### 4. Funcionalidades Avanzadas

- **Guardado Automático**:
  - Algunas configuraciones se guardan automáticamente
  - Otras requieren confirmación explícita

- **Validación**:
  - Validación de formatos de API keys
  - Test de conexión antes de guardar
  - Mensajes de error claros

- **Actualización en Tiempo Real**:
  - Cambios se reflejan inmediatamente
  - Notificaciones de éxito/error

- **Historial de Cambios** (futuro):
  - Ver historial de modificaciones
  - Revertir cambios

- **Exportar/Importar Configuración**:
  - Exportar configuración actual
  - Importar desde archivo
  - Útil para migraciones

---

## Configuración de Notificaciones

**Ruta**: `/notification-settings`

### Descripción General

Configura las notificaciones que recibes en la plataforma. Personaliza qué eventos te notifican y cómo los recibes.

### Funcionalidades Principales

#### 1. Configuración de Notificaciones Push

- **Habilitar Notificaciones Push**:
  - Checkbox para activar/desactivar todas las notificaciones
  - Requiere permiso del navegador
  - Configuración de permisos del navegador

#### 2. Tipos de Notificaciones

- **Asignación de Ticket**:
  - Notificar cuando se asigna un ticket a ti
  - Checkbox para activar/desactivar
  - Opción de sonido

- **Transferencia de Ticket**:
  - Notificar cuando un ticket se transfiere
  - Checkbox para activar/desactivar

- **Cierre de Ticket**:
  - Notificar cuando se cierra un ticket
  - Checkbox para activar/desactivar

- **Nuevo Mensaje**:
  - Notificar cuando llega un nuevo mensaje
  - Checkbox para activar/desactivar
  - Opción de notificar solo en tickets asignados a ti
  - O notificar en todos los tickets

- **Advertencia de SLA**:
  - Notificar cuando un ticket está cerca de exceder el SLA
  - Checkbox para activar/desactivar
  - Configurar tiempo de advertencia (ej: 1 hora antes)

- **Mensaje en Ticket No Asignado**:
  - Notificar cuando hay mensajes en tickets sin asignar
  - Checkbox para activar/desactivar

- **Campaña Completada**:
  - Notificar cuando una campaña se completa
  - Checkbox para activar/desactivar

- **Pago Recibido**:
  - Notificar cuando se recibe un pago
  - Checkbox para activar/desactivar

#### 3. Configuración de Alertas

- **Sonido de Notificación**:
  - Habilitar/desactivar sonido
  - Seleccionar sonido (si hay opciones)

- **Vibración** (móviles):
  - Habilitar vibración en dispositivos móviles

- **Mostrar en Escritorio**:
  - Mostrar notificaciones en el escritorio
  - Incluso cuando la pestaña no está activa

- **Persistencia de Notificaciones**:
  - Mantener notificaciones hasta que se lean
  - O desaparecer automáticamente después de X segundos

#### 4. Funcionalidades Avanzadas

- **Horarios de Notificaciones**:
  - Configurar horarios en los que recibir notificaciones
  - Ej: Solo de 9 AM a 6 PM
  - Días de la semana

- **Filtros por Prioridad**:
  - Notificar solo tickets de alta prioridad
  - Ignorar notificaciones de baja prioridad

- **Agrupación de Notificaciones**:
  - Agrupar múltiples notificaciones similares
  - Reducir ruido de notificaciones

- **Guardado Automático**:
  - Los cambios se guardan automáticamente
  - Sin necesidad de botón de guardar

- **Test de Notificaciones**:
  - Botón para probar notificaciones
  - Verificar que funcionan correctamente

- **Sincronización**:
  - Configuración sincronizada entre dispositivos
  - Si usas la plataforma en múltiples navegadores

---

## Documentación de API

**Ruta**: `/api-docs`

### Descripción General

Documentación interactiva de la API REST de la plataforma. Permite explorar endpoints, ver ejemplos y probar llamadas directamente desde la interfaz.

### Funcionalidades Principales

#### 1. Navegación de Endpoints

- **Categorías de Endpoints**:
  - **Autenticación**: Login, signup, refresh token
  - **Conexiones de WhatsApp**: CRUD de conexiones
  - **Tickets**: Gestión de tickets
  - **Contactos**: CRUD de contactos
  - **Mensajes**: Envío y recepción de mensajes
  - **Colas**: Gestión de colas
  - **Sesiones de WhatsApp**: Control de sesiones
  - **Usuarios**: Gestión de usuarios
  - **Configuración**: Configuración del sistema
  - **Respuestas Rápidas**: CRUD de respuestas rápidas
  - **Pagos**: Gestión de pagos
  - **Logs de Webhooks**: Historial de webhooks
  - **Mensajes API**: Envío de mensajes vía API
  - **Etiquetas**: Gestión de etiquetas
  - **Analytics**: Endpoints de analytics
  - **Horarios Comerciales**: Configuración de horarios
  - **Respuestas Automáticas**: CRUD de auto-replies
  - **Configuración de Notificaciones**: Gestión de notificaciones
  - **Campañas**: Gestión de campañas
  - **Mensajes Programados**: CRUD de mensajes programados
  - **Plantillas de Mensajes**: Gestión de plantillas
  - **Configuración de Chatbot**: CRUD de configuraciones de chatbot
  - **Widgets**: Gestión de widgets
  - **Workflows**: Gestión de workflows automatizados
  - **Segmentos de Contactos**: Gestión de segmentos de contactos
  - **Contactos Duplicados**: Detección y fusión de contactos duplicados
  - **Listas de Difusión**: Gestión de listas de difusión (broadcast lists)
  - **Integración con Apptivo**: Endpoints para integración con Apptivo CRM

#### 2. Información de Cada Endpoint

- **Método HTTP**: GET, POST, PUT, DELETE, PATCH
- **Ruta**: URL completa del endpoint
- **Descripción**: Explicación de qué hace el endpoint
- **Autenticación**: Si requiere autenticación y cómo
- **Parámetros**:
  - Query parameters
  - Path parameters
  - Body parameters
- **Ejemplo de Request**: Código de ejemplo
- **Ejemplo de Response**: Respuesta de ejemplo
- **Códigos de Estado**: Posibles códigos HTTP de respuesta

#### 3. Probador Interactivo

- **Configuración de Autenticación**:
  - Campo para ingresar API token
  - Token se guarda en sesión
  - Validación de token

- **Parámetros de Entrada**:
  - Campos editables para cada parámetro
  - Validación de tipos
  - Valores por defecto
  - Descripción de cada parámetro

- **Enviar Request**:
  - Botón para ejecutar la petición
  - Loading durante la petición
  - Manejo de errores

- **Visualización de Response**:
  - Código de estado HTTP
  - Headers de respuesta
  - Body de respuesta (formateado JSON)
  - Tiempo de respuesta
  - Tamaño de la respuesta

#### 4. Funcionalidades Avanzadas

- **Búsqueda de Endpoints**:
  - Buscar endpoints por nombre o ruta
  - Filtrado rápido

- **Favoritos**:
  - Marcar endpoints como favoritos
  - Acceso rápido a endpoints frecuentes

- **Historial de Requests**:
  - Ver historial de peticiones realizadas
  - Reutilizar peticiones anteriores
  - Exportar historial

- **Colecciones**:
  - Agrupar endpoints relacionados
  - Crear colecciones personalizadas

- **Exportar Documentación**:
  - Exportar a formato OpenAPI/Swagger
  - Exportar a Postman Collection
  - Exportar a cURL

- **Copiar Código**:
  - Copiar ejemplo de código en múltiples lenguajes:
    - JavaScript (fetch, axios)
    - Python (requests)
    - cURL
    - PHP
    - etc.

- **Validación de Schema**:
  - Validación de parámetros según schema
  - Sugerencias de corrección

---

## Monitor de Socket.IO

**Ruta**: `/socket-monitor`

### Descripción General

Monitor en tiempo real de eventos de Socket.IO. Útil para debugging, monitoreo de conexiones y entender el flujo de datos en tiempo real.

### Funcionalidades Principales

#### 1. Panel de Control

- **Iniciar Monitoreo**:
  - Botón para comenzar a monitorear eventos
  - Estado: Iniciado/Detenido

- **Pausar/Reanudar**:
  - Pausar temporalmente el monitoreo
  - Reanudar sin perder historial

- **Limpiar Log**:
  - Limpiar todos los eventos registrados
  - Confirmación antes de limpiar

#### 2. Tipos de Eventos Monitoreados

- **Eventos Recibidos**:
  - Eventos que el cliente recibe del servidor
  - Color distintivo
  - Timestamp
  - Datos del evento

- **Eventos Emitidos**:
  - Eventos que el cliente envía al servidor
  - Color distintivo
  - Timestamp
  - Datos del evento

- **Conexiones**:
  - Cuando se establece una conexión
  - Información de la conexión

- **Desconexiones**:
  - Cuando se pierde la conexión
  - Motivo de desconexión (si está disponible)

- **Errores**:
  - Errores de Socket.IO
  - Detalles del error
  - Stack trace (si está disponible)

#### 3. Visualización de Eventos

- **Lista de Eventos**:
  - Lista cronológica de eventos
  - Scroll automático al final (opcional)
  - Filtrado por tipo de evento

- **Detalles del Evento**:
  - Click en evento para ver detalles
  - JSON formateado y coloreado
  - Expandir/colapsar objetos anidados
  - Copiar datos del evento

- **Búsqueda**:
  - Buscar eventos por nombre
  - Buscar en datos del evento
  - Filtrado en tiempo real

#### 4. Funcionalidades Avanzadas

- **Filtros**:
  - Filtrar por tipo de evento
  - Filtrar por rango de tiempo
  - Mostrar solo errores

- **Estadísticas**:
  - Contador de eventos por tipo
  - Tasa de eventos por segundo
  - Tiempo de conexión
  - Número de reconexiones

- **Exportar Logs**:
  - Exportar eventos a archivo JSON
  - Exportar a texto plano
  - Incluir timestamps

- **Auto-scroll**:
  - Opción para auto-scroll al final
  - Útil para monitoreo continuo

- **Límite de Eventos**:
  - Configurar máximo de eventos en memoria
  - Eliminar eventos antiguos automáticamente

- **Highlight de Eventos Importantes**:
  - Resaltar eventos críticos
  - Alertas visuales para errores

---

## Monitor de Webhooks

**Ruta**: `/webhook-monitor`

### Descripción General

Monitor en tiempo real de webhooks entrantes. Permite ver todas las peticiones webhook recibidas, su estado, datos y origen.

### Funcionalidades Principales

#### 1. Lista de Webhooks

**Tabla de Webhooks**:

- **Columnas Disponibles**:
  - **Proveedor**: Proveedor que envió el webhook (PayPal, Stripe, etc.)
  - **Estado**: Estado de procesamiento (success, error, pending)
  - **URL**: URL del endpoint que recibió el webhook
  - **Método**: Método HTTP (POST, GET, etc.)
  - **IP**: Dirección IP de origen
  - **Fecha/Hora**: Timestamp de recepción
  - **Tiempo de Respuesta**: Tiempo de procesamiento (ms)
  - **Acciones**: Ver detalles

#### 2. Filtros

- **Por Proveedor**:
  - Filtrar por proveedor específico
  - Opción "Todos"

- **Por Estado**:
  - Success: Procesado exitosamente
  - Error: Error al procesar
  - Pending: Pendiente de procesar

- **Por Rango de Fechas**:
  - Filtrar por fecha/hora de recepción
  - Última hora, últimas 24 horas, última semana, etc.

- **Búsqueda**:
  - Buscar en URL
  - Buscar en body del webhook

#### 3. Detalles del Webhook

- **Información General**:
  - Proveedor
  - Estado
  - URL
  - Método HTTP
  - IP de origen
  - User-Agent
  - Timestamp

- **Headers**:
  - Todos los headers HTTP recibidos
  - Formato JSON coloreado
  - Expandir/colapsar

- **Body**:
  - Cuerpo de la petición
  - JSON formateado y coloreado
  - Expandir/colapsar objetos anidados
  - Copiar body completo

- **Response**:
  - Respuesta enviada al proveedor
  - Código de estado HTTP
  - Body de respuesta

- **Logs**:
  - Logs de procesamiento
  - Errores (si los hay)
  - Stack trace de errores

#### 4. Estadísticas

- **Resumen General**:
  - Total de webhooks recibidos
  - Webhooks exitosos
  - Webhooks con error
  - Webhooks pendientes

- **Por Proveedor**:
  - Contador por cada proveedor
  - Tasa de éxito por proveedor

- **Por Estado**:
  - Distribución de estados
  - Gráfico de pastel

- **Tiempo de Respuesta**:
  - Tiempo promedio de procesamiento
  - Tiempo mínimo/máximo
  - Gráfico de tiempos

#### 5. Funcionalidades Avanzadas

- **Actualización en Tiempo Real**:
  - Nuevos webhooks aparecen automáticamente
  - Sin necesidad de refrescar

- **Reintentar Webhook**:
  - Reintentar procesamiento de webhook fallido
  - Útil para debugging

- **Exportar**:
  - Exportar webhooks a CSV/JSON
  - Filtrar antes de exportar

- **Búsqueda Avanzada**:
  - Búsqueda en body completo
  - Búsqueda por campos específicos
  - Expresiones regulares (futuro)

- **Límite de Registros**:
  - Configurar máximo de webhooks en memoria
  - Eliminar webhooks antiguos automáticamente

- **Alertas**:
  - Alertar si hay muchos errores
  - Notificar webhooks críticos

---

## Páginas de Pago Públicas

### Descripción General

Estas páginas son accesibles públicamente (sin autenticación) y permiten a los clientes realizar pagos a través de los links generados en la plataforma.

---

### Página de Pago Público

**Ruta**: `/pay/:token`

#### Funcionalidades

1. **Información del Pago**:
   - Monto a pagar
   - Moneda
   - Descripción del pago
   - Información del contacto/cliente
   - Fecha de vencimiento (si aplica)

2. **Selección de Método de Pago**:
   - Botones para cada proveedor disponible:
     - PayPal
     - Stripe
     - Mercado Pago
     - Clip

3. **Proceso de Pago**:
   - Al seleccionar proveedor:
     - **PayPal/Stripe/Mercado Pago**: Redirige a la pasarela externa
     - **Clip**: Muestra checkout integrado en la página
   - Procesamiento del pago
   - Redirección según resultado

4. **Validaciones**:
   - Verificación de token válido
   - Verificación de que el pago no esté vencido
   - Verificación de que el pago no esté ya completado

5. **Mensajes de Error**:
   - Token inválido
   - Pago vencido
   - Pago ya completado
   - Error al procesar

---

### Página de Pago Exitoso

**Ruta**: `/pay/:token/success`

#### Funcionalidades

1. **Mensaje de Éxito**:
   - Confirmación visual de pago exitoso
   - Información del pago completado
   - Monto pagado
   - Fecha y hora del pago

2. **Procesamiento**:
   - Para PayPal: Captura automática del pago si es necesario
   - Actualización del estado del pago en la base de datos
   - Notificaciones al administrador

3. **Información Adicional**:
   - ID de transacción
   - Método de pago utilizado
   - Comprobante (si está disponible)

4. **Redirección**:
   - Opción de redirección a URL personalizada (si está configurada)
   - O permanecer en la página de éxito

---

### Página de Pago Cancelado

**Ruta**: `/pay/:token/cancel`

#### Funcionalidades

1. **Mensaje de Cancelación**:
   - Información de que el pago fue cancelado
   - El pago permanece en estado PENDING
   - Puede reintentarse

2. **Reintentar Pago**:
   - Botón para volver a la página de pago
   - Link funcional para reintentar

3. **Información**:
   - El pago no se ha procesado
   - No se ha cobrado ningún monto
   - Puede intentarse nuevamente

---

### Página de Pago Fallido

**Ruta**: `/pay/:token/failed`

#### Funcionalidades

1. **Mensaje de Error**:
   - Información de que el pago falló
   - Detalles del error (si están disponibles)
   - Razón del fallo

2. **Reintentar Pago**:
   - Botón para reintentar (si no se excedió el límite)
   - Límite de reintentos configurable
   - Mensaje si se excedió el límite

3. **Información del Error**:
   - Código de error (si está disponible)
   - Mensaje de error del proveedor
   - Sugerencias para resolver el problema

4. **Contacto**:
   - Información de contacto para soporte
   - Si el problema persiste

---

## Características Generales de la Plataforma

### 1. Interfaz de Usuario

- **Diseño Moderno**: Interfaz limpia y moderna con Material-UI
- **Modo Oscuro/Claro**: Toggle para cambiar tema
- **Responsive**: Adaptable a móviles, tablets y escritorio
- **Navegación Intuitiva**: Menú lateral con acceso rápido a todas las secciones

### 2. Actualización en Tiempo Real

- **Socket.IO**: Actualizaciones instantáneas sin recargar
- **Notificaciones**: Notificaciones push del navegador
- **Sincronización**: Cambios reflejados en todos los dispositivos conectados

### 3. Búsqueda y Filtros

- **Búsqueda Global**: Buscar en múltiples secciones
- **Filtros Avanzados**: Filtros complejos con múltiples criterios
- **Guardar Filtros**: Guardar filtros favoritos para uso rápido

### 4. Exportación de Datos

- **CSV/Excel**: Exportar tablas a formatos estándar
- **Filtros Aplicados**: Las exportaciones respetan los filtros activos
- **Selección**: Exportar solo elementos seleccionados

### 5. Personalización

- **Columnas Visibles**: Mostrar/ocultar columnas en tablas
- **Ordenamiento**: Ordenar por cualquier columna
- **Preferencias**: Preferencias guardadas en localStorage

### 6. Seguridad

- **Autenticación**: Sistema de autenticación seguro
- **Roles y Permisos**: Control de acceso basado en roles
- **Tokens**: API tokens para integraciones
- **HTTPS**: Comunicación encriptada

### 7. Internacionalización

- **Múltiples Idiomas**: Soporte para varios idiomas
- **Traducción Completa**: Interfaz completamente traducida
- **Formato de Fechas**: Formato según región

### 8. Performance

- **Carga Rápida**: Optimización de carga de páginas
- **Paginación**: Paginación eficiente de grandes listas
- **Lazy Loading**: Carga diferida de componentes

### 9. Accesibilidad

- **Atajos de Teclado**: Navegación por teclado
- **Screen Readers**: Compatible con lectores de pantalla
- **Contraste**: Colores con buen contraste

### 10. Soporte y Ayuda

- **Documentación**: Documentación integrada
- **Tooltips**: Información contextual en elementos
- **Mensajes de Error**: Mensajes claros y accionables

---

## Conclusión

Este manual cubre todas las funcionalidades principales de la plataforma de gestión de WhatsApp. Para más información específica o soporte técnico, consulta la documentación adicional o contacta al equipo de soporte.

---

**Última actualización**: [Fecha de actualización]  
**Versión del Manual**: 1.0




