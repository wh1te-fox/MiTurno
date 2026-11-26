# MiTurno — Product Requirements Document (PRD)

**Versión:** 1.0
**Estado:** Propuesta funcional
**Idioma:** Español
**Zona horaria:** America/Managua
**Plataforma:** Aplicación web responsive
**Mercado inicial:** Nicaragua

## 1. Descripción del producto

MiTurno es una plataforma digital de gestión de citas médicas que conecta pacientes, médicos, clínicas y hospitales mediante un sistema centralizado de programación, disponibilidad y administración de servicios médicos.

El objetivo es simplificar el acceso a los servicios de salud, reducir los tiempos administrativos y mejorar la organización de las instituciones médicas.

La plataforma permitirá:

- Buscar clínicas y hospitales.
- Consultar servicios y especialistas.
- Reservar citas médicas.
- Cancelar y reagendar citas.
- Administrar horarios.
- Consultar expedientes clínicos autorizados.
- Gestionar perfiles institucionales.
- Recibir notificaciones.
- Generar reportes médicos y administrativos.

## 2. Objetivos

### Objetivo general

Desarrollar una plataforma que centralice la gestión de citas médicas y facilite la interacción entre pacientes e instituciones de salud.

### Objetivos específicos

- Digitalizar la programación de citas médicas.
- Reducir la gestión manual de agendas.
- Facilitar la búsqueda de clínicas y especialistas.
- Automatizar recordatorios y notificaciones.
- Centralizar expedientes clínicos autorizados.
- Mejorar la trazabilidad de operaciones.
- Facilitar la generación de reportes.

## 3. Actores del sistema

### 3.1 Paciente

- Registrarse e iniciar sesión.
- Administrar su perfil.
- Buscar clínicas y hospitales.
- Consultar servicios médicos.
- Buscar especialistas.
- Consultar disponibilidad.
- Reservar citas.
- Cancelar citas.
- Reagendar citas.
- Consultar citas anteriores.
- Consultar su expediente clínico autorizado.
- Recibir notificaciones.

### 3.2 Médico

- Administrar su perfil profesional.
- Consultar su agenda.
- Gestionar disponibilidad.
- Consultar citas asignadas.
- Recibir notificaciones.
- Actualizar estados de atención.
- Consultar expedientes autorizados.
- Registrar información médica.

### 3.3 Administrador institucional

- Administrar perfiles institucionales.
- Registrar servicios.
- Administrar especialistas.
- Configurar horarios.
- Consultar citas.
- Gestionar usuarios institucionales.
- Generar reportes.

### 3.4 Administrador general

- Gestionar instituciones.
- Gestionar usuarios.
- Gestionar roles y permisos.
- Supervisar operaciones.
- Consultar auditorías.
- Administrar configuraciones globales.

## 4. Requerimientos funcionales

### RF-01. Gestión de citas

El sistema debe permitir crear, consultar, modificar y cancelar citas médicas.

Características:

- Registrar nuevas citas.
- Consultar citas por paciente.
- Consultar citas por especialista.
- Consultar citas por institución.
- Consultar citas por fecha y estado.
- Modificar citas existentes.
- Mantener historial de modificaciones.
- Validar disponibilidad antes de confirmar.

Reglas:

- Una cita debe estar asociada a un paciente.
- Una cita debe estar asociada a un especialista.
- Una cita debe pertenecer a una institución.
- No permitir superposición de citas activas para un mismo especialista.
- Respetar los permisos del usuario.

No se permite eliminar físicamente citas mediante operaciones ordinarias. Se utilizará cancelación lógica para conservar trazabilidad.

### RF-02. Gestión de usuarios

El sistema debe permitir el registro, autenticación y administración de usuarios.

Tipos:

- Paciente.
- Médico.
- Administrador institucional.
- Administrador general.

Características:

- Registro.
- Inicio y cierre de sesión.
- Recuperación de contraseña.
- Actualización de perfiles.
- Activación y desactivación.
- Administración de roles.
- Control de permisos.

Reglas:

- Cada cuenta debe tener identificador único.
- Las contraseñas deben almacenarse mediante hash seguro.
- Los permisos deben verificarse en backend.
- Un usuario no puede consultar información de otros usuarios sin autorización.

### RF-03. Disponibilidad horaria

El sistema debe permitir consultar y administrar horarios disponibles por especialista e institución.

Características:

- Configurar días de atención.
- Configurar horarios de inicio y finalización.
- Definir duración de citas.
- Consultar horarios disponibles.
- Consultar disponibilidad por especialidad.
- Desactivar horarios.
- Gestionar excepciones de calendario.

Reglas:

- No permitir horarios contradictorios.
- No ofrecer horarios ocupados.
- Respetar restricciones institucionales.
- Considerar vacaciones, cierres y ausencias.

### RF-04. Cancelación de citas

Características:

- Cancelación por pacientes autorizados.
- Cancelación por personal autorizado.
- Registro del motivo.
- Actualización del estado.
- Liberación del horario.
- Notificación a las partes involucradas.
- Registro de auditoría.

Estado resultante: CANCELLED.

### RF-05. Reagendar citas

Características:

- Consultar nueva disponibilidad.
- Seleccionar nuevo horario.
- Validar conflictos.
- Mantener el identificador de la cita.
- Registrar horario anterior.
- Registrar horario nuevo.
- Notificar los cambios.

La operación debe ejecutarse de forma transaccional.

### RF-06. Historia de expediente clínico

El sistema debe permitir registrar y consultar información clínica de pacientes según los permisos correspondientes.

Características:

- Registro de consultas.
- Registro de antecedentes médicos.
- Registro de diagnósticos.
- Registro de tratamientos.
- Registro de observaciones clínicas.
- Consulta de historial médico.
- Asociación de registros clínicos con citas.

Reglas:

- El expediente pertenece a un paciente.
- Los registros clínicos deben tener autor y fecha.
- No permitir modificaciones no autorizadas.
- Registrar auditoría de consultas y modificaciones sensibles.
- Proteger la confidencialidad.
- Mantener trazabilidad.

La información clínica no debe almacenarse directamente en las tablas de pacientes o citas.

### RF-07. Gestión de perfiles de clínicas y hospitales

Características:

- Nombre institucional.
- Descripción.
- Dirección.
- Teléfono.
- Correo institucional.
- Horarios generales.
- Servicios disponibles.
- Especialistas asociados.
- Imágenes institucionales.
- Estado de publicación.

Reglas:

- Cada institución tendrá identificador único.
- Los servicios estarán asociados a una institución.
- Los administradores solo podrán gestionar instituciones autorizadas.

### RF-08. Registro de ubicación clínica

El sistema debe almacenar información geográfica de clínicas y hospitales.

Características:

- Dirección.
- Departamento.
- Municipio.
- Coordenadas geográficas.
- Latitud.
- Longitud.
- Ubicación en mapa.
- Consulta geográfica de instituciones.

No se requiere seguimiento continuo del paciente.

### RF-09. Agendamiento guiado

Flujo de reserva:

1. Seleccionar institución.
2. Seleccionar servicio médico.
3. Seleccionar especialista.
4. Consultar disponibilidad.
5. Seleccionar fecha y hora.
6. Confirmar información personal.
7. Revisar datos de la reserva.
8. Confirmar cita.
9. Mostrar comprobante.
10. Enviar notificación.

El sistema debe permitir regresar a pasos anteriores sin perder los datos introducidos.

### RF-10. Lista de servicios clínicos

Características:

- Crear servicios.
- Editar servicios.
- Activar y desactivar servicios.
- Asociar servicios con instituciones.
- Asociar servicios con especialidades.
- Consultar servicios disponibles.

Ejemplos:

- Medicina general.
- Pediatría.
- Ginecología.
- Cardiología.
- Odontología.
- Laboratorio clínico.
- Consulta especializada.

Los servicios no deben confundirse con las especialidades médicas.

### RF-11. Registro de citas y datos personales

Datos previstos:

- Identificación del paciente.
- Nombre completo.
- Fecha de nacimiento.
- Información de contacto.
- Institución.
- Servicio médico.
- Especialista.
- Fecha y hora.
- Motivo de consulta, cuando corresponda.

Reglas:

- Solicitar únicamente datos necesarios.
- Validar información en backend.
- Evitar duplicación de registros.
- Proteger datos personales y médicos.
- No exponer información sensible innecesariamente.

### RF-12. Generación de reportes

Reportes administrativos:

- Total de citas.
- Citas por institución.
- Citas por especialista.
- Citas canceladas.
- Citas completadas.
- Pacientes registrados.
- Servicios más solicitados.
- Ocupación de agendas.

Documentos médicos:

- Constancias médicas.
- Resúmenes clínicos autorizados.
- Reportes de atención.

Reglas:

- Los documentos médicos solo podrán generarse por usuarios autorizados.
- Los reportes deben respetar los permisos.
- Registrar quién genera documentos sensibles.
- Incluir fecha de generación e identificación institucional.

## 5. Requerimientos no funcionales

### RNF-01. Diseño intuitivo

- Navegación clara.
- Formularios comprensibles.
- Mensajes de error descriptivos.
- Indicadores de carga.
- Confirmaciones de operaciones.
- Diseño centrado en el usuario.

### RNF-02. Seguridad y cifrado

- HTTPS mediante TLS.
- Contraseñas con Argon2id.
- Autorización basada en roles y permisos.
- Protección contra accesos no autorizados.
- Validación de entradas.
- Gestión segura de sesiones.
- Auditoría de operaciones sensibles.
- Protección de secretos.

### RNF-03. Alta disponibilidad

El sistema debe estar diseñado para funcionar 24/7.

Características:

- Supervisión del servicio.
- Recuperación ante fallos.
- Copias de seguridad.
- Monitoreo de infraestructura.
- Procedimientos de restauración.

El objetivo de disponibilidad debe establecerse mediante un SLA medible.

### RNF-04. Ubicación geográfica

- Mostrar instituciones en mapas.
- Consultar direcciones.
- Visualizar coordenadas.
- Facilitar búsqueda geográfica.

La integración debe utilizar un proveedor de mapas definido por el proyecto.

### RNF-05. Diseño responsivo

La aplicación debe funcionar correctamente en:

- Teléfonos móviles.
- Tablets.
- Computadoras de escritorio.
- Diferentes resoluciones.

### RNF-06. Agenda automática y cola

Características:

- Identificación de horarios disponibles.
- Gestión de solicitudes pendientes.
- Ordenamiento de solicitudes.
- Asignación según disponibilidad.
- Gestión de listas de espera.
- Notificación cuando exista disponibilidad.

Las reglas de prioridad deben ser configurables y explícitas.

No se deben asignar prioridades médicas mediante decisiones automáticas no supervisadas.

### RNF-07. Respaldo automático

- Respaldo de PostgreSQL.
- Protección de archivos asociados.
- Almacenamiento seguro.
- Retención configurable.
- Verificación de integridad.
- Pruebas periódicas de restauración.

### RNF-08. Rendimiento web

- Carga optimizada.
- División de código.
- Optimización de recursos.
- Caché cuando corresponda.
- Consultas paginadas.
- Optimización de imágenes.

### RNF-09. Optimización de consultas

- Índices apropiados.
- Consultas optimizadas.
- Paginación.
- Transacciones.
- Evitar consultas N+1.
- Monitoreo de consultas lentas.

### RNF-10. Recordatorios automáticos

Canales:

- SMS.
- Correo electrónico.
- Notificaciones dentro de la aplicación.

Eventos:

- Confirmación de reserva.
- Recordatorio previo.
- Cancelación.
- Reprogramación.

### RNF-11. Notificaciones a especialistas

Eventos:

- Nueva cita.
- Cita cancelada.
- Cita reprogramada.
- Cambio de horario.
- Nueva solicitud de disponibilidad.

Las notificaciones deben evitar exponer información clínica sensible.

## 6. Reglas generales del negocio

1. Una cita siempre debe pertenecer a un paciente.
2. Una cita siempre debe estar asociada a una institución.
3. Una cita debe estar asignada a un especialista cuando sea confirmada.
4. Un especialista no puede tener citas activas superpuestas.
5. Una cita cancelada no ocupa disponibilidad.
6. Una cita completada no puede eliminarse físicamente.
7. Una reprogramación debe conservar trazabilidad.
8. Los pacientes solo pueden consultar sus propios datos.
9. Los médicos solo pueden acceder a expedientes autorizados.
10. Los administradores institucionales deben respetar el alcance de su institución.
11. Las operaciones sensibles deben registrarse en auditoría.
12. Toda regla de autorización debe verificarse en backend.

## 7. Estados de las citas

| Estado | Descripción |
|---|---|
| PENDING | Solicitud pendiente |
| SCHEDULED | Cita programada |
| CONFIRMED | Cita confirmada |
| IN_PROGRESS | Atención en curso |
| COMPLETED | Atención finalizada |
| CANCELLED | Cita cancelada |
| NO_SHOW | Paciente no asistió |

Las transiciones deben controlarse mediante un servicio centralizado.

## 8. Módulos funcionales

| Módulo | Responsabilidad |
|---|---|
| Authentication | Registro y autenticación |
| Users | Administración de usuarios |
| Roles | Roles y permisos |
| Hospitals | Gestión institucional |
| Locations | Ubicaciones geográficas |
| Patients | Registro de pacientes |
| Professionals | Gestión de especialistas |
| Specialties | Catálogo de especialidades |
| Services | Catálogo de servicios clínicos |
| Schedules | Configuración de horarios |
| Appointments | Programación de citas |
| WaitingList | Cola y lista de espera |
| MedicalRecords | Expedientes clínicos |
| Notifications | Notificaciones y recordatorios |
| Reports | Reportes y documentos |
| Audit | Registro de operaciones |

## 9. Criterios de aceptación

La primera versión funcional deberá permitir:

1. Registrar y autenticar usuarios.
2. Administrar perfiles de pacientes.
3. Administrar clínicas y hospitales.
4. Registrar especialistas.
5. Configurar especialidades y servicios.
6. Administrar horarios.
7. Consultar disponibilidad.
8. Reservar citas.
9. Cancelar citas.
10. Reprogramar citas.
11. Evitar conflictos de horarios.
12. Consultar agendas.
13. Enviar notificaciones básicas.
14. Aplicar controles de acceso.
15. Registrar operaciones sensibles.

## 10. Priorización

### P0 — MVP

- Gestión de usuarios.
- Autenticación.
- Gestión de pacientes.
- Gestión de hospitales.
- Gestión de especialistas.
- Gestión de servicios.
- Disponibilidad horaria.
- Creación de citas.
- Cancelación de citas.
- Reprogramación de citas.
- Control de permisos.
- Auditoría básica.

### P1 — Segunda fase

- Notificaciones automáticas.
- Recordatorios.
- Ubicación geográfica.
- Mapas.
- Reportes administrativos.
- Lista de espera.
- Agenda automática.

### P2 — Tercera fase

- Expediente clínico completo.
- Constancias médicas.
- Reportes clínicos avanzados.
- Portal ampliado del paciente.
- Analítica institucional.

## 11. Indicadores de éxito

- Número de pacientes registrados.
- Número de citas programadas.
- Porcentaje de citas canceladas.
- Porcentaje de citas completadas.
- Tiempo promedio de programación.
- Ocupación de especialistas.
- Tiempo promedio de respuesta.
- Tasa de entrega de notificaciones.
- Disponibilidad del servicio.

## 12. Decisiones pendientes

Antes de implementar las funcionalidades avanzadas deben definirse:

1. Proveedor de SMS.
2. Proveedor de correo electrónico.
3. Proveedor de mapas.
4. Política de retención de expedientes clínicos.
5. Política de consentimiento y tratamiento de datos.
6. Reglas de prioridad para listas de espera.
7. Reglas de cancelación y reprogramación.
8. Política de disponibilidad por institución.
9. Formatos oficiales de constancias médicas.
10. Objetivos de disponibilidad y recuperación.

## 13. Control de cambios

Toda modificación de requerimientos funcionales debe actualizar este documento.

Cuando una modificación afecte:

- Arquitectura: actualizar ARCHITECTURE.md.
- Base de datos: actualizar SPEC.md y schema.prisma.
- API: actualizar contratos OpenAPI.
- Seguridad: revisar controles de acceso y auditoría.
- Interfaz: actualizar especificaciones funcionales.

Ningún agente de IA debe asumir que un requerimiento nuevo autoriza a cambiar estructuras existentes sin analizar sus dependencias.

**Este documento es la fuente oficial de requerimientos del producto MiTurno.**