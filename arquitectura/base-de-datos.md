# Base de Datos

Esquema PostgreSQL y diseño de tablas.

---

## 📊 Esquema General

**Base de datos:** `psicologia_app`  
**PostgreSQL:** puerto `5433` (configurable en `application.properties`)

---

## 📋 Tablas Principales

### 1. `usuarios`

Cuenta de usuario autenticable.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_usuario` | BIGINT (PK, IDENTITY) | Identificador único |
| `nombre` | VARCHAR(100) | Nombre de pila |
| `apellido` | VARCHAR(100) | Apellido |
| `dni` | VARCHAR(50) | Documento nacional |
| `email` | VARCHAR(150) UNIQUE | Email (login) |
| `password` | VARCHAR(255) | Hash BCrypt |
| `rol` | rol_usuario (ENUM) | admin/psicologo/paciente |
| `activo` | BOOLEAN | Estado de la cuenta |
| `fecha_registro` | TIMESTAMP | Alta de usuario |
| `fecha_baja` | TIMESTAMP | Baja (nullable) |
| `fcm_token` | VARCHAR(512) | Token Firebase |
| `foto_perfil_url` | VARCHAR(500) | URL de avatar |

### 2. `pacientes`

Perfil de paciente (link con `usuarios`).

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_paciente` | BIGINT (PK) | ID paciente |
| `id_usuario` | BIGINT (FK) | Link a `usuarios` |
| `id_psicologo` | BIGINT (FK, nullable) | Psicólogo asignado |
| `fecha_nacimiento` | DATE | Edad del paciente |
| `genero` | VARCHAR(30) | Género |
| `telefono` | VARCHAR(30) | Teléfono |
| `metodo_pago` | metodo_pago (ENUM) | PRESENCIAL/ONLINE |
| `estado_pago` | estado_pago (ENUM) | PENDIENTE/PAGADO |
| `created_at` | TIMESTAMP | |
| `updated_at` | TIMESTAMP | |

### 3. `psicologos`

Perfil de psicólogo (link con `usuarios`).

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_psicologo` | BIGINT (PK) | ID psicólogo |
| `id_usuario` | BIGINT (FK) | Link a `usuarios` |
| `especialidad` | VARCHAR(150) | Especialidad |
| `experiencia` | INT | Años de experiencia |
| `descripcion` | TEXT | Bio del profesional |
| `licencia` | VARCHAR(100) | Número de licencia |
| `created_at` | TIMESTAMP | |
| `updated_at` | TIMESTAMP | |

### 4. `citas`

Agenda de citas paciente-psicólogo.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_cita` | BIGINT (PK) | ID cita |
| `id_paciente` | BIGINT (FK) | Paciente |
| `id_psicologo` | BIGINT (FK) | Psicólogo |
| `start_datetime` | TIMESTAMP | Hora inicio |
| `duration_minutes` | INT (default 50) | Duración |
| `estado` | estado_cita (ENUM) | pendiente/confirmada/cancelada/completada |
| `motivo` | TEXT | Razón de la cita |
| `created_at` | TIMESTAMP | |
| `updated_at` | TIMESTAMP | |

### 5. `sesiones`

Registro de sesiones de terapia.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_sesion` | BIGINT (PK) | ID sesión |
| `id_cita` | BIGINT (FK, UNIQUE) | Link a cita |
| `id_paciente` | BIGINT (FK) | Paciente |
| `id_psicologo` | BIGINT (FK) | Psicólogo |
| `session_date` | TIMESTAMP | Fecha sesión |
| `duration_minutes` | INT | Duración real |
| `notas` | TEXT | Notas del psicólogo |
| `recomendaciones` | TEXT | Recomendaciones |
| `created_at` | TIMESTAMP | |
| `updated_at` | TIMESTAMP | |

### 6. `diario_emociones`

Registro diario de emociones del paciente.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_diario` | BIGINT (PK) | ID registro |
| `id_paciente` | BIGINT (FK) | Paciente |
| `fecha` | TIMESTAMP | |
| `emocion` | VARCHAR(100) | Emoción registrada |
| `intensidad` | INT (1-10) | Nivel intensidad |
| `nota` | TEXT | Observaciones |

### 7. `progreso_emocional`

Resumen semanal/diario de progreso.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_progress` | BIGINT (PK) | ID registro |
| `id_paciente` | BIGINT (FK) | Paciente |
| `fecha` | DATE | |
| `nivel_estres` | INT | 0-10 |
| `nivel_ansiedad` | INT | 0-10 |
| `nivel_animo` | INT | 0-10 |
| `created_at` | TIMESTAMP | |

### 8. `mensajes`

Chat entre usuarios.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_mensaje` | BIGINT (PK) | ID mensaje |
| `sender_id` | BIGINT (FK) | Emisor |
| `receiver_id` | BIGINT (FK) | Receptor |
| `id_cita` | BIGINT (FK, nullable) | Link a cita (opcional) |
| `mensaje` | TEXT | Texto |
| `enviado_en` | TIMESTAMP | |
| `leido` | BOOLEAN | Estado lectura |

### 9. `historial_clinico`

Historial médico del paciente.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_history` | BIGINT (PK) | ID historial |
| `id_paciente` | BIGINT (FK) | Paciente |
| `titulo` | VARCHAR(200) | Título del caso |
| `diagnostico` | TEXT | Diagnóstico |
| `tratamiento` | TEXT | Tratamiento |
| `observaciones` | TEXT | Notas adicionales |
| `creado_en` | TIMESTAMP | |

### 10. `ajustes`

Preferencias del usuario.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_ajuste` | BIGINT (PK) | ID ajuste |
| `id_usuario` | BIGINT (FK) | Usuario |
| `idioma` | VARCHAR(10) | es/en |
| `notificaciones` | BOOLEAN | Habilitado |
| `dark_mode` | BOOLEAN | |
| `timezone` | VARCHAR(100) | Zona horaria |
| `updated_at` | TIMESTAMP | |

### 11. `direcciones`

Direcciones del paciente.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_direccion` | BIGINT (PK) | ID dirección |
| `id_paciente` | BIGINT (FK) | Paciente |
| `calle` | VARCHAR(150) | |
| `ciudad` | VARCHAR(100) | |
| `provincia` | VARCHAR(100) | |
| `codigo_postal` | VARCHAR(20) | |
| `pais` | VARCHAR(100) | |
| `descripcion` | TEXT | Notas |

---

## 🔗 Tablas de Relación

### `psicologo_paciente`

Relación N:M psicólogo-paciente.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id` | BIGINT (PK) | |
| `id_paciente` | BIGINT (FK) | |
| `id_psicologo` | BIGINT (FK) | |
| `fecha_inicio` | TIMESTAMP | |
| `fecha_fin` | TIMESTAMP | NULL = activo |

### `tutores`

Tutores legales de menores.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_tutor` | BIGINT (PK) | |
| `id_paciente` | BIGINT (FK) | |
| `nombre` | VARCHAR(255) | |
| `telefono` | VARCHAR(30) | |
| `email` | VARCHAR(150) | |
| `dni` | VARCHAR(20) | |
| `tipo` | VARCHAR(50) | MADRE/PADRE/TUTOR |

### `consentimientos`

Consentimientos informados.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_consentimiento` | BIGINT (PK) | |
| `id_paciente` | BIGINT (FK) | |
| `fecha_aceptacion` | TIMESTAMP | |
| `version_documento` | VARCHAR(255) | |
| `acepta_videoconferencia` | BOOLEAN | |
| `acepta_comunicacion` | BOOLEAN | |

### `situaciones`

Catálogo de situaciones (ansiedad, duelo, etc.).

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `id_situacion` | BIGINT (PK) | |
| `nombre` | VARCHAR(150) | |
| `categoria` | VARCHAR(100) | |
| `descripcion` | TEXT | |
| `activo` | BOOLEAN | |
| `created_at` | TIMESTAMP | |

### `preguntas` / `opciones` / `respuestas`

Test inicial de evaluación.

| Tabla | Descripción |
|-------|-------------|
| `preguntas` | Preguntas del test |
| `opciones` | Opciones de respuesta |
| `respuestas` | Respuestas del paciente |

---

## 🗂️ Vistas

### `vista_agenda_psicologo`

Agenda consolidada: citas + bloqueos por día.

```sql
-- Devuelve: fecha, hora_inicio, hora_fin, tipo (cita/bloqueo), detalle
```

### `vista_pacientes_psicologo`

Listado pacientes por psicólogo (relación activa).

---

## ⚙️ Configuración

En `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=Sandia4you

spring.jpa.hibernate.ddl-auto=validate
spring.jpa.properties.hibernate.default_schema=psicologia_app
```

---

## 🔄 Migraciones

Script SQL: `src/main/resources/amani.sql`

Crea:
- Esquema `psicologia_app`
- Todos los tipos ENUM
- Todas las tablas con índices
- Vistas
- Datos iniciales
