
# MiTurno — Functional and Technical Specification

**Versión:** 1.0  
**Estado:** Especificación objetivo propuesta  
**Idioma de interfaz:** Español  
**Zona horaria:** America/Managua

## 1. Objetivo del sistema

MiTurno permite administrar el registro de pacientes, la programación de citas y las operaciones administrativas de hospitales públicos.

El sistema debe proporcionar:

- Registro y consulta de pacientes.
- Administración de profesionales médicos.
- Gestión de especialidades.
- Configuración de horarios de atención.
- Programación y cancelación de citas.
- Consulta del historial de citas.
- Administración de usuarios y permisos.
- Auditoría de operaciones relevantes.

## 2. Actores del sistema

### SUPER_ADMIN

Responsable de la administración general del sistema.

Permisos:

- Gestionar usuarios.
- Asignar roles.
- Administrar hospitales.
- Configurar especialidades.
- Consultar auditorías.

### INSTITUTION_ADMIN

Responsable de la administración de una institución.

Permisos:

- Administrar el perfil de su institución.
- Gestionar especialistas y agendas de su institución.
- Consultar citas y reportes institucionales.
- Gestionar usuarios institucionales autorizados.

No puede acceder a otras instituciones ni modificar configuraciones globales.

### RECEPTIONIST

Personal encargado de la atención administrativa.

Permisos:

- Registrar pacientes.
- Consultar pacientes.
- Programar citas.
- Reprogramar citas.
- Cancelar citas según sus permisos.
- Consultar disponibilidad médica.

### DOCTOR

Profesional médico.

Permisos:

- Consultar sus citas.
- Consultar información de pacientes autorizada para su atención.
- Consultar su agenda.
- Actualizar el estado de atención de sus citas.

### PATIENT

Usuario paciente, si se habilita el portal de autoservicio.

Permisos:

- Consultar sus propias citas.
- Solicitar citas.
- Consultar su información personal.
- Cancelar solicitudes según las reglas configuradas.

El acceso del paciente a la plataforma es una funcionalidad prevista, no debe considerarse implementada automáticamente.

## 3. Modelo de base de datos

Este modelo corresponde al alcance del MVP definido en `PRD.md` §10: 10 tablas.

Quedan diferidos a fases posteriores (P1/P2): servicios clínicos, ubicaciones geográficas, expedientes clínicos, notificaciones, lista de espera y reportes. No deben crearse sus tablas hasta que se apruebe formalmente su incorporación al esquema.

### 3.1 Tabla: hospitals

Representa los centros hospitalarios.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| name | VARCHAR(150) | NOT NULL |
| code | VARCHAR(30) | UNIQUE |
| address | TEXT | NOT NULL |
| phone | VARCHAR(30) | NULL |
| is_active | BOOLEAN | DEFAULT TRUE |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

### 3.2 Tabla: roles

Define los roles del sistema.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| name | VARCHAR(50) | UNIQUE |
| description | TEXT | NULL |
| created_at | TIMESTAMPTZ | NOT NULL |

Roles iniciales:

- SUPER_ADMIN
- INSTITUTION_ADMIN
- RECEPTIONIST
- DOCTOR
- PATIENT

Correspondencia con los actores descritos en `PRD.md` §3:

| Rol | Actor |
|---|---|
| SUPER_ADMIN | Administrador general |
| INSTITUTION_ADMIN | Administrador institucional |
| RECEPTIONIST | Personal de recepción |
| DOCTOR | Médico |
| PATIENT | Paciente |

Los permisos deben verificarse en el backend. El nombre del rol por sí solo no sustituye las comprobaciones de autorización. Este conjunto de roles es el canónico y debe coincidir con `SECURITY.md` §5.

### 3.3 Tabla: users

Representa las cuentas de acceso.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| hospital_id | UUID | FK, NULL según política |
| role_id | UUID | FK |
| email | VARCHAR(150) | UNIQUE, NULL permitido |
| username | VARCHAR(80) | UNIQUE |
| password_hash | TEXT | NOT NULL |
| is_active | BOOLEAN | DEFAULT TRUE |
| last_login_at | TIMESTAMPTZ | NULL |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

Reglas:

- Nunca almacenar contraseñas en texto plano.
- El correo puede ser opcional si el mecanismo de identificación utiliza otro identificador.
- Un usuario desactivado no puede autenticarse.
- No eliminar usuarios que tengan registros históricos asociados.

### 3.4 Tabla: patients

Contiene los datos de identificación de los pacientes.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| first_name | VARCHAR(100) | NOT NULL |
| middle_name | VARCHAR(100) | NULL |
| last_name | VARCHAR(100) | NOT NULL |
| second_last_name | VARCHAR(100) | NULL |
| national_id | VARCHAR(30) | NULL |
| birth_date | DATE | NOT NULL |
| sex | VARCHAR(20) | NULL |
| phone | VARCHAR(30) | NULL |
| email | VARCHAR(150) | NULL |
| address | TEXT | NULL |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

Reglas:

- El identificador nacional no debe ser obligatorio para pacientes que no puedan proporcionarlo.
- Si existe, su unicidad debe aplicarse según la política de identificación establecida.
- No utilizar el nombre como identificador único.
- No duplicar datos del paciente en la tabla de citas.
- Los datos personales deben tener controles de acceso.

### 3.5 Tabla: specialties

Representa las especialidades médicas.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| name | VARCHAR(100) | UNIQUE |
| description | TEXT | NULL |
| is_active | BOOLEAN | DEFAULT TRUE |
| created_at | TIMESTAMPTZ | NOT NULL |

Ejemplos:

- Medicina General.
- Pediatría.
- Ginecología.
- Cardiología.
- Dermatología.

La lista definitiva dependerá de las especialidades disponibles en cada hospital.

### 3.6 Tabla: professionals

Representa a los profesionales médicos.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| user_id | UUID | FK, UNIQUE |
| hospital_id | UUID | FK |
| specialty_id | UUID | FK |
| first_name | VARCHAR(100) | NOT NULL |
| last_name | VARCHAR(100) | NOT NULL |
| license_number | VARCHAR(50) | UNIQUE |
| is_active | BOOLEAN | DEFAULT TRUE |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

Reglas:

- Un profesional pertenece a un hospital.
- Un profesional tiene una especialidad principal en esta versión.
- Un profesional puede tener una cuenta de acceso asociada.
- No todos los profesionales necesitan utilizar directamente el sistema.

### 3.7 Tabla: schedules

Define los horarios de disponibilidad de los profesionales.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| professional_id | UUID | FK |
| hospital_id | UUID | FK |
| day_of_week | SMALLINT | NOT NULL |
| start_time | TIME | NOT NULL |
| end_time | TIME | NOT NULL |
| slot_duration_minutes | INTEGER | NOT NULL |
| is_active | BOOLEAN | DEFAULT TRUE |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

Reglas:

- day_of_week utiliza ISO 8601: lunes = 1, domingo = 7.
- end_time debe ser posterior a start_time.
- slot_duration_minutes debe ser mayor que cero.
- No permitir intervalos de disponibilidad contradictorios para el mismo profesional.
- Los horarios semanales son plantillas de disponibilidad, no citas confirmadas.
- Las excepciones, vacaciones y cierres extraordinarios deben modelarse mediante una entidad específica cuando se implemente esa funcionalidad.

### 3.8 Tabla: appointments

Es la entidad central del sistema.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| patient_id | UUID | FK |
| professional_id | UUID | FK, NULL |
| hospital_id | UUID | FK |
| created_by | UUID | FK users |
| starts_at | TIMESTAMPTZ | NOT NULL |
| ends_at | TIMESTAMPTZ | NOT NULL |
| status | VARCHAR(30) | NOT NULL |
| reason | TEXT | NULL |
| cancellation_reason | TEXT | NULL |
| created_at | TIMESTAMPTZ | NOT NULL |
| updated_at | TIMESTAMPTZ | NOT NULL |

Estados permitidos:

- PENDING
- SCHEDULED
- CONFIRMED
- IN_PROGRESS
- COMPLETED
- CANCELLED
- NO_SHOW

Reglas:

- ends_at debe ser posterior a starts_at.
- El paciente debe existir.
- `professional_id` puede ser NULL únicamente cuando `status = 'PENDING'`. En cualquier otro estado es obligatorio.
- Cuando `professional_id` esté definido, el profesional debe estar activo.
- El hospital debe estar activo.
- Cuando `professional_id` esté definido, la especialidad del profesional debe corresponder a la configuración de disponibilidad.
- Una cita activa con `professional_id` asignado no puede superponerse con otra cita activa del mismo profesional.
- No se puede modificar una cita completada como si fuera una cita pendiente.
- Las cancelaciones deben conservarse en el historial.
- Toda reprogramación debe dejar trazabilidad.

Se consideran ocupados los siguientes estados:

- PENDING
- SCHEDULED
- CONFIRMED
- IN_PROGRESS

Una cita `PENDING` sin `professional_id` no bloquea la agenda de ningún profesional, porque la restricción de exclusión ignora las filas con `professional_id` NULL. Los estados CANCELLED, COMPLETED y NO_SHOW no bloquean nuevas reservas.

La política definitiva de transición de estados debe centralizarse en AppointmentService.

### 3.9 Tabla: appointment_history

Registra los cambios de estado de una cita.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| appointment_id | UUID | FK |
| changed_by | UUID | FK users |
| previous_status | VARCHAR(30) | NULL |
| new_status | VARCHAR(30) | NOT NULL |
| comment | TEXT | NULL |
| changed_at | TIMESTAMPTZ | NOT NULL |

Reglas:

- Toda transición relevante debe generar un registro.
- No eliminar el historial al cancelar o reprogramar.
- El historial no debe modificarse mediante operaciones ordinarias de usuario.

### 3.10 Tabla: audit_logs

Registra operaciones administrativas y acciones sensibles.

| Campo | Tipo | Restricción |
|---|---|---|
| id | UUID | PK |
| user_id | UUID | FK, NULL |
| action | VARCHAR(100) | NOT NULL |
| entity_type | VARCHAR(80) | NOT NULL |
| entity_id | UUID | NULL |
| metadata | JSONB | NULL |
| ip_address | VARCHAR(45) | NULL |
| created_at | TIMESTAMPTZ | NOT NULL |

No guardar contraseñas, tokens, datos clínicos innecesarios ni información sensible en metadata.

## 4. Relaciones entre entidades

```text
HOSPITALS
  ├── USERS
  ├── PROFESSIONALS
  ├── SCHEDULES
  └── APPOINTMENTS

ROLES
  └── USERS

USERS
  ├── PROFESSIONALS
  ├── APPOINTMENTS (created_by)
  ├── APPOINTMENT_HISTORY
  └── AUDIT_LOGS

SPECIALTIES
  └── PROFESSIONALS

PATIENTS
  └── APPOINTMENTS

PROFESSIONALS
  ├── SCHEDULES
  └── APPOINTMENTS

APPOINTMENTS
  └── APPOINTMENT_HISTORY
```

Todas las relaciones deben tener claves foráneas reales en PostgreSQL.

## 5. Restricciones de integridad

### Identificadores

- UUID para todas las claves primarias.
- Claves foráneas UUID.
- Índices para las columnas utilizadas en búsquedas frecuentes.

### Citas

Crear una protección de solapamientos en PostgreSQL mediante una restricción de exclusión GiST.

```sql
CREATE EXTENSION IF NOT EXISTS btree_gist;

ALTER TABLE appointments
ADD CONSTRAINT appointments_no_overlap
EXCLUDE USING gist (
  professional_id WITH =,
  tstzrange(starts_at, ends_at, '[)') WITH &&
)
WHERE (
  status IN (
    'PENDING',
    'SCHEDULED',
    'CONFIRMED',
    'IN_PROGRESS'
  )
);
```

Esta restricción evita reservas simultáneas para el mismo profesional, incluso ante solicitudes concurrentes.

La instalación y migración de la extensión deben verificarse en el entorno PostgreSQL.

Además, debe garantizarse que el profesional esté asignado salvo en estado PENDING:

```sql
ALTER TABLE appointments
ADD CONSTRAINT appointments_professional_required
CHECK (status = 'PENDING' OR professional_id IS NOT NULL);
```

Esta restricción es compatible con la exclusión GiST: las filas con `professional_id` NULL no participan en la comparación de solapamientos.

Si se necesita impedir también la duplicación de pacientes en un mismo horario, esa regla deberá definirse expresamente y añadirse como una restricción independiente.

## 6. Reglas de negocio

### Registro de pacientes

1. Validar los campos obligatorios.
2. Normalizar los identificadores.
3. Verificar posibles duplicados.
4. Crear el registro.
5. Registrar la operación de auditoría.

### Programación de citas

1. Recibir el paciente.
2. Recibir el profesional.
3. Recibir el hospital.
4. Recibir la fecha y hora.
5. Validar la disponibilidad.
6. Verificar que el intervalo corresponda al horario permitido.
7. Verificar que el profesional esté activo.
8. Comprobar que no exista solapamiento.
9. Crear la cita mediante una transacción.
10. Registrar el historial inicial.

### Cancelación de citas

1. Verificar los permisos.
2. Verificar el estado actual.
3. Recibir el motivo cuando corresponda.
4. Cambiar el estado a CANCELLED.
5. Registrar el historial.
6. Liberar el horario.

### Reprogramación

La reprogramación debe conservar la identidad de la cita.

Debe registrar:

- Fecha y hora anteriores.
- Fecha y hora nuevas.
- Usuario responsable.
- Momento del cambio.

Debe ejecutarse de forma transaccional.

## 7. API REST

Prefijo obligatorio:

`/api/v1`

Endpoints iniciales:

### Autenticación

```text
POST /api/v1/auth/login
POST /api/v1/auth/logout
GET  /api/v1/auth/me
```

### Pacientes

```text
POST   /api/v1/patients
GET    /api/v1/patients
GET    /api/v1/patients/:id
PATCH  /api/v1/patients/:id
```

### Profesionales

```text
POST  /api/v1/professionals
GET   /api/v1/professionals
GET   /api/v1/professionals/:id
PATCH /api/v1/professionals/:id
```

### Especialidades

```text
POST /api/v1/specialties
GET  /api/v1/specialties
GET  /api/v1/specialties/:id
```

### Horarios

```text
POST  /api/v1/schedules
GET   /api/v1/schedules
GET   /api/v1/schedules/:id
PATCH /api/v1/schedules/:id
```

### Citas

```text
POST /api/v1/appointments
GET  /api/v1/appointments
GET  /api/v1/appointments/:id
PATCH /api/v1/appointments/:id

POST /api/v1/appointments/:id/cancel
POST /api/v1/appointments/:id/reschedule

GET /api/v1/appointments/availability
```

Los contratos de solicitud y respuesta deben documentarse mediante Swagger.

## 8. Formato de respuestas

Respuesta satisfactoria:

```json
{
  "success": true,
  "data": {},
  "message": "Operación realizada correctamente"
}
```

Respuesta de error:

```json
{
  "success": false,
  "error": {
    "code": "APPOINTMENT_CONFLICT",
    "message": "El horario seleccionado no está disponible"
  }
}
```

No devolver trazas internas ni información sensible.

Los listados deben utilizar paginación.

## 9. Reglas de validación

- Validación en frontend para mejorar la experiencia.
- Validación obligatoria en backend.
- Validación de tipos y formatos.
- Validación de relaciones existentes.
- Validación de permisos.
- Validación de estados permitidos.
- Validación de fechas y horarios.

El backend siempre tiene la última autoridad.

## 10. Requisitos no funcionales

### Seguridad

- Autenticación obligatoria para operaciones privadas.
- Autorización por roles y permisos.
- Protección de información personal.
- HTTPS en producción.
- Gestión segura de sesiones y tokens.
- Auditoría de operaciones sensibles.

### Rendimiento

- Paginación en listados.
- Índices para búsquedas.
- Consultas eficientes.
- Evitar consultas N+1.
- No cargar historiales completos innecesariamente.

### Disponibilidad

- Copias de seguridad periódicas.
- Procedimientos de restauración.
- Registro de errores.
- Monitoreo del backend.

### Mantenibilidad

- Código modular.
- Tipado estricto.
- Migraciones versionadas.
- Pruebas automatizadas.
- Documentación actualizada.

## 11. Criterios de aceptación

La primera versión funcional deberá cumplir:

1. Un administrador puede gestionar usuarios.
2. Un administrador puede gestionar hospitales.
3. Se pueden registrar pacientes.
4. Se pueden registrar profesionales.
5. Se pueden configurar especialidades.
6. Se pueden establecer horarios.
7. Se pueden consultar disponibilidades.
8. Se pueden programar citas.
9. No se permiten citas simultáneas para el mismo profesional.
10. Se pueden cancelar y reprogramar citas.
11. Se conserva el historial de cambios.
12. Se aplican controles de autorización.
13. Las operaciones sensibles quedan auditadas.

## 12. Control de cambios

Ninguna implementación debe cambiar el esquema de datos por conveniencia.

Toda nueva entidad, campo, relación o estado requiere:

1. Justificación funcional.
2. Justificación técnica.
3. Revisión de las relaciones existentes.
4. Actualización de SPEC.md.
5. Actualización de schema.prisma.
6. Migración de base de datos.
7. Pruebas automatizadas.

Este documento establece el contrato funcional y de datos de MiTurno.
