# Amani

**Plataforma de conexión entre pacientes y psicólogos.**

Amani permite a los pacientes encontrar y reservar citas con psicólogos, comunicarse mediante chat en tiempo real y recibir notificaciones de sus consultas.

---

## Estructura del proyecto

| Repositorio | Tecnología | Descripción |
|---|---|---|
| `amani-apirest` | Spring Boot | API REST + WebSocket + Notificaciones |
| `amani-android` | Jetpack Compose | App Android para pacientes y psicólogos |

## Roles del sistema

| Rol | Descripción |
|---|---|
| `ROLE_PACIENTE` | Reserva citas y se comunica con su psicólogo |
| `ROLE_PSICOLOGO` | Gestiona su agenda y atiende a sus pacientes |
| `ROLE_ADMIN` | Administra la plataforma |

## Tecnologías principales

**Backend**

- Spring Boot 3
- PostgreSQL + Hibernate/JPA
- WebSocket + STOMP
- Firebase Cloud Messaging (FCM)
- Spring Mail + Thymeleaf

**Android**

- Jetpack Compose
- Material Design 3
- STOMP sobre WebSocket
- Kotlin + Coroutines

## Inicio rápido

```bash
# Levantar todo el entorno
cd ../amani-apirest
docker compose up -d
```

Consulta la sección de [Despliegue](despliegue/docker-compose.md) para más detalles.
