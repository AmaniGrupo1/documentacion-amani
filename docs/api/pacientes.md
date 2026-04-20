# Pacientes

Gestión de pacientes y sus datos personales.

---

## Endpoints

### GET /api/pacientes/admin

Lista todos los pacientes.

**Response 200:**

```json
[
  {
    "idPaciente": 1,
    "idUsuario": 2,
    "nombre": "Juan",
    "apellido": "Pérez",
    "email": "juan@example.com",
    "fechaNacimiento": "1990-01-15",
    "genero": "Masculino",
    "telefono": "600123456",
    "metodoPago": "PRESENCIAL",
    "estadoPago": "PENDIENTE"
  }
]
```

---

### POST /api/pacientes

Crea un nuevo paciente (admin).

**Request:**

```json
{
  "idUsuario": 2,
  "idPsicologo": 3,
  "fechaNacimiento": "1990-01-15",
  "genero": "Masculino",
  "telefono": "600123456",
  "metodoPago": "PRESENCIAL"
}
```

**Response 201:** Paciente creado.

**Errores:**

- `400` — Datos inválidos
- `401` — No autenticado

---

### GET /api/pacientes/{id}

Obtiene un paciente por ID.

**Response 200:** Datos del paciente.

**Errores:**

- `404` — Paciente no encontrado

---

### PUT /api/pacientes/{id}

Actualiza datos de un paciente.

**Request:**

```json
{
  "idPsicologo": 3,
  "fechaNacimiento": "1990-01-15",
  "genero": "Masculino",
  "telefono": "600123456",
  "metodoPago": "ONLINE"
}
```

**Response 200:** Paciente actualizado.

**Errores:**

- `400` — Datos inválidos
- `404` — Paciente no encontrado

---

### DELETE /api/pacientes/{id}

Elimina un paciente.

**Response 204:** No Content.

**Errores:**

- `404` — Paciente no encontrado

---

## Relaciones

### Psicólogo asignado

```json
{
  "idPaciente": 1,
  "idPsicologo": 3
}
```

### Direcciones

```json
[
  {
    "calle": "Calle 123",
    "ciudad": "Madrid",
    "provincia": "Madrid",
    "codigoPostal": "28001",
    "pais": "España"
  }
]
```

---

## Checklist

- [ ] Validar `fechaNacimiento` (mayor de edad)
- [ ] Validar `metodoPago` (ENUM)
- [ ] Validar `estadoPago` (ENUM)
- [ ] Validar `idPsicologo` (exista)
