# Situaciones

Catálogo de situaciones (contexto del paciente).

---

## 📋 Endpoints

### GET /api/situaciones

Lista todas las situaciones (público).

**Response 200:**

```json
[
  {
    "idSituacion": 1,
    "nombre": "Duelo",
    "categoria": "Emocional",
    "descripcion": "Proceso de luto por pérdida",
    "activo": true
  },
  {
    "idSituacion": 2,
    "nombre": "Ansiedad",
    "categoria": "Trastorno",
    "descripcion": "Trastorno de ansiedad generalizada",
    "activo": true
  }
]
```

---

## 🔐 Endpoints Admin

### POST /api/admin/situaciones

Crea una nueva situación.

**Request:**

```json
{
  "nombre": "Depresión",
  "categoria": "Trastorno",
  "descripcion": "Trastorno depresivo mayor"
}
```

**Response 201:** Situación creada

---

### PUT /api/admin/situaciones/{id}

Actualiza una situación.

**Request:**

```json
{
  "nombre": "Depresión Mayor",
  "categoria": "Trastorno",
  "descripcion": "Trastorno depresivo mayor severo",
  "activo": true
}
```

**Response 200:** Situación actualizada

---

### DELETE /api/admin/situaciones/{id}

Elimina una situación.

**Response 204:** No Content

---

## 📋 Categorías Comunes

| Categoría | Situaciones |
|-----------|-------------|
| `Emocional` | Duelo, soledad, estrés |
| `Trastorno` | Ansiedad, depresión, T.O.C. |
| `Relacional` | Pareja, familia, amistades |
| `Laboral` | Estrés laboral, desempleo |

---

## 📋 Checklist

- [ ] Validar `nombre` único
- [ ] Validar `categoria` en enum
