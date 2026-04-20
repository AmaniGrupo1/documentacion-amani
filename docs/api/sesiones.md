# Sesiones

Registro de sesiones de terapia.

---

## Endpoints (Paciente)

### GET /api/sesiones

Lista sesiones del paciente.

**Response 200:**

```json
[
  {
    "idSesion": 1,
    "idCita": 1,
    "idPaciente": 1,
    "idPsicologo": 2,
    "sessionDate": "2024-04-20T10:00:00",
    "durationMinutes": 50,
    "notas": "Sesión inicial de terapia",
    "recomendaciones": "Practicar respiración profunda"
  }
]
```

---

### POST /api/sesiones

Crea una nueva sesión.

**Request:**

```json
{
  "idCita": 1,
  "sessionDate": "2024-04-20T10:00:00",
  "durationMinutes": 50,
  "notas": "Sesión inicial de terapia",
  "recomendaciones": "Practicar respiración profunda"
}
```

**Response 201:** Sesión creada

---

### GET /api/sesiones/{id}

Obtiene una sesión por ID.

**Response 200:** Datos de la sesión

---

### DELETE /api/sesiones/{id}

Elimina una sesión.

**Response 204:** No Content

---

## Endpoints Psicólogo

### GET /api/psicologo/sesiones

Lista sesiones del psicólogo.

**Response 200:** Lista de sesiones

---

### POST /api/psicologo/sesiones

Crea sesión para paciente.

**Request:** Mismo formato que paciente

---

## Endpoints Admin

### GET /api/admin/sesiones

Lista todas las sesiones.

**Response 200:** Lista completa

---

## Checklist

- [ ] Validar `idCita` exista
- [ ] Validar `durationMinutes` > 0
- [ ] Validar `idPsicologo` y `idPaciente` existan
