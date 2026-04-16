# Ajustes

Preferencias de usuario.

---

## 📋 Endpoints

### GET /api/ajustes

Obtiene los ajustes del usuario.

**Response 200:**

```json
{
  "idAjuste": 1,
  "idUsuario": 1,
  "idioma": "es",
  "notificaciones": true,
  "darkMode": false,
  "timezone": "Europe/Madrid"
}
```

---

### PUT /api/ajustes

Actualiza ajustes del usuario.

**Request:**

```json
{
  "idioma": "es",
  "notificaciones": true,
  "darkMode": true,
  "timezone": "Europe/Madrid"
}
```

**Response 200:** Ajustes actualizados

---

## 🔐 Endpoints Admin

### GET /api/admin/ajustes

Lista todos los ajustes.

**Response 200:** Lista completa

---

## 📋 Opciones de Configuración

| Opción | Valores | Descripción |
|--------|---------|-------------|
| `idioma` | `es`, `en` | Idioma de la interfaz |
| `notificaciones` | `true`, `false` | Habilitar notificaciones |
| `darkMode` | `true`, `false` | Modo oscuro |
| `timezone` | TZ DB zones | Zona horaria |

---

## 📋 Checklist

- [ ] Guardar `idUsuario` del token JWT
- [ ] Validar `timezone` (formato TZ DB)
