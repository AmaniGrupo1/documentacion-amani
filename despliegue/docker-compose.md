# Despliegue con Docker Compose

## Requisitos previos

- Docker Desktop instalado
- Variables de entorno configuradas (ver [Variables de entorno](variables-entorno.md))

## Levantar el entorno completo

```bash
cd amani-apirest
docker compose up -d
```

## Servicios incluidos

| Servicio | Puerto | Descripción |
|---|---|---|
| `amani-api` | `8080` | Backend Spring Boot |
| `postgres` | `5432` | Base de datos PostgreSQL |
| `mailpit` | `8025` | Interfaz web de emails (dev) |

## Verificar que todo funciona

```bash
# Estado de los contenedores
docker compose ps

# Logs del backend
docker compose logs -f amani-api

# Swagger UI
http://localhost:8080/swagger-ui.html

# Mailpit (emails de desarrollo)
http://localhost:8025
```

## Parar el entorno

```bash
docker compose down          # Para los contenedores
docker compose down -v       # Para + elimina volúmenes (BD incluida)
```
