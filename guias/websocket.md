# WebSocket

Guía de integración con WebSocket (STOMP sobre SockJS).

---

## 📡 Endpoint

```
ws://localhost:8080/ws
```

---

## 🔧 Configuración

La configuración está en `WebSocketConfig.java`:

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
                .setAllowedOrigins("*")
                .withSockJS();
    }
    
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.setApplicationDestinationPrefixes("/app");
        registry.enableSimpleBroker("/topic", "/queue");
    }
}
```

### Detalles

- **Endpoint STOMP:** `/ws`
- **Broker:** `/topic` (broadcast), `/queue` (punto a punto)
- **Prefijo apps:** `/app`

---

## 📡 Uso con JavaScript (SockJS)

```javascript
// Instalar STOMP client
npm install @stomp/stompjs sockjs-client

// Conectar
const sock = new SockJS('http://localhost:8080/ws');
const stompClient = Stomp.over(sock);

stompClient.connect({ headers: { Authorization: 'Bearer ' + token } },
  (frame) => {
    console.log('Connected: ' + frame);
    
    // Suscribirse a mensajes
    stompClient.subscribe('/topic/mensajes', (message) => {
      console.log('Mensaje:', message.body);
    });
    
    // Enviar mensaje
    stompClient.send('/app/chat', {}, JSON.stringify({
      receiverId: 123,
      mensaje: 'Hola!'
    }));
  },
  (error) => {
    console.log('Error:', error);
  }
);
```

---

## 📡 Uso con Java (Cliente)

```java
@Configuration
public class WebSocketClientConfig {
    
    @Bean
    public WebSocketStompClient webSocketStompClient() {
        WebSocketClient client = new SockJsClient(Collections.singletonList(
            new WebSocketTransport(new StandardWebSocketClient())
        ));
        return new WebSocketStompClient(client);
    }
    
    @Bean
    public StompSession stompSession(WebSocketStompClient stompClient) {
        return stompClient.connect("ws://localhost:8080/ws", 
            new StompSessionHandlerAdapter() {});
    }
}
```

---

## 📡 Eventos WebSocket en Amani

### 1. Mensajes en Tiempo Real

**Destino:** `/topic/mensajes`

```java
// En el controller
@MessageMapping("/chat")
@SendTo("/topic/mensajes")
public ChatMessageDTO enviarMensaje(ChatMessageDTO message) {
    return mensajeService.enviarMensaje(message);
}
```

### 2. Notificaciones Push

Si el usuario está offline → Firebase Push.

```java
// En FirebaseNotificationService
public void enviarPush(String fcmToken, String titulo, String cuerpo) {
    // Envía notificación via FCM
}
```

---

## 📡 Protocolo STOMP

### Headers Comunes

| Header | Valor |
|--------|-------|
| `Authorization` | `Bearer {token_jwt}` |
| `content-type` | `application/json` |

### Comandos

| Comando | Descripción |
|---------|-------------|
| `CONNECT` | Iniciar conexión |
| `SUBSCRIBE` | Suscribirse a un destino |
| `SEND` | Enviar mensaje |
| `DISCONNECT` | Cerrar conexión |

---

## 📡 Seguridad

Los endpoints WebSocket requieren autenticación JWT:

```java
// En SecurityConfig
.requestMatchers("/ws").authenticated()
```

El token se envía en el header `Authorization`:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## 📡 Troubleshooting

### Error: "Connection refused"

- Verificar WebSocketConfig.java existe
- Verificar `spring-boot-starter-websocket` en dependencies

### Error: "Not authenticated"

- Incluir token JWT en header `Authorization`
- Verificar token no ha expirado

### Error: "No destination mapping"

- Verificar `@MessageMapping` en el controller
- Verificar destino coincide con `@SendTo`

---

## 📡 Ejemplo Completo

```javascript
// chat.js
class ChatClient {
    constructor(token) {
        this.token = token;
        this.stompClient = null;
    }
    
    connect() {
        const sock = new SockJS('http://localhost:8080/ws');
        this.stompClient = Stomp.over(sock);
        
        this.stompClient.connect(
            { headers: { Authorization: 'Bearer ' + this.token } },
            this.onConnected.bind(this),
            this.onError.bind(this)
        );
    }
    
    onConnected(frame) {
        console.log('Conectado:', frame);
        
        // Suscribirse
        this.stompClient.subscribe('/topic/mensajes', (msg) => {
            this.onMessageReceived(JSON.parse(msg.body));
        });
    }
    
    onMessageReceived(message) {
        console.log('Mensaje:', message);
        // Renderizar mensaje
    }
    
    sendMessage(receiverId, texto) {
        this.stompClient.send('/app/chat', {}, JSON.stringify({
            receiverId: receiverId,
            mensaje: texto,
            enviadoEn: new Date().toISOString()
        }));
    }
    
    disconnect() {
        if (this.stompClient) {
            this.stompClient.disconnect();
        }
    }
}
```
