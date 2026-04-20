# Docker Compose

Guía de despliegue con Docker Compose.

---

## Requisitos previos

- Docker Desktop instalado
- Variables de entorno configuradas (ver [Variables de entorno](../despliegue/variables-entorno.md))

---

## Levantar el entorno

```bash
cd amani-apirest
docker compose up -d
```

---

## Servicios incluidos

| Servicio | Puerto | Descripción |
|----------|--------|-------------|
| `amani-api` | `8080` | Backend Spring Boot |
| `postgres` | `5432` | Base de datos PostgreSQL |
| `mailpit` | `8025` | Interfaz web de emails (desarrollo) |

---

## Verificar

```bash
# Estado de los contenedores
docker compose ps

# Logs del backend
docker compose logs -f amani-api
```

| Recurso | URL |
|---------|-----|
| Swagger UI | `http://localhost:8080/swagger-ui.html` |
| Mailpit | `http://localhost:8025` |

---

## Parar el entorno

```bash
# Parar contenedores
docker compose down

# Parar y eliminar volúmenes (incluye BD)
docker compose down -v
```
