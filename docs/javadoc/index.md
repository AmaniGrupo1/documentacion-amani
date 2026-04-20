# Backend (JavaDoc)

Documentación generada por Dokka Maven del backend Java.

---

## Generar la documentación

Desde la raíz del proyecto `amani-apirest`:

```bash
./mvnw dokka:dokka
```

La salida se genera en `target/dokka/`.

---

## Paquetes principales

| Paquete | Contenido |
|---------|-----------|
| `com.amani.amaniapirest.configuration` | Seguridad, JWT, WebSocket, Firebase |
| `com.amani.amaniapirest.controllers` | Controladores REST por rol |
| `com.amani.amaniapirest.services` | Lógica de negocio |
| `com.amani.amaniapirest.repository` | Repositorios JPA |
| `com.amani.amaniapirest.models` | Entidades y DTOs |

---

## Acceso

Si el backend está ejecutándose localmente, la documentación interactiva de la API (OpenAPI/Swagger) está disponible en:

```
http://localhost:8080/docs
```
