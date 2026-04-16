# Autenticación

Endpoints para login, registro y gestión de tokens JWT.

---

## 🔐 Endpoints

### POST /auth/login

Inicia sesión con credenciales y devuelve token JWT.

**Request:**

```json
{
  "email": "admin@amani.com",
  "password": "admin1234"
}
```

**Response 200:**

```json
{
  "idUsuario": 1,
  "nombre": "Admin",
  "rol": "ADMIN",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "idPsicologo": null,
  "idPaciente": null
}
```

**Errors:**
- `401` — Credenciales inválidas
- `404` — Usuario no encontrado

---

### POST /auth/register-paciente

Registra un nuevo paciente.

**Request:**

```json
{
  "usuario": {
    "nombre": "Juan",
    "apellido": "Pérez",
    "email": "juan@example.com",
    "password": "contraseña123",
    "dni": "12345678"
  },
  "fechaNacimiento": "1990-01-15",
  "genero": "Masculino",
  "telefono": "600123456",
  "direccion": [
    {
      "calle": "Calle 123",
      "ciudad": "Madrid",
      "provincia": "Madrid",
      "codigoPostal": "28001",
      "pais": "España"
    }
  ],
  "metodoPago": "PRESENCIAL",
  "aceptaTerminos": true,
  "aceptaVideoconferencia": true,
  "aceptaComunicacion": true
}
```

**Response 200:** Mismo formato que login

**Errors:**
- `400` — Datos inválidos
- `409` — Email ya registrado

---

### POST /auth/register-admin

Registra un nuevo administrador.

**Request:**

```json
{
  "nombre": "Nuevo Admin",
  "apellido": "Apellido",
  "email": "admin@amani.com",
  "password": "contraseña123"
}
```

**Response 200:** Mismo formato que login

**Errors:**
- `400` — Datos inválidos
- `409` — Email ya registrado
- `401` — No autenticado (requiere rol ADMIN)

---

### POST /auth/register-psicologo

Registra un nuevo psicólogo.

**Request:**

```json
{
  "nombre": "Psicólogo",
  "apellido": "Apellido",
  "email": "psicologo@amani.com",
  "password": "contraseña123",
  "especialidad": "Psicología Clínica",
  "experiencia": 5,
  "descripcion": "Psicólogo con experiencia en terapia cognitiva"
}
```

**Response 200:** Mismo formato que login

**Errors:**
- `400` — Datos inválidos
- `409` — Email ya registrado
- `401` — No autenticado (requiere rol ADMIN)

---

### GET /auth/admins

Lista todos los administradores.

**Response 200:**

```json
[
  {
    "idUsuario": 1,
    "nombre": "Admin",
    "apellido": "Principal",
    "email": "admin@amani.com",
    "rol": "ADMIN",
    "activo": true
  }
]
```

**Errors:**
- `401` — No autenticado (requiere rol ADMIN)

---

### PUT /auth/pacientes/{id}/baja

Da de baja a un paciente.

**Path:**
- `{id}` — ID del paciente

**Response 200:**

```
Paciente dado de baja correctamente
```

**Errors:**
- `401` — No autenticado
- `404` — Paciente no encontrado

---

## 🔑 JWT Token

### Estructura del Token

```
{
  "sub": "email@usuario.com",
  "rol": "ADMIN",
  "iat": 1713160000,
  "exp": 1713246400
}
```

### Caducidad

- **24 horas** (86400000 ms configurable)

### Revocación

Actualmente no hay mecanismo de revocación. El token es válido hasta su expiración.

---

## 🧪 Ejemplo con cURL

```bash
# Login
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@amani.com","password":"admin1234"}'

# Request protegido
curl -X GET http://localhost:8080/api/psicologo \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

---

## 📋 Checklist de Autenticación

- [ ] `jwt.secret` configurado en `application.properties`
- [ ] `jwt.expiration` configurado
- [ ] `BCryptPasswordEncoder` en `SecurityConfig`
- [ ] `JwtAuthFilter` añadido al filter chain
- [ ] `UserDetailsServiceImpl` implementado

---

## 🚨 Seguridad

### Recomendaciones de Producción

1. **JWT Secret:** Usar clave de al menos 256 bits
2. **HTTPS:** Siempre usar HTTPS en producción
3. **CORS:** Configurar orígenes permitidos explícitamente
4. **Rate Limiting:** Implementar para evitar brute force
5. **Password Policy:** Forzar contraseñas fuertes
6. **Token Rotation:** Considerar refresh tokens
