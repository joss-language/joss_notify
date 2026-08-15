# joss_notify 2.1.0

Plugin oficial de notificaciones Push (Firebase Cloud Messaging HTTP v1 con Service Account OAuth2), Webhooks e In-App para el lenguaje de programación Joss.

Empaquetado como un paquete `.jp` (JP v2) autocontenido, determinista y firmado con Ed25519. Se carga automáticamente en cualquier proyecto Joss que lo declare en `joss.yaml` o lo instale en el directorio `plugins/`.

---

## 🚀 Instalación

```bash
joss pub add joss_notify
```

O agrégalo en tu `joss.yaml`:

```yaml
dependencies:
  joss_notify: "^2.1.0"
```

---

## ⚙️ Configuración (.env)

El plugin detecta automáticamente la mejor estrategia de transporte configurada:

| Variable | Descripción |
| --- | --- |
| `FCM_CREDENTIALS_PATH` | Ruta al archivo JSON de credenciales de Firebase Service Account (ej: `storage/secrets/firebase-service-account.json`). Permite autenticación automática OAuth2 JWT RS256 para **FCM HTTP v1**. |
| `FIREBASE_CREDENTIALS` | Alias opcional para la ruta del Service Account. |
| `GOOGLE_APPLICATION_CREDENTIALS` | Estándar Google Cloud para Service Account. |
| `FCM_ACCESS_TOKEN` y `FCM_PROJECT_ID` | Envío directo a FCM HTTP v1 usando Bearer Token OAuth2 pre-generado. |
| `NOTIFY_WEBHOOK_URL` | URL de Webhook / Gateway personalizado para despachar payload JSON. |
| `FCM_URL` y `FCM_SERVER_KEY` | Endpoint y Server Key para pasarelas FCM Legacy / proxies personalizados. |

> **Nota de Seguridad**: Nunca incluyas archivos de credenciales (`firebase-service-account.json`) en paquetes cliente o repositorios públicos.

---

## 💻 Uso

### 1. Invocación Fluida Orientada a Objetos:

```joss
$notify = new Notify()
$res = $notify->app("joss_red")
    ->user($deviceToken)
    ->title("Nueva Alerta de Seguridad")
    ->message("Se ha detectado un nuevo inicio de sesión.")
    ->data({"source": "auth", "priority": "high"})
    ->send()

($res["ok"]) ? {
    print("Notificación enviada con éxito")
} : {
    print("Error enviando notificación: " . $notify->lastError())
}
```

### 2. Envío a Tópico / Segmento:

```joss
$notify = new Notify()
$res = $notify->apps(["joss_red", "estrella_music"])
    ->segment("noticias")
    ->title("Actualización de la Plataforma")
    ->message("Nuevas funciones disponibles en la app.")
    ->send()
```

### 3. Notificación In-App:

```joss
$notify = new Notify()
$res = $notify->user($userId)
    ->inApp()
    ->title("Mensaje de bienvenida")
    ->message("Gracias por unirte.")
    ->send()
```

### 4. Invocación Directa / Funcional:

```joss
$res = joss_notify::send({
    "app": "joss_red",
    "user": $deviceToken,
    "title": "Aviso Urgente",
    "message": "Por favor revisa tu bandeja de entrada.",
    "type": "push"
})
```

---

## 🛠️ Compilación a Paquete .jp

Para compilar el plugin tras realizar modificaciones:

```bash
joss plugin compile .
joss plugin inspect joss_notify.jp
```
