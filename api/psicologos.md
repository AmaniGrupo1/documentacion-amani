# Psicólogos

Gestión de psicólogos y su perfil profesional.

---

## 📋 Endpoints

### GET /api/psicologo

Lista todos los psicólogos (admin o psicólogo).

**Response 200:**

```json
[
  {
    "idPsicologo": 1,
    "idUsuario": 3,
    "nombre": "Dr. Juan",
    "apellido": "Médico",
    "email": "juan@amani.com",
    "especialidad": "Psicología Clínica",
    "experiencia": 10,
    "descripcion": "Especialista en terapia cognitiva",
    "licencia": "PSI123456"
  }
]
```

---

### POST /api/psicologo/create

Crea un nuevo psicólogo (admin).

**Request:**

```json
{
  "idUsuario": 3,
  "especialidad": "Psicología Clínica",
  "experiencia": 5,
  "descripcion": "Psicólogo con experiencia en terapia cognitiva",
  "licencia": "PSI123456"
}
```

**Response 201:** Psicólogo creado

**Errors:**
- `400` — Datos inválidos
- `401` — No autenticado

---

### GET /api/psicologo/{id}/perfil

Obtiene el perfil completo de un psicólogo.

**Response 200:**

```json
{
  "idPsicologo": 1,
  "idUsuario": 3,
  "nombre": "Dr. Juan",
  "apellido": "Médico",
  "email": "juan@amani.com",
  "especialidad": "Psicología Clínica",
  "experiencia": 10,
  "descripcion": "Especialista en terapia cognitiva",
  "licencia": "PSI123456",
  "fotoPerfilUrl": "/files/uuid.jpg"
}
```

---

### PUT /api/psicologo/{id}

Actualiza datos de un psicólogo.

**Request:**

```json
{
  "especialidad": "Psicología Clínica",
  "experiencia": 12,
  "descripcion": "Especialista en terapia cognitiva y terapia de pareja",
  "licencia": "PSI123456"
}
```

**Response 200:** Psicólogo actualizado

**Errors:**
- `400` — Datos inválidos
- `404` — Psicólogo no encontrado

---

### DELETE /api/psicologo/{id}

Elimina un psicólogo.

**Response 204:** No Content

**Errors:**
- `404` — Psicólogo no encontrado

---

### POST /api/admin/psicologos/asignar-psicologo

Asigna un paciente a un psicólogo (admin).

**Request:**

```json
{
  "idPaciente": 1,
  "idPsicologo": 2
}
```

**Response 200:**

```json
true
```

**Errors:**
- `400` — Datos inválidos
- `401` — No autenticado
- `404` — Paciente o psicólogo no encontrado

---

### GET /api/admin/psicologos/pacientes

Lista psicólogos con sus pacientes asignados.

**Response 200:**

```json
[
  {
    "idPsicologo": 1,
    "nombrePsicologo": "Dr. Juan Médico",
    "pacientes": [
      {
        "idPaciente": 1,
        "nombrePaciente": "Juan Pérez",
        "email": "juan@example.com"
      }
    ]
  }
]
```

---

## 📋 Checklist

- [ ] Validar `licencia` única
- [ ] Validar `experiencia` >= 0
- [ ] Validar relación `idUsuario` -> `rol = PSICOLOGO`
