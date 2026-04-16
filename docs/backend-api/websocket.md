# WebSocket y Chat en Tiempo Real

## Configuración del endpoint

```
ws://localhost:8080/ws
```

El servidor usa **STOMP** sobre WebSocket.

## Topics disponibles

| Topic | Descripción |
|---|---|
| `/topic/chat/{citaId}` | Mensajes de una cita concreta |
| `/topic/notificaciones/{usuarioId}` | Notificaciones en tiempo real |
| `/user/queue/errors` | Errores personalizados por usuario |

## Destinos de envío (cliente → servidor)

| Destino | Descripción |
|---|---|
| `/app/chat/{citaId}` | Enviar mensaje de chat |

## Conexión desde Android

```kotlin
stompClient = Stomp.over(
    Stomp.ConnectionProvider.OKHTTP,
    "ws://10.0.2.2:8080/ws/websocket"
)

stompClient?.connect(listOf(
    StompHeader("Authorization", "Bearer $token")
))
```

## Flujo de un mensaje

```
Android ──SEND /app/chat/{id}──▶ Spring
                                    ↓
                             Guardar en BD
                                    ↓
                        Broadcast /topic/chat/{id}
                                    ↓
Android ◀──MESSAGE──────────────────
```
