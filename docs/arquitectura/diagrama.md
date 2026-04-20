# Diagrama de Arquitectura

Esquema visual de la arquitectura del sistema.

---

## Diagrama de capas

```mermaid
graph TD
    subgraph Client["Client Layer"]
        WA[Web App]
        MA[Mobile App<br/>Android]
        SW[Swagger UI]
    end
    
    subgraph Controller["Controller Layer (REST)"]
        AUTH["/auth/*"]
        ADMIN["/api/admin/*"]
        PSI["/api/psicologo/*"]
        PAC["/api/paciente/*"]
        WS["/ws"]
    end
    
    subgraph Service["Service Layer"]
        AS[Auth Service]
        CS[Cita Service]
        US[Usuario Service]
        ES[Email Service]
        FS[Firebase Notif]
        MS[Mensaje Service]
        EV[Application Events<br/>CitaCreadaEvent → Listeners]
    end
    
    subgraph Repository["Repository Layer (JPA)"]
        UR[UsuarioRepo]
        CR[CitaRepo]
        MR[MensajeRepo]
    end
    
    subgraph DB["Database Layer"]
        PG[(PostgreSQL<br/>psicologia_app)]
    end
    
    Client --> Controller
    Controller --> Service
    Service --> Repository
    Repository --> DB
```

---

## Flujo de autenticación

```mermaid
sequenceDiagram
    participant C as Cliente
    participant S as Servidor
    participant DB as Base de datos
    
    C->>S: POST /auth/login
    S->>DB: Validar credenciales (BCrypt)
    DB-->>S: Usuario encontrado
    S->>S: Generar JWT (HS256, 24h)
    Note over S: Claims: email, rol
    S-->>C: { token, idUsuario, rol }
```

---

## Flujo WebSocket

```mermaid
sequenceDiagram
    participant C as Cliente
    participant S as Servidor
    
    C->>S: CONNECT /ws (STOMP handshake)
    C->>S: SUBSCRIBE /topic/mensajes
    C->>S: SEND /app/chat
    S->>S: Broadcast /topic/mensajes
    S-->>C: MESSAGE /topic/mensajes
```

---

## Flujo de notificaciones

```mermaid
flowchart LR
    E[ApplicationEvent] --> L["@TransactionalEventListener"]
    L --> EMAIL[EmailService → SMTP]
    L --> FCM[FirebaseNotificationService → FCM]
    
    subgraph Eventos
        E1[CitaCreadaEvent]
        E2[CitaCanceladaEvent]
        E3[CitaRecordatorioEvent]
    end
    
    E1 --> L
    E2 --> L
    E3 --> L
```

---

## Flujo de seguridad

```mermaid
flowchart TD
    REQ[Request] --> JWT[JwtAuthFilter]
    JWT --> EXT[Extraer token]
    EXT --> VAL[Validar firma JWT]
    VAL --> UD[Obtener UserDetails]
    UD --> SEC[SecurityFilterChain]
    SEC --> PUB{¿Endpoint público?}
    PUB -->|Sí| PERMIT[PermitAll]
    PUB -->|No| ROLE{¿Rol autorizado?}
    ROLE -->|Sí| CTRL[Controller]
    ROLE -->|No| DENY[403 Forbidden]
```

---

## Relación Entidad-Entidad (ER)

```mermaid
erDiagram
    USUARIO ||--o| PACIENTE : tiene
    USUARIO ||--o| PSICOLOGO : tiene
    PACIENTE }o--|| PSICOLOGO : asignado_a
    PACIENTE ||--o{ CITA : agenda
    PSICOLOGO ||--o{ CITA : atiende
    CITA ||--o| SESION : genera
    PACIENTE ||--o{ DIARIO_EMOCIONES : registra
    PACIENTE ||--o{ MENSAJE : envia
    PACIENTE ||--o{ HISTORIAL_CLINICO : tiene
    PACIENTE ||--o{ DIRECCION : posee
    
    USUARIO {
        bigint id_usuario PK
        varchar nombre
        varchar email UK
        varchar password
        enum rol
        boolean activo
    }
    
    PACIENTE {
        bigint id_paciente PK
        bigint id_usuario FK
        bigint id_psicologo FK
        date fecha_nacimiento
        varchar genero
    }
    
    PSICOLOGO {
        bigint id_psicologo PK
        bigint id_usuario FK
        varchar especialidad
        int experiencia
    }
    
    CITA {
        bigint id_cita PK
        bigint id_paciente FK
        bigint id_psicologo FK
        timestamp start_datetime
        enum estado
    }
```

---

## Patrones implementados

| Patrón | Uso en Amani |
|--------|--------------|
| **Layered Architecture** | Controller → Service → Repository → Model |
| **Dependency Injection** | Spring @Autowired en todos los services |
| **DTO Pattern** | DTOs separados por rol y propósito |
| **Event-Driven** | @TransactionalEventListener para notificaciones |
| **Decorator** | FileStorageService wrap de Files.copy |
| **Strategy** | PasswordEncoder (BCrypt) |
| **Template Method** | Abstract classes con métodos comunes |
| **Singleton** | @Service y @Component por defecto |
