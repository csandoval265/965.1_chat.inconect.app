# 👨‍💻 Guía de Desarrollo

Esta guía está dirigida a desarrolladores que quieren contribuir o personalizar la plataforma.

## 📋 Tabla de Contenidos

- [Configuración del Entorno de Desarrollo](#-configuración-del-entorno-de-desarrollo)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Backend](#-backend)
- [Frontend](#-frontend)
- [Base de Datos](#-base-de-datos)
- [Testing](#-testing)
- [Convenciones de Código](#-convenciones-de-código)
- [Flujo de Trabajo](#-flujo-de-trabajo)
- [Debugging](#-debugging)

---

## 🛠 Configuración del Entorno de Desarrollo

### Requisitos

- **Node.js** >= 16.0.0
- **npm** >= 8.0.0
- **MySQL/MariaDB** >= 10.6
- **Docker** (opcional, para desarrollo con contenedores)
- **Git**

### Configuración Inicial

```bash
# 1. Clonar el repositorio
git clone <repository-url>
cd WhatsApp

# 2. Instalar dependencias del backend
cd backend
npm install --legacy-peer-deps

# 3. Instalar dependencias del frontend
cd ../frontend
npm install

# 4. Configurar variables de entorno
cd ..
cp env.example .env
# Editar .env con tus configuraciones
```

### Variables de Entorno para Desarrollo

**⚠️ NOTA**: Este proyecto usa Nginx como reverse proxy. Incluso en desarrollo necesitas configurar un dominio y certificados SSL.

```env
# Backend
NODE_ENV=development
DB_HOST=mysql  # o localhost si no usas Docker
DB_USER=root
DB_PASS=tu_contraseña
DB_NAME=whatsapp_dev

# JWT
JWT_SECRET=tu_secret_jwt
JWT_REFRESH_SECRET=tu_refresh_secret

# Encriptación
MESSAGE_ENCRYPTION_KEY=tu_clave_32_caracteres
FILE_ENCRYPTION_KEY=tu_clave_archivos_32_caracteres

# URLs (usar HTTPS con dominio, NO localhost:puerto)
BACKEND_URL=https://whatsapp-api.dev.tu-dominio.com
FRONTEND_URL=https://whatsapp.dev.tu-dominio.com

# Nginx - Configuración de dominios
APP_ENV=develop  # debe coincidir con carpeta en docker/nginx/conf/
FRONTEND_SERVER_NAME=whatsapp.dev.tu-dominio.com
BACKEND_SERVER_NAME=whatsapp-api.dev.tu-dominio.com

# Frontend
VITE_BACKEND_URL=https://whatsapp-api.dev.tu-dominio.com
```

---

## 📁 Estructura del Proyecto

```
WhatsApp/
├── backend/
│   ├── src/
│   │   ├── controllers/      # Controladores (28+ archivos)
│   │   │   ├── AnalyticsController.ts
│   │   │   ├── CampaignController.ts
│   │   │   ├── ChatbotConfigController.ts
│   │   │   ├── ContactController.ts
│   │   │   ├── MessageController.ts
│   │   │   ├── PaymentController.ts
│   │   │   ├── TicketController.ts
│   │   │   └── ... (más controladores)
│   │   ├── services/          # Lógica de negocio (150+ servicios)
│   │   │   ├── AIChatbotServices/    # Servicios de chatbot IA
│   │   │   ├── CampaignServices/      # Servicios de campañas
│   │   │   ├── ContactServices/       # Servicios de contactos
│   │   │   ├── PaymentProviders/      # Proveedores de pago
│   │   │   ├── PaymentServices/       # Servicios de pagos
│   │   │   ├── TicketServices/        # Servicios de tickets
│   │   │   └── ... (más servicios)
│   │   ├── models/            # Modelos de Sequelize (27+ modelos)
│   │   │   ├── Campaign.ts
│   │   │   ├── ChatbotConfig.ts
│   │   │   ├── Contact.ts
│   │   │   ├── Message.ts
│   │   │   ├── Payment.ts
│   │   │   ├── Ticket.ts
│   │   │   └── ... (más modelos)
│   │   ├── routes/            # Definición de rutas (26+ archivos)
│   │   │   ├── campaignRoutes.ts
│   │   │   ├── chatbotConfigRoutes.ts
│   │   │   ├── contactRoutes.ts
│   │   │   ├── paymentRoutes.ts
│   │   │   └── ... (más rutas)
│   │   ├── middleware/        # Middlewares personalizados
│   │   ├── helpers/           # Funciones auxiliares (22+ archivos)
│   │   ├── config/            # Configuraciones
│   │   ├── database/          # Migraciones y seeds (127+ archivos)
│   │   ├── errors/            # Manejo de errores
│   │   ├── libs/              # Librerías personalizadas
│   │   └── utils/             # Utilidades
│   ├── dist/                  # Código compilado (TypeScript → JavaScript)
│   └── package.json
│
├── frontend/
│   ├── src/
│   │   ├── components/        # Componentes React reutilizables (50+)
│   │   ├── pages/             # Páginas/views
│   │   ├── services/          # Servicios API
│   │   ├── context/           # Context API (estado global)
│   │   ├── hooks/             # Custom hooks
│   │   ├── routes/             # Configuración de rutas
│   │   ├── translate/         # Traducciones i18n (ES/EN)
│   │   ├── css/               # Estilos globales
│   │   └── utils/             # Utilidades
│   └── package.json
│
├── docker/                     # Configuración Docker
│   ├── nginx/                 # Configuración Nginx
│   ├── mysql/                 # Configuración MySQL
│   └── certbot/               # Certificados SSL
│
└── docs/                      # Documentación completa
```

---

## 🔧 Backend

### Tecnologías

- **Node.js** + **TypeScript** (4.7+)
- **Express.js** - Framework web
- **Sequelize** + **Sequelize TypeScript** - ORM
- **Socket.IO** - WebSockets para tiempo real
- **JWT** - Autenticación con refresh tokens
- **Jest** - Testing
- **whatsapp-web.js** - Integración con WhatsApp
- **Pino** - Logging
- **Yup** - Validación de esquemas

### Comandos Disponibles

```bash
cd backend

# Desarrollo
npm run dev              # Ejecutar en modo desarrollo (hot reload)

# Compilación
npm run build            # Compilar TypeScript
npm run watch            # Compilar en modo watch

# Base de datos
npm run db:migrate       # Ejecutar migraciones
npm run db:seed          # Ejecutar seeds
npm run db:migrate:undo  # Revertir última migración

# Testing
npm test                 # Ejecutar tests
npm run test:watch       # Tests en modo watch

# Formato
npm run format           # Formatear código con Prettier
```

### Estructura de un Controlador

```typescript
// backend/src/controllers/ExampleController.ts
import { Request, Response } from "express";
import ExampleService from "../services/ExampleService";

export const index = async (req: Request, res: Response): Promise<Response> => {
  const examples = await ExampleService.list();
  return res.status(200).json(examples);
};

export const store = async (req: Request, res: Response): Promise<Response> => {
  const { name, email } = req.body;
  const example = await ExampleService.create({ name, email });
  return res.status(201).json(example);
};
```

### Estructura de un Servicio

```typescript
// backend/src/services/ExampleService.ts
import Example from "../models/Example";
import AppError from "../errors/AppError";
import { ErrorCode } from "../errors/ErrorCodes";

interface Request {
  name: string;
  email: string;
}

const create = async ({ name, email }: Request): Promise<Example> => {
  // Validaciones
  if (!name || !email) {
    throw new AppError(ErrorCode.ERR_VALIDATION_ERROR);
  }

  // Lógica de negocio
  const example = await Example.create({ name, email });
  return example;
};

export default { create };
```

### Crear una Nueva Ruta

1. **Crear el controlador** en `backend/src/controllers/`
2. **Crear el servicio** en `backend/src/services/`
3. **Definir la ruta** en `backend/src/routes/`
4. **Registrar la ruta** en `backend/src/routes/index.ts`

Ejemplo:

```typescript
// backend/src/routes/exampleRoutes.ts
import { Router } from "express";
import * as ExampleController from "../controllers/ExampleController";
import isAuth from "../middleware/isAuth";

const exampleRoutes = Router();

exampleRoutes.get("/examples", isAuth, ExampleController.index);
exampleRoutes.post("/examples", isAuth, ExampleController.store);

export default exampleRoutes;
```

### Migraciones de Base de Datos

```bash
# Crear una nueva migración
npx sequelize-cli migration:generate --name nombre-de-la-migracion

# Ejecutar migraciones
npm run db:migrate

# Revertir última migración
npm run db:migrate:undo
```

Ejemplo de migración:

```typescript
// backend/src/database/migrations/XXXXXX-create-example.ts
import { QueryInterface, DataTypes } from "sequelize";

export default {
  up: (queryInterface: QueryInterface) => {
    return queryInterface.createTable("Examples", {
      id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
      },
      name: {
        type: DataTypes.STRING,
        allowNull: false
      },
      createdAt: {
        type: DataTypes.DATE,
        allowNull: false
      },
      updatedAt: {
        type: DataTypes.DATE,
        allowNull: false
      }
    });
  },
  down: (queryInterface: QueryInterface) => {
    return queryInterface.dropTable("Examples");
  }
};
```

---

## 🎨 Frontend

### Tecnologías

- **React** 16.13+
- **Material-UI** - Componentes UI
- **Vite** - Build tool
- **Socket.IO Client** - WebSockets
- **React Router** - Routing
- **i18next** - Internacionalización

### Comandos Disponibles

```bash
cd frontend

# Desarrollo
npm run dev              # Servidor de desarrollo (hot reload)
npm start                # Alias de dev

# Build
npm run build            # Construir para producción
npm run preview          # Preview del build de producción
```

### Estructura de un Componente

```jsx
// frontend/src/components/Example/index.js
import React, { useState, useEffect } from "react";
import { makeStyles } from "@material-ui/core/styles";
import api from "../../services/api";

const useStyles = makeStyles((theme) => ({
  root: {
    padding: theme.spacing(2)
  }
}));

const Example = () => {
  const classes = useStyles();
  const [data, setData] = useState([]);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const { data } = await api.get("/examples");
        setData(data);
      } catch (err) {
        console.error(err);
      }
    };
    fetchData();
  }, []);

  return (
    <div className={classes.root}>
      {/* Tu componente aquí */}
    </div>
  );
};

export default Example;
```

### Agregar una Nueva Página

1. **Crear el componente** en `frontend/src/pages/Example/index.js`
2. **Agregar la ruta** en `frontend/src/routes/index.js`

```jsx
// frontend/src/routes/index.js
import Example from "../pages/Example";

// En el Switch
<Route exact path="/example" component={Example} isPrivate />
```

### Servicios API

```javascript
// frontend/src/services/api.js
import axios from "axios";

const api = axios.create({
  baseURL: process.env.REACT_APP_BACKEND_URL || "http://localhost:8080"
});

// Interceptor para agregar token
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

### Agregar Traducciones

```javascript
// frontend/src/translate/languages/es.js
const messages = {
  es: {
    translations: {
      example: {
        title: "Ejemplo",
        description: "Descripción del ejemplo"
      }
    }
  }
};

// Uso en componente
import { i18n } from "../../translate/i18n";
const title = i18n.t("example.title");
```

---

## 🗄 Base de Datos

### Modelos

Los modelos están definidos en `backend/src/models/` usando Sequelize TypeScript.

```typescript
// backend/src/models/Example.ts
import {
  Table,
  Column,
  Model,
  PrimaryKey,
  AutoIncrement,
  CreatedAt,
  UpdatedAt
} from "sequelize-typescript";

@Table
class Example extends Model<Example> {
  @PrimaryKey
  @AutoIncrement
  @Column
  id: number;

  @Column
  name: string;

  @CreatedAt
  createdAt: Date;

  @UpdatedAt
  updatedAt: Date;
}

export default Example;
```

### Relaciones

```typescript
// Ejemplo de relación
@HasMany(() => Ticket)
tickets: Ticket[];

@BelongsTo(() => User)
user: User;
```

---

## 🧪 Testing

### Backend

```bash
cd backend

# Ejecutar todos los tests
npm test

# Tests en modo watch
npm run test:watch

# Coverage
npm test -- --coverage
```

Ejemplo de test:

```typescript
// backend/src/__tests__/services/ExampleService.test.ts
import ExampleService from "../../services/ExampleService";

describe("ExampleService", () => {
  it("should create an example", async () => {
    const example = await ExampleService.create({
      name: "Test",
      email: "test@example.com"
    });

    expect(example).toHaveProperty("id");
    expect(example.name).toBe("Test");
  });
});
```

---

## 📝 Convenciones de Código

### TypeScript/JavaScript

- **Nombres de archivos**: camelCase para servicios/helpers, PascalCase para componentes
- **Nombres de funciones**: camelCase
- **Nombres de clases**: PascalCase
- **Constantes**: UPPER_SNAKE_CASE
- **Indentación**: 2 espacios
- **Comillas**: Comillas simples para strings

### Git Commits

Usa mensajes descriptivos:

```
feat: agregar funcionalidad de exportar tickets
fix: corregir error en conexión de WhatsApp
docs: actualizar documentación de API
refactor: mejorar estructura de servicios
test: agregar tests para pagos
```

### Estructura de Commits

```
<tipo>: <descripción corta>

[descripción detallada opcional]

[referencias a issues opcionales]
```

Tipos: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

---

## 🔄 Flujo de Trabajo

### 1. Crear una Rama

```bash
git checkout -b feature/nueva-funcionalidad
# o
git checkout -b fix/corregir-bug
```

### 2. Desarrollar

- Escribe código siguiendo las convenciones
- Agrega tests si es necesario
- Actualiza documentación si es necesario

### 3. Commit

```bash
git add .
git commit -m "feat: descripción del cambio"
```

### 4. Push y Pull Request

```bash
git push origin feature/nueva-funcionalidad
```

Luego crea un Pull Request en GitHub/GitLab.

---

## 🐛 Debugging

### Backend

```bash
# Ver logs en tiempo real
docker-compose logs -f backend

# O si ejecutas localmente
npm run dev
# Los logs aparecerán en la consola
```

### Frontend

- Usa las **DevTools** del navegador
- **React DevTools** para inspeccionar componentes
- **Network tab** para ver peticiones API

### Base de Datos

```bash
# Acceder a MySQL
docker-compose exec mysql mysql -u root -p

# O localmente
mysql -u root -p whatsapp_dev
```

### Socket.IO

Los eventos de Socket.IO se pueden monitorear en:
- Frontend: `frontend/src/pages/SocketMonitor`
- Backend: logs del servidor

---

## 📚 Recursos Adicionales

- [Documentación de Express.js](https://expressjs.com/)
- [Documentación de Sequelize](https://sequelize.org/)
- [Documentación de React](https://react.dev/)
- [Documentación de Material-UI](https://mui.com/)
- [Documentación de Socket.IO](https://socket.io/docs/)

---

## ❓ Preguntas Frecuentes

**¿Cómo agrego una nueva funcionalidad?**
1. Crea una rama nueva
2. Implementa la funcionalidad siguiendo la estructura existente
3. Agrega tests
4. Crea un Pull Request

**¿Dónde debo poner la lógica de negocio?**
- En los **servicios** (`backend/src/services/`)
- Los controladores solo deben manejar request/response

**¿Cómo manejo errores?**
- Usa `AppError` y `ErrorCode` del sistema de errores
- Los errores se manejan automáticamente por el middleware

**¿Cómo agrego una nueva tabla?**
1. Crea el modelo en `backend/src/models/`
2. Crea la migración
3. Ejecuta la migración

---

¿Necesitas más ayuda? Consulta la [documentación principal](../README.md) o crea un issue.

