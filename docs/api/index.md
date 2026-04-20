# API REST

Documentación completa de los endpoints de la API de Amani.

---

## Documentación por sección

| Sección | Descripción |
|---------|-------------|
| [**Autenticación**](auth.md) | Login, registro, tokens JWT |
| [**Pacientes**](pacientes.md) | CRUD de pacientes, perfil |
| [**Psicólogos**](psicologos.md) | Gestión de psicólogos |
| [**Administración**](admin.md) | Endpoints de administración |
| [**Citas**](citas.md) | Agendas y citas |
| [**Diario Emocional**](diario-emocional.md) | Registro de emociones |
| [**Progreso Emocional**](progreso-emocional.md) | Estadísticas de progreso |
| [**Historial Clínico**](historial-clinico.md) | Historial médico |
| [**Sesiones**](sesiones.md) | Sesiones de terapia |
| [**Mensajes**](mensajes.md) | Chat entre usuarios |
| [**Ajustes**](ajustes.md) | Preferencias de usuario |
| [**Direcciones**](direcciones.md) | Direcciones del paciente |
| [**Situaciones**](situaciones.md) | Catálogo de situaciones |

---

## Credenciales de prueba

| Rol | Email | Contraseña |
|-----|-------|------------|
| Admin | `admin@amani.com` | `admin1234` |
| Psicólogo | `psicologo@amani.com` | `psicologo123` |
| Paciente | `paciente@amani.com` | `paciente123` |

---

## Probar con Swagger

La documentación interactiva está disponible en:

```
http://localhost:8080/docs
```

---

## Configuración

### Base path

```
/api
```

### Autenticación

Todos los endpoints protegidos requieren el header:

```
Authorization: Bearer {token_jwt}
```

### Headers comunes

| Header | Valor |
|--------|-------|
| `Content-Type` | `application/json` |
| `Authorization` | `Bearer {token}` |

---

## DTOs comunes

### Login Request

```json
{
  "email": "usuario@ejemplo.com",
  "password": "contraseña123"
}
```

### Login Response

```json
{
  "idUsuario": 1,
  "nombre": "Nombre",
  "rol": "ADMIN",
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "idPsicologo": null,
  "idPaciente": null
}
```

### Error Response

```json
{
  "mensaje": "Credenciales inválidas",
  "timestamp": "2024-04-15T10:30:00",
  "status": 401,
  "error": "Unauthorized"
}
```

---

## Rate limiting

Actualmente no hay rate limiting implementado. Para producción, considerar:

- `@RateLimiter` (Resilience4j)
- `@Retryable` para reintentos
- Circuit breaker para servicios externos

---

## Versionado

Versión actual: **v1.0.0**

---

## Estructura de URLs

```
/auth/*
    ├── POST /login                # Iniciar sesión
    ├── POST /register-paciente    # Registrar paciente
    ├── POST /register-admin       # Registrar admin

/api/citas/*
    ├── GET /                      # Listar citas
    ├── GET /{id}                  # Obtener cita
    ├── POST /                     # Crear cita
    ├── PUT /{id}                  # Actualizar cita
    ├── DELETE /{id}               # Eliminar cita
    └── GET /paciente/{id}/agenda  # Agenda mensual

/api/psicologo/*
    ├── POST /{id}/foto            # Subir foto
    ├── GET /{id}/perfil           # Obtener perfil
    └── GET /pacientes/{id}/psicologo # Psicólogo asignado

/api/admin/psicologos/*
    ├── POST /asignar-psicologo    # Asignar paciente
    ├── GET /                      # Listar psicólogos
    ├── POST /create               # Crear psicólogo
    ├── PUT /{id}                  # Actualizar
    ├── DELETE /{id}               # Eliminar
    └── GET /pacientes             # Psicólogos + pacientes
```

---

## Seguridad

### Endpoints públicos (sin autenticación)

- `POST /auth/login`
- `POST /auth/register-paciente`
- `GET /api/situaciones`
- `GET /api/psicologo/pacientes/*/psicologo`
- `/docs/**`, `/v3/api-docs/**`

### Endpoints de administración

- `POST /auth/register-admin`
- `GET /auth/admins`
- `PUT /auth/pacientes/{id}/baja`
- `GET /api/pacientes/admin`
- `GET /api/admin/**`

### Endpoints de psicólogo

- `GET /api/psicologo/**`
- `GET /api/psicologo/pacientes/getAll/**`

---

## Notas

- Todos los endpoints validan JWT y roles
- Los errores devuelven status code 4xx/5xx
- Las respuestas exitosas devuelven 200/201/204
- Paginación: no implementada (añadir si es necesario)
