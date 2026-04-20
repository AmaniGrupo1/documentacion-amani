# Diario Emocional

Registro diario de emociones y estado de ánimo.

---

## Endpoints (Paciente)

### GET /api/diario-emocional

Lista diarios emocionales del paciente.

**Response 200:**

```json
[
  {
    "idDiario": 1,
    "idPaciente": 1,
    "fecha": "2024-04-15T10:30:00",
    "emocion": "Ansiedad",
    "intensidad": 7,
    "nota": "Ansiedad antes de la cita"
  }
]
```

---

### POST /api/diario-emocional

Crea un nuevo registro emocional.

**Request:**

```json
{
  "emocion": "Ansiedad",
  "intensidad": 7,
  "nota": "Ansiedad antes de la cita"
}
```

**Response 201:** Diario creado

**Validaciones:**
- `emocion` — string (ej: "Ansiedad", "Tristeza", "Alegría")
- `intensidad` — int entre 1 y 10
- `nota` — string (opcional)

---

### GET /api/diario-emocional/{id}

Obtiene un diario por ID.

**Response 200:** Datos del diario

---

### DELETE /api/diario-emocional/{id}

Elimina un diario.

**Response 204:** No Content

---

## Endpoints Psicólogo

### GET /api/psicologo/diario-emocional

Lista diarios emocionales de pacientes.

**Query:**
- `idPaciente` — ID del paciente

**Response 200:** Lista de diarios del paciente

---

## Endpoints Admin

### GET /api/admin/diario-emocional

Lista todos los diarios emocionales.

**Response 200:** Lista completa

---

## Uso en Psicología

**Objetivo:** Seguimiento del estado emocional del paciente.

**Frecuencia:** Recomendado diario o semanal.

**Emociones comunes:**
- Ansiedad
- Tristeza
- Alegría
- Irritabilidad
- Calma
- Frustración

---

## Checklist

- [ ] Validar `intensidad` (1-10)
- [ ] Validar `emocion` (enum o string)
- [ ] Guardar `idPaciente` del token JWT
