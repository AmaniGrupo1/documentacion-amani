# OpenAPI - Especificación de la API

Documentación generada automáticamente desde el código Java usando SpringDoc OpenAPI.

## Acceso Interactivo

### Swagger UI

La documentación interactiva Swagger UI está disponible en:

```
http://localhost:8080/swagger-ui.html
```

### Endpoints de metadatos

- **YAML**: `http://localhost:8080/v3/api-docs.yaml`
- **JSON**: `http://localhost:8080/v3/api-docs`

## Endpoints de la API

### Autenticación
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/auth/login` | Iniciar sesión |
| POST | `/auth/register-paciente` | Registrar paciente |
| POST | `/auth/register-admin` | Registrar administrador |

### Paciente
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/citas` | Listar citas |
| GET | `/api/citas/paciente/{id}/agenda` | Agenda mensual |

### Psicólogo
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/psicologo/pacientes` | Listar pacientes |

### Administrador
| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/admin/psicologos` | Listar psicólogos |
| GET | `/api/admin/pacientes` | Listar pacientes |

## Versión

OpenAPI 3.0 - SpringDoc 2.5.0
