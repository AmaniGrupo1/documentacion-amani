# Administración

Endpoints exclusivos para administradores.

---

## 📋 Endpoints

### GET /api/admin/psicologos

Lista todos los psicólogos.

**Response 200:**

```json
[
  {
    "idPsicologo": 1,
    "idUsuario": 3,
    "nombre": "Dr. Juan",
    "apellido": "Médico",
    "email": "juan@amani.com",
    "especialidad": "Psicología Clínica"
  }
]
```

---

### POST /api/admin/psicologos/create

Crea un nuevo psicólogo.

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

---

### PUT /api/admin/psicologos/{id}

Actualiza un psicólogo.

**Request:**

```json
{
  "especialidad": "Psicología Clínica",
  "experiencia": 12
}
```

**Response 200:** Psicólogo actualizado

---

### DELETE /api/admin/psicologos/{id}

Elimina un psicólogo.

**Response 204:** No Content

---

### POST /api/admin/psicologos/asignar-psicologo

Asigna paciente a psicólogo.

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

---

### GET /api/admin/psicologos/pacientes

Lista psicólogos con pacientes.

**Response 200:**

```json
[
  {
    "idPsicologo": 1,
    "nombrePsicologo": "Dr. Juan",
    "pacientes": [
      { "idPaciente": 1, "nombrePaciente": "Juan Pérez" }
    ]
  }
]
```

---

## 📋 Endpoints Admin (Otro Rol)

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
    "fechaNacimiento": "1990-01-15"
  }
]
```

---

### GET /api/admin/historial-clinico

Lista historiales clínicos (admin).

**Response 200:** Lista de historiales

---

### GET /api/admin/diario-emocional

Lista diarios emocionales (admin).

**Response 200:** Lista de diarios

---

### GET /api/admin/progreso-emocional

Lista progreso emocional (admin).

**Response 200:** Lista de progresos

---

### GET /api/admin/mensajes

Lista mensajes (admin).

**Response 200:** Lista de mensajes

---

### GET /api/admin/sesiones

Lista sesiones (admin).

**Response 200:** Lista de sesiones

---

## 🔐 Roles

- `ADMIN` — Acceso completo
- `PSICOLOGO` — Acceso limitado a su consultorio
