# Mensajes

Chat y mensajería en tiempo real entre usuarios.

---

## 📋 Endpoints (Paciente)

### GET /api/mensajes

Lista mensajes del paciente.

**Response 200:**

```json
[
  {
    "idMensaje": 1,
    "senderId": 2,
    "receiverId": 3,
    "idCita": 1,
    "mensaje": "Hola, buenas tardes",
    "enviadoEn": "2024-04-20T10:00:00",
    "leido": true
  }
]
```

---

### POST /api/mensajes

Envía un nuevo mensaje.

**Request:**

```json
{
  "receiverId": 3,
  "mensaje": "Hola, buenas tardes",
  "idCita": 1
}
```

**Response 201:** Mensaje enviado

**Validaciones:**
- `receiverId` — ID del destinatario
- `mensaje` — texto (obligatorio)

---

## 🔐 Endpoints Psicólogo

### GET /api/psicologo/mensajes

Lista mensajes del psicólogo.

**Response 200:** Lista de mensajes

---

### POST /api/psicologo/mensajes

Envía mensaje a paciente.

**Request:** Mismo formato que paciente

---

## 📋 WebSocket (En tiempo real)

### Destino para suscripción

```
/topic/mensajes
```

### Enviar mensaje

```javascript
stompClient.send('/app/chat', {}, JSON.stringify({
  receiverId: 123,
  mensaje: 'Hola!',
  enviadoEn: new Date().toISOString()
}));
```

---

## 📋 Endpoints Admin

### GET /api/admin/mensajes

Lista todos los mensajes.

**Response 200:** Lista completa

---

## 📋 Checklist

- [ ] Validar `receiverId` exista
- [ ] Validar `mensaje` no vacío
- [ ] Guardar `senderId` del token JWT
