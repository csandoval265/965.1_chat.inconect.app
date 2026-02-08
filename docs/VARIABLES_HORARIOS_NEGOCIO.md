# 📅 Variables de Horarios de Negocio - Explicación

Estas variables se usan en los **mensajes de fuera de horario** para mostrar información dinámica sobre cuándo estará disponible el negocio.

---

## 🎯 ¿Cuándo se Usan?

Estas variables están disponibles cuando:
- El negocio está **fuera de horario de atención**
- Se envía un mensaje automático de "fuera de horario"
- Se usa en el campo **"Mensaje fuera de horario"** de una cola

---

## 📋 Variables Disponibles

### 🔵 Variables de Próxima Apertura (Next Opening)

Estas variables muestran información sobre **cuándo abrirá el negocio próximamente**.

#### `{{nextOpeningTime}}`
- **Formato**: `HH:mm` (ejemplo: `09:00`)
- **Descripción**: Hora de la próxima apertura
- **Ejemplo**: `09:00`

#### `{{nextOpeningDay}}`
- **Formato**: Nombre del día en español
- **Descripción**: Día de la semana de la próxima apertura
- **Ejemplos**: `Lunes`, `Martes`, `Miércoles`, etc.

#### `{{nextOpeningDate}}`
- **Formato**: `dd/MM/yyyy`
- **Descripción**: Fecha de la próxima apertura en formato corto
- **Ejemplo**: `15/01/2024`

#### `{{nextOpeningDateFull}}`
- **Formato**: Fecha completa en español
- **Descripción**: Fecha de la próxima apertura en formato largo
- **Ejemplo**: `15 de enero de 2024`

#### `{{nextOpeningText}}`
- **Formato**: Texto completo formateado
- **Descripción**: Texto listo para usar que combina día y hora
- **Ejemplos**: 
  - `hoy a las 09:00` (si abre hoy)
  - `el Lunes a las 09:00` (si abre otro día)

---

### 🔴 Variables de Próximo Cierre (Next Closing)

Estas variables muestran información sobre **cuándo cerrará el negocio** (útil cuando está abierto pero va a cerrar pronto).

#### `{{nextClosingTime}}`
- **Formato**: `HH:mm` (ejemplo: `18:00`)
- **Descripción**: Hora del próximo cierre
- **Ejemplo**: `18:00`

#### `{{nextClosingDay}}`
- **Formato**: Nombre del día en español
- **Descripción**: Día de la semana del próximo cierre
- **Ejemplos**: `Lunes`, `Martes`, etc.

#### `{{nextClosingDate}}`
- **Formato**: `dd/MM/yyyy`
- **Descripción**: Fecha del próximo cierre en formato corto
- **Ejemplo**: `15/01/2024`

#### `{{nextClosingDateFull}}`
- **Formato**: Fecha completa en español
- **Descripción**: Fecha del próximo cierre en formato largo
- **Ejemplo**: `15 de enero de 2024`

#### `{{nextClosingText}}`
- **Formato**: Texto completo formateado
- **Descripción**: Texto listo para usar que combina día y hora
- **Ejemplos**: 
  - `hoy a las 18:00` (si cierra hoy)
  - `el Lunes a las 18:00` (si cierra otro día)

---

### 🟢 Variables de Horario Actual (Current Hours)

Estas variables muestran información sobre el **horario actual** cuando el negocio está abierto.

#### `{{openTime}}`
- **Formato**: `HH:mm`
- **Descripción**: Hora de apertura del horario actual
- **Ejemplo**: `09:00`

#### `{{openDay}}`
- **Formato**: Nombre del día en español
- **Descripción**: Día de la semana de la apertura actual
- **Ejemplo**: `Lunes`

#### `{{openDate}}`
- **Formato**: `dd/MM/yyyy`
- **Descripción**: Fecha de la apertura actual
- **Ejemplo**: `15/01/2024`

#### `{{openDateFull}}`
- **Formato**: Fecha completa en español
- **Descripción**: Fecha completa de la apertura actual
- **Ejemplo**: `15 de enero de 2024`

#### `{{openText}}`
- **Formato**: Texto completo formateado
- **Descripción**: Texto listo para usar de la apertura actual
- **Ejemplo**: `hoy a las 09:00`

#### `{{closeTime}}`
- **Formato**: `HH:mm`
- **Descripción**: Hora de cierre del horario actual
- **Ejemplo**: `18:00`

#### `{{closeDay}}`
- **Formato**: Nombre del día en español
- **Descripción**: Día de la semana del cierre actual
- **Ejemplo**: `Lunes`

#### `{{closeDate}}`
- **Formato**: `dd/MM/yyyy`
- **Descripción**: Fecha del cierre actual
- **Ejemplo**: `15/01/2024`

#### `{{closeDateFull}}`
- **Formato**: Fecha completa en español
- **Descripción**: Fecha completa del cierre actual
- **Ejemplo**: `15 de enero de 2024`

#### `{{closeText}}`
- **Formato**: Texto completo formateado
- **Descripción**: Texto listo para usar del cierre actual
- **Ejemplo**: `hoy a las 18:00`

---

## 💡 Ejemplos de Uso

### Ejemplo 1: Mensaje Simple
```
Estamos fuera de horario. Volveremos a abrir {{nextOpeningText}}.
```

**Resultado**: "Estamos fuera de horario. Volveremos a abrir el Lunes a las 09:00."

---

### Ejemplo 2: Mensaje Detallado
```
¡Hola! Actualmente estamos fuera de horario de atención.

Nuestro próximo horario de atención es:
📅 Día: {{nextOpeningDay}}
🕐 Hora: {{nextOpeningTime}}
📆 Fecha: {{nextOpeningDateFull}}

Te responderemos tan pronto como sea posible.
```

**Resultado**:
```
¡Hola! Actualmente estamos fuera de horario de atención.

Nuestro próximo horario de atención es:
📅 Día: Lunes
🕐 Hora: 09:00
📆 Fecha: 15 de enero de 2024

Te responderemos tan pronto como sea posible.
```

---

### Ejemplo 3: Mensaje con Múltiples Variables
```
Estamos cerrados ahora. 

Abrimos nuevamente el {{nextOpeningDay}} {{nextOpeningDateFull}} a las {{nextOpeningTime}}.

Horario de atención: {{openTime}} - {{closeTime}}
```

**Resultado**:
```
Estamos cerrados ahora. 

Abrimos nuevamente el Lunes 15 de enero de 2024 a las 09:00.

Horario de atención: 09:00 - 18:00
```

---

### Ejemplo 4: Mensaje Corto y Directo
```
Fuera de horario. Abrimos {{nextOpeningText}}.
```

**Resultado**: "Fuera de horario. Abrimos el Lunes a las 09:00."

---

## 🔄 ¿Cómo Funciona?

1. **El sistema verifica** si el negocio está abierto o cerrado según los horarios configurados
2. **Si está cerrado**, calcula automáticamente:
   - Cuándo será la próxima apertura
   - Qué día y hora será
   - Formatea las fechas en español
3. **Reemplaza las variables** en el mensaje con los valores reales
4. **Envía el mensaje** al cliente con la información actualizada

---

## ⚙️ Configuración

Para que estas variables funcionen:

1. **Habilita horarios de negocio** en la cola
2. **Configura los horarios** de cada día
3. **Escribe el mensaje de fuera de horario** usando las variables
4. **El sistema reemplazará automáticamente** las variables con los valores correctos

---

## 📝 Notas Importantes

- Las variables se reemplazan **automáticamente** cuando se envía el mensaje
- Si no hay horarios configurados, algunas variables pueden estar vacías
- Las fechas se muestran en **español** (meses y días)
- El formato de hora es **24 horas** (09:00, 18:00, etc.)
- Las variables `*Text` ya incluyen el formato completo (día + hora)

---

## 🎯 Recomendación

Para mensajes más simples, usa las variables `*Text`:
- `{{nextOpeningText}}` - Ya incluye "hoy a las..." o "el [día] a las..."
- `{{nextClosingText}}` - Similar para cierres

Para mensajes más personalizados, usa las variables individuales:
- `{{nextOpeningDay}}`, `{{nextOpeningTime}}`, `{{nextOpeningDateFull}}`

---

**¡Estas variables hacen que tus mensajes de fuera de horario sean más informativos y útiles para tus clientes!**




