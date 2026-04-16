# API REST

La documentación interactiva completa está disponible en Swagger UI una vez levantado el backend:

```
http://localhost:8080/swagger-ui.html
```

---

## Endpoints principales

### Autenticación

| Método | Endpoint | Descripción |
|---|---|---|
| `POST` | `/api/auth/login` | Obtener JWT |
| `POST` | `/api/auth/registro` | Registrar nuevo usuario |

### Pacientes

| Método | Endpoint | Descripción |
|---|---|---|
| `GET` | `/api/pacientes` | Listar pacientes |
| `GET` | `/api/pacientes/{id}` | Obtener paciente |
| `PATCH` | `/api/pacientes/{id}` | Actualizar paciente |

### Psicólogos

| Método | Endpoint | Descripción |
|---|---|---|
| `GET` | `/api/psicologos` | Listar psicólogos |
| `GET` | `/api/psicologos/{id}` | Obtener psicólogo |
| `PATCH` | `/api/psicologos/{id}` | Actualizar psicólogo |

### Citas

| Método | Endpoint | Descripción |
|---|---|---|
| `GET` | `/api/citas` | Listar citas |
| `POST` | `/api/citas` | Crear cita |
| `PATCH` | `/api/citas/{id}` | Actualizar estado |
| `DELETE` | `/api/citas/{id}` | Cancelar cita |

---

## Cabeceras requeridas

```http
Authorization: Bearer <jwt_token>
Content-Type: application/json
```
