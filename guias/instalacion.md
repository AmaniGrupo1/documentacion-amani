# Instalación

Guía para instalar y configurar Amani localmente.

---

## 📋 Requisitos Previos

| Herramienta | Versión | Comando |
|-------------|---------|---------|
| Java | 21+ | `java --version` |
| Maven | 3.9+ | `mvn --version` |
| PostgreSQL | 14+ | `psql --version` |
| Git | - | `git --version` |

---

## 1️⃣ Clonar Repositorio

```bash
git clone https://github.com/AmaniGrupo1/amani-apirest.git
cd amani-apirest
```

---

## 2️⃣ Configurar Base de Datos

### Opción A: Usar Docker Compose (recomendado)

```bash
docker-compose up -d postgres
```

### Opción B: Instalación manual

```bash
# Crear base de datos
psql -U postgres -c "CREATE DATABASE psicologia_app;"

# Ejecutar script SQL
psql -U postgres -d psicologia_app -f src/main/resources/amani.sql
```

---

## 3️⃣ Configurar Variables de Entorno

### Archivo: `src/main/resources/application.properties`

```properties
# Database
spring.datasource.url=jdbc:postgresql://localhost:5433/psicologia_app
spring.datasource.username=postgres
spring.datasource.password=Sandia4you

# JWT
jwt.secret=tu-segredo-jwt-aqui-minimo-256-bits
jwt.expiration=86400000

# Firebase (opcional - para notificaciones push)
firebase.database.url=https://tu-proyecto.firebasedatabase.app/

# Email (SMTP)
spring.mail.host=localhost
spring.mail.port=1025
spring.mail.username=noreply@apppsicologas.com

# Upload
file.upload-dir=uploads
```

> **Nota:** El archivo de credenciales Firebase (`serviceAccountKey.json`) es opcional.

---

## 4️⃣ Compilar y Ejecutar

### Con Maven Wrapper

```bash
./mvnw spring-boot:run
```

### Con Maven global

```bash
mvn spring-boot:run
```

### Como JAR

```bash
./mvnw clean package -DskipTests
java -jar target/amaniApirest-0.0.1-SNAPSHOT.jar
```

---

## 5️⃣ Verificar

La aplicación estará disponible en:

- **API REST:** `http://localhost:8080`
- **Swagger UI:** `http://localhost:8080/docs`
- **API Docs (JSON):** `http://localhost:8080/v3/api-docs`

### Endpoint de inicio (admin por defecto)

| Email | Contraseña |
|-------|------------|
| `admin@amani.com` | `admin1234` |

---

## 🔧 Comandos Útiles

### Limpiar proyecto

```bash
./mvnw clean
```

### Ejecutar tests

```bash
./mvnw test
```

### Generar informe de calidad

```bash
./mvnw site
bash generate-pdf-report.sh
```

### Compilar sin tests

```bash
./mvnw clean package -DskipTests
```

---

## 🐳 Despliegue con Docker

### Archivo: `Dockerfile`

```dockerfile
FROM openjdk:21-jdk-slim
COPY target/amaniApirest-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### Docker Compose

Ver: `docker-compose.yml` en repositorio.

```bash
docker-compose up -d
```

---

## 🚨 Problemas Comunes

### Error: "Falta el esquema psicologia_app"

```bash
psql -U postgres -d psicologia_app -f src/main/resources/amani.sql
```

### Error: "JWT secret not configured"

Configurar `jwt.secret` en `application.properties`.

### Error: "Database connection refused"

Verificar PostgreSQL esté corriendo y las credenciales sean correctas.

---

## 📋 Checklist de Instalación

- [ ] Java 21 instalado
- [ ] PostgreSQL ejecutándose (puerto 5433)
- [ ] Base de datos `psicologia_app` creada
- [ ] Script SQL ejecutado
- [ ] `application.properties` configurado
- [ ] `./mvnw spring-boot:run` funciona
- [ ] Swagger UI visible en `/docs`
