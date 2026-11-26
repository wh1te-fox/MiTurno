# MiTurno — Security Requirements & Restrictions

**Proyecto:** MiTurno
**Versión:** 1.0
**Clasificación:** Documento técnico obligatorio
**Estado:** Especificación de seguridad
**Aplicación:** Frontend, Backend, API, Base de datos e Infraestructura

---

# 1. Objetivo

Establecer las políticas, restricciones y controles de seguridad que deben cumplirse durante el desarrollo, mantenimiento y operación de MiTurno.

MiTurno procesa información personal, datos de identificación, información médica, expedientes clínicos y datos de contacto.

Por esta razón, la seguridad debe considerarse un requisito obligatorio de arquitectura y no una característica opcional.

Todos los desarrolladores y agentes de IA deben respetar estas restricciones.

# 2. Principios de seguridad

El sistema debe aplicar los siguientes principios:

1. Denegar por defecto.
2. Mínimo privilegio.
3. Defensa en profundidad.
4. Validación en servidor.
5. Separación de responsabilidades.
6. Protección de datos sensibles.
7. Trazabilidad de operaciones.
8. Seguridad desde el diseño.
9. No confiar en el cliente.
10. No exponer información innecesaria.

Ninguna funcionalidad debe implementarse sacrificando estos principios.

# 3. Clasificación de información

La información debe clasificarse según su sensibilidad.

| Clasificación | Información | Protección |
|---|---|---|
| Pública | Clínicas, servicios publicados, horarios públicos | Acceso público controlado |
| Interna | Configuraciones institucionales | Autenticación |
| Confidencial | Datos personales y contacto | Autorización específica |
| Sensible | Expedientes, diagnósticos y tratamientos | Autorización estricta y auditoría |
| Crítica | Credenciales, tokens y secretos | Protección especializada |

Los datos médicos nunca deben tratarse como información pública.

# 4. Autenticación

## 4.1 Contraseñas

- Utilizar Argon2id para almacenar contraseñas.
- Nunca almacenar contraseñas en texto plano.
- Nunca registrar contraseñas en logs.
- Nunca devolver hashes de contraseñas en respuestas API.
- No incluir credenciales en URLs.
- Aplicar políticas de longitud y validación.
- Implementar protección contra intentos automatizados.

## 4.2 Sesiones y tokens

- Utilizar mecanismos seguros de autenticación.
- Definir expiración de sesiones.
- Implementar revocación de sesiones.
- No almacenar tokens sensibles en localStorage.
- Preferir cookies HttpOnly, Secure y SameSite cuando se utilicen sesiones mediante navegador.
- Implementar protección CSRF cuando corresponda.
- No incluir información médica en JWT.
- Validar firma, emisor, audiencia y expiración de tokens.

## 4.3 Recuperación de contraseña

- Utilizar tokens aleatorios criptográficamente seguros.
- Establecer expiración corta.
- Permitir un solo uso.
- No revelar si un correo está registrado.
- Invalidar el token después de utilizarlo.
- Notificar cambios de credenciales.

# 5. Autorización y control de acceso

Implementar RBAC (Role-Based Access Control) con validaciones adicionales de propiedad y alcance institucional.

Roles iniciales:

- PATIENT
- DOCTOR
- INSTITUTION_ADMIN
- SUPER_ADMIN

## 5.1 Restricciones por rol

### PATIENT

Puede:

- Consultar su perfil.
- Modificar sus datos permitidos.
- Consultar sus citas.
- Reservar citas.
- Cancelar citas propias según las reglas.
- Consultar su expediente autorizado.

No puede:

- Consultar expedientes de otros pacientes.
- Modificar diagnósticos.
- Administrar especialistas.
- Modificar agendas institucionales.

### DOCTOR

Puede:

- Consultar su agenda.
- Consultar pacientes asociados a sus atenciones autorizadas.
- Registrar información clínica permitida.
- Actualizar estados de atención.

No puede:

- Consultar todos los expedientes del sistema.
- Administrar otras instituciones sin autorización.
- Modificar registros clínicos de otros profesionales sin permiso.

### INSTITUTION_ADMIN

Puede:

- Administrar su institución.
- Gestionar especialistas autorizados.
- Administrar servicios.
- Configurar agendas.
- Consultar reportes institucionales.

No puede:

- Acceder a otras instituciones.
- Consultar información clínica sin autorización.
- Modificar permisos globales.
- Administrar cuentas de superadministrador.

### SUPER_ADMIN

Puede administrar configuraciones globales.

Sin embargo, los privilegios administrativos no deben implicar acceso automático e indiscriminado al contenido de expedientes clínicos.

## 5.2 Reglas obligatorias

- Toda autorización debe verificarse en backend.
- Nunca confiar en roles enviados desde frontend.
- No utilizar identificadores proporcionados por el cliente como prueba de propiedad.
- Validar el alcance institucional.
- Verificar propiedad antes de modificar o eliminar recursos.
- Aplicar autorización en cada endpoint.
- Denegar operaciones no autorizadas.

# 6. Protección de datos médicos

Los expedientes clínicos requieren controles especiales.

## Restricciones

- No exponer expedientes mediante endpoints públicos.
- No permitir acceso únicamente por conocer un UUID.
- No almacenar diagnósticos en campos genéricos sin especificación.
- No incluir información médica en logs.
- No compartir información clínica entre instituciones sin autorización.
- No permitir modificaciones sin identificar al autor.
- Registrar accesos y modificaciones sensibles.
- Mantener trazabilidad histórica.
- Aplicar políticas de conservación de información.
- Proteger documentos médicos almacenados.

## Auditoría clínica

Registrar como mínimo:

- Usuario responsable.
- Paciente afectado.
- Tipo de operación.
- Fecha y hora.
- Recurso afectado.
- Resultado de la operación.

No registrar el contenido completo del expediente en la auditoría.

# 7. Seguridad de la API

## 7.1 Validación de entradas

- Validar todos los DTO.
- Rechazar propiedades desconocidas cuando corresponda.
- Validar UUID.
- Validar fechas.
- Validar tipos de datos.
- Validar límites de longitud.
- Validar enumeraciones.
- Sanitizar entradas según el contexto de uso.

## 7.2 Restricciones

- No construir consultas SQL mediante concatenación de entradas.
- No confiar en datos del frontend.
- No permitir asignaciones masivas de propiedades sensibles.
- No exponer errores internos.
- No devolver stack traces en producción.
- No aceptar campos de autorización arbitrarios.

## 7.3 Seguridad HTTP

Implementar:

- HTTPS obligatorio.
- HSTS.
- Content Security Policy.
- X-Content-Type-Options.
- Protección contra clickjacking.
- CORS con lista explícita de orígenes.
- Limitación de métodos HTTP.
- Límites de tamaño de solicitudes.

# 8. Protección contra vulnerabilidades

El sistema debe considerar como mínimo los riesgos de OWASP Top 10.

| Riesgo | Control requerido |
|---|---|
| Broken Access Control | RBAC y validación de propiedad |
| Cryptographic Failures | TLS y protección criptográfica |
| Injection | Consultas parametrizadas |
| Insecure Design | Revisión de amenazas |
| Security Misconfiguration | Configuración segura |
| Vulnerable Components | Auditoría de dependencias |
| Authentication Failures | Sesiones seguras y rate limiting |
| Software Integrity Failures | Dependencias verificadas |
| Logging Failures | Auditoría y monitoreo |
| SSRF | Validación de destinos externos |

También se deben contemplar:

- XSS.
- CSRF.
- IDOR/BOLA.
- SQL Injection.
- Brute Force.
- Session Hijacking.
- Mass Assignment.
- Path Traversal.
- File Upload vulnerabilities.

# 9. Seguridad de citas médicas

La gestión de citas debe proteger la integridad de la agenda.

Restricciones:

- No permitir reservas duplicadas.
- No permitir superposición de citas activas.
- No permitir modificar citas ajenas.
- No permitir cancelar citas sin autorización.
- Validar disponibilidad en backend.
- Ejecutar reservas mediante transacciones.
- Evitar condiciones de carrera.
- Registrar reprogramaciones.
- Mantener historial de cambios.

La disponibilidad no debe validarse únicamente en frontend.

La base de datos debe incorporar restricciones apropiadas para prevenir conflictos concurrentes.

# 10. Seguridad de base de datos

## PostgreSQL

- No exponer PostgreSQL directamente a Internet.
- Utilizar usuarios con privilegios mínimos.
- Separar credenciales de aplicación y administración.
- Utilizar conexiones cifradas cuando corresponda.
- Aplicar migraciones versionadas.
- Mantener integridad referencial.
- Configurar backups cifrados.
- Restringir acceso administrativo.
- Monitorear consultas y errores.

## Prisma ORM

- Utilizar consultas parametrizadas.
- Evitar SQL raw inseguro.
- No utilizar entradas de usuario para construir fragmentos SQL.
- No exponer entidades completas sin filtrar campos.
- Evitar devolver propiedades sensibles.
- Mantener el esquema sincronizado con las migraciones.

# 11. Seguridad de archivos

Los archivos adjuntos y documentos médicos deben cumplir:

- Validación de tipo MIME.
- Validación de extensión.
- Límites de tamaño.
- Nombres de almacenamiento aleatorios.
- Almacenamiento privado.
- Verificación de autorización para descargas.
- Análisis antimalware cuando corresponda.
- No ejecutar archivos subidos.
- No permitir rutas de almacenamiento proporcionadas directamente por el usuario.

Los documentos médicos no deben almacenarse en directorios públicos.

# 12. Seguridad de notificaciones

- No incluir diagnósticos en SMS.
- No incluir expedientes en correos.
- No incluir información clínica sensible en notificaciones push.
- No exponer tokens en enlaces.
- Utilizar enlaces temporales cuando corresponda.
- Registrar el estado de entrega.
- Respetar preferencias de comunicación.
- No revelar información personal a destinatarios incorrectos.

# 13. Protección de secretos

Está prohibido:

- Guardar contraseñas en el repositorio.
- Guardar tokens de producción en código.
- Subir archivos .env reales.
- Compartir claves privadas.
- Incluir secretos en imágenes Docker.
- Imprimir secretos en logs.
- Exponer credenciales en frontend.

Utilizar:

- Variables de entorno.
- Secret managers.
- Rotación de credenciales.
- Separación de ambientes.
- Archivos de ejemplo sin valores reales.

# 14. Seguridad de infraestructura

## Docker

- Utilizar imágenes oficiales o verificadas.
- Fijar versiones de imágenes.
- Evitar ejecutar contenedores como root.
- No incluir secretos en imágenes.
- Limitar capacidades de contenedores.
- No exponer puertos innecesarios.
- Utilizar redes internas.
- Actualizar imágenes periódicamente.

## CI/CD

- Analizar dependencias.
- Ejecutar pruebas automatizadas.
- Ejecutar análisis estático.
- Verificar secretos accidentalmente publicados.
- Revisar vulnerabilidades.
- Proteger ramas principales.
- Restringir permisos de workflows.
- No desplegar versiones con fallos críticos conocidos.

# 15. Logs y auditoría

Los registros deben permitir investigar incidentes sin exponer información sensible.

Registrar:

- Autenticaciones exitosas.
- Autenticaciones fallidas.
- Cambios de permisos.
- Modificaciones de perfiles.
- Accesos a expedientes.
- Modificaciones clínicas.
- Cancelaciones.
- Reprogramaciones.
- Operaciones administrativas.

Nunca registrar:

- Contraseñas.
- Tokens completos.
- Secretos.
- Datos completos de tarjetas.
- Expedientes clínicos completos.

Los registros de auditoría deben tener acceso restringido y protección contra modificaciones no autorizadas.

# 16. Rate Limiting

Aplicar límites de solicitudes en operaciones sensibles.

Especialmente:

- Login.
- Registro.
- Recuperación de contraseña.
- Validación de códigos.
- Búsquedas.
- Reserva de citas.
- Envío de notificaciones.

Los límites deben ser configurables y considerar la protección contra abuso automatizado.

# 17. Privacidad y cumplimiento

MiTurno debe implementar principios de:

- Minimización de datos.
- Limitación de finalidad.
- Consentimiento informado cuando corresponda.
- Transparencia.
- Acceso controlado.
- Conservación limitada.
- Eliminación conforme a obligaciones aplicables.
- Trazabilidad.
- Protección de datos desde el diseño.

Antes de operar en producción se debe revisar la legislación aplicable en Nicaragua y las obligaciones correspondientes a las instituciones médicas.

# 18. Restricciones obligatorias para agentes de IA

Los agentes de IA que trabajen en MiTurno deben cumplir estas reglas.

## Prohibiciones

1. No desactivar autenticación para facilitar pruebas.
2. No desactivar autorización.
3. No introducir endpoints públicos para expedientes.
4. No eliminar validaciones de seguridad.
5. No almacenar contraseñas en texto plano.
6. No almacenar tokens sensibles en localStorage.
7. No exponer secretos.
8. No modificar permisos sin justificación.
9. No confiar en identificadores enviados por frontend.
10. No permitir acceso entre instituciones sin autorización.
11. No registrar información médica sensible.
12. No realizar consultas SQL inseguras.
13. No implementar controles críticos únicamente en frontend.
14. No eliminar auditorías para simplificar operaciones.
15. No modificar migraciones aplicadas en producción.
16. No introducir dependencias vulnerables sin evaluación.
17. No utilizar datos reales de pacientes en pruebas.
18. No generar información clínica ficticia en producción.

## Obligaciones

Antes de implementar cualquier funcionalidad:

1. Revisar los permisos existentes.
2. Identificar los datos involucrados.
3. Evaluar el impacto sobre privacidad.
4. Validar entradas.
5. Implementar autorización backend.
6. Agregar pruebas de acceso permitido y denegado.
7. Registrar operaciones sensibles.
8. Actualizar documentación.
9. Ejecutar análisis de seguridad.
10. Informar riesgos encontrados.

## Regla ante incertidumbre

Si una decisión afecta:

- Datos médicos.
- Permisos.
- Autenticación.
- Cifrado.
- Privacidad.
- Integridad de la base de datos.

El agente debe detener el cambio y solicitar aprobación antes de introducir una solución incompatible o insegura.

# 19. Pruebas de seguridad obligatorias

Antes de producción deben existir pruebas para:

- Acceso no autorizado.
- Acceso a expedientes de terceros.
- Acceso entre instituciones.
- Escalamiento de privilegios.
- Manipulación de identificadores.
- Reservas concurrentes.
- Reutilización de tokens.
- Recuperación de contraseña.
- Exposición de secretos.
- Validación de archivos.
- Inyección SQL.
- XSS.
- CSRF.
- Rate limiting.
- Auditoría.

# 20. Criterios de aprobación de seguridad

Una versión no puede considerarse lista para producción si:

- Existen endpoints sensibles sin autorización.
- Hay secretos publicados.
- Se almacenan contraseñas sin hash seguro.
- Los expedientes son accesibles sin autorización.
- Existen vulnerabilidades críticas sin resolver.
- Las reservas permiten conflictos concurrentes.
- Los logs contienen información médica sensible.
- No existen copias de seguridad verificadas.
- No se han probado los controles de acceso.

# 21. Regla final

La seguridad tiene prioridad sobre la conveniencia de implementación.

Ningún desarrollador, agente de IA o administrador debe introducir cambios que debiliten los controles de seguridad de MiTurno sin una revisión y aprobación documentadas.

**La confidencialidad, integridad y disponibilidad de la información médica son requisitos obligatorios del sistema.**