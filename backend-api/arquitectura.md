# Arquitectura del Backend

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| Framework | Spring Boot 3 |
| Base de datos | PostgreSQL |
| ORM | Hibernate / JPA |
| Seguridad | Spring Security + JWT |
| Tiempo real | WebSocket + STOMP |
| Push notifications | Firebase Cloud Messaging |
| Email | Spring Mail + Thymeleaf |
| Documentación API | SpringDoc / OpenAPI |

## Estructura de paquetes

```
amani-apirest/
└── src/main/java/
    └── com.amani.apirest/
        ├── config/          # Configuración (Security, WebSocket, FCM...)
        ├── controller/      # Controladores REST
        ├── dto/             # Objetos de transferencia de datos
        ├── entity/          # Entidades JPA
        ├── event/           # Eventos de dominio
        ├── listener/        # Listeners de eventos (@TransactionalEventListener)
        ├── repository/      # Repositorios JPA
        └── service/         # Lógica de negocio
```

## Roles y seguridad

El sistema tiene tres roles principales gestionados con Spring Security:

- `ROLE_PACIENTE`
- `ROLE_PSICOLOGO`
- `ROLE_ADMIN`

La autenticación se realiza mediante **JWT** pasado en la cabecera `Authorization: Bearer <token>`.

## Sistema de notificaciones

Las notificaciones siguen una estrategia de tres canales por orden de prioridad:

```
Evento en base de datos (AFTER_COMMIT)
        ↓
¿Usuario conectado por WebSocket?
    ├── SÍ → Enviar por WebSocket (STOMP)
    └── NO → Enviar FCM (push)
               + Email de confirmación (si aplica)
```

Los eventos se disparan con `@TransactionalEventListener(phase = AFTER_COMMIT)` + `@Async` para no bloquear la transacción principal.
