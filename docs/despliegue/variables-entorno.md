# Variables de entorno

Crea un archivo `.env` en la raíz de `amani-apirest/` con las siguientes variables:

```env
# Base de datos
DB_URL=jdbc:postgresql://postgres:5432/amani
DB_USERNAME=amani
DB_PASSWORD=amani

# JWT
JWT_SECRET=tu_clave_secreta_muy_larga

# Firebase FCM
FIREBASE_CREDENTIALS_PATH=/app/serviceAccountKey.json

# Email (producción — Resend)
MAIL_HOST=smtp.resend.com
MAIL_PORT=465
MAIL_USERNAME=resend
MAIL_PASSWORD=re_xxxxxxxxxxxx

# Email (desarrollo — Mailpit)
# MAIL_HOST=mailpit
# MAIL_PORT=1025
```

> ⚠️ El archivo `.env` nunca debe subirse a Git. Asegúrate de que está en `.gitignore`.

## Credenciales de Firebase

Descarga el archivo `serviceAccountKey.json` desde la consola de Firebase y colócalo en:

```
amani-apirest/
└── src/main/resources/
    └── serviceAccountKey.json   ← también en .gitignore
```
