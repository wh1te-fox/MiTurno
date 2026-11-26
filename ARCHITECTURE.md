
# MiTurno — Architecture

**Versión:** 1.1  
**Estado:** Arquitectura objetivo propuesta  
**Proyecto:** Sistema de gestión de pacientes y citas médicas para hospitales públicos de Nicaragua.

## 1. Propósito

MiTurno es una aplicación web destinada a digitalizar y organizar los procesos de registro de pacientes, gestión hospitalaria y programación de citas médicas.

Este documento define la arquitectura técnica oficial del proyecto.

Cualquier desarrollador o agente de IA debe respetar estas decisiones. No debe introducir arquitecturas alternativas, tecnologías adicionales o estructuras incompatibles sin una decisión explícita del responsable del proyecto.

## 2. Estado actual y arquitectura objetivo

El repositorio contiene un prototipo frontend basado en HTML, CSS y JavaScript, además de una estructura de proyecto Vue.

La arquitectura objetivo será una aplicación cliente-servidor con:

- Frontend SPA desarrollado con Vue 3.
- TypeScript para tipado estático.
- Tailwind CSS para estilos.
- API REST desarrollada con NestJS.
- PostgreSQL como base de datos relacional.
- Prisma ORM para persistencia.

La migración desde el prototipo debe ser progresiva. No se deben asumir funcionalidades backend existentes.

## 3. Stack tecnológico oficial

### 3.1 Frontend

| Tecnología | Responsabilidad |
|---|---|
| Vue 3 | Construcción de interfaces |
| TypeScript | Tipado estático |
| Composition API | Organización de lógica y componentes |
| Vite | Desarrollo y compilación |
| Vue Router | Navegación y rutas |
| TanStack Vue Query | Estado remoto y caché de API |
| VeeValidate | Gestión y validación de formularios |
| Zod | Validación de datos y esquemas |
| Tailwind CSS | Sistema de estilos y diseño visual |

Se utilizará Vue 3 con Composition API y `<script setup lang="ts">` como estándar para los componentes.

No utilizar React, JSX, React Router, React Hook Form ni CSS Modules.

Los estilos deberán implementarse mediante clases utilitarias de Tailwind CSS.

### 3.2 Backend

| Tecnología | Responsabilidad |
|---|---|
| Node.js LTS | Runtime |
| TypeScript | Lenguaje |
| NestJS | Framework backend |
| REST API | Comunicación |
| Prisma ORM | Acceso a datos |
| class-validator | Validación de DTOs |
| Swagger / OpenAPI | Contratos de API |
| JWT | Autenticación mediante tokens |

Los DTOs y las validaciones de entrada son obligatorios. No se deben aceptar directamente objetos de entrada como entidades de persistencia.

### 3.3 Base de datos

- PostgreSQL.
- Prisma ORM.
- Migraciones versionadas.
- Integridad referencial.
- Transacciones para operaciones críticas.
- UUID como identificador primario.
- Fechas almacenadas con zona horaria.

### 3.4 Infraestructura

- Docker y Docker Compose.
- GitHub Actions.
- Variables de entorno.
- Entornos separados: development, test y production.

## 4. Arquitectura lógica

El sistema seguirá una arquitectura modular por dominios.

Los módulos principales serán:

1. Authentication
2. Users
3. Roles
4. Hospitals
5. Patients
6. Specialties
7. Professionals
8. Schedules
9. Appointments
10. Audit

Cada módulo tendrá responsabilidades delimitadas.

### 4.1 Organización del backend

```text
backend/
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   ├── common/
│   │   ├── decorators/
│   │   ├── filters/
│   │   ├── guards/
│   │   ├── interceptors/
│   │   └── validators/
│   ├── config/
│   ├── database/
│   │   └── prisma/
│   └── modules/
│       ├── auth/
│       ├── users/
│       ├── roles/
│       ├── hospitals/
│       ├── patients/
│       ├── specialties/
│       ├── professionals/
│       ├── schedules/
│       ├── appointments/
│       └── audit/
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
├── test/
├── Dockerfile
└── package.json
```

Cada módulo de negocio debe separar:

- Controller: entrada HTTP.
- Service: reglas de negocio.
- DTO: contratos de entrada y salida.
- Repository: acceso a persistencia cuando se requiera abstraerlo.
- Tests: pruebas unitarias y de integración.

No crear capas o abstracciones innecesarias.

### 4.2 Organización del frontend

El frontend se organizará por funcionalidades y dominios de negocio.

```text
frontend/
├── public/
├── src/
│   ├── app/
│   │   ├── router/
│   │   │   └── index.ts
│   │   └── providers/
│   ├── assets/
│   ├── components/
│   │   ├── common/
│   │   └── layout/
│   ├── features/
│   │   ├── auth/
│   │   ├── patients/
│   │   ├── appointments/
│   │   ├── professionals/
│   │   └── administration/
│   ├── composables/
│   ├── lib/
│   ├── services/
│   ├── types/
│   ├── styles/
│   │   └── main.css
│   ├── App.vue
│   └── main.ts
├── index.html
├── vite.config.ts
├── tsconfig.json
└── package.json
```

Responsabilidades:

- `app/`: configuración general de la aplicación.
- `router/`: definición de rutas y navegación.
- `providers/`: configuración de proveedores globales.
- `assets/`: imágenes, iconos y recursos estáticos.
- `components/common/`: componentes reutilizables.
- `components/layout/`: estructuras visuales y navegación.
- `features/`: funcionalidades organizadas por dominio.
- `composables/`: lógica reutilizable de Vue.
- `lib/`: configuración de bibliotecas y utilidades.
- `services/`: comunicación con la API REST.
- `types/`: tipos e interfaces compartidos.
- `styles/`: entrada global de Tailwind CSS.

Cada funcionalidad dentro de `features/` podrá organizar sus propios componentes, composables, servicios, tipos y esquemas de validación.

No concentrar toda la lógica de negocio en `App.vue` ni en componentes genéricos.

## 5. Reglas de arquitectura

### 5.1 Comunicación

- El frontend se comunica exclusivamente con el backend mediante API REST.
- El frontend no se conecta directamente a PostgreSQL.
- El backend es responsable de validar y autorizar todas las operaciones.
- Los contratos de API deben documentarse en OpenAPI.
- Todas las respuestas deben mantener formatos consistentes.
- Las peticiones HTTP deben centralizarse en servicios.
- No realizar llamadas HTTP directamente desde componentes cuando exista un servicio correspondiente.
- La URL de la API debe configurarse mediante variables de entorno.

### 5.2 Gestión del estado

Se utilizará TanStack Vue Query para gestionar:

- Consultas a la API.
- Caché de información remota.
- Estados de carga.
- Estados de error.
- Actualización e invalidación de consultas.
- Mutaciones de datos.

El estado local de interfaz se administrará mediante las herramientas reactivas de Vue (`ref`, `reactive`, `computed`).

No incorporar Pinia por defecto. Solo podrá evaluarse mediante una decisión explícita si aparece una necesidad real de estado global del cliente que no pueda resolverse adecuadamente con las herramientas existentes.

No duplicar información del servidor innecesariamente en el estado local.

### 5.3 Componentes Vue

- Utilizar Vue 3 Composition API.
- Utilizar `<script setup lang="ts">`.
- Preferir componentes pequeños y reutilizables.
- Definir correctamente `props` y `emits`.
- Evitar componentes excesivamente acoplados.
- Separar la lógica de negocio de la presentación.
- Reutilizar composables cuando exista lógica compartida.
- Evitar manipulación directa del DOM.
- No utilizar Options API como estándar para nuevos componentes.

### 5.4 Identificadores

- Utilizar UUID en entidades principales.
- No utilizar índices autoincrementales como identificadores públicos.
- Las claves foráneas deben conservar el tipo UUID.

### 5.5 Fechas y horarios

- Zona horaria operativa: America/Managua.
- Guardar instantes en PostgreSQL como TIMESTAMPTZ.
- Transmitir fechas mediante ISO 8601.
- No almacenar fechas como texto libre.
- La interfaz debe presentar los horarios en hora local de Nicaragua.
- Las fechas de nacimiento deben representarse como DATE, no como timestamps.

### 5.6 Seguridad

- Contraseñas almacenadas mediante Argon2id.
- Autorización basada en roles y permisos.
- Validación de todos los DTO.
- Protección contra acceso horizontal no autorizado.
- No exponer datos clínicos o personales en logs.
- Registrar operaciones administrativas relevantes.
- No almacenar secretos en el repositorio.
- Aplicar mínimo privilegio en los accesos a datos.
- Proteger las rutas del frontend según los permisos del usuario, sin considerar estas protecciones como sustituto de la autorización del backend.

Los datos de pacientes son información sensible. Deben existir controles de acceso, trazabilidad, copias de seguridad y políticas de conservación.

### 5.7 Integridad de citas

Una cita no puede ser asignada a un profesional en un intervalo que ya esté ocupado por otra cita activa.

Esta validación debe implementarse en el backend y protegerse también mediante restricciones transaccionales en PostgreSQL.

No confiar exclusivamente en validaciones del frontend.

## 6. Estándares de interfaz y estilos

### 6.1 Tailwind CSS

Tailwind CSS será el sistema oficial de estilos del frontend.

Reglas:

- Utilizar clases utilitarias de Tailwind CSS.
- Mantener consistencia visual entre módulos.
- Evitar hojas CSS independientes para componentes cuando las utilidades sean suficientes.
- No introducir Bootstrap, Bulma, Material UI ni otros frameworks CSS.
- No utilizar CSS Modules.
- Evitar estilos inline cuando puedan expresarse mediante Tailwind.
- Centralizar los estilos globales y las personalizaciones del sistema de diseño.
- Mantener una interfaz responsive para escritorio, tabletas y dispositivos móviles.

### 6.2 Diseño visual

La interfaz deberá priorizar:

- Accesibilidad.
- Claridad de navegación.
- Legibilidad.
- Consistencia visual.
- Facilidad de uso para personal administrativo y sanitario.
- Formularios claros y comprensibles.
- Estados de carga, error, vacío y éxito.
- Confirmación de operaciones sensibles.

Los textos visibles para los usuarios deben estar en español.

## 7. Formularios y validaciones

- Utilizar VeeValidate para gestionar formularios.
- Utilizar Zod para definir esquemas de validación.
- Mantener los esquemas organizados por funcionalidad.
- Mostrar mensajes de validación en español.
- Validar los datos tanto en frontend como en backend.
- No considerar las validaciones del frontend como una medida de seguridad.
- Evitar duplicar reglas de negocio en las interfaces.

Las validaciones definitivas y las reglas de integridad deben permanecer en el backend.

## 8. Convenciones de código

- Utilizar TypeScript en frontend y backend.
- Preferir nombres en inglés para entidades, tablas, propiedades y código.
- Utilizar español para los textos visibles de la interfaz.
- Utilizar camelCase para variables y propiedades.
- Utilizar PascalCase para clases y componentes Vue.
- Utilizar kebab-case para nombres de archivos cuando corresponda.
- Utilizar nombres descriptivos para componentes, composables y servicios.
- Mantener una única fuente de verdad para cada entidad.
- Evitar duplicar reglas de negocio en frontend y backend.
- Utilizar tipado explícito en contratos y respuestas de API.
- Evitar `any` cuando exista una alternativa tipada.

## 9. Política de dependencias

No incorporar una nueva librería sin justificar:

1. Qué problema resuelve.
2. Por qué las dependencias existentes no son suficientes.
3. Qué impacto tiene en mantenimiento y seguridad.
4. Si introduce incompatibilidades.

No cambiar versiones principales del stack sin aprobación.

Las tecnologías oficiales definidas en este documento deben mantenerse como referencia para nuevas funcionalidades.

## 10. Migraciones

- Todas las modificaciones de estructura deben realizarse mediante migraciones Prisma versionadas.
- Nunca modificar directamente la base de datos de producción.
- No eliminar columnas o tablas sin analizar dependencias.
- Las migraciones deben ser revisables y reproducibles.
- Mantener datos existentes cuando una migración requiera transformar estructuras.

## 11. Regla para agentes de IA

Antes de implementar cualquier funcionalidad:

1. Leer `ARCHITECTURE.md`.
2. Leer `SPEC.md`.
3. Revisar `schema.prisma`.
4. Revisar los módulos existentes.
5. Identificar las entidades y relaciones ya definidas.
6. Reutilizar contratos y servicios existentes.
7. Revisar la estructura Vue existente antes de crear nuevos componentes.
8. Respetar Tailwind CSS como sistema oficial de estilos.

Un agente de IA NO debe:

- Introducir React o tecnologías frontend alternativas.
- Crear tablas duplicadas.
- Inventar nuevos campos sin justificar su necesidad.
- Cambiar UUID por IDs numéricos.
- Introducir otra base de datos.
- Crear endpoints que contradigan los contratos existentes.
- Modificar relaciones sin actualizar las migraciones.
- Reemplazar tecnologías del stack por preferencias propias.
- Simular persistencia con datos locales cuando la funcionalidad requiere backend.
- Introducir frameworks CSS adicionales.
- Crear estilos aislados innecesarios.
- Duplicar servicios o lógica de negocio.
- Implementar reglas de seguridad únicamente en el frontend.

Si falta una definición, debe señalar la ambigüedad y proponer una decisión antes de implementar.

## 12. Principio de evolución

La arquitectura puede evolucionar, pero toda modificación estructural requiere:

- Justificación técnica.
- Actualización de `ARCHITECTURE.md`.
- Actualización de `SPEC.md` si afecta reglas funcionales.
- Actualización del esquema de datos.
- Migración correspondiente.
- Pruebas de compatibilidad.

**Este documento es la referencia técnica obligatoria de MiTurno.**
