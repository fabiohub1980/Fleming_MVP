# Guía — Proxy de Make + Google Gemini (gratis) para el chatbot Fleming

El chatbot no puede llamar a la IA directamente desde el navegador (lo bloquea la seguridad
del navegador y expondría la clave). Este proxy en Make hace de intermediario seguro:
recibe la pregunta → llama a Google Gemini con tu clave (guardada en Make) → devuelve la respuesta.

El chatbot ya está adaptado al formato de Gemini. Solo tienes que montar el proxy y pegar su URL.

---

## Antes de empezar: consigue una clave de Gemini (GRATIS, sin tarjeta)
1. Entra en https://aistudio.google.com/apikey (inicia sesión con tu cuenta Google).
2. Pulsa **Create API key** → **Create API key in new project**.
3. Copia la clave (empieza por `AIza...`). La pegarás **solo dentro de Make**.
   > La capa gratuita de Gemini no requiere tarjeta y tiene límites amplios para una demo.

---

## Escenario en Make: "Fleming – Proxy Gemini" (3 módulos)

### Módulo 1 — Webhooks › Custom webhook
1. Crea un escenario nuevo en https://www.make.com.
2. Añade **Webhooks → Custom webhook** → **Add** → nómbralo `Fleming chat proxy` → **Save**.
3. **Copy address to clipboard** → esa es la **URL del proxy** (la usarás al final). Déjalo escuchando.

### Módulo 2 — HTTP › Make a request  (llama a Gemini)
Conecta un módulo **HTTP → Make a request** y configúralo así:

| Campo | Valor |
|---|---|
| **URL** | `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent` |
| **Method** | `POST` |
| **Headers** | (añade estas 2 filas) |
| &nbsp;&nbsp;• `x-goog-api-key` | tu clave `AIza...` |
| &nbsp;&nbsp;• `content-type` | `application/json` |
| **Body type** | `Raw` |
| **Content type** | `JSON (application/json)` |
| **Request content** | el **cuerpo recibido** por el webhook (ver nota abajo) |
| **Parse response** | `No` |

> **Nota "Request content":** el navegador ya envía la pregunta en el formato que espera Gemini.
> Para capturarla: guarda el escenario, deja el webhook escuchando, abre tu web y escribe un
> mensaje en el chat (dará error, es normal: solo sirve para que Make "aprenda" el dato).
> Vuelve a Make: el módulo webhook mostrará el contenido recibido. Selecciónalo en el campo
> **Request content** del módulo HTTP.

### Módulo 3 — Webhooks › Webhook response  (devuelve la respuesta al navegador)
Conecta un módulo **Webhooks → Webhook response** al final:

| Campo | Valor |
|---|---|
| **Status** | `200` |
| **Body** | `{{2.data}}`  (respuesta cruda del módulo HTTP; el "2" es el nº del módulo HTTP) |
| **Custom headers** | (añade estas 2 filas) |
| &nbsp;&nbsp;• `Access-Control-Allow-Origin` | `*` |
| &nbsp;&nbsp;• `Content-Type` | `application/json` |

> El header `Access-Control-Allow-Origin: *` es imprescindible: sin él, el navegador
> descarta la respuesta aunque Make funcione.

### Activar
- Pulsa **Save** y activa el escenario (interruptor **ON / Scheduling**).

---

## Último paso — conectar el chatbot al proxy
1. En `chatbot.html`, sustituye `PEGA_AQUI_LA_URL_DEL_WEBHOOK_PROXY_DE_MAKE`
   por la **URL del webhook** del Módulo 1. (Línea `apiEndpoint:` dentro de `var CONFIG`.)
2. Guarda, y en **GitHub Desktop**: *Commit to main* → *Push origin*.
3. Espera 1-2 min y prueba el chat en https://fabiohub1980.github.io/Fleming_MVP/

---

## Si algo falla
- **Sigue el "error de conexión":** revisa que `apiEndpoint` tiene la URL real (no el placeholder)
  y que el escenario de Make está **ON**.
- **Error 400 / API key / "API_KEY_INVALID":** la clave `x-goog-api-key` está mal copiada.
- **Error 404 / "model not found":** cambia en la URL `gemini-2.0-flash` por `gemini-1.5-flash`.
- **La respuesta llega vacía:** en el Módulo 3 confirma que el Body es `{{2.data}}` y que en
  el Módulo 2 **Parse response** está en `No`.

> Recordatorio: el otro webhook (`webhookUrl`) es **distinto**: solo envía el email de
> escalado al Dpto. Científico. Este proxy nuevo (`apiEndpoint`) es el que hace hablar al chat.
