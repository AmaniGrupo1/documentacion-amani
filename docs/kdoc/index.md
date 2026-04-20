# Android (KDoc)

Documentación generada por Dokka Gradle para la aplicación Android.

---

## Generar la documentación

Desde la raíz del proyecto Android:

```bash
./gradlew dokkaHtml
```

La salida se genera en `build/dokka/html/`.

---

## Módulos principales

| Módulo | Contenido |
|--------|-----------|
| `data.model` | DTOs y modelos de red |
| `data.network` | Retrofit, interceptores, cliente HTTP |
| `data.repository` | Repositorios de datos |
| `domain.usecase` | Casos de uso |
| `presentation.viewmodel` | ViewModels |
| `presentation.ui.screen` | Pantallas (Jetpack Compose) |
| `presentation.ui.componente` | Componentes reutilizables |

---

## Stack tecnológico

| Capa | Tecnología |
|------|------------|
| UI | Jetpack Compose |
| Diseño | Material Design 3 |
| Lenguaje | Kotlin |
| DI | Hilt |
| Navegación | Navigation Compose |
| Red | Retrofit + OkHttp |
| WebSocket | StompProtocolAndroid + RxJava |
| Imágenes | Coil |
