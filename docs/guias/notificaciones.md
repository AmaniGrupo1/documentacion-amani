# Notificaciones

Guía de configuración del sistema de notificaciones.

---

## Email (SMTP)

### Configuración

```properties
# src/main/resources/application.properties

spring.mail.host=localhost
spring.mail.port=1025
spring.mail.username=noreply@apppsicologas.com
spring.mail.properties.mail.smtp.auth=false
spring.mail.properties.mail.smtp.starttls.enable=false
```

### Servicio: `EmailService.java`

**Métodos:**

| Método | Descripción |
|--------|-------------|
| `enviarBienvenida(email, nombre)` | Bienvenida nueva usuario |
| `enviarCitaCreada(email, nombre, cita)` | Notificar nueva cita |
| `enviarCitaCancelada(email, nombre, cita, canceladaPor)` | Cita cancelada |
| `enviarRecordatorioCita(email, nombre, cita)` | Recordatorio 24h antes |
| `enviarNotificacionMensaje(email, remitente, contenido)` | Nuevo mensaje |

### Uso

```java
@Service
public class EmailService {
    private final JavaMailSender mailSender;
    
    public void enviarBienvenida(String destinatario, String nombre) {
        SimpleMailMessage mail = new SimpleMailMessage();
        mail.setFrom("noreply@amanipsicologia.com");
        mail.setTo(destinatario);
        mail.setSubject("¡Bienvenido/a a Amani!");
        mail.setText("Hola " + nombre + ",\n\nTu registro se completó correctamente.");
        mailSender.send(mail);
    }
}
```

---

## Notificaciones Push (Firebase)

### Configuración

1. **Crear proyecto en Firebase Console**
   - https://console.firebase.google.com

2. **Descargar credenciales**
   - Configuración del proyecto → Cuentas de servicio
   - Generar nueva clave privada → `serviceAccountKey.json`

3. **Colocar en recursos**
   ```
   src/main/resources/serviceAccountKey.json
   ```

4. **Configurar URL Realtime Database** (opcional)
   ```properties
   firebase.database.url=https://tu-proyecto.firebasedatabase.app/
   ```

### Servicio: `FirebaseNotificationService.java`

**Método:**

```java
public void enviarPush(String fcmToken, String titulo, String cuerpo)
```

### Uso

```java
@Service
public class FirebaseNotificationService {
    public void enviarPush(String fcmToken, String titulo, String cuerpo) {
        Message message = Message.builder()
            .setToken(fcmToken)
            .setNotification(Notification.builder()
                .setTitle(titulo)
                .setBody(cuerpo)
                .build())
            .build();
        FirebaseMessaging.getInstance().send(message);
    }
}
```

---

## Integración con Events

### Eventos Disparadores

| Evento | Listener | Acción |
|--------|----------|--------|
| `CitaCreadaEvent` | `CitaEventListener` | Email + Push |
| `CitaCanceladaEvent` | `CitaEventListener` | Email + Push |
| `CitaRecordatorioEvent` | `CitaEventListener` | Email |
| `UsuarioRegistradoEvent` | `UsuarioRegistradoListener` | Email |
| `MensajeNuevoEvent` | `MensajeEventListener` | Push (si offline) |

### Ejemplo: `CitaEventListener.java`

```java
@Component
@Log4j
public class CitaEventListener {
    
    @TransactionalEventListener(phase = TransactionalEventListenerPhase.AFTER_COMMIT)
    public void onCitaCreada(CitaCreadaEvent event) {
        Cita cita = event.getCita();
        
        // Notificar paciente
        emailService.enviarCitaCreada(pacienteEmail, pacienteNombre, cita);
        
        // Notificar psicólogo
        emailService.enviarCitaCreada(psicologoEmail, psicologoNombre, cita);
    }
}
```

---

## Configuración Mobile (Android)

### 1. Agregar dependencia

```gradle
implementation 'com.google.firebase:firebase-messaging-ktx:24.0.0'
```

### 2. Configurar AndroidManifest.xml

```xml
<service android:name=".MyFirebaseMessagingService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

### 3. Implementar Service

```kotlin
class MyFirebaseMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        val notification = remoteMessage.notification
        val title = notification?.title ?: "Amani"
        val body = notification?.body ?: ""
        
        // Mostrar notificación
        showNotification(title, body)
    }
    
    override fun onNewToken(token: String) {
        // Enviar token a tu backend
        sendTokenToBackend(token)
    }
}
```

### 4. Enviar token al backend

Cuando el token se obtiene o cambia:

```kotlin
FirebaseMessaging.getInstance().token.addOnCompleteListener { task ->
    if (task.isSuccessful) {
        val token = task.result
        // Enviar a tu API
        api.updateFcmToken(token)
    }
}
```

---

## Checklist de Configuración

### Email

- [ ] Servidor SMTP configurado (o usar Mailtrap/EmailTrap para desarrollo)
- [ ] `spring.mail.host` y `port` correctos
- [ ] `EmailService` inyectado en los services necesarios

### Firebase

- [ ] Proyecto Firebase creado
- [ ] `serviceAccountKey.json` en `src/main/resources/`
- [ ] `firebase.database.url` configurado (opcional)
- [ ] Firebase Admin SDK en dependencies (ya incluido en `pom.xml`)

---

## Testing

### Email local (sin SMTP real)

Usar **Mailtrap** o **Mailhog**:

```bash
docker run -p 1025:1025 -p 8025:8025 mailhog/mailhog
```

- SMTP: `localhost:1025`
- Web UI: `http://localhost:8025`

### Firebase (simulación)

Firebase no requiere configuración real para que la app arranque. Los intentos de envío se loguearán con `log.debug()` si no hay token.

---

## Logs de Notificaciones

### Email

```
INFO  [SMTP] Enviando email a: usuario@ejemplo.com
INFO  [SMTP] Email enviado exitosamente
```

### Firebase

```
INFO  [FCM] Notificación enviada. messageId=123456789
ERROR [FCM] Error enviando notificación: Token invalido
DEBUG [FCM] Token vacío — notificación omitida
```

---

## Troubleshooting

### Email no llega

- Verificar `spring.mail.host` y `port`
- Verificar credenciales SMTP
- Revisar logs de Spring: `logging.level.org.springframework.mail=DEBUG`

### Push no llega

- Verificar `serviceAccountKey.json` en classpath
- Verificar `fcmToken` no está vacío
- Verificar token no ha expirado
- Verificar logs de Firebase Admin SDK

### Error: "FirebaseApp no inicializado"

El bean de `FirebaseApp` solo se crea si existe `serviceAccountKey.json`. Si el archivo falta, Firebase no estará disponible (pero la app arranca).
