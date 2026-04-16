# Citas

Gestión de citas, agendas y disponibilidad.

---

## 📋 Endpoints (Paciente)

### GET /api/citas

Lista todas las citas del paciente.

**Response 200:**

```json
[
  {
    "idCita": 1,
    "idPaciente": 1,
    "idPsicologo": 2,
    "nombrePsicologo": "Dr. Juan",
    "startDatetime": "2024-04-20T10:00:00",
    "durationMinutes": 50,
    "estado": "CONFIRMADA",
    "motivo": "Sesión de terapia mensual"
  }
]
```

---

### GET /api/citas/{id}

Obtiene una cita por ID.

**Response 200:** Datos de la cita

**Errors:**
- `404` — Cita no encontrada

---

### POST /api/citas

Crea una nueva cita.

**Request:**

```json
{
  "idPsicologo": 2,
  "startDatetime": "2024-04-20T10:00:00",
  "durationMinutes": 50,
  "motivo": "Sesión de terapia inicial"
}
```

**Response 201:** Cita creada

**Errors:**
- `400` — Datos inválidos
- `409` — Conflito de agenda

---

### PUT /api/citas/{id}

Actualiza una cita existente.

**Request:**

```json
{
  "startDatetime": "2024-04-21T11:00:00",
  "motivo": "Sesión de terapia modificada"
}
```

**Response 200:** Cita actualizada

**Errors:**
- `400` — Datos inválidos
- `404` — Cita no encontrada

---

### DELETE /api/citas/{id}

Elimina una cita.

**Response 204:** No Content

**Errors:**
- `404` — Cita no encontrada

---

### GET /api/citas/paciente/{idPaciente}/agenda

Obtiene la agenda del paciente para un mes.

**Query:**
- `month` — formato `YYYY-MM`

**Response 200:**

```json
[
  {
    "fecha": "2024-04-20",
    "horaInicio": "10:00",
    "horaFin": "10:50",
    "tipo": "cita",
    "detalle": "CONFIRMADA",
    "referenciaId": 1
  },
  {
    "fecha": "2024-04-22",
    "horaInicio": "00:00",
    "horaFin": "23:59",
    "tipo": "bloqueo",
    "detalle": "Día bloqueado",
    "referenciaId": 5
  }
]
```

---

## 🔐 Endpoints Psicólogo

### GET /api/psicologo/citas

Lista citas del psicólogo.

**Response 200:** Lista de citas

---

### GET /api/psicologo/citas/{id}

Obtiene una cita específica.

**Response 200:** Datos de la cita

---

### PUT /api/psicologo/citas/{id}

Confirma/actualiza cita.

**Request:**

```json
{
  "estado": "CONFIRMADA"
}
```

---

## 📋 Endpoints Admin

### GET /api/admin/citas

Lista todas las citas (admin).

**Response 200:** Lista de todas las citas

---

### GET /api/admin/citas/{id}

Obtiene una cita (admin).

**Response 200:** Datos de la cita

---

## 📋 Estados de Cita

| Estado | Descripción |
|--------|-------------|
| `PENDIENTE` | Cita creada, sin confirmar |
| `CONFIRMADA` | Cita aceptada por psicólogo |
| `CANCELADA` | Cita cancelada |
| `COMPLETADA` | Cita realizada |

---

## 📋 Checklist

- [ ] Validar `startDatetime` >= ahora
- [ ] Validar `durationMinutes` > 0
- [ ] Detectar conflictos de agenda
- [ ] Notificar por email al crear/cancelar
