# Documentación de la API REST Amani

Bienvenido a la documentación completa de la API REST de Amani.

## 📚 Documentación

- **[API REST](docs/api/index.md)** - Documentación completa de todos los endpoints
- **[Arquitectura](docs/arquitectura/index.md)** - Diseño del sistema y patrones
- **[Guías](docs/guias/index.md)** - Tutoriales y guías de uso

## 🔌 Endpoints Rápidos

### Autenticación
- `POST /auth/login` - Iniciar sesión
- `POST /auth/register-paciente` - Registrar paciente
- `POST /auth/register-admin` - Registrar administrador

### Gestión de Pacientes
- `GET /api/pacientes` - Listar pacientes
- `POST /api/pacientes` - Crear paciente

### Citas
- `GET /api/citas` - Listar citas
- `POST /api/citas` - Crear cita

---

## 🛠️ Generar Documentación

La documentación se genera automáticamente desde el OpenAPI spec usando MkDocs.

### Requisitos

- Python 3.8+
- MkDocs
- Backend Spring Boot corriendo

### Pasos

1. **Levantar el backend:**
   ```bash
   cd ~/amani-apirest
   mvn spring-boot:run
   ```

2. **Generar la documentación:**
   ```bash
   cd ~/documentacion-amani
   ./scripts/docs.sh generate-docs
   ```

3. **O en dos pasos (descargar spec + generar docs):**
   ```bash
   ./scripts/docs.sh fetch-spec
   ./scripts/docs.sh generate-docs
   ```

4. **Construir el sitio:**
   ```bash
   ./scripts/docs.sh build
   ```

5. **Servir localmente (para desarrollo):**
   ```bash
   ./scripts/docs.sh serve
   ```

---

## 📖 Estructura de Documentación

```
~/documentacion-amani/
├── docs/
│   ├── api/              # Documentación OpenAPI (auto-generada)
│   │   ├── index.md      # Índice de endpoints
│   │   ├── auth.md
│   │   ├── pacientes.md
│   │   ├── psicologos.md
│   │   ├── admin.md
│   │   ├── citas.md
│   │   ├── diario-emocional.md
│   │   ├── progreso-emocional.md
│   │   ├── historial-clinico.md
│   │   ├── sesiones.md
│   │   ├── mensajes.md
│   │   ├── ajustes.md
│   │   ├── direcciones.md
│   │   └── situaciones.md
│   ├── arquitectura/     # Arquitectura del sistema
│   ├── guias/            # Tutoriales
│   └── javadoc/          # JavaDoc generado
├── mkdocs.yml            # Configuración MkDocs
└── scripts/              # Scripts de generación
    └── generate_docs.py
```

---

## 📝 Notas

- Los endpoints de la API REST ya tienen documentación OpenAPI (`@Operation`, `@ApiResponse`) en los controllers.
- La documentación se actualiza automáticamente al ejecutar `generate_docs.py`.
- Para contribuir a la documentación, editar los archivos en `docs/` o mejorar los comentarios JavaDoc en los controllers.

---

## 📄 Licencia

MIT - Amani Grupo 1
