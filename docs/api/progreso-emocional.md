# Progreso Emocional

Estadísticas y seguimiento del progreso emocional.

---

## Endpoints (Paciente)

### GET /api/progreso-emocional

Lista progreso emocional del paciente.

**Response 200:**

```json
[
  {
    "idProgress": 1,
    "idPaciente": 1,
    "fecha": "2024-04-15",
    "nivelEstres": 5,
    "nivelAnsiedad": 6,
    "nivelAnimo": 7
  }
]
```

---

### POST /api/progreso-emocional

Crea un nuevo registro de progreso.

**Request:**

```json
{
  "fecha": "2024-04-15",
  "nivelEstres": 5,
  "nivelAnsiedad": 6,
  "nivelAnimo": 7
}
```

**Response 201:** Progreso creado

**Validaciones:**
- `fecha` — date (YYYY-MM-DD)
- `nivelEstres` — int (0-10)
- `nivelAnsiedad` — int (0-10)
- `nivelAnimo` — int (0-10)

---

## Endpoints Psicólogo

### GET /api/psicologo/progreso-emocional

Lista progreso de pacientes.

**Query:**
- `idPaciente` — ID del paciente

**Response 200:** Lista de progresos

---

## Endpoints Admin

### GET /api/admin/progreso-emocional

Lista todos los registros de progreso.

**Response 200:** Lista completa

---

## Uso en Psicología

**Objetivo:** Medir cambios en niveles de:
- **Estrés** — nivel de presión/estrés percibido
- **Ansiedad** — nivel de ansiedad general
- **Ánimo** — estado de ánimo general

**Frecuencia:** Semanal o mensual.

---

## Checklist

- [ ] Validar fechas (no futuras)
- [ ] Validar niveles (0-10)
- [ ] Guardar `idPaciente` del token JWT
