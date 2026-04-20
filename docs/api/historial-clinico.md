# Historial Clínico

Historial médico y diagnósticos del paciente.

---

## Endpoints (Paciente)

### GET /api/historial-clinico

Lista historial clínico del paciente.

**Response 200:**

```json
[
  {
    "idHistory": 1,
    "idPaciente": 1,
    "titulo": "Tratamiento Ansiedad",
    "diagnostico": "Trastorno de ansiedad generalizada",
    "tratamiento": "Terapia cognitivo-conductual",
    "observaciones": "Mejoría progresiva",
    "creadoEn": "2024-01-15T10:00:00"
  }
]
```

---

### POST /api/historial-clinico

Crea un nuevo registro de historial.

**Request:**

```json
{
  "titulo": "Tratamiento Ansiedad",
  "diagnostico": "Trastorno de ansiedad generalizada",
  "tratamiento": "Terapia cognitivo-conductual",
  "observaciones": "Mejoría progresiva"
}
```

**Response 201:** Historial creado

---

### GET /api/historial-clinico/{id}

Obtiene un historial por ID.

**Response 200:** Datos del historial

---

### DELETE /api/historial-clinico/{id}

Elimina un historial.

**Response 204:** No Content

---

## Endpoints Psicólogo

### GET /api/psicologo/historial-clinico

Lista historial clínico de pacientes.

**Query:**
- `idPaciente` — ID del paciente

**Response 200:** Lista de historiales

---

### POST /api/psicologo/historial-clinico

Crea historial para paciente.

**Request:** Mismo formato que paciente

---

## Endpoints Admin

### GET /api/admin/historial-clinico

Lista todos los historiales clínicos.

**Response 200:** Lista completa

---

## Checklist

- [ ] Validar `idPaciente` exista
- [ ] Validar `titulo` no vacío
- [ ] Guardar `idPaciente` del token JWT
