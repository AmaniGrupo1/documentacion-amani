# Direcciones

Direcciones del paciente.

---

## Endpoints (Paciente)

### GET /api/direcciones

Lista direcciones del paciente.

**Response 200:**

```json
[
  {
    "idDireccion": 1,
    "idPaciente": 1,
    "calle": "Calle 123",
    "ciudad": "Madrid",
    "provincia": "Madrid",
    "codigoPostal": "28001",
    "pais": "España",
    "descripcion": "Casa"
  }
]
```

---

### POST /api/direcciones

Crea una nueva dirección.

**Request:**

```json
{
  "calle": "Calle 123",
  "ciudad": "Madrid",
  "provincia": "Madrid",
  "codigoPostal": "28001",
  "pais": "España",
  "descripcion": "Casa"
}
```

**Response 201:** Dirección creada

---

### GET /api/direcciones/{id}

Obtiene una dirección por ID.

**Response 200:** Datos de la dirección

---

### DELETE /api/direcciones/{id}

Elimina una dirección.

**Response 204:** No Content

---

## Endpoints Psicólogo

### GET /api/psicologo/direcciones

Lista direcciones de pacientes.

**Query:**
- `idPaciente` — ID del paciente

**Response 200:** Lista de direcciones

---

## Endpoints Admin

### GET /api/admin/direcciones

Lista todas las direcciones.

**Response 200:** Lista completa

---

## Checklist

- [ ] Validar `idPaciente` exista
- [ ] Validar `calle` no vacío
- [ ] Validar `codigoPostal` formato
