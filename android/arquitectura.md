# Arquitectura Android

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| UI | Jetpack Compose |
| Diseño | Material Design 3 |
| Lenguaje | Kotlin |
| Inyección de dependencias | Hilt |
| Navegación | Navigation Compose |
| Red | Retrofit + OkHttp |
| WebSocket | StompProtocolAndroid + RxJava |
| Imágenes | Coil |

## Estructura de paquetes

```
applicationamani/
└── src/main/java/
    └── org.ies.tierno.applicationamani/
        ├── data/
        │   ├── model/        # DTOs y modelos de red
        │   ├── network/      # Retrofit + interceptores
        │   └── repository/   # Repositorios
        ├── domain/
        │   └── usecase/      # Casos de uso
        └── presentation/
            ├── ui/
            │   ├── componente/  # Componentes reutilizables
            │   └── screen/      # Pantallas
            └── viewmodel/       # ViewModels
```

## Componentes reutilizables

| Componente | Descripción |
|---|---|
| `CalendarioView` | Calendario mensual con citas destacadas |
| `BurbujaMensaje` | Burbuja de chat con estado de lectura |
| `TarjetaCita` | Card con resumen de una cita |
| `BadgeEstado` | Chip de estado de cita |

## Comunicación con el backend

- **REST** → Retrofit para operaciones CRUD
- **WebSocket + STOMP** → Chat en tiempo real y notificaciones
