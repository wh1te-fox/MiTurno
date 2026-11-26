

# AGENTS.md — MiTurno

## 1. Instrucciones obligatorias para agentes de IA

Antes de escribir código, consultar:

1. `ARCHITECTURE.md`
2. `SPEC.md`
3. `prisma/schema.prisma`
4. La implementación existente del módulo correspondiente.

Estas fuentes son las referencias obligatorias del proyecto.

## 2. Prohibiciones

- No inventar tablas.
- No inventar relaciones.
- No modificar tipos de identificadores.
- No cambiar el stack tecnológico.
- No duplicar entidades existentes.
- No crear endpoints alternativos para funcionalidades ya existentes.
- No implementar reglas de negocio únicamente en frontend.
- No utilizar datos ficticios como sustituto de persistencia real.
- No modificar migraciones ya aplicadas en producción.
- No introducir dependencias sin justificación.
- No almacenar secretos, contraseñas ni credenciales en el repositorio.
- No introducir tecnologías incompatibles con `ARCHITECTURE.md`.
- No realizar refactorizaciones ajenas al objetivo de la tarea.

## 3. Obligaciones

- Reutilizar entidades existentes.
- Respetar las convenciones de nombres.
- Utilizar DTOs.
- Validar entradas.
- Implementar autorización.
- Mantener compatibilidad con la base de datos.
- Crear migraciones para los cambios de esquema.
- Crear pruebas.
- Actualizar documentación cuando cambie el comportamiento.
- Utilizar Vue 3, TypeScript y Tailwind CSS en el frontend.
- Utilizar NestJS, Prisma y PostgreSQL en el backend.
- Mantener los componentes y funcionalidades organizados por dominio.
- Verificar que los cambios no rompan funcionalidades existentes.

## 4. Cuando falte información

No asumir estructuras.

Primero:

1. Identificar qué información falta.
2. Revisar si existe una solución equivalente.
3. Presentar una propuesta.
4. Esperar aprobación si afecta arquitectura, relaciones o reglas de negocio.

No crear soluciones provisionales que contradigan la arquitectura establecida.

## 5. Convenciones de Git y commits

Todos los commits del proyecto deben seguir el estándar Conventional Commits.

### 5.1 Formato obligatorio

```text
<type>(<scope>): <description>
```

Donde:

- `type`: tipo de cambio realizado.
- `scope`: módulo o área afectada.
- `description`: descripción breve del cambio.

El scope es obligatorio, excepto cuando el cambio sea verdaderamente global y no corresponda a un módulo específico.

### 5.2 Tipos de commits permitidos

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de errores |
| `refactor` | Refactorización sin cambiar comportamiento |
| `docs` | Cambios en documentación |
| `style` | Cambios de formato o presentación sin alterar lógica |
| `test` | Incorporación o modificación de pruebas |
| `chore` | Mantenimiento y tareas generales |
| `perf` | Mejoras de rendimiento |
| `build` | Cambios en compilación o dependencias |
| `ci` | Cambios en integración continua |

No utilizar tipos diferentes sin una justificación técnica.

### 5.3 Convenciones de escritura

Todos los mensajes de commit deben:

- Estar escritos en inglés.
- Utilizar minúsculas en el tipo, scope y descripción.
- Empezar con un verbo en modo imperativo.
- Ser breves y descriptivos.
- Explicar el cambio realizado, no la intención futura.
- Evitar puntos finales en la descripción.
- Identificar correctamente el módulo afectado.

No utilizar mensajes genéricos como:

```text
update
changes
fix
new changes
final version
test
corrections
```

### 5.4 Ejemplos correctos

#### Frontend

```text
feat(auth): implement login interface
feat(patients): add patient registration form
feat(appointments): implement appointment scheduling
fix(dashboard): correct responsive sidebar
style(patients): improve patient table layout
refactor(components): extract reusable modal
```

#### Backend

```text
feat(auth): implement jwt authentication
feat(patients): add patient registration endpoint
fix(appointments): prevent overlapping appointments
refactor(users): simplify user service
test(patients): add registration unit tests
```

#### Base de datos

```text
feat(prisma): add patient relationships
fix(prisma): correct appointment constraints
chore(prisma): update database seed
```

#### Documentación e infraestructura

```text
docs(architecture): update frontend architecture
docs(agents): define commit conventions
chore(deps): update frontend dependencies
ci(github): configure automated tests
build(frontend): configure production build
```

### 5.5 Commits con descripción extendida

Cuando un cambio requiera explicación adicional, utilizar:

```text
feat(appointments): implement appointment scheduling

- Add appointment creation flow
- Validate professional availability
- Integrate appointment API service
- Add form validation

Refs: #12
```

La descripción extendida debe explicar los detalles relevantes del cambio.

No incluir información redundante.

### 5.6 Breaking changes

Cuando un cambio rompa compatibilidad con funcionalidades o contratos existentes, indicarlo explícitamente:

```text
feat(api): restructure appointment response

BREAKING CHANGE: appointment responses now include a nested professional object
```

Los cambios incompatibles requieren aprobación previa cuando afecten contratos, arquitectura o reglas de negocio.

### 5.7 Organización de commits

Cada commit debe representar una unidad lógica de trabajo.

Reglas:

- No mezclar funcionalidades independientes en un mismo commit.
- No mezclar correcciones no relacionadas.
- No incluir archivos temporales.
- No incluir archivos generados innecesariamente.
- No incluir secretos ni archivos locales de configuración.
- Mantener los cambios pequeños y revisables.
- Separar documentación, pruebas y funcionalidades cuando representen cambios independientes.

### 5.8 Flujo de trabajo Git

Antes de realizar cambios:

1. Ejecutar `git status`.
2. Revisar la rama actual.
3. Revisar los últimos commits.
4. Identificar cambios locales existentes.
5. No sobrescribir cambios realizados por otros desarrolladores.

Para nuevas funcionalidades, utilizar ramas descriptivas:

```text
feat/patient-registration
feat/appointment-management
fix/authentication-error
fix/appointment-validation
docs/update-architecture
refactor/patient-services
```

No trabajar directamente sobre `main` salvo autorización explícita.

### 5.9 Validación antes de realizar un commit

Antes de proponer o ejecutar un commit:

1. Revisar los archivos modificados.
2. Revisar el diff completo.
3. Verificar que no existan cambios ajenos a la tarea.
4. Ejecutar las pruebas correspondientes.
5. Ejecutar el proceso de compilación cuando corresponda.
6. Corregir errores detectados.
7. Verificar que no existan secretos ni credenciales.
8. Preparar un mensaje que cumpla Conventional Commits.

No declarar que las pruebas fueron exitosas si no se ejecutaron.

No realizar commits, push, amend o rebase sin autorización explícita del responsable del proyecto.

### 5.10 Revisión del historial

Cuando sea necesario revisar el historial de Git, utilizar:

```bash
git log --oneline -10
git log --oneline --decorate
git status
git diff
git show HEAD
```

Para revisar los cambios de un commit específico:

```bash
git show <commit-hash>
```

El historial debe permitir identificar claramente:

- Qué funcionalidad se incorporó.
- Qué error se corrigió.
- Qué módulo fue modificado.
- Qué documentación fue actualizada.

## 6. Calidad y pruebas

Toda implementación debe incluir pruebas acordes con el cambio.

### Frontend

- Verificar el funcionamiento de los componentes Vue.
- Verificar formularios y validaciones.
- Verificar estados de carga y error.
- Verificar integración con los servicios REST.
- Comprobar compilación de producción.
- Verificar comportamiento responsive cuando corresponda.

### Backend

- Verificar controladores.
- Verificar servicios.
- Verificar DTOs y validaciones.
- Verificar autorización.
- Verificar reglas de negocio.
- Verificar integridad de datos.
- Ejecutar pruebas unitarias y de integración pertinentes.

### Base de datos

- Verificar relaciones.
- Verificar restricciones.
- Verificar migraciones.
- Verificar compatibilidad con datos existentes.

## 7. Documentación

Cuando una implementación modifique el comportamiento del sistema:

- Actualizar `SPEC.md` cuando corresponda.
- Actualizar `ARCHITECTURE.md` si cambia la arquitectura.
- Actualizar documentación de API cuando cambien contratos.
- Actualizar este archivo cuando cambien las reglas de trabajo.
- Documentar nuevas decisiones técnicas.

No duplicar documentación innecesariamente.

## 8. Resultado esperado

Cada implementación debe integrarse con la arquitectura existente sin introducir incompatibilidades ni duplicaciones.

Todo cambio debe ser:

- Compatible con la arquitectura.
- Tipado correctamente.
- Validado.
- Probado.
- Documentado cuando corresponda.
- Revisable mediante Git.
- Identificable mediante un commit descriptivo.

La documentación y el esquema Prisma son las referencias obligatorias del proyecto.

**Ningún agente de IA puede ignorar estas instrucciones sin autorización explícita del responsable del proyecto.**
