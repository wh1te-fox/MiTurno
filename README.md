<h1 align="center">MiTurno</h1>

<p align="center">
  <img src="vue-project/src/assets/logoMI.png" alt="MiTurno logo" width="180" />
</p>

<p align="center">
  <strong>Sistema de gestión de pacientes y citas médicas para hospitales públicos de Nicaragua</strong>
</p>

<p align="center">
  <a href="https://github.com/wh1te-fox/MiTurno"><img src="https://img.shields.io/badge/status-in--development-yellow?style=for-the-badge" alt="Status" /></a>
  <a href="https://vuejs.org/"><img src="https://img.shields.io/badge/Vue-3-42B883?style=for-the-badge&logo=vue.js&logoColor=white" alt="Vue" /></a>
  <a href="https://vite.dev/"><img src="https://img.shields.io/badge/Vite-8-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite" /></a>
  <a href="https://tailwindcss.com/"><img src="https://img.shields.io/badge/Tailwind-CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="Tailwind" /></a>
  <a href="https://nestjs.com/"><img src="https://img.shields.io/badge/NestJS-backend-E0234E?style=for-the-badge&logo=nestjs&logoColor=white" alt="NestJS" /></a>
  <a href="https://www.postgresql.org/"><img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-GPLv3-blue?style=for-the-badge" alt="License" /></a>
</p>

<p align="center">
  <a href="#1-descripción-general-del-proyecto">Descripción</a> •
  <a href="#3-funcionalidades-principales">Funcionalidades</a> •
  <a href="#4-arquitectura-del-sistema">Arquitectura</a> •
  <a href="#8-instalación">Instalación</a> •
  <a href="#15-pruebas">Pruebas</a> •
  <a href="#20-limitaciones-y-hoja-de-ruta">Roadmap</a>
</p>

---

> **Estado:** MiTurno está en desarrollo activo. El repositorio contiene el prototipo del frontend en Vue 3; el backend (NestJS + Prisma + PostgreSQL) es parte de la arquitectura objetivo y aún no está implementado. Este README es el índice operativo del proyecto; la documentación especializada vive en los documentos enlazados.

---

## 1. Descripción general del proyecto

MiTurno es una aplicación web para digitalizar el registro de pacientes, la programación de citas y las operaciones administrativas de hospitales públicos de Nicaragua.

Resuelve la gestión manual de agendas, la falta de disponibilidad centralizada y la ausencia de trazabilidad en los procesos de reserva, cancelación y reprogramación de citas.

## 2. Contexto de negocio

- **Mercado inicial:** Nicaragua.
- **Zona horaria operativa:** `America/Managua`.
- **Idioma de interfaz:** español.
- **Actores:** administrador, recepcionista, médico y paciente (portal de autoservicio previsto).
- **Dominio:** salud pública; los datos personales y clínicos son información sensible y requieren controles de acceso y auditoría.

Detalle completo en [`CONTEXT.md`](CONTEXT.md), [`PRD.md`](PRD.md) y [`SECURITY.md`](SECURITY.md).

## 3. Funcionalidades principales

**Implementadas en el prototipo frontend:**

- Portal del paciente: panel, citas, historial, perfil y configuración.
- Portal institucional: panel, agenda, citas, pacientes, sala de espera, personal, reportes y configuración.
- Flujo de agendamiento y modales de gestión de citas.
- Formulario de registro de pacientes.
- Interfaz responsive.

**Objetivo funcional (backend pendiente):**

- Autenticación y autorización por roles.
- Gestión de usuarios, hospitales, especialidades y profesionales.
- Configuración de horarios y consulta de disponibilidad.
- Creación, cancelación y reprogramación de citas con historial.
- Auditoría de operaciones sensibles.

## 4. Arquitectura del sistema

Arquitectura objetivo cliente-servidor:

```text
Frontend SPA (Vue 3 + TypeScript + Tailwind)
        │  REST /api/v1
        ▼
Backend API (NestJS + Prisma)
        │
        ▼
PostgreSQL (UUID, TIMESTAMPTZ, migraciones versionadas)
```

Reglas clave:

- El frontend solo se comunica mediante la API REST; nunca accede directamente a PostgreSQL.
- El backend valida, autoriza y aplica las reglas de negocio.
- La integridad de citas (sin solapamientos por profesional) se protege en backend y en PostgreSQL.
- Módulos por dominio: auth, users, roles, hospitals, patients, specialties, professionals, schedules, appointments y audit.

Referencia obligatoria: [`ARCHITECTURE.md`](ARCHITECTURE.md).

## 5. Decisiones tecnológicas

| Tecnología | Rol | Estado |
|---|---|---|
| Vue 3 (Composition API) | Interfaz de usuario | Implementado (JavaScript) |
| Vite | Desarrollo y compilación | Implementado |
| Tailwind CSS | Sistema de estilos | Implementado vía CDN |
| TypeScript | Tipado estático | Objetivo |
| Vue Router / TanStack Vue Query / VeeValidate + Zod | Navegación, estado remoto y formularios | Objetivo |
| NestJS | API REST | Objetivo |
| Prisma ORM | Persistencia y migraciones | Objetivo |
| PostgreSQL | Base de datos relacional | Objetivo |
| JWT + Argon2id | Autenticación y hash de contraseñas | Objetivo |

Justificación y reglas de dependencias en [`ARCHITECTURE.md`](ARCHITECTURE.md).

## 6. Estructura del proyecto

```text
MiTurno
├── vue-project/            # Frontend Vue 3 + Vite
│   ├── public/
│   ├── src/
│   │   ├── assets/         # Recursos estáticos
│   │   ├── components/     # Componentes reutilizables y de layout
│   │   ├── views/          # Vistas por dominio (paciente e institución)
│   │   ├── App.vue
│   │   └── main.js
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── ARCHITECTURE.md         # Arquitectura técnica oficial
├── SPEC.md                 # Especificación funcional y de datos
├── PRD.md                  # Requerimientos del producto
├── SECURITY.md             # Políticas de seguridad
├── CONTEXT.md              # Contexto del negocio
├── AGENTS.md               # Reglas de trabajo para agentes
├── LICENSE
└── README.md
```

> La documentación especializada se mantiene actualmente en la raíz. La estructura objetivo contempla `docs/` para arquitectura, módulos, API y onboarding.

## 7. Requisitos previos

- **Node.js** `^20.19.0` o `>=22.12.0`.
- **npm** (incluido con Node.js).
- **Git**.
- Backend objetivo: PostgreSQL 16 y Docker (pendiente de incorporación).

## 8. Instalación

```bash
# 1. Clonar el repositorio
git clone git@github.com:wh1te-fox/MiTurno.git
cd MiTurno

# 2. Entrar al frontend
cd vue-project

# 3. Instalar dependencias
npm install

# 4. Ejecutar en desarrollo
npm run dev

# 5. Compilar para producción
npm run build

# 6. Previsualizar la compilación
npm run preview
```

## 9. Variables de entorno

Actualmente **no existen variables de entorno** en el repositorio (no hay archivos `.env`).

Previstas según la arquitectura objetivo (nombres y valores por definir):

| Variable | Propósito | Ámbito |
|---|---|---|
| `VITE_API_URL` | URL base de la API REST | Frontend |
| `DATABASE_URL` | Conexión a PostgreSQL | Backend |
| `JWT_SECRET` / `JWT_EXPIRES_IN` | Firma y expiración de tokens | Backend |
| `PORT` / `NODE_ENV` | Puerto y entorno de ejecución | Backend |

Nunca se deben versionar secretos ni credenciales. Ver [`SECURITY.md`](SECURITY.md).

## 10. Base de datos

Aún **no existe `schema.prisma` ni migraciones**. El modelo objetivo definido en [`SPEC.md`](SPEC.md) incluye:

| Entidad | Propósito |
|---|---|
| `hospitals` | Centros hospitalarios |
| `roles` | Roles del sistema |
| `users` | Cuentas de acceso |
| `patients` | Datos de identificación de pacientes |
| `specialties` | Catálogo de especialidades |
| `professionals` | Profesionales médicos |
| `schedules` | Disponibilidad horaria |
| `appointments` | Citas médicas |
| `appointment_history` | Trazabilidad de estados |
| `audit_logs` | Operaciones sensibles |

Reglas: UUID como PK/FK, `TIMESTAMPTZ` para instantes, `DATE` para fechas de nacimiento, migraciones Prisma versionadas y restricción de exclusión GiST para evitar solapamientos de citas.

## 11. Flujo de la aplicación

**Estado actual:** `App.vue` monta `AppHeader`, `HomeView` y el `BookingModal`. Las vistas de paciente e institución existen pero aún no están conectadas mediante router.

**Flujo objetivo:**

1. El usuario se autentica y obtiene un token.
2. El frontend consulta datos mediante servicios REST.
3. Recepción registra al paciente y consulta disponibilidad.
4. Se selecciona profesional, fecha y hora.
5. El backend valida reglas y evita solapamientos en una transacción.
6. Se crea la cita y se registra el historial y la auditoría.
7. Cancelaciones y reprogramaciones actualizan el estado conservando trazabilidad.

## 12. Documentación de la API

La API aún **no está implementada**. Contrato objetivo (prefijo `/api/v1`) según [`SPEC.md`](SPEC.md):

| Recurso | Endpoints previstos |
|---|---|
| Autenticación | `POST /auth/login`, `POST /auth/logout`, `GET /auth/me` |
| Pacientes | `POST/GET /patients`, `GET/PATCH /patients/:id` |
| Profesionales | `POST/GET /professionals`, `GET/PATCH /professionals/:id` |
| Especialidades | `POST/GET /specialties`, `GET /specialties/:id` |
| Horarios | `POST/GET /schedules`, `GET/PATCH /schedules/:id` |
| Citas | `POST/GET /appointments`, `GET/PATCH /appointments/:id`, `POST /appointments/:id/cancel`, `POST /appointments/:id/reschedule`, `GET /appointments/availability` |

Los contratos deberán documentarse con Swagger/OpenAPI.

## 13. Autenticación y autorización

**Objetivo (pendiente de implementación):**

- Autenticación mediante JWT.
- Contraseñas con hash **Argon2id**; nunca en texto plano ni en logs.
- Autorización basada en roles: `SUPER_ADMIN`, `INSTITUTION_ADMIN`, `RECEPTIONIST`, `DOCTOR`, `PATIENT`.
- Verificación de permisos y alcance institucional en el backend.
- Protección contra acceso horizontal no autorizado.
- Auditoría de operaciones sensibles.

Las protecciones del frontend no sustituyen la autorización del backend. Ver [`SECURITY.md`](SECURITY.md).

## 14. Flujo de desarrollo

- Trabajar en ramas descriptivas (`feat/`, `fix/`, `docs/`, `refactor/`); no sobre `main` sin autorización.
- Commits con **Conventional Commits**: `<type>(<scope>): <description>`, en inglés e imperativo.
- Reutilizar entidades, DTOs y servicios existentes; no duplicar lógica de negocio.
- Validar entradas y aplicar autorización en el backend.
- Actualizar documentación y migraciones cuando cambie el comportamiento.
- No introducir dependencias sin justificación.

Reglas completas en [`AGENTS.md`](AGENTS.md).

## 15. Pruebas

Actualmente **no hay framework de pruebas configurado** ni script `test` en `package.json`.

Estrategia objetivo:

- **Backend:** pruebas unitarias de servicios/DTOs y de integración de endpoints y reglas de citas.
- **Frontend:** pruebas de componentes, formularios, estados de carga/error e integración con servicios REST.
- **Base de datos:** verificación de relaciones, restricciones y migraciones.
- Ejecutar pruebas y compilación antes de proponer un commit.

## 16. Despliegue

Aún **no hay configuración de despliegue**.

Objetivo según [`ARCHITECTURE.md`](ARCHITECTURE.md):

- Docker y Docker Compose para backend y base de datos.
- GitHub Actions para integración continua.
- Entornos separados: `development`, `test` y `production`.
- Frontend: `npm run build` genera `vue-project/dist`.
- HTTPS obligatorio en producción.

## 17. Observabilidad

No implementada. Objetivo:

- Logs estructurados sin datos clínicos ni personales sensibles.
- Registro de errores y monitoreo del backend.
- Auditoría de operaciones administrativas mediante `audit_logs`.
- Copias de seguridad y procedimientos de restauración.
- Monitoreo de consultas lentas y disponibilidad del servicio.

## 18. Manejo de errores

**Actual:** el frontend no cuenta con manejo centralizado de errores.

**Objetivo:** respuestas consistentes de la API:

```json
{ "success": true, "data": {}, "message": "Operación realizada correctamente" }
```

```json
{ "success": false, "error": { "code": "APPOINTMENT_CONFLICT", "message": "El horario seleccionado no está disponible" } }
```

No exponer trazas internas ni información sensible. La interfaz debe mostrar estados de carga, error, vacío y éxito.

## 19. Solución de problemas

| Problema | Causa probable | Solución |
|---|---|---|
| `npm install` falla | Versión de Node incompatible | Usar Node `^20.19.0` o `>=22.12.0` |
| Estilos no se aplican | Tailwind se carga por CDN en `index.html` | Verificar conexión a red o migrar a Tailwind integrado |
| Vistas no navegables | No hay Vue Router configurado | Solo `HomeView` está montada actualmente |
| Puerto ocupado en `npm run dev` | Otro proceso usa el puerto | Detener el proceso o ejecutar Vite con otro puerto |
| Cambios no reflejados | Caché de dependencias | Eliminar `node_modules` y reinstalar |

## 20. Limitaciones y hoja de ruta

**Limitaciones conocidas:**

- Backend, base de datos y API no implementados.
- Frontend en JavaScript, sin TypeScript ni Tailwind integrado.
- Sin Vue Router, gestión de estado remoto ni validación de formularios.
- Sin autenticación, autorización, pruebas ni CI/CD.

**Hoja de ruta (según [`PRD.md`](PRD.md)):**

- **P0 — MVP:** usuarios y autenticación, pacientes, hospitales, especialistas, disponibilidad, citas, cancelación/reprogramación, permisos y auditoría básica.
- **P1:** gestión de servicios, notificaciones, recordatorios, ubicación/mapas, reportes, lista de espera y agenda automática.
- **P2:** expediente clínico completo, constancias médicas, reportes avanzados y portal ampliado del paciente.

---

## Documentación especializada

| Documento | Contenido |
|---|---|
| [`ARCHITECTURE.md`](ARCHITECTURE.md) | Arquitectura técnica y stack oficial |
| [`SPEC.md`](SPEC.md) | Especificación funcional, modelo de datos y API |
| [`PRD.md`](PRD.md) | Requerimientos del producto y priorización |
| [`SECURITY.md`](SECURITY.md) | Políticas y controles de seguridad |
| [`CONTEXT.md`](CONTEXT.md) | Contexto de negocio y visión del producto |
| [`AGENTS.md`](AGENTS.md) | Convenciones de trabajo y commits |

## Licencia

Distribuido bajo la licencia **GPL-3.0**. Consulta [`LICENSE`](LICENSE) para obtener más información.

([volver arriba](#miturno))
