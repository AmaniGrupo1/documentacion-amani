---
hide:
  - navigation
  - toc
---

# Plataforma Amani

Documentación técnica de la plataforma que conecta pacientes y psicólogos en un entorno seguro y unificado. Arquitectura, API REST, guías de integración y referencias.

<div class="grid cards" markdown>

- :material-api: **API REST**
  
  Endpoints, modelos de datos, autenticación JWT y recursos técnicos para consumir la plataforma.

  [Explorar la API :octicons-arrow-right-24:](api/index.md)

- :material-transit-connection-variant: **Arquitectura**
  
  Estructura de servicios, base de datos, comunicación en tiempo real (WebSockets) y patrones de diseño.

  [Ver arquitectura :octicons-arrow-right-24:](arquitectura/index.md)

- :material-book-open-page-variant: **Guías**
  
  Instalación del entorno local, Docker Compose, notificaciones push y compilación de aplicaciones.

  [Leer guías :octicons-arrow-right-24:](guias/index.md)

- :material-laptop: **Referencias**
  
  Documentación generada (KDoc/JavaDoc) para el cliente Android y el backend Java.

  [Ver referencias :octicons-arrow-right-24:](javadoc/index.md)

</div>

## Características principales

| Característica | Descripción |
|---|---|
| **Autenticación JWT** | Flujos separados para pacientes, psicólogos y administradores |
| **WebSockets** | Comunicación en tiempo real para chat y sesiones |
| **Diario emocional** | API para el seguimiento del progreso psicológico del paciente |
| **Infraestructura Docker** | Despliegue reproducible con Docker Compose |
