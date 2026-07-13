# Proyecto Fleming — MVP (Mag-nifico®)

Plataforma web formativa: login → píldora de vídeo → test de comprensión automático → pop-up de descuento, con asistente Fleming (chatbot) integrado.

**Credenciales de acceso**
- Usuario: `Esade`
- Contraseña: `Fleming`

## Contenido
- `index.html` — plataforma (login, dashboard, test y descuento)
- `pildora.mp4` — píldora formativa de Mag-nifico®
- `chatbot.html` — asistente Fleming (se abre en pop-up)
- `.nojekyll` — evita que GitHub Pages procese la carpeta con Jekyll

## Publicar en GitHub Pages

### 1. Crear el repositorio en GitHub
Entra en https://github.com/new y crea un repositorio **vacío** (sin README ni .gitignore).
Ejemplo de nombre: `fleming-mvp`. Puede ser público o privado (para Pages gratis, público).

### 2. Inicializar y subir esta carpeta
Abre PowerShell dentro de esta carpeta y ejecuta (sustituye `TU-USUARIO` y `TU-REPO`):

```powershell
cd "C:\Users\ADMIN\Desktop\ESADE\Unidades\U7\Fleming_MVP"

# Borra cualquier repo git a medias que pudiera existir en la carpeta
if (Test-Path .git) { Remove-Item -Recurse -Force .git }

# Inicializa el repositorio y crea el primer commit
git init
git add -A
git commit -m "MVP Proyecto Fleming: plataforma, pildora, test y chatbot"

# Conecta con tu repositorio de GitHub y sube
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git branch -M main
git push -u origin main
```

> Si Git te pide identificarte, usa tu usuario de GitHub y un **token de acceso personal**
> (Settings → Developer settings → Personal access tokens) como contraseña.
> Necesitas tener [Git instalado](https://git-scm.com/download/win).

### 3. Activar GitHub Pages
En el repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**
→ Branch: `main` / carpeta `/ (root)` → **Save**.

En 1–2 minutos tu MVP estará en:
```
https://TU-USUARIO.github.io/TU-REPO/
```

## Notas
- El vídeo pesa ~75 MB (dentro del límite de 100 MB por archivo de GitHub). La primera subida tarda un poco.
- El chatbot muestra la interfaz siempre. Para que **responda** de verdad, edita en `chatbot.html` el bloque `CONFIG`: `apiEndpoint` (tu backend/proxy que añade la API key en servidor) y opcionalmente `webhookUrl`. La API key **nunca** debe ir en este archivo.
