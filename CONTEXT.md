# MiTurno — Project Context

**Proyecto:** MiTurno  
**Versión:** 1.0  
**Estado:** En desarrollo / arquitectura propuesta  
**Idioma principal:** Español  
**Mercado objetivo:** Nicaragua  
**Zona horaria:** America/Managua

---

## 1. ¿Qué es MiTurno?

MiTurno es una plataforma web de gestión de citas médicas que conecta pacientes, médicos, clínicas y hospitales.

Su propósito es facilitar el acceso a los servicios de salud mediante la digitalización de procesos administrativos y médicos.

La plataforma permite a los pacientes buscar instituciones médicas, consultar especialistas, visualizar disponibilidad horaria, reservar citas, cancelar o reagendar reservas y consultar información clínica autorizada.

También proporciona herramientas administrativas para que clínicas y hospitales gestionen sus perfiles, servicios, profesionales, agendas y operaciones.

MiTurno busca convertirse en una solución centralizada, escalable y mantenible para la gestión de citas médicas.

## 2. Problema que resuelve

Los procesos tradicionales de programación de citas médicas presentan dificultades como:

- Gestión manual de agendas.
- Dificultad para consultar disponibilidad.
- Procesos de reserva lentos.
- Falta de centralización de información.
- Dificultad para cancelar o reagendar citas.
- Comunicación limitada entre pacientes e instituciones.
- Falta de trazabilidad administrativa.
- Uso de sistemas independientes por institución.

MiTurno busca resolver estos problemas mediante una plataforma digital unificada.

## 3. Visión del producto

Construir una plataforma de gestión médica que permita conectar diferentes instituciones de salud y facilitar la administración de citas mediante una experiencia sencilla para pacientes y profesionales.

La plataforma debe priorizar:

- Simplicidad.
- Seguridad.
- Accesibilidad.
- Organización.
- Escalabilidad.
- Integridad de datos.
- Facilidad de mantenimiento.

## 4. Tipos de usuarios

### 4.1 Paciente

Puede:

- Registrarse.
- Iniciar sesión.
- Administrar su perfil.
- Buscar clínicas y hospitales.
- Consultar servicios médicos.
- Buscar especialistas.
- Consultar disponibilidad.
- Reservar citas.
- Cancelar citas.
- Reagendar citas.
- Consultar su historial de citas.
- Consultar expedientes autorizados.
- Recibir notificaciones.

### 4.2 Médico

Puede:

- Iniciar sesión.
- Administrar su perfil profesional.
- Consultar su agenda.
- Gestionar disponibilidad.
- Consultar citas asignadas.
- Recibir notificaciones.
- Actualizar estados de atención.
- Consultar información clínica autorizada.
- Registrar información médica.

### 4.3 Administrador institucional

Puede:

- Administrar perfiles institucionales.
- Registrar servicios médicos.
- Gestionar especialistas.
- Configurar horarios.
- Consultar citas.
- Gestionar usuarios institucionales.
- Generar reportes.

### 4.4 Administrador general

Puede:

- Administrar instituciones.
- Gestionar usuarios.
- Gestionar roles y permisos.
- Supervisar operaciones.
- Consultar auditorías.
- Administrar configuraciones globales.

## 5. Funcionalidades principales

MiTurno contempla los siguientes dominios:

| Dominio | Descripción |
|---|---|
| Authentication | Registro y autenticación |
| Users | Administración de usuarios |
| Roles | Roles y permisos |
| Hospitals | Clínicas y hospitales |
| Locations | Ubicaciones geográficas |
| Patients | Gestión de pacientes |
| Professionals | Gestión de médicos |
| Specialties | Especialidades médicas |
| Services | Servicios clínicos |
| Schedules | Horarios disponibles |
| Appointments | Gestión de citas |
| WaitingList | Lista de espera |
| MedicalRecords | Expedientes clínicos |
| Notifications | Notificaciones |
| Reports | Reportes y documentos |
| Audit | Auditoría |

## 6. Stack tecnológico

La arquitectura tecnológica objetivo definida para MiTurno es:

### Frontend

- React.
- TypeScript.
- Vite.
- React Router.
- TanStack Query.
- React Hook Form.
- Zod.
- CSS Modules.

### Backend

- Node.js LTS.
- TypeScript.
- NestJS.
- REST API.
- Prisma ORM.
- class-validator.
- Swagger / OpenAPI.
- JWT.
- Argon2id para contraseñas.

### Base de datos

- PostgreSQL.
- Prisma.
- Migraciones versionadas.
- UUID.
- Integridad referencial.
- Transacciones.

### Infraestructura

- Docker.
- Docker Compose.
- GitHub Actions.
- Variables de entorno.

**Importante:** este stack representa la arquitectura objetivo propuesta. Antes de implementar cambios, comprobar las dependencias y tecnologías realmente presentes en el repositorio.

## 7. Arquitectura del proyecto

MiTurno utiliza una arquitectura modular organizada por dominios funcionales.

El backend debe separar:

- Controllers.
- Services.
- DTOs.
- Repositories cuando sean necesarios.
- Tests.

El frontend debe organizarse mediante funcionalidades independientes.

Cada módulo debe mantener responsabilidades claras y evitar dependencias innecesarias con otros dominios.

## 8. Modelo de datos principal

Las entidades principales propuestas son:

- Hospital
- Role
- User
- Patient
- Specialty
- Professional
- Schedule
- Appointment
- AppointmentHistory
- AuditLog

Entidades adicionales previstas:

- ClinicService
- ProfessionalService
- ClinicLocation
- MedicalRecord
- MedicalRecordEntry
- WaitingList
- Notification
- NotificationPreference
- NotificationTemplate
- AppointmentDocument
- ScheduleException
- ReportExport

Las relaciones, campos, tipos y restricciones deben definirse en `SPEC.md` y materializarse en `prisma/schema.prisma`.

No crear entidades duplicadas ni estructuras paralelas.

## 9. Reglas fundamentales del negocio

1. Una cita pertenece a un paciente.
2. Una cita pertenece a una institución.
3. Una cita debe estar asociada a un especialista cuando sea confirmada.
4. Un especialista no puede tener citas activas superpuestas.
5. Las citas canceladas no ocupan disponibilidad.
6. Las citas completadas deben conservarse.
7. Las reprogramaciones deben mantener trazabilidad.
8. Los pacientes solo pueden acceder a sus propios datos.
9. Los médicos solo pueden acceder a expedientes autorizados.
10. Los administradores institucionales deben respetar el alcance de su institución.
11. Las operaciones sensibles deben registrarse en auditoría.
12. La autorización debe validarse siempre en backend.

## 10. Estados de las citas

Estados permitidos:

- PENDING
- SCHEDULED
- CONFIRMED
- IN_PROGRESS
- COMPLETED
- CANCELLED
- NO_SHOW

Las transiciones deben gestionarse mediante un servicio centralizado.

No permitir cambios arbitrarios de estado desde el frontend.

## 11. Seguridad y privacidad

MiTurno administra información personal y médica.

Por tanto:

- Utilizar HTTPS/TLS.
- Almacenar contraseñas mediante Argon2id.
- Implementar autenticación segura.
- Aplicar autorización basada en roles y permisos.
- Validar todos los datos en backend.
- Evitar exposición de información clínica.
- Registrar operaciones sensibles.
- No guardar secretos en el repositorio.
- Aplicar mínimo privilegio.
- Implementar copias de seguridad.
- Definir políticas de conservación de información.

Los expedientes clínicos deben contar con controles de acceso específicos.

## 12. Convenciones de desarrollo

- Utilizar TypeScript.
- Utilizar inglés para nombres de entidades, propiedades y código.
- Utilizar español para los textos de interfaz.
- Utilizar camelCase para variables y propiedades.
- Utilizar PascalCase para clases y componentes.
- Mantener nombres consistentes.
- Reutilizar componentes existentes.
- Evitar duplicación de código.
- Mantener responsabilidades delimitadas.
- Escribir pruebas para funcionalidades críticas.

## 13. Documentación oficial

Los siguientes archivos constituyen la documentación de referencia:

| Archivo | Responsabilidad |
|---|---|
| CONTEXT.md | Contexto general y visión del proyecto |
| PRD.md | Requerimientos del producto |
| ARCHITECTURE.md | Arquitectura tecnológica |
| SPEC.md | Especificaciones funcionales y modelo de datos |
| AGENTS.md | Instrucciones para agentes de IA |
| prisma/schema.prisma | Esquema ejecutable de persistencia |

En caso de contradicción:

1. Los requerimientos aprobados del producto determinan el comportamiento esperado.
2. ARCHITECTURE.md determina las decisiones arquitectónicas aprobadas.
3. SPEC.md determina los contratos funcionales y de datos aprobados.
4. schema.prisma y las migraciones determinan la estructura de persistencia realmente implementada.
5. El código existente debe revisarse para conocer el estado real de implementación.

Si existe una contradicción, no resolverla inventando una estructura nueva. Documentar el conflicto y solicitar una decisión.

## 14. Instrucciones para agentes de IA

Antes de desarrollar cualquier funcionalidad:

1. Leer CONTEXT.md.
2. Leer AGENTS.md.
3. Leer PRD.md.
4. Leer ARCHITECTURE.md.
5. Leer SPEC.md.
6. Revisar el código existente.
7. Revisar schema.prisma y las migraciones.
8. Identificar componentes, servicios y entidades reutilizables.

No asumir que una funcionalidad está implementada únicamente porque aparece en la documentación.

### Prohibiciones

- No inventar tablas.
- No inventar relaciones.
- No duplicar entidades.
- No cambiar UUID por identificadores numéricos.
- No cambiar el stack sin aprobación.
- No crear endpoints incompatibles.
- No introducir dependencias innecesarias.
- No modificar migraciones aplicadas en producción.
- No implementar reglas críticas únicamente en frontend.
- No almacenar información médica en campos genéricos sin especificación.
- No simular persistencia real con almacenamiento local.

### Obligaciones

- Reutilizar estructuras existentes.
- Mantener compatibilidad.
- Validar entradas.
- Aplicar controles de autorización.
- Crear migraciones cuando corresponda.
- Escribir pruebas.
- Actualizar documentación.
- Informar decisiones técnicas relevantes.

### Cuando falte información

Si una funcionalidad no está suficientemente definida:

1. Identificar la información faltante.
2. Revisar si existe una solución equivalente.
3. Explicar el impacto técnico.
4. Proponer una solución compatible.
5. Solicitar aprobación cuando afecte arquitectura, datos o reglas de negocio.

## 15. Estado y alcance del proyecto

MiTurno contempla una evolución progresiva.

### Primera fase: MVP

- Autenticación.
- Gestión de usuarios.
- Gestión de pacientes.
- Gestión de instituciones.
- Gestión de especialistas.
- Gestión de servicios.
- Disponibilidad horaria.
- Gestión de citas.
- Cancelación y reprogramación.
- Auditoría básica.

### Segunda fase

- Notificaciones.
- Recordatorios.
- Mapas.
- Geolocalización.
- Reportes administrativos.
- Lista de espera.
- Agenda automática.

### Tercera fase

- Expediente clínico.
- Constancias médicas.
- Reportes clínicos avanzados.
- Funcionalidades ampliadas para pacientes.
- Analítica institucional.

## 16. Principio rector

MiTurno debe evolucionar como un único sistema coherente.

Toda nueva funcionalidad debe integrarse con las entidades, relaciones, contratos y reglas existentes.

La prioridad es mantener consistencia arquitectónica, integridad de datos, seguridad y facilidad de mantenimiento.

**No improvisar estructuras. No duplicar funcionalidades. No introducir incompatibilidades.**