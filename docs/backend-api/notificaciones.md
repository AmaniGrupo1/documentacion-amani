# Sistema de Notificaciones

## Estrategia de tres canales

Las notificaciones siguen este orden de prioridad:

```
1. WebSocket (STOMP)   → usuario conectado en ese momento
2. FCM (push)          → usuario con app en segundo plano
3. Email               → confirmaciones de citas
```

## Implementación

Los eventos se disparan tras el commit de la transacción principal para garantizar consistencia:

```java
@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)
@Async
public void onCitaCreada(CitaCreadaEvent event) {
    // 1. Intentar WebSocket
    // 2. Si no conectado → FCM
    // 3. Email de confirmación
}
```

## Configuración FCM

Las credenciales de Firebase se configuran como variable de entorno:

```env
FIREBASE_CREDENTIALS_PATH=/ruta/al/serviceAccountKey.json
```

## Configuración Email

```env
MAIL_HOST=smtp.resend.com
MAIL_PORT=465
MAIL_USERNAME=resend
MAIL_PASSWORD=re_xxxxxxxxxxxx
```

Para desarrollo local se usa **Mailpit** en `localhost:1025`.
